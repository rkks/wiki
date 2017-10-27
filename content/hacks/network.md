% Linux Networking Nits
% Ravikiran K.S.
% January 1, 2006

## List service and their open ports
$ netstat -tulpn
$ lsof                  # List Open Files.
$ lsof -i :<port>       # ex. lsof -i :80

## Show ports user (Who is using my port???)
$ fuser                 # Identify process using file. -n: name (tcp udp), -s: (silent), -k: kill
[root@xe30-1][~]$ fuser -u -n udp 944
here: 944
944/udp:              2037(root)
[root@xe30-1][~]$ fuser -v -n udp 944
here: 944
                     USER        PID ACCESS COMMAND
944/udp              root       2037 f....  rpc.statd
``
[root@xe30-1][~]$ netstat -nlp | grep 944
udp        0      0 0.0.0.0:944                 0.0.0.0:*                               2037/rpc.statd
``

lsof - List Open Files.
fuser - Identify process using file.
[root@xe30-1][~]$ fuser -u -n udp 944
here: 944
944/udp:              2037(root)
[root@xe30-1][~]$ fuser -v -n udp 944
here: 944
                     USER        PID ACCESS COMMAND
944/udp              root       2037 f....  rpc.statd
``
[root@xe30-1][~]$ netstat -nlp | grep 944
udp        0      0 0.0.0.0:944                 0.0.0.0:*                               2037/rpc.statd
[root@xe30-1][~]$ netstat -nlp | grep 944 | awk '{print $4}'
0.0.0.0:944
[root@xe30-1][~]$ netstat -nlp | grep 944 | awk '{print $5}'
0.0.0.0:*
[root@xe30-1][~]$ netstat -nlp | grep 944 | awk '{print $6}'
2037/rpc.statd
``

## Some useful tcpdump options
-n - Do not convert domain names.
-s 0 - Capture entire packet length
-XX - Print entire contents of packet, with ethernet header in hex and ASCII
"dst host <name/IP>" - All packets with destination IP
"src host <name/IP>" - All packets with source IP
"host <name/IP>" - either source or destination IP
"dst port <number>" - All packets with destination port
"src port <number>" - All packets with source port
"port <number>" - either source or destination port

### Capture packets
``
tcpdump -S -tttt -XX -s 0 -w $(uname -n)-$(date +%m%d%y-%H.%M.%S).pcap -i eth2_0 esp &
``

### Sniff bcast packets with given ethertype on given interface
tcpdump -nn -l -e -vvv -XX ether broadcast -y 8080 -i eth4

## Private DNS addresses

Google private DNS addresses : 8.8.8.8 and 8.8.4.4
OpenDNS private DNS addresses : 208.67.222.222 and 208.67.220.220

## Finding interface on which packet would go
use 'route get' command to identify the route on which packet would traverse

## Add static route for particular IP address
- route add [net|host] <Addr> netmask <Mask> [GatewayAddr|-interface ] <metric>
$ route add net 10.10.10.34 netmask 255.255.255.0 192.168.1.1 1

Change /etc/rc2.d/S76static-routes for permanent fix, and then do
$ chmod 744 /etc/rc2.d/S76static-routes

## IPC check
To check on different IPCs (semaphores, shmem, msg-q) opened by given proc use:
$ ipcs -spt
To remove/close those IPCs, use:
$ ipcrm

## Netmask and Gateway

* Configuring Gateway allows switch to talk to external networks. Otherwise, the
accessing host will have to be in same LAN as switch.

* Netmask is a 32 bit mask used to divide IP addresses into subnets. Netmask
determines the number of hosts possible in given subnet. For instance:

/32 - 255.255.255.255 - Each device is in its own subnet. Single IP address.
/31 - 255.255.255.254 - Point-to-Point (P2P) links only.
/30 - 255.255.255.252 - 4 address, 2 hosts. (2 bits => 2^2 = 4 combinations)
/29 - 255.255.255.248 - 8 address, 6 hosts. (3 bits => 2^3 = 8 combinations)
/28 - 255.255.255.240 - 16 address, 14 hosts. (4 bits => 2^4 = 16 combinations)
/24 - 255.255.255.0   - 256 address, 264 hosts (8 bits => 2^8 = 256 combinations)
/16 - 255.255.0.0     - 65536 address, 65534 hosts (16 bits => 2^16 = 65536)
/8  - 255.0.0.0       - 16777216 address, 16777214 hosts (24 bits)
/1  - 1.0.0.0         - 2147483648 address, 2147483646 hosts (31 bits)

In a netmask, two bits are always automatically assigned and cant be used. For
Ex. Given IP address range 192.168.0.0 to 192.168.0.255, The first & last of IP
address of the range when subnetted are reserved as:
- x.x.x.0 - Is always the assigned network address (reserved)
    - subnet-zero OR host-all-zeroes : used to address entire network
    - 192.168.0.0 - subnet-zero OR host-all-zeroes OR zero broadcast
- x.x.x.255 - Is always the assigned broadcast address (reserved)
    - all-ones subnet OR host-all-ones : used as broadcast address in given subnet
    - 192.168.0.255 - host-all-ones OR ones broadcast
- x.x.x.1 to x.x.x.254 - usable/assignable range for end devices in network
    - 192.168.0.1 to 192.168.0.254 - usable range for various hosts

Commonly used netmasks:
- 24-bit netmask - 255.255.255.0, Addressable hosts 254 - Class C
- 16-bit netmask - 255.255.0.0, Addressable hosts 65534 - Class B
- 8-bit netmask - 255.0.0.0, Addressable hosts 16777214 - Class A

0.0.0.0/0 - zero-gateway, I don't gateway. If switch doesn't know IP address it
 forwards to gateway configured

## To cleanup the assigned network addresses in Windows, do:
C:\Documents and Settings\ravikiran> netstat -rn
C:\Documents and Settings\ravikiran> nslookup
C:\Documents and Settings\ravikiran> ping mailhub.ltp.soft.net
C:\Documents and Settings\ravikiran> ping 10.2.1.83
C:\Documents and Settings\ravikiran> ping mailhub.ltp.soft.net
C:\Documents and Settings\ravikiran> nslookup
C:\Documents and Settings\ravikiran> ping mailhub
C:\Documents and Settings\ravikiran> telnet mailhub.ltp.soft.net 25
C:\Documents and Settings\ravikiran> ^]
C:\Documents and Settings\ravikiran> ipconfig /flushdns
C:\Documents and Settings\ravikiran> ipconfig /all
C:\Documents and Settings\ravikiran> tracert mailhub

ipconfig /all
ipconfig /release
ipconfig /flushdns
ipconfig /renew
pathping google.com
tracert google.com
