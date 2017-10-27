% Linux notes
% Ravikiran K.S.
% January 1, 2006

## Linux tips n tricks

### service command
list running services
$ service --status-all

Print the status of any service
$ service httpd status

List all known services (configured via SysV)
$ chkconfig --list

Turn on/off service
$ ntsysv
$ chkconfig service off     Ex. chkconfig httpd off
$ chkconfig service on      Ex. chkconfig ntpd on

### Which package or RPM does this file belongs to?
$ rpm -q -f /usr/bin/hexdump

### Reboot machine when everything is hanging
``
<alt> + <print screen/sys rq> + <R> - <S> - <E> - <I> - <U> - <B>
``

### Get page size under Linux
``
$ getconf PAGESIZE  or  getconf PAGE_SIZE
``

### simple python smtp server
$ python -m smtpd -n -c DebuggingServer localhost:1025

### To check given package existance on Ubuntu
$ dpkg --get-selections | grep php

### To list all binaries belonging to a package on Ubuntu
$ dpkg -L php5-gd

### APR indent style options for 'indent' command (in indent.pro file)
 -i4 -npsl -di0 -br -nce -d0 -cli0 -npcs -nfc1 -nut

### A very simple and useful stopwatch
$ time read (ctrl-d to stop)

### Summarize a patch file
$ cat patches.log | grep "Patch:" | cut -c 1-16 > patches.log.small

### When some output is piped through 'less', it can be saved before exiting by pressing 's'.

### Match both process name and argument list for process being searched.
Similarly, prints a verbose output
$ pgrep -lf screen

### To find and delete all hard links to a file:
$ find /home -xdev -samefile file1 | xargs rm
$ find /home -xdev -inum 2655341

### Replace a file with new file while not loosing original file attributes
This is useful for cases, where you would like to do replace while preserving
the date & time of original file
$ cp --attributes-only file file.tmp
$ cp -l -b -f file file
$ $filter < file > file.tmp &&
$ mv file.tmp file

## Linux Process debugging related commands:

1. stat - display file or filesystem status
[root@xe30-1][~]$ stat rkks/.bashrc
  File: `rkks/.bashrc'
  Size: 124             Blocks: 8          IO Block: 4096   regular file
Device: 802h/2050d      Inode: 220095      Links: 1
Access: (0644/-rw-r--r--)  Uid: (   20/    rkks)   Gid: (    0/    root)
Access: 2008-03-11 12:25:59.000000000 +0530
Modify: 2008-03-11 12:24:43.000000000 +0530
Change: 2008-03-11 12:24:43.000000000 +0530

2. pmap - report memory map of a process
3. pmap_dump - print a list of all registered RPC programs
4. pmap_set - set the list of registered RPC programs
5. pgrep,  pkill  -  look  up or signal processes based on name and other attributes
6. pstree - display a tree of processes
7. pstack - print a stack trace of a running process
8. nm - list symbols from object files
9. ptrace - process trace
10. strace - trace system calls and signals
11. top - display Linux tasks
12. ltrace - A library call trace
13. free - Display amount of free and used memory in the system (also see '/proc/meminfo')
14. uptime - Tell how long the system has been running.
15. slabtop - display kernel slab cache information in real time
16. vmstat - Report virtual memory statistics (also see /proc/stat or /proc/*/stat)
17. w - Show who is logged on and what they are doing.
18. iostat - Report Central Processing Unit (CPU) statistics and input/output statistics for devices and partitions.
19. sar - Collect, report, or save system activity information.
20. mpstat - Report processors related statistics.
21. utmp
22. sa1 - Collect and store binary data in the system activity daily data file.
23. sa2 - Write a daily report in the /var/log/sa directory.
24. fuser - identify processes using files or sockets
25. lsof - list open files
26. access - determine whether a file can be accessed
27. crash - Analyze Linux crash data or a live system
28. netdump - send oops data and memory dumps over the network
29. netstat - Print network connections, routing tables, interface statistics, masquerade connections, and multicast memberships
30. readlink - display value of a symbolic link
31. portmap - DARPA port to RPC program number mapper
32. tcpd - access control facility for internet services
33. rpcinfo - report RPC information
34. hosts_access,  hosts_ctl,  request_init,  request_set - access control library
35. hosts_options - host access control language extensions
36. xinetd - the extended Internet services daemon
37. xinetd.conf - Extended Internet Services Daemon configuration file
38. hardlink - Consolidate duplicate files via hardlinks
39. strings - print the strings of printable characters in files.
40. objdump - display information from object files.
41. readelf - Displays information about ELF files.
42. readprofile - a tool to read kernel profiling information.

