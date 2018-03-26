Protocol:
Conenction oreinted, full duplex, in sequence and reliable transfer of data.

Connection is identified by source & destination port number in the TCP header (given that the source & destination IP address are unique anyway). Start of the conncetion is using SYN bit in the header and end of teh connection using FIN bit.

Sequence number is used for in-sequence delivery and reliablility. Header carries the send-sequence number and ack-seuquence number (32 bit field). Sequence number is incremented based on the number of bytes transfered and also for how much bytes received in the ack field. There is no constant window size. Header carries the currnet widow size. Also note, the sequence number doesn't start from 0 when SYN bit is sent. It could start at any number and then gets incremented based on number of bytes sent. 

RST bit in the header is used to indicate reset of the connection if there is some ambiguity in the connection establishment or release.

PSH (Push) bit can be used to get the ack of the data asap.
ACK bit indicates the acknowledgment number field is meaningful or not.

    0                   1                   2                   3   
    0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
   |          Source Port          |       Destination Port        |
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
   |                        Sequence Number                        |
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
   |                    Acknowledgment Number                      |
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
   |  Data |           |U|A|P|R|S|F|                               |
   | Offset| Reserved  |R|C|S|S|Y|I|            Window             |
   |       |           |G|K|H|T|N|N|                               |
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
   |           Checksum            |         Urgent Pointer        |
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
   |                    Options                    |    Padding    |
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
   |                             data                              |
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+



Tracing the packets:

tcpdump can be used to dump the TCP packets (snoop in some Sun m/c). Simple usage is dump using TCP port number.

tcpdump port <listen port number>


TCP socket: 

TCP is full duplex, i.e, even when one direction of the connection is closed using FIN other direction can keep successfully sending the data. But there is no such facility in the socket programming. Connection & disconnection of both direction are converged into single socket call (accpet() on server side and connect() on client side).

In case of blocking sockets, successful connection is established if the connect() or accept() returns with success and disconnection is detected using the recv() call returning zero length or send() returning error.

For non-blocking sockets, the situation becomes complicated. On the client side, successful conncetion is indicated by calling the connect() repeatedly until the error code is "Already connected" or getting the  write event using the select(). In case of server, the incoming connection is detected using read event on the listen fd using select() call and after accept(), wait on the write event for the accepted fd using select(). For disconnect indication, it was assumed that read event with select() and followed by recv() returning zero length (or even ioctl with FIONREAD) as disconnect. Looks like that is not the case. At least in the Sun OS, read event comes and receive length is zero and but there is no disconnection (tcpdump shows no FIN/RST/SYN at all) and data keep coming after that also. Only way to detect the disconnection is by sending some data. Since it is socket, sending the data results in RST from other side and then the recv() gives "Connection reset by peer"!. So in case of TCP relay, it is mandatory that we send keep alive messages to detect this kind of scenario! Following program illustrates this point. Just comment the send() in the client side and while running, kill the server, client side doesn't detect at all. Also, running the setup overnight, we will see zero len received even though connection is alive.


sock.c:

#include <stdio.h>
#include <errno.h>
#include <time.h>      // nanosleep
#include <unistd.h>    // sleep() decl
#include <strings.h>   // bzero() decl
#include <stropts.h>   // ioctl() decl
#include <sys/ioctl.h> // FIO defines
#include <sys/socket.h>
#include <netinet/in.h> // sockaddr_in decl

#define MAX_BUF_LEN 100
char buf[MAX_BUF_LEN];

#define LISTEN_PORT 9555
#define SERVER_ADDR 0xAC190003 // 172.25.0.03

#ifdef CLIENT
int
client(void)
{
	int fd;
	int ret;
	int ioctlParam;
	fd_set fdset;
	struct sockaddr_in saddr;
	struct timeval timeout;
	
	fd = socket(PF_INET, SOCK_STREAM, 0);
	if (fd < 0 )
	{
		perror("open error:");
		return (-1);
	}

	ioctlParam = 1; // disable
	ret = ioctl(fd, FIONBIO, &ioctlParam); // Disable block
	if (ret < 0)
	{
		perror("Block disable error:");
		return (-1);
	}

	bzero(&saddr, sizeof (saddr));
	saddr.sin_family = AF_INET;
	saddr.sin_port   = 0;
	saddr.sin_addr.s_addr   = INADDR_ANY;

	ret = bind(fd, (struct sockaddr *)&saddr, sizeof (saddr));
	if (ret < 0)
	{
		perror("Bind error:");
		return (-1);
	}
	
	bzero(&saddr, sizeof (saddr));
	saddr.sin_family = AF_INET;
	saddr.sin_port   = 9555;
	saddr.sin_addr.s_addr   = htonl(SERVER_ADDR);

	while (1)
	{
		ret = connect(fd, (struct sockaddr *)&saddr, sizeof (saddr));
		if (ret < 0)
		{
			perror("Connect error:");
			if ((errno == EISCONN) || (errno == EALREADY))
			{
				break;
			}
			if ((errno == EINPROGRESS) || (errno == EWOULDBLOCK))
			{
				sleep(1);
				continue;
			}
			return (-1);
		}
	}
	
	timeout.tv_sec = 4;
	timeout.tv_usec = 0;
	while (1)
	{
		FD_ZERO(&fdset);
		FD_SET(fd, &fdset);
		ret = select(fd+1, &fdset, 0, 0, &timeout);
		if (ret < 0)
		{
			perror("Select error:");
			continue;
		}
		if (ret == 0)
		{
			printf("Nothing!\n");
			continue;
		}
		if (FD_ISSET(fd, &fdset))
		{
			ioctlParam = 0;
			ret = ioctl(fd, FIONREAD, &ioctlParam);
			if (ret < 0)
			{
				perror("Ioctl FIONREAD error:");
			}
			if (ioctlParam != 0)
			{
				ret = recv(fd, &buf[0], MAX_BUF_LEN, 0);
				if (ret < 0)
				{
					perror("Recv error:");
				}
			}
			else
			{
				// Disconnect or spurious
				ret = recv(fd, &buf[0], MAX_BUF_LEN, 0);
				if (ret < 0)
				{
					perror("Recv:Disconnect!");
					break;
				}
				// Only the send can detect the connection close
				// That is also in the next iteration at ioctl
				ret = send(fd, &buf[0], MAX_BUF_LEN, 0);
				if (ret < 0)
				{
					perror("Send:Disconnect!");
					break;
				}
				printf("Zero len after select!\n");
				sleep(1);
			}
		}
		else
		{
			printf("What is happening: %d\n", ret);
		}
	} /* while 1 */
}

#else

int
server(void)
{
	int fd;
	int connFd;
	int ret;
	int ioctlParam;
	struct sockaddr_in saddr;
	int count;
	struct timespec tspec;
	
	fd = socket(PF_INET, SOCK_STREAM, 0);
	if (fd < 0 )
	{
		perror("open error:");
		return (-1);
	}

	// Keep it blocked socket

	bzero(&saddr, sizeof (saddr));
	saddr.sin_family = AF_INET;
	saddr.sin_port   = LISTEN_PORT;
	saddr.sin_addr.s_addr   = INADDR_ANY;

	ret = bind(fd, (struct sockaddr *)&saddr, sizeof (saddr));
	if (ret < 0)
	{
		perror("Bind error:");
		return (-1);
	}
	
	bzero(&saddr, sizeof (saddr));
	saddr.sin_family = AF_INET;
	saddr.sin_port   = 9555;
	saddr.sin_addr.s_addr   = htonl(0x7F000001); /* Local addr */

	ret = listen(fd, 1);
	if (ret < 0)
	{
		perror("Listen error:");
		return (-1);
	}

	printf("Waiting for connection\n");
	bzero(&saddr, sizeof (saddr));
	connFd = accept(fd, (struct sockaddr *)&saddr, &ret);
	if (connFd < 0)
	{
		perror("Connect error:");
		return (-1);
	}
	
	count = 1;
	while (1)
	{
#if 0
		ret = ioctl(connFd, FIONREAD, &ioctlParam);
		if (ret < 0)
		{
			perror("Ioctl FIONREAD error:");
		}
		printf("Pending: %d\n", ioctlParam);
#endif

		ret = send(connFd, &buf[0], (count % MAX_BUF_LEN)+1, 0);
		if (ret < 0)
		{
			perror("Send error:");
		}
		count++;

		tspec.tv_sec = 0;
		tspec.tv_nsec      = 1000; // 1 usec
		if ((count % 100) == 0)
		{
			nanosleep(&tspec, 0);
		}
	}
}

#endif /* CLIENT */

int
main(int argc, char *argv[])
{
#ifdef CLIENT
	client();
#else
	server();
#endif
}

sock.mak :

all: client server

client: sock.c
	cc -g -o client -DCLIENT -DBSD_COMP -D__EXTENSIONS__ sock.c -lsocket -lnsl

server: sock.c
	cc -g -o server -DBSD_COMP -D__EXTENSIONS__ sock.c -lsocket -lnsl -lrt

clean:
	/bin/rm -f client server



