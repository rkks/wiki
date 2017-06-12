% GDB CheatSheet
% Ravikiran K.S.
% Jan 1, 2006

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

…

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

