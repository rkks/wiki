% GDB CheatSheet
% Ravikiran K.S.
% Jan 1, 2006

## GDB symbol lookup/resolution/approximation algorithm

- Parse through sym table line by line
    - Compare $pc address to sym table address
    - if addresses exactly match, then that is the function
    - if address from sym table is greater than $pc address, previous address/function is the correct one
        - the difference between address matched and $pc value is the offset in function
    - if address from sym table is lesser than $pc address, continue parsing

## Debugging Inline Functions in GDB
Debugging Inline Functions:
Go to the stack frame in question, print the instruction point (e.g. p $rip),
then use it to look it up manually with e.g. "addr2line -e -i 0x84564756".

## Dummy Frame
The return value of an api is always kept at $eax in Intel processors. This may
also have the pointer to address. The user program doesn't get started by
main(), rather it gets started by start() library call, followed by ``_init()``,
followed by a ``dummy_frame()`` pointer, then by ``main()``.

``
(gdb) r
Starting program: /root/rkks/test/lib/prog
Reading symbols from shared object read from target memory...done.
Loaded system supplied DSO at 0xffffe000

Breakpoint 2, 0x0804827e in _init ()
(gdb) i f
Stack level 0, frame at 0xbfe8f130:
 eip = 0x804827e in _init; saved eip 0x804840a
 called by frame at 0xbfe8f150
 Arglist at 0xbfe8f128, args:
 Locals at 0xbfe8f128, Previous frame's sp is 0xbfe8f130
 Saved registers:
  ebp at 0xbfe8f128, eip at 0xbfe8f12c
(gdb) i f
Stack level 0, frame at 0xbfe8f130:
 eip = 0x804827e in _init; saved eip 0x804840a
 called by frame at 0xbfe8f150
 Arglist at 0xbfe8f128, args:
 Locals at 0xbfe8f128, Previous frame's sp is 0xbfe8f130
 Saved registers:
  ebp at 0xbfe8f128, eip at 0xbfe8f12c
``
(gdb) i s
``
#0  0x0804827e in _init ()
#1  0x0804840a in __libc_csu_init ()
#2  0x009bdd91 in __libc_start_main () from /lib/tls/libc.so.6
#3  0x080482e1 in _start ()
``
(gdb) c
Continuing.

``
Breakpoint 6, 0x08048342 in frame_dummy ()
(gdb) bt
#0  0x08048342 in frame_dummy ()
#1  0x08048288 in _init ()
#2  0x0804840a in __libc_csu_init ()
#3  0x009bdd91 in __libc_start_main () from /lib/tls/libc.so.6
#4  0x080482e1 in _start ()
(gdb) c
Continuing.
``

Breakpoint 1, main (argc=1, argv=0xbfe8f1d4) at prog.c:21
21              test1(&x);
(gdb) bt
#0  main (argc=1, argv=0xbfe8f1d4) at prog.c:21
(gdb)

### GDB Tips

1\. Redraw screen: `[Ctrl-L]`

2\. Open TUI interface: `[Ctrl-x a]`

### GDB Commands

1\. Dump some area of memory for observation:

``` code
dump binary memory dump.raw 0x4005f8 0x400633
```

2\. display expression each time program stops

``` code
display <expr>
```

3\. Pass control from C program to GDB.

``` code
asm("int $0x3\n");
```

This instruction in code will pass control from running program to gdb;
so that program state could be analyzed at that point. To catch this
interrupt, the process should be running under debugger. This is useful
in conjunction with 'ld –wrap' option. With 'ld –wrap' option, you can
write a wrapper api which should be called when wrapped function is to
be invoked. In this wrapper api, you could check for input values and
see how they appear to be library function.

6\. Display list of frames (same as bt) and move through frames

``` code
info frame
up, down, frame
```

7\. Break once, and then remove the breakpoint

``` code
tbreak
```

8\. Gives complete backtrace with local variables

``` code
bt full
```

9\. Give backtrace of all threads running

``` code
thread apply all bt
```

10\. Print PC (program counter) register of all threads running

``` code
thread apply all print $pc
```

11\. Continue till end of function

``` code
finish
```

12\. Suspend the process when a certain condition is met

``` code
watch
```

13\. break on function matching regular expression

``` code
rbreak
```

14\. Creating coredump of a running process

``` code
(gdb) attach <pid>
(gdb) generate-core-file <file-name>
(gdb) detach
```

OR

``` code
$ kill -s SIGABRT <pid>
```

Note: the SIGABRT will terminate the process and generate corefile.
Also, for it to work, the signal shouldn't be caught by a custom signal
handler.

To obtain a good call stack, it is important that gdb loads the same
libraries that were loaded by the program that generated the core dump.
If the analyzing machine is different than the one where core happened,
then copy the libraries over to analyzing machine in a way that mirrors
dump machine. Ex.

``` code
$ tree .
.
-\ juggler-29964.core
-\ lib64
  |-\ ld-linux-x86-64.so.2
  |-\ libc.so.6
  |-\ libm.so.6
  |-\ libpthread.so.0
  |-\ librt.so.1
```

...

``` code
(gdb) set solib-absolute-prefix ./
(gdb) set solib-search-path .
(gdb) file ../../../../../threadpool/bin.v2/libs/threadpool/example/juggler/gcc-4.1.2/debug/link-static/threading-multi/juggler
(gdb) core-file juggler-29964.core
```

15\. Find zombie threads. Any thread that has terminated but has not
been joined or detached will leak OS resources until the process
terminates. Unfortunately, neither /proc nor gdb will show you these
zombie threads, at least not on some kernels. One way to get them is
with a gdb canned command:

``` code
define trace_call
  b $arg0
  commands
    bt full
    continue
  end
end
```

``` code
document trace_call
Trace specified call with call stack to screen. Example:
  set breakpoint pending on
  set pagination off
  set logging on
  trace_call __pthread_create_2_1
end
```

TUI Key Bindings

TUI installs several key bindings in readline keymaps (Command Line
Editing).

  - \`C-x C-a'/\`C-x a'/\`C-x A' - Enter or leave the TUI mode.

  - \`C-x 1' - Use a TUI layout with only one window.

  - \`C-x 2' - Use a TUI layout with at least two windows.

  - \`C-x o' - Change the active window. Gives the focus to the next TUI
    window.

  - \`C-x s' - Use TUI \_SingleKey\_ keymap that binds single key to gdb
    commands (TUI Single Key Mode).

  - \<PgUp\> - Scroll the active window one page up.

  - \<PgDn\> - Scroll the active window one page down.

  - \<Up\> - Scroll the active window one line up.

  - \<Down\> - Scroll the active window one line down.

  - \<Left\> - Scroll the active window one column left.

  - \<Right\> - Scroll the active window one column right.

  - \<C-L\> - Refresh the screen.

In TUI mode, arrow keys are used by the active window for scrolling.
This means they are available for readline when the active window is the
command window. When command window does not have focus, it is necessary
to use other readline key bindings such as: \<C-p\>, \<C-n\>, \<C-b\>
and \<C-f\>.

## Embedded Breakpoints in Source Code

Embedding Breakpoints in Source Code (For x86 platforms):
``
/* There are two problems with this approach:
 * - They aren't real GDB breakpoints. You can't disable them, count how many times they've been hit, etc.
 * - If you run the program outside GDB, the breakpoint instruction will crash your process.
 */
``
``#define EMBEDDED_BREAKPOINT  asm volatile ("int3;")``

OR
``
/*
 * Here is a small hack which solves both problems:
 * Place a local label into the instruction stream, and then save its address in the embed-breakpoints linker section.
 * Then we need to convert these addresses into GDB breakpoint commands. If we run the program normally, or in GDB
 * without the wrapper converter, the EMBED_BREAKPOINT statements do nothing. The breakpoint addresses aren't even
 * loaded into memory, because the embed-breakpoints section is not marked as allocatable.
 * This hack requires GNU toolchain as it uses GNU C extensions, GNU assembler syntax, and BFD. GDB wrapper uses the
 * Linux /proc filesystem, so that it can pass to GDB a temporary file which has already been unlinked. This could be
 * ported to other UNIX systems by changing the tempfile handling. It should work on a variety of CPU architectures,
 * but I've only tested it on 32- and 64-bit x86.
 * http://mainisusuallyafunction.blogspot.in/2012/01/embedding-gdb-breakpoints-in-c-source.html
 */
``
``#define EMBEDDED_BREAKPOINT \
    asm("0:"                              \
        ".pushsection embed-breakpoints;" \
        ".quad 0b;"                       \
        ".popsection;")``

GDB wrapper, header file and example at: /homes/raviks/test/debug/embedded-breakpoints

## Debugging Secrets:
1. GDB Help :
    (gdb) h
    (gdb) apropos
2. User defined commands:
    (gdb) show user <command-name>
    Ex:
    (gdb) define adder
        print $arg0 + $arg1 + $arg2
        end
    (gdb) adder 1 2 3
3. Define .gdbinit file to have basic execution options.
4. Enable command history save into file. Reuse this history file each time.
    (gdb) set history filename <file>
    (gdb) set history save <on/off>
    (gdb) show history
    (gdb) show commands
5. Handle Signals gracefully.
    Print table of all signals and GDB's current configuration to handle them.
    (gdb) i handle      or      (gdb) i signals
    Configure signals to be handled as:
    (gdb) handle <signal> <stop/nostop> <print/noprint> <pass/nopass>
    Ex.
    (gdb) handle SIG35 nostop print pass
    (gdb) handle SIG36 stop (implies print as well)
6. Remote Debugging
    GDB runs on one machine (host) and the program  being debugged (exe.verXYZ.stripped ) runs on another (target).
    GDB communicates via Serial or TCP/IP.
    Host and target: exactly match between the executables and libraries, with one exception: stripped on the target.
    Complication: compiling on one machine (CC view), keeping code in different place (ex. /your/path/verXYZ)
    Solution:
        Connect gdb to source in the given place:
        (gdb) set substitute-path /usr/src /mnt/cross
        (gdb) dir /your/path/verXYZ
    Using gdbserver through TCP connection:
        remote (10.10.0.225)>  gdbserver :9999 program_stripped
        or
        remote> ./gdbserver :9999 -attach <pid>
        host> gdb program
        host>(gdb) handle SIGTRAP nostop noprint pass
        to avoid pausing when launching the threads
        host> (gdb) target remote 10.10.0.225:9999
7. Convinience Variables:
    Convenience variables are used to store values that you may want to refer later. Any string preceded by $ is regarded as a convenience variable. Ex.:  set $table = *table_ptr
    To show all convinience variables defined, command is:
    (gdb) show conv
8. Checkpoint:
    Checkpoint is a snapshot of a program's state
    (gdb) checkpoint
    (gdb) i checkpoint
    (gdb) restart checkpoint-id
9. Value history:
    Values printed by the 'print' command
10. Macros can be seen in GDB by setting compiler option '-gdwarf':
    $ g++ -gdwarf-2 -g3 a.cpp -o prog
11. 64 bit .vs. 32bit machines:
    Code can be compiled for 32 bits machine by giving '-m32' flag, and for 64 bits machine by '-m64' flag.
    -m32 flag
    On 64-bit machine, install another 32-bit version of GDB
    $ ls -l `which gdb32`
    /usr/bin/gdb32 ->  /your/install/path
12. How to remove a symbol table from a file?
    User 'strip' command to do that.
13. How to supply arguments to your program in GDB?
    It can be done in 3 ways:
    A. With --args option. Example, #sudo gdb -silent --args /bin/ping google.com
    B. As arguments to run:     Example, (gdb) run arg1 arg2
    (run without arguments uses the same arguments used by the previous run.)
    C. With set args  command:
        (gdb) set args arg1 arg2
        (gdb) show args
        (set args without arguments - removes all arguments)
14. How to know where you are (file, next execution line)?
    (gdb) f
15. How to find out the crash file executable?
    It can be done in 3 ways:
    A. #file core.1234
    B. #gdb core.1234
    C. use /proc/sys/kernel/core_pattern
        #echo "core_%e.%p" > /proc/sys/kernel/core_pattern
        if the program foo dumps its core, then the core_foo.1234 will be created.
16. How to find out why your program stopped?
    (gdb) i prog
17. Which command(s) can be used to exit from loops?
    (gdb) until lineNo
18. print, info, show - what is a difference?
    print - print value of expression
    info - showing things about the program being debugged
    show - showing things about the debugger
19. Problem Determination Tools for Linux:
    A. Compiler option -Wall
    B. Code review
    C. Programs traces, syslog, profilers
    D. Static Source Code Analysis:
    E. scan.coverity.com - free for FOSS
    F. Flexelint
    G. Dynamic analysis: Valgrind,
    H. strace, /proc filesystem, lsof, ldd, nm, objdump, wireshark
20. How do I examine memory?
    A. Examine the variable as a string:
        (gdb) x/s s
        0x8048434 <_IO_stdin_used+4>:    "Hello World\n"
    B. Examine the variable as a character:
        (gdb) x/c s
        0x8048434 <_IO_stdin_used+4>:   72 'H'
    C. Examine the variable as 4 characters:
        (gdb) x/4c s
        0x8048434 <_IO_stdin_used+4>:   72 'H'  101 'e' 108 'l' 108 'l'
    D. Examine the first 32 bits of the variable:
        (gdb) x/t s
        0x8048434 <_IO_stdin_used+4>:   01101100011011000110010101001000
    Examine the first 24 bytes of the variable in hex:
        (gdb) x/3x s
        0x8048434 <_IO_stdin_used+4>:   0x6c6c6548      0x6f57206f      0x0a646c72
21. How do I see what is in the processor registers?
    (gdb) info registers
    eax            0x40123460       1074934880
    ecx            0x1      1
    edx            0x80483c0        134513600
    ebx            0x40124bf4       1074940916
    esp            0xbffffa74       0xbffffa74
    ebp            0xbffffa8c       0xbffffa8c
    esi            0x400165e4       1073833444
22. How do I step through my code at the instruction level?
    'nexti' and 'stepi' commands.
23. How do I disassemble an API?
    (gdb) info line <api-name>
    (gdb) disas STARTADDRESS ENDADDRESS
24. How do I get source annotated disassembly of a program/binary?
    objump -S -d program
    Or
    gcc -S <file.c>
25. How do I disassemble analyse code as instructions?
    (gdb) x/i 0xdeadbeef
26. How do I Dump Memory Regions with GDB ?
    (gdb) dump binary memory dump.raw 0x00800000 0x01000000
27. How do I debug stripped binary?
    Compile a new binary using -g option and load symbols using new binary. If that's not feasible, look up the address in symbol table generated.
28. How do I redirect process output to a log Or feed process input using a file?
    The redirection operators in shell work the same way in gdb. Use >, <, and | operators for piping the outputs.
29. Use Ctrl-d, Ctrl-C does not exit from gdb, but halts the current gdb command
30. How to continue until return/end of current function?
    (gdb) finish
31. Auto display the information each time a breakpoint/watchpoint is hit
    (gdb) display $eax          (print contents of %eax every time the program stops)
    (gdb) display               (print the auto-displayed items)
    (gdb) delete display <NUM>         (stop displaying item NUM)
32. How to execute the shell commands on gdb command prompt?
    (gdb) ! <command>   OR
    (gdb) shell <command>
33. How to track child processes after forked?
    (gdb) set follow-fork-mode child
34. How to echo an input command to screen? Two way, use echo command explicitly
35. How to tell gdb to run a binary and blockon wait4() for child to terminate
    (gdb) attach -waitfor a.out
36. How to create watchpoints?
    (gdb) watch global_var  - Set a watchpoint on a variable when it is written to.
    (gdb) watch -location g_char_ptr - Set a watchpoint on a memory location when it is written into. The size of the region to watch for defaults to the pointer size if no '-x byte_size' is specified. This command takes raw input, evaluated as an expression returning an unsigned integer pointing to the start of the region, after the '--' option terminator.
37. Show the stack backtraces for all threads.
    (gdb) thread apply all bt
38. Show the values for the registers named "rax", "rsp" and "rbp" in the current thread.
    (gdb) info all-registers rax rsp rbp

## Command list

r <prog>- run program
b 36    - break at line 36 of current source file.
i b     - info breakpoints. Lists all breakpoints now set up.
d       - delete all breakpoints (d 1 for just #1).
n       - execute next line of code.
s       - like next, but steps inside function calls.
c       - continue.  Run from this point until breakpoint or program exit.
x/NFU ADDR  - print contents at ADDR in memory:
    N = number of units to display
    F = display format. /s = string, /i = instruction
    U = b (bytes), h (2 bytes), w (4 bytes))
``
x/4xw <addr>    - Read memory from address 0xbffff3c0 and show 4 hex uint32_t values.
l       - list source code (list 18 = code near line 18, list main, etc.)
h b     - help on break command
q       - quit gdb
i s     - info stack: calls made to get to this execution point.
p <foo> - print foo. (/x = hex, /o = octal, /d = decimal, /a = address, /t = binary, /c = char)
f       - frame
i lo    - info locals: values of all local vars, current function
i var   - info variables: values of global/static vars
i fun   - list of all functions in the program.
p/a $pc - print the program counter
p $sp   - print the stack pointer
p $eax  - print the contents of %eax
p/t $rax- Show the values for the register named "rax" in the current thread formatted as binary.
disa <addr1> <addr2>    - Disassemble an address range.
i shared        - List the main executable and all dependent shared libraries.
i symbol <addr> - Lookup information for a raw address in the executable or any shared libraries.
``
