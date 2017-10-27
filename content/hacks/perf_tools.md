% Performance Profiling on Linux
% Ravikiran K.S.
% January 1, 2006

## Using Perf on Linux

System wide profiling :- Linux kernel has recently implemented a very useful
'perf' infrastructure for profiling various CPU and software events. To get
the perf command, install linux-tools-common on ubuntu, linux-base on debian,
perf-utils on archlinux, or perf on fedora. Then
$ perf record -a -g sleep 10  # record system for 10s
$ perf report --sort comm,dso # display report using kernel config like menu

``
profiling can be problematic on x86_64, due to -fno-omit-frame-pointer being
removed to increase performance
``

Application level profiling :- One can use perf to profile a particular
command too, with a variant of the above, like 'perf record -g $command'. Ex
``
$ perf list
$ perf record -e stalled-cycles -F 10000 ./git gc
$ perf report
$ perf annotate find_pack_entry_one
$ perf stat --sync --repeat 10 ./git gc
$ perf record --event=L1-dcache-load-misses ./perftest
$ perf report --stdio
$ perf record -g ./perftest
$ perf report -g --stdio
``

Alternatively, valgrind can be used.
``
$ valgrind --tool=callgrind ./a.out
$ kcachegrind callgrind.out.*
``

Profiling hardware events :- Efficient use of the memory hierarchy is crucial
for performance. Newer CPUs are providing counters to help tune your use of
this hierarchy. Taking example code like below:
static char array[1000][1000];
int main (void)
{
	int i, j;
	for (i = 0; i < 1000; i++)
		for (j = 0; j < 1000; j++)
			array[j][i]++;
	return 0;
}
On hardware that supports enumerating cache hits and misses, you can run:
$ perf stat --repeat 10 -e cycles:u -e instructions:u -e l1-dcache-loads:u \
-e l1-dcache-load-misses:u ./a.out

Just changing array[j][i]++; to array[i][j]++; can do miracles. Similarly
$ perf top -e l1-dcache-load-misses -e l1-dcache-loads

Specialised profiling :- To profile system entry-points (syscalls), one can
do:
$ strace -c $cmd
$ ltrace -c $cmd

Bootchart application can be used to profile application/service startup
times on different platforms. It is included in most of the distros.

LatencyTop can be used to see resource usage bottlenecks in system.

## GProf:

GNU Profiling Tools is used for profiling a code, checking for mostly executed
API's and least executed ones etc.

Simple steps to profile the code using gprof is:
* compile and link the code using -pg option
* run the application. it will create gmon.out in current folder of app
* in folder of gmon.out, do $ gprof <path-to-app>

[rkks@in-fuchsia][~/test]$ gprof a.out
gmon.out: No such file or directory

[rkks@in-fuchsia][~/test]$ gcc -o test_sw_num -g -pg test_sw_num.c
test_sw_num.c: In function `l2ha_switch_id':
test_sw_num.c:26: warning: initialization makes pointer from integer without a c                                                                             ast
[rkks@in-fuchsia][~/test]$
[rkks@in-fuchsia][~/test]$ ./test_sw_num
Global switch id is: 0
Damon : Opened socket , mac 00:52:BB:40:06:41
Damon : Opened socket , mac 00:52:BB:40:06:44

[rkks@in-fuchsia][~/test]$ ls -l gmon.out
-rw-r--r--  1 rkks ce    491 May 13 16:10 gmon.out

[rkks@in-fuchsia][~/test]$ gprof test_sw_num > test_sw_num.prof
[rkks@in-fuchsia][~/test]$ cat test_sw_num.prof
Flat profile:

Each sample counts as 0.01 seconds.
 no time accumulated

  %   cumulative   self              self     total
 time   seconds   seconds    calls  Ts/call  Ts/call  name
  0.00      0.00     0.00        1     0.00     0.00  get_sys_mac
  0.00      0.00     0.00        1     0.00     0.00  l2ha_switch_id

 %         the percentage of the total running time of the
time       program used by this function.

cumulative a running sum of the number of seconds accounted
 seconds   for by this function and those listed above it.

 self      the number of seconds accounted for by this
seconds    function alone.  This is the major sort for this
           listing.

calls      the number of times this function was invoked, if
           this function is profiled, else blank.

 self      the average number of milliseconds spent in this
ms/call    function per call, if this function is profiled,
           else blank.

 total     the average number of milliseconds spent in this
ms/call    function and its descendents per call, if this
           function is profiled, else blank.

name       the name of the function.  This is the minor sort
           for this listing. The index shows the location of
           the function in the gprof listing. If the index is
           in parenthesis it shows where it would appear in
           the gprof listing if it were to be printed.

                     Call graph (explanation follows)


granularity: each sample hit covers 2 byte(s) no time propagated

index % time    self  children    called     name
                0.00    0.00       1/1           main [9]
[1]      0.0    0.00    0.00       1         get_sys_mac [1]
-----------------------------------------------
                0.00    0.00       1/1           main [9]
[2]      0.0    0.00    0.00       1         l2ha_switch_id [2]
-----------------------------------------------

 This table describes the call tree of the program, and was sorted by
 the total amount of time spent in each function and its children.

 Each entry in this table consists of several lines.  The line with the
 index number at the left hand margin lists the current function.
 The lines above it list the functions that called this function,
 and the lines below it list the functions this one called.
 This line lists:
     index      A unique number given to each element of the table.
                Index numbers are sorted numerically.
                The index number is printed next to every function name so
                it is easier to look up where the function in the table.

     % time     This is the percentage of the `total' time that was spent
                in this function and its children.  Note that due to
                different viewpoints, functions excluded by options, etc,
                these numbers will NOT add up to 100%.

     self       This is the total amount of time spent in this function.

     children   This is the total amount of time propagated into this
                function by its children.

     called     This is the number of times the function was called.
                If the function called itself recursively, the number
                only includes non-recursive calls, and is followed by
                a `+' and the number of recursive calls.

     name       The name of the current function.  The index number is
                printed after it.  If the function is a member of a
                cycle, the cycle number is printed between the
                function's name and the index number.


 For the function's parents, the fields have the following meanings:

     self       This is the amount of time that was propagated directly
                from the function into this parent.

     children   This is the amount of time that was propagated from
                the function's children into this parent.

     called     This is the number of times this parent called the
                function `/' the total number of times the function
                was called.  Recursive calls to the function are not
                included in the number after the `/'.

     name       This is the name of the parent.  The parent's index
                number is printed after it.  If the parent is a
                member of a cycle, the cycle number is printed between
                the name and the index number.

 If the parents of the function cannot be determined, the word
 `<spontaneous>' is printed in the `name' field, and all the other
 fields are blank.

 For the function's children, the fields have the following meanings:

     self       This is the amount of time that was propagated directly
                from the child into the function.

     children   This is the amount of time that was propagated from the
                child's children to the function.

     called     This is the number of times the function called
                this child `/' the total number of times the child
                was called.  Recursive calls by the child are not
                listed in the number after the `/'.

     name       This is the name of the child.  The child's index
                number is printed after it.  If the child is a
                member of a cycle, the cycle number is printed
                between the name and the index number.

 If there are any cycles (circles) in the call graph, there is an
 entry for the cycle-as-a-whole.  This entry shows who called the
 cycle (as parents) and the members of the cycle (as children.)
 The `+' recursive calls entry shows the number of function calls that
 were internal to the cycle, and the calls entry for each member shows,
 for that member, how many times it was called from other members of
 the cycle.

Index by function name

   [1] get_sys_mac             [2] l2ha_switch_id (test_sw_num.c)

### Interesting gprof options
"-z"
       "--display-unused-functions"
           If you give the -z option, "gprof" will mention  all  functions  in
           the  flat  profile, even those that were never called, and that had
           no time spent in them.  This is useful in conjunction with  the  -c
           option for discovering which routines were never called.

### Removed unused functions and parameters during link stage
CFLAGS += -Wno-unused-function -Wno-unused-parameter

### To avoid the "warning: dereferencing type-punned pointer will break strict-aliasing rules" in sb_*.c
CFLAGS += -fno-strict-aliasing

Do this in your Makefile...
--------8<--------
DEADCODESTRIP := -Wl,-static -fvtable-gc -fdata-sections -ffunction-sections -Wl,--gc-sections -Wl,-s

foo : foo.c
    g++ $(DEADCODESTRIP) $< -o $@
--------8<--------

Step by step...

-Wl,-static -fvtable-gc -fdata-sections -ffunction-sections -Wl,--gc-sections -Wl,-s
    |           |               |               |               |               |
    |           |               |               |               |               +- Strip debug info, to make code as small
    |           |               |               |               +- Tell linker to garbage collect & discard unused sections.
    |           |               |               +- Keeps functions in separate data sections (so as to discard unused)
    |           |               +- Keeps data in separate data sections, so they can be discarded if unused.
    |           +- C++ virtual method table instrumented with garbage collection information for linker.
    +- Link against static libraries. Required for dead-code elimination.

### Optimization options ``(rmios_lib/scripts/Makefile.mk)``

ifeq ("$(GDBDEBUGGER)","1")
  opt-level = 0
  CFLAGS += -g3 -DGDB_REMOTE_DEBUG=1
endif
CFLAGS += -O$(opt-level)
ifneq ($(unroll-loops),N)
  CFLAGS += -funroll-loops
endif
ifeq ($(inline-limit),)
  inline-limit = 20000
endif
CFLAGS += -finline-limit=$(inline-limit)
CFLAGS += -fomit-frame-pointer -mno-branch-likely
CFLAGS += -march=xlr -mabi=o64
CFLAGS += -Wall -Werror
CFLAGS += -G0
ASFLAGS += $(CFLAGS) -D__ASSEMBLY__
LINKFLAGS += -d -warn-common --no-warn-mismatch
LINKFLAGS += -L$(RMIOS_LIB)/lib/newlib/mips64xlr-elf/lib/soft-float/
CFLAGS += -mno-abicalls -fno-pic -fno-common
LINKFLAGS += --export-dynamic

## PAPI:

* First measure with non-optimized code (-O0) by instrumenting with PAPI
* Then measure successively with -O1, -O2, and -O3.
* Then optimize by hand as required.

## Oprofile (Your 4-step plan to profiling.)

1. Compile and install oprofile, making sure to use --with-linux option to point to your current kernel source, or --with-kernel-support for 2.6 kernels.

2.Start up the profiler :
$ opcontrol --vmlinux=/path/to/vmlinux
$ opcontrol --start

3. Check if Oprofile is present in kernel
``
$ lsmod  | grep oprofile
$ cat /dev/oprofile/buffer
$ cat /dev/oprofile/cpu_type
``

4. Once Oprofile module is in kernel, do the profiling on any binary given

5.Now let's generate a profile summary :
$ opreport -l /path/to/mybinary
Or, if we built our binary with -g, we can produce some annotated source :
$ opannotate --source --output-dir=/path/to/annotated-source /path/to/mybinary
Or we can look at the rankings of the various system components as a whole :
$ opreport
If you reinstall Oprofile over a previous version you need, before starting the profiler, to:
$ opcontrol --reset

Couple of tries I did.
``
$ cd /boot/
$ od -A d -t x1 vmlinuz | grep '1f 8b 08 00'
$ od -A d -t x1 vmlinuz-2.6.18-53.el5PAE | grep '1f 8b 08 00'
$ dd if=vmlinuz bs=1 skip=8328 | zcat > vmlinux
$ dd if=vmlinuz-2.6.18-53.el5PAE bs=1 skip=8328 | zcat > vmlinux
$ dd if=vmlinuz-2.6.18-53.el5PAE bs=1 skip=8328 | > vmlinux
$ opcontrol -V --init
$ opcontrol -l
$ opreport --exclude-dependent
$ opcontrol --start
$ opcontrol --setup
$ vi /var/lib/oprofile/oprofiled.log
$ vi /var/lib/oprofile/complete_dump
$ vi /var/lib/oprofile/samples/oprofiled.log
$ opcontrol --dump
$ opcontrol --reset
$ opreport --demangle=smart --symbols /root/test_diam_cli/aq_acc  --symbols /boot/vmlinux
$ opcontrol --stop
$ cat /dev/core
$ sh -x opcontrol --setup --vmlinux=/boot/vmlinux --event=CPU_CLK_UNHALTED:100000
$ less /proc/cpuinfo
$ opcontrol --list-events
$ opcontrol --vmlinux=/boot/vmlinux-`uname -r`
$ opcontrol --deinit
``

## Strace cheatsheet

1. To trace all system calls invoked by given application
$ strace <cmd> [cmd-args]
The output of strace itself comes on stderr. To capture strace output onto a
file, do redirect stderr to stdout. 2>&1 Since output of cmd itself may be of
little interest, it makes sense to redirect all stdout to /dev/null. i.e.
>/dev/null.
Ex. strace ls 2>&1 >/dev/null | tee log

2. To trace only single system call type invoked
$ strace -e <syscall-type> <cmd> [cmd-args]
$ strace -e open ls

3. To trace multiple system calls as specified
$ strace -e trace=<type1>,<type2>,... <cmd> [cmd-args]
$ strace -e trace=open,read ls
types are: network, open, read, write, connect, socket, clone, fork, execve,
wait4, kill, pipe, clock_gettime, etc.

4. To redirect execution output to a file
$ strace -o output.txt <cmd>

5. Trace an already running process
$ strace -p <pid>

6. To print timestamp for each output line
$ strace -t

7. To print relative time for syscalls
$ strace -r

8. To generate statistics report of system calls
$ strace -c 

9. To increase the string length (for good output)
$ strace -s 512
