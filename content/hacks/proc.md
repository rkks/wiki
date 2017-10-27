% Process behavior in Unix Linux
% Ravikiran K.S.
% January 1, 2006

### In Unix, a process can be in either of these states:
D - Uninterruptible sleep (usually IO)
R - Running or runnable (on run queue)
S - Interruptible sleep (waiting for an event to complete)
T - Stopped, either by a job control signal or because it is being traced.
W - paging (not valid since the 2.6.xx kernel)
X - dead (should never be seen)
Z - Defunct ("zombie") process, terminated but not reaped by its parent.

### For BSD formats and when the stat keyword is used, additional characters
may be displayed
``
< - high-priority (not nice to other users)
N - low-priority (nice to other users)
L - has pages locked into memory (for real-time and custom IO)
s - is a session leader
l - is multi-threaded (using CLONE_THREAD, like NPTL pthreads do)
+ - is in the foreground process group
``

### Shell script states
When a script is invoked, it is bound to a terminal (TT) like pts/0, STAT is S+.
If suspended using Ctrl-z, SIGTSTP signal is sent to script and it will have
STAT changed to T. If resumed as a background process using bg, STAT will be S.
You now need to disown the process before exiting the shell for process to
continue as `disown %1`. After exit/logout, the process/shell PPID (parent)
changes to 1 (init) and terminal (TT) is unbound

Note:
1. SIGTSTP is like SIGSTOP except that you can install own signal handler to
ignore it.
2. When a script/process is disowned, it gets removed from jobs list of shell.
Otherwise, if a job is not disowned; then when we do exit in shell, it sends
SIGHUP to all jobs listed. And by default all jobs will be killed. This is done
unless huponexit shell option is disabled by user. If disabled, SIGHUP is not
propagated to all jobs in the job list. One can check/toggle huponexit option
using 'shopt' command.

Commands:
List STAT and PPID for given process
$ ps -o pid,ppid,stat,tty,cmd $(pgrep -f <proc-name>)
List different process in system sorted out by their STAT status
$ ps -e h -o stat | sort | uniq -c | sort -rn
Display the scheduling priorities of different process running (nice numbers)
$ ps -e h -o ni | sort -n | uniq -c
$ ps -e -o stat,cmd | awk '{if ($1 ~/R/) print}'

### Process nice numbers range from 19 (low priority) to -20 (high priority)

On most of the systems, only few processes can be seen in R (running or
run-queued) status.  That is because most of the process will be
sleeping/waiting on some event/resource. Hence, they don't actually run.

### To know total number of processors recognized by OS.
$ grep processor /proc/cpuinfo

### To get a sense of how running procs change over time, throw 'ps' under watch
$ watch -n 1 "ps -e -o stat,cmd | awk '{if (\$1 ~/R/) print}'"

### Zombie process
A process becomes a zombie when it has completed execution but hasn't been
reaped by its parent.  Usually a bug. zombies are undesirable because they take
up proc IDs, which are a limited resource.  A simple example:

#include <sys/types.h>

int main () {
    pid_t child_pid = fork();
    if (child_pid > 0) {
        sleep(60);
    }
    return 0;
}

Parent exits, while child sleeps for 60s. Here is a catch. This process, though
should be displayed as defunct, it doesn't remain same forever. This is
because, soon that parent dies, the init takes over as the parent, quickly
reaps the child by making wait system call on child process ID.  This removes
child process from proc table. Otherwise, if the child were to continue with
execution forever, it would show as 'zombie' process.

### Uninterruptible sleep

A process is put in an uninterruptible sleep when it needs to wait on something
(typically I/O) and shouldn't be handling signals while waiting. This means you
can't kill it, because all kill does is send it signals. This might happen in
the real world if you unplug your NFS server while other machines have open
network connections to it. A simple example can be to use vfork(). vfork is
like fork, except the address space is not copied from the parent into the
child, in anticipation of an exec which would just throw out the copied data.
In this case, the parent waits uninterruptibly (by way of ``wait_on_completion``)
on the child's exec or exit:

int main() {
    vfork();
    sleep(60);
    return 0;
}

Here, parent waits for child to either call exec or exit, then the parent
itself exits after 60s.

One neat (mischievous?) thing about this script is that processes in an
uninterruptible sleep contribute to the load average for a machine. So you
could run this script 100 times to temporarily give a machine a load average
elevated by 100, as reported by uptime.

``
Adopted: https://blogs.oracle.com/ksplice/entry/disown_zombie_children_and_the
``

## Fork System Call : int fork()

### Theory
fork() causes a process to duplicate. The child process is an almost-exact 
duplicate of the original parent process; it inherits a copy of its parent's 
code, data, stack, open file descriptors, and signal table. However, the parent 
and child have different process id numbers and parent process id numbers.

If fork() succeeds, it returns the PID of the child to the parent process, 
and returns 0 to the child process. If it fails, it returns -1 to the parent 
process and no child is created.

fork() is a strange system call, because one process(the original) calls it, but 
two processes(the original and its child) return from it. Both processes continue 
to run the same code concurrently, but have completely separate stack and data 
spaces.

A process may obtain its own process id and parent process id numbers by 
using the getpik() and getppid() system calls, respectively. Here's a synopsis 
of these system calls:

System Call: int getpid()
             int getppid()

getpid() and getppid() return a process's id and parent process's id numbers,
respectively. They always succeed. The parent process id number of PID 1 is 1.

To illustrate the operation of fork(), here's a small program that duplicates and 
then branches based on the return value of fork():

$ cat fork.c
#include <stdio.h>
main()
{
    int pid;
    printf("I'm the original process with PID %d and PPID %d.\n",
           getpid(),getppid());
    pid=fork();  /* Duplicate. Child and parent continue from here.*/
    if (pid!=0)  /* pid is non-zero, so I must be the parent  */
        {
           printf("I'm the parent process with PID %d and PPID %d.\n",
                   getpid(),getppid());
           printf("My child's PID is %d.\n", pid);
        }
    else  /* pid is zero, so I must be the child. */
        {
           printf("I'm the child process with PID %d and PPID %d.\n",
                   getpid(),getppid());
        }
    printf("PID %d terminates.\n",pid);  /* Both processes execute this */
}

$fork.exe                ... run the program.
I'm the original process with PID 13292 and PPID 13273.
I'm the parent process with PID 13292 and PPID 13273.
My child's PID is 13293.
I'm the child process with PID 13293 and PPID 13292.
PID 13293 terminates.     ... child terminates. 
PID 13292 terminates.     ... parent terminates.

     WARNING: As you will soon see, it is dangerous for a parent to terminate
without waiting for the death of its child. The only reason that the parent
doesn't wait for its child in this example is because I haven't yet described
the wait() system call!

### Orphan Processes

If a parent dies before its child, the child is automatically adopted by the
original "init" proces, PID 1. To illustrate this, I modified the previous
program by inserting a sleep statement into the child's code. This ensured that
the parent process terminated before the child.
Here's the program and the resultant output:

$cat orpan.c             ... list the program
#include <stdio.h>
main()
{
    int pid;
    printf("I'm the original process with PID %d and PPID %d.\n",
           getpid(),getppid());
    pid=fork();  /* Duplicate. Child and parent continue from here.*/
    if (pid!=0)  /* Branch based on return value from fork() */
        {  /* pid is non-zero, so I must be the parent  */
           printf("I'm the parent process with PID %d and PPID %d.\n",
                   getpid(),getppid());
           printf("My child's PID is %d.\n", pid);
        }
    else  
        {  /* pid is zero, so I must be the child. */
           sleep(5); /*Make sure that the parent terminates first. */
           printf("I'm the child process with PID %d and PPID %d.\n",
                  getpid(),getppid());
        }
    printf("PID %d terminates.\n",pid);  /* Both processes execute this */
}

$orphan.exe                ... run the program.
I'm the original process with PID 13364 and PPID 13346.
I'm the parent process with PID 13364 and PPID 13346.
PID 13364 terminates.
I'm the child process with PID 13365 and PPID 1.   ...orphaned!
PID 13365 terminates.     ... child terminates. 
$ -

System Call: int exit(int status)
exit() closes all of a process's file descriptors, deallocates its code, data,
and stack, and then terminates the process. When a child process terminates, it
sends its parent a SIGCHLD signal and waits for its termination code status to be
accepted. A process that is waiting for its parent to accept its return code is
called a zombie process. A parent accepts a child's termination code by executing
wait(), which is described shortly.
    The kernel ensures that all of a terminating process's children are orphaned
and adopted by "init" by setting their PPID to 1. The "init" process always accepts
its children's termination codes.
    exit() never returns.

The termination code of a child process may be used for a variety of purposes
by the parent process. Shells may access the termination code of their last child
process via one of their special variables. For example, the C shell stores the
ternimation code of the last command in the variable $status:

%cat exit.c           ...list the program
#include <stdio.h>
main()
{
 printf("I'm going to exit with return code 42\n");
 exit(42);
}

% exit.exe          ... run the program
I'm going to exit with return code 42
% echo $status      ... display the termination code.
42
% -

Zombie Processes
$ cat zombie.c       ... list the program.
#include <stdio.h>
main()
{
    int pid;
    pid=fork();  /* Duplicate  */
    if (pid!=0) /* Branch based on return value from fork() */
       {
          while (1) /* never terminate, and never execute a wait() */
             sleep(1000);
       }
    else
       {
          exit(42); /* Exit with a silly number */
       }
}

$ zombie.exe &   ... execute the program in the background.
[1] 13545
$ ps
  PID TT STAT TIME COMMAND
13535 p2 s    0:00 -ksh(ksh) ...the shell
13545 p2 s    0:00 aombie.exe...the parent process
13536 p2 z    0:00 <defunct> ...the zombie child process
13537 p2 R    0:00 ps
$ kill 13545                 ... kill the parent process.
[1]  Terminated   zombie.exe
$ ps                         ... notice the zombie is gone now.
   PID TT STAT TIME COMMAND
13535 p2 s    0:00 -csh(csh)
13548 p2 R    0:00 ps

Waiting For A Child: wait()

A parent process may wait for one of its children to terminate and then accept
its child's termination code by executing wait():

### System Call: int wait(int* status)
wait() causes a process to suspend until one of its children terminates. A
successful call to wait() returns the pid of the child that terminated and
places a status code into status that is encoded as follows:

. If the rightmost byte of status is zero, the leftmost byte conains the low
eight bits of the value returned by chils's exit()/return().
. If the rightmost byte is non-zero, the rightmost seven bits are equal to the
number of the signal that caused the child to terminate, and the remaining bit
of the rightmost byte is set to 1 if the child produced a core dump.

If a process executes a wait() and has no children, wait() returns immediately
with -1. If a process executes a wait() and one or more of its children are already
zombies, wait() returns immediately with the status of one of the zombies.

$ cat wait.c
#include <stdio.h>
main()
{
    int pid, status, childPid;
    printf("I'm the parent process and my PID is %d\n", getpid());
    pid=fork();  /* Duplicate.*/
    if (pid!=0)  /* Branch based on return value from fork() */
        {
           printf("I'm the parent process with PID %d and PPID %d.\n",
                   getpid(),getppid());
           childPid=wait(&status); /* wait for a child to terminate */
           printf("A child with PID %d terminated with exit code %d\n",
                  childPid, status>>8);
        }
    else  
        {
           printf("I'm the child process with PID %d and PPID %d.\n",
                   getpid(),getppid());
           exit(42);  /* Exit with a silly number. */
        }
    printf("PID %d terminates.\n",pid); 
}

$wait.exe                ... run the program.
I'm the parent process and my PID is 13464
I'm the child process with PID 13465 and PPID 13464.
I'm the parent process with PID 13464 and PPID 13409
A child with PID 13465 terminated with exit code 42
PID 13465 terminates
$ - 

