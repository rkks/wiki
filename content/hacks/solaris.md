% Solaris notes
% Ravikiran K.S.
% January 1, 2006

## Tips and tricks
### Failsafe booting in Solaris
> boot -F failsafe

### sniff packets on network card
$ snoop -d uplink0 -s 250 -o uplink0.cap arp > uplink0.log 2>&1 &

### Check file system
$ dumpe2fs [mount point]  - gives file system statistics , options & other datails for given mount point.

### Dump system call tracing for given process
$ strace [binary name]    - shows system call trace by given binay
    - truss is the command used in solaris

* Check and enable http service (if not already enabled)
$ svcs -a | grep -i http                    # gives service path and status
$ svcadm -v enable /network/http:apache2    # enable http service
$ svcs -p /network/http:apache2             # lists processes for service
$ svcs -x http OR $ svcs -l http            # provides detailed info
$ svcadm restart /network/http:apache2      # restart http service
$ svcadm disable /network/http:apache2      # disable http service

## Solaris process debugging tools

### Solaris 8 Process debugging Tools

     /usr/bin/pflags [ -r ]  [ pid | core ]  ...
     /usr/bin/pcred [ pid | core ]  ...
     /usr/bin/pmap [ -rxlF ]  [ pid | core ]  ...
     /usr/bin/pldd [ -F ]  [ pid | core ]  ...
     /usr/bin/psig pid ...
     /usr/bin/pstack [ -F ]  [ pid | core ]  ...
     /usr/bin/pfiles [ -F ]  pid ...
     /usr/bin/pwdx [ -F ]  pid ...
     /usr/bin/pstop pid ...
     /usr/bin/prun pid ..
     /usr/bin/pwait [ -v ]  pid ...
     /usr/bin/ptree [ -a ]  [  [ pid | user ]  ... ]
     /usr/bin/ptime command  [ arg ... ]
     /usr/bin/pgrep  [  -flnvx  ]   [  -d delim  ]   [  -P ppidlist  ]
     [ -g pgrplist ]  [ -s sidlist ]  [ -u euidlist ]  [ -U uidlist ]
     [ -G gidlist ]  [ -J projidlist ]   [  -t termlist  ]   [ -T
     taskidlist ]  [ pattern ]
     /usr/bin/pkill [ -signal ]  [ -fnvx ]  [ -P ppidlist ]  [ -g
     pgrplist ]   [  -s sidlist  ]   [  -u euidlist  ]   [ -U uidlist]
     [ -G gidlist  ]   [  -J projidlist  ]   [  -t termlist  ]    [-T
     taskidlist ]  [ pattern ]

### Solaris 9 Process Tools
     /usr/bin/pflags [-r] [pid | core] ...
     /usr/bin/pcred [pid | core] ...
     /usr/bin/pldd [-F] [pid | core] ...
     /usr/bin/psig [-n] pid...
     /usr/bin/pstack [-F] [pid | core] ...
     /usr/bin/pfiles [-F] pid...
     /usr/bin/pwdx [-F] pid...
     /usr/bin/pstop pid...
     /usr/bin/prun pid...
     /usr/bin/pwait [-v] pid...
     /usr/bin/ptree [-a] [pid | user] ...
     /usr/bin/ptime command [arg...]
     /usr/bin/pmap -[xS] [-rslF] [pid | core] ...
     /usr/bin/pgrep  [-flvx]  [-n  |  -o]   [-d delim]  [-P ppidlist]   [-
     g pgrplist]   [-s sidlist]   [-u euidlist]  [-U uidlist]  [-
     G gidlist]  [-J projidlist]  [-t termlist]   [-T taskidlist]
     [pattern]
     /usr/bin/pkill  [-signal]  [-fvx]  [-n  |   -o]    [-P ppidlist]   [-
     g pgrplist]   [-s sidlist]   [-u euidlist]  [-U uidlist]  [-
     G gidlist]  [-J projidlist]  [-t termlist]   [-T taskidlist]
     [pattern]
     /usr/bin/plimit [-km] pid...
                     {-cdfnstv} soft,hard... pid...
     /usr/bin/ppgsz  [-F] -o option[,option]  cmd | -p pid...
     /usr/bin/prctl [-t [basic | privileged | system] ] [ -e | -d  action]
                    [-rx] [ -n name [-v value]] [-i idtype] [id...]
     /usr/bin/preap [-F] pid
     /usr/bin/pargs [-aceFx] [pid | core] ...

### Solaris Process Tool Examples

sol8$ pflags $$
482764: -ksh
        data model = _ILP32  flags = PR_ORPHAN
  /1:   flags = PR_PCINVAL|PR_ASLEEP [ waitid(0x7,0x0,0xffbff938,0x7) ]


sol8$ pcred $$
482764: e/r/suid=36413  e/r/sgid=10
        groups: 10 10512 570

sol8$ pldd $$
482764: -ksh
/usr/lib/libsocket.so.1
/usr/lib/libnsl.so.1
/usr/lib/libc.so.1
/usr/lib/libdl.so.1
/usr/lib/libmp.so.2

sol8$ psig $$
15481:  -zsh
HUP     caught  0
INT     blocked,caught  0
QUIT    blocked,ignored
ILL     blocked,default
TRAP    blocked,default
ABRT    blocked,default
EMT     blocked,default
FPE     blocked,default
KILL    default
BUS     blocked,default
SEGV    blocked,default
SYS     blocked,default
PIPE    blocked,default
ALRM    blocked,caught  0
TERM    blocked,ignored
USR1    blocked,default
USR2    blocked,default
CLD     caught  0
PWR     blocked,default
WINCH   blocked,caught  0
URG     blocked,default
POLL    blocked,default
STOP    default
TSTP    blocked,ignored
CONT    blocked,default
TTIN    blocked,ignored
TTOU    blocked,ignored
VTALRM  blocked,default
PROF    blocked,default
XCPU    blocked,default
XFSZ    blocked,default
WAITING blocked,default
LWP     blocked,default
FREEZE  blocked,default
THAW    blocked,default
CANCEL  blocked,default
LOST    blocked,default
RTMIN   blocked,default
RTMIN+1 blocked,default
RTMIN+2 blocked,default
RTMIN+3 blocked,default
RTMAX-3 blocked,default
RTMAX-2 blocked,default
RTMAX-1 blocked,default
RTMAX   blocked,default

sol8$ pstack 5591
5591:   /usr/local/mozilla/mozilla-bin
-----------------  lwp# 1 / thread# 1  --------------------
 fe99a254 poll     (513d530, 4, 18)
 fe8dda58 poll     (513d530, fe8f75a8, 18, 4, 513d530, ffbeed00) + 5c
 fec38414 g_main_poll (18, 0, 0, 27c730, 0, 0) + 30c
 fec37608 g_main_iterate (1, 1, 1, ff2a01d4, ff3e2628, fe4761c9) + 7c0
 fec37e6c g_main_run (27c740, 27c740, 1, fe482b30, 0, 0) + fc
 fee67a84 gtk_main (b7a40, fe482874, 27c720, fe49c9c4, 0, 0) + 1bc
 fe482aa4 ???????? (d6490, fe482a6c, d6490, ff179ee4, 0, ffe)
 fe4e5518 ???????? (db010, fe4e5504, db010, fe4e6640, ffbeeed0, 1cf10)
 00019ae8 ???????? (0, ff1c02b0, 5fca8, 1b364, 100d4, 0)
 0001a4cc main     (0, ffbef144, ffbef14c, 5f320, 0, 0) + 160
 00014a38 _start   (0, 0, 0, 0, 0, 0) + 5c
-----------------  lwp# 2 / thread# 2  --------------------
 fe99a254 poll     (fe1afbd0, 2, 88b8)
 fe8dda58 poll     (fe1afbd0, fe840000, 88b8, 2, fe1afbd0, 568) + 5c
 ff0542d4 ???????? (75778, 2, 3567e0, b97de891, 4151f30, 0)
 ff05449c PR_Poll  (75778, 2, 3567e0, 0, 0, 0) + c
 fe652bac ???????? (75708, 80470007, 7570c, fe8f6000, 0, 0)
 ff13b5f0 Main__8nsThreadPv (f12f8, ff13b5c8, 0, 0, 0, 0) + 28
 ff055778 ???????? (f5588, fe840000, 0, 0, 0, 0)
 fe8e4934 _lwp_start (0, 0, 0, 0, 0, 0)
-----------------  lwp# 3 / thread# 3  --------------------
 fe8e4a74 SYS#77   (0, 0, 0)
 fe8e2448 cond_wait_queue (1, f5670, 0, fe1cbe00, 0, fe8f6000) + cc
 fe8e2bcc cond_wait (125ee8, f5670, 125ee8, 6b7800, 6b65d4, fe840298) + 10
 fe8e2c08 pthread_cond_wait (125ee8, f5670, fe65d5f0, 0, fe64f300, 6b65d4) + 8
 ff05054c PR_WaitCondVar (125ee0, ffffffff, 689c90, ff390ce4, ff3e2628, ff0a3daf) + 64
 fe65cd60 ???????? (f5634, fe65aebc, 6b65c0, 0, 0, 0)
 fe65c728 ???????? (f55f8, fe65c704, f55fc, fe8f6000, 0, 0)
 ff13b5f0 Main__8nsThreadPv (118b70, ff13b5c8, 0, 0, 0, 0) + 28
 ff055778 ???????? (f5730, fe840200, 0, 0, 0, 0)
 fe8e4934 _lwp_start (0, 0, 0, 0, 0, 0)
-----------------  lwp# 5302 / thread# 5302  --------------------
 fe8e4a74 SYS#77   (0, fc7dfb80, 0)
 fe8e2448 cond_wait_queue (1, f90330, 0, fe1ca940, 0, fe8f6000) + cc
 fe8e29ac cond_wait_common (0, 0, fc7dfb80, 0, 0, fe8f6000) + 1ec
 fe8e2e18 _cond_timedwait (0, 0, fc7dfcb0, 41ffad0, f90330, 1) + 1f0
 fe8e2e48 cond_timedwait (41ffad0, f90330, fc7dfcb0, 41ffad0, ff141960, ff00) + 14
 fe8e2e88 pthread_cond_timedwait (41ffad0, f90330, fc7dfcb0, ff17797c, 1a7f2c8, fc7dfcb8) + c
 ff050358 ???????? (41ffad0, f90330, 0, ff152a3c, 1526858, fc7dfcd8)
 ff050564 PR_WaitCondVar (41ffac8, 186a0, 13a9180, ff152964, 0, ff152a84) + 7c
 ff050814 PR_Wait  (f90328, 186a0, 539c118, ff13aa64, 0, 0) + 18
 fcacf8e4 ???????? (1a7f0b8, fc7dfdf8, 1a7f130, ff13b0b8, 0, 0)
 fcacf05c ???????? (0, fcaceed4, 1a7f0bc, fe8f6000, 0, 0)
 ff13b5f0 Main__8nsThreadPv (452e648, ff13b5c8, 0, 0, 0, 0) + 28
 ff055778 ???????? (391d408, fe841000, 0, 0, 0, 0)
 fe8e4934 _lwp_start (0, 0, 0, 0, 0, 0)
-----------------  lwp# 5 / thread# 5  --------------------
 fe8e4a74 SYS#77   (0, fd87fc80, 0)
 fe8e2448 cond_wait_queue (1, cd688, 0, fe1cf400, 0, fe8f6000) + cc
 fe8e29ac cond_wait_common (0, 0, fd87fc80, 0, 0, fe8f6000) + 1ec
 fe8e2e18 _cond_timedwait (0, 0, fd87fdb0, ca8d0, cd688, 1) + 1f0
 fe8e2e48 cond_timedwait (ca8d0, cd688, fd87fdb0, ca8d0, 0, 1526858) + 14
 fe8e2e88 pthread_cond_timedwait (ca8d0, cd688, fd87fdb0, 2710, 0, fd87fdb8) + c
 ff050358 ???????? (ca8d0, cd688, 609d, b7a40, 0, 0)
 ff050564 PR_WaitCondVar (ca8c8, 609d, 0, b9865b88, 0, 0) + 7c
 ff13ecc8 Run__11TimerThread (d6ea8, ff13eb1c, d6ea8, fe8f6000, 0, 0) + 1ac
 ff13b5f0 Main__8nsThreadPv (cb868, ff13b5c8, 0, 0, 0, 0) + 28
 ff055778 ???????? (1a0618, fe840600, 0, 0, 0, 0)
 fe8e4934 _lwp_start (0, 0, 0, 0, 0, 0)
-----------------  lwp# 4840 / thread# 4840  --------------------
 fe8e4a74 SYS#77   (0, 0, 0)
 fe8e2448 cond_wait_queue (1, f5c88, 0, fe1cbac0, 0, fe8f6000) + cc
 fe8e2bcc cond_wait (125e70, f5c88, 125e70, 0, fe6418b4, fe840a98) + 10
 fe8e2c08 pthread_cond_wait (125e70, f5c88, 0, ff0f6004, fe6559a4, ff152844) + 8
 ff05054c PR_WaitCondVar (125e68, ffffffff, f5bc8, ff0f64cc, 0, 0) + 64
 ff13c56c GetRequest__12nsThreadPoolP9nsIThread (f56d0, 3845470, 0, 0, 0, 0) + 1d8
 ff13cd08 Run__20nsThreadPoolRunnable (50f62f0, ff13cce4, 50f62f0, fe8f6000, 0, 0) + 24
 ff13b5f0 Main__8nsThreadPv (3845470, ff13b5c8, 0, 0, 0, 0) + 28
 ff055778 ???????? (1619a08, fe840a00, 0, 0, 0, 0)
 fe8e4934 _lwp_start (0, 0, 0, 0, 0, 0)
-----------------  lwp# 3654 / thread# 3654  --------------------
 fe8e4a74 SYS#77   (0, 0, 0)
 fe8e2448 cond_wait_queue (1, 22e4960, 0, fe1c8700, 0, fe8f6000) + cc
 fe8e2bcc cond_wait (40ff718, 22e4960, 40ff718, 5, fe956618, fe840898) + 10
 fe8e2c08 pthread_cond_wait (40ff718, 22e4960, 0, fd23ff1c, 0, 0) + 8
 ff05054c PR_WaitCondVar (40ff710, ffffffff, 0, fd23ff1c, ffffffff, 0) + 64
 ff058e40 ???????? (0, ff058d58, 0, 0, 0, 0)
 ff055778 ???????? (1f18868, fe840800, 0, 0, 0, 0)
 fe8e4934 _lwp_start (0, 0, 0, 0, 0, 0)

sol8$ pfiles $$
pfiles $$
15481:  -zsh
  Current rlimit: 256 file descriptors
   0: S_IFCHR mode:0620 dev:118,0 ino:459678 uid:36413 gid:7 rdev:24,11
      O_RDWR
   1: S_IFCHR mode:0620 dev:118,0 ino:459678 uid:36413 gid:7 rdev:24,11
      O_RDWR
   2: S_IFCHR mode:0620 dev:118,0 ino:459678 uid:36413 gid:7 rdev:24,11
      O_RDWR
   3: S_IFDOOR mode:0444 dev:250,0 ino:51008 uid:0 gid:0 size:0
      O_RDONLY|O_LARGEFILE FD_CLOEXEC  door to nscd[328]
  10: S_IFCHR mode:0620 dev:118,0 ino:459678 uid:36413 gid:7 rdev:24,11
      O_RDWR|O_LARGEFILE

sol8$ pwdx $$
15481:  /home/rmc

sol8$ pstop $$
[argh!]

sol8$ prun 21523

sol8$ pwait 23141

sol8$ ptree $$
285   /usr/sbin/inetd -ts
  15554 in.rlogind
    15556 -zsh
      15562 ksh
        15657 ptree 15562

sol9$ pmap -x $$
492328: -ksh
 Address  Kbytes     RSS    Anon  Locked Mode   Mapped File
00010000     192     192       -       - r-x--  ksh
00040000       8       8       8       - rwx--  ksh
00042000      40      40       8       - rwx--    [ heap ]
FF180000     680     680       -       - r-x--  libc.so.1
FF23A000      24      24       -       - rwx--  libc.so.1
FF240000       8       8       8       - rwx--  libc.so.1
FF280000     576     576       -       - r-x--  libnsl.so.1
FF310000      40      40       -       - rwx--  libnsl.so.1
FF31A000      24      16       -       - rwx--  libnsl.so.1
FF350000      16      16       -       - r-x--  libmp.so.2
FF364000       8       8       -       - rwx--  libmp.so.2
FF380000      40      40       -       - r-x--  libsocket.so.1
FF39A000       8       8       -       - rwx--  libsocket.so.1
FF3A0000       8       8       -       - r-x--  libdl.so.1
FF3B0000       8       8       8       - rwx--    [ anon ]
FF3C0000     152     152       -       - r-x--  ld.so.1
FF3F6000       8       8       8       - rwx--  ld.so.1
FFBFC000      16      16       8       - rw---    [ stack ]
-------- ------- ------- ------- -------
total Kb    1856    1848      48       -

sol8$ pgrep -u rmc
481
480
478
482
483
484
.....

     /usr/bin/plimit [-km] pid...
                     {-cdfnstv} soft,hard... pid...
     /usr/bin/ppgsz  [-F] -o option[,option]  cmd | -p pid...
     /usr/bin/prctl [-t [basic | privileged | system] ] [ -e | -d  action]
                    [-rx] [ -n name [-v value]] [-i idtype] [id...]
     /usr/bin/preap [-F] pid
     /usr/bin/pargs [-aceFx] [pid | core] ...

## MDB Debugger Tips n Tricks

1. ::walk thread | ::findstack ! munges
2. 30013e05220::findstack
3. ::walk thread | ::findstack ! grep biowait | wc -l
4. 3000eda5328::as2proc | ::ps
