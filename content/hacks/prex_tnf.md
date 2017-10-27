% TNF and Prex info on Solaris
% Ravikiran K.S.
% January 1, 2006

prex> help

Usage: help [topic|command]

Topics
        intro         functions    kernel_mode
        probe_spec    processes    set_spec

User and Kernel Mode Commands
        disable       enable       help       list
        quit          source       trace      untrace

Additional user-mode-only commands
        clear         connect      continue

Additional kernel-mode-only (prex -k) commands
        buffer        ktrace       pfilter
prex> help intro
Introduction to prex

prex is used to control probes in a target process, or in the kernel.
If you are reading this help text, you have sucessfully invoked prex,
by either connecting to an existing process (prex -p), a new
process (prex myprogram), or to the kernel (prex -k).

Most often, the user will want to enable some probes in the target,
continue the target - either to completion, or until the target is
interrupted - and then exit from prex to perform analysis on the resulting
tracefile.  An ascii dump of the tracefile can be obtained using the
tnfdump(1) command.

The tracefile can be found in /tmp/trace-<pid> by default, or in a location
of your choice, if you specify the -o option when you invoke prex.
You can query the name of the current trace file by using the command.
list tracefile.

Upon invocation, prex reads commands from the files ~/.prexrc and
./.prexrc (in that order).  The "source" command may be used to take
commands from an arbitrary file or set of files.

Help is available for a variety of topics, and for each prex command.
Type help with no arguments for a list of available help topics.

end of help for topic intro
prex> help functions
Probe Functions

Note - probe functions are not available from kernel mode

It is possible to use prex to connect functions to probe points, such that
the function is invoked each time a given probe point is hit.  Currently,
the only function available from prex is the &debug function, which prints
out the arguments sent in to the probe, as well as the value (if any)
associated with the sunw%debug attribute in the detail field.

Relevant commands:
    list fcns                    # list the defined probe functions
    connect &debug name=myprobe  # attach probe &debug to probe myprobe
    connect &debug $myset        # attach probe &debug to probes in $myset
    clear name=myprobe           # disconnect probe functions from myprobe
    clear $myset                 # disconnect probe functions from $myset

end of help for topic functions
prex> help kernel_mode
Controlling Kernel Probes

The Solaris kernel is instrumented with a small number of strategically
placed probes, documented in tnf_kernel_probes(4).  The superuser can
control these probes by running prex with the "-k" option.

In kernel mode, trace output is written to an in-core buffer, rather
than to a file on disk.  This buffer can be extracted with the tnfxtract(1)
commmand.  This buffer must be set up before tracing can begin, through the
use of the "buffer alloc" command.  After kernel tracing is complete (and
after the buffer has been extracted), the buffer can be deallocated from
prex using the "buffer dealloc" command.

As in user mode, kernel probe control is accomplished using the commands
"trace", "untrace", "enable", and "disable".  Additionally, in
kernel mode, a "master switch" is provided to turn all tracing activity
on or off.  This switch is toggled using the commands "ktrace on" and
"ktrace off".  Unlike user mode, where the target is stopped while
tracing paramaters are manipulated from prex, the kernel does not stop
running during a tracing session.  Using the "ktrace" command, one can
set up all tracing parameters in advance of a session, without actually
writing trace records until a "ktrace on" command is given.

Kernel mode also provides the ability to limit tracing to those kernel
probes hit on behalf of a specific user process.  The pfilter command
is provided to toggle process filtering on or off, and to specify the
set of processes that comprise the filter.  If pid 0 is a member of the
filter list, then any threads not associated with a process are included.

Note that after a kernel tracing session, all tracing parameters are left
as-is.  One should re-enter prex to disable and untrace probes, turn off
process filtering, and deallocate the in-core trace buffer.

Relevant Commands:
    buffer alloc 2m    # allocate a 2M in-core buffer
    enable $all        # enable all kernel probes
    trace $all         # trace all kernel probes
    ktrace on          # turn on kernel tracing
    pfilter add 1234   # add pid 1234 to the filter list
    pfilter on         # turn on process filtering
    ktrace off         # turn off kernel tracing
Also see tnfxtract(1), which is used to extract the in-core trace buffer
to an on-disk tracefile.

end of help for topic kernel_mode

prex> help probe_spec
Probe Specification

Many prex commands operate on probes or sets of probes.  Probes are
specified by a list of space-separated selectors of the form:
      <attribute>=<value>
If the "<attribute>=" is omitted, the attribute defaults to "keys=".
The "<value>" can be either a string or an ed(1) regular expression
enclosed in slashes.  Regular expressions in prex are unanchored, meaning
that any value that contains the given regex as a substring is a valid
match, regardless of position.  To anchor a regular expression, use "^"
to match the beginning of a line, or "$" to match the end of a line.
If a list of selectors is specified, an OR operation is applied - the
resulting probe_spec includes probes that match any of the selectors.
See the prex(1) man page for a complete specification of the accepted
grammar.

The "list" command is used to view available probes in the target,
and to display their attributes. The "trace" and "untrace" commands
determine whether a probe will write a trace record when hit.  The
"enable" and "disable" commands indicate whether a probe will perform
any action (such as a calling a connected function or creating a trace
record) when hit.   Normally, a probe is enabled and traced for tracing,
and disabled and untraced otherwise.  It is possible to enable a probe
with tracing off to get debug output without writing trace records.

Relevant Commands:
   list probes $all         # list probes in set $all (all probes)
   list probes file=test.c  # list probes with a specific attribute
   list 'file' probes $all  # list the file attribute in all probes
   list probes name=/^thr/  # list probes whose name attribute matches
                            # the given regular expression
   list probes name=/^thr/ keys=vm  # list probes matching either selector
   enable name=/^thr/       # enable probes whose name matches this regex
   trace $all               # trace all probes
   untrace $myset           # untrace probes in set $myset

end of help for topic probe_spec
prex> help processes
Controlling Processes with prex

Prex is used to control probes in a given process, or in the kernel.
The process which prex is to control is identified as an argument when
prex is invoked.  If the "-p <pid>" switch is used, prex connects to the
specified process.  Otherwise prex exec's the command supplied at the
end of its argument list.  In either case, prex stops the target process
immediately so that the user may set up probe control.

Once probe control is set up (typically using the "enable" and "trace"
commands), the process is continued using the "continue" command.  Prex
remains attached to the target, and the user can force the target to
stop again by typing control-C, at which time additional probe control
directives may be given.

Upon quitting from prex, the target process is normally resumed if prex
attached to it, or killed if prex invoked it.  An optional argument may
be given with the "quit" command to explicitly specify whether to kill
the target, continue it, or leave it suspended.

If the target forks, any probe that the child encounters will be logged to
the same trace file as the parent.  If the child calls exec, it will no
longer be traced.

In kernel mode (prex -k), process filtering may be enabled, to limit
tracing to those kernel probes hit on behalf of a specific process or
set of processes.  Kernel-mode process filtering is controlled using
the "pfilter" command.


Relevant Commands:
    continue               # continue target (user mode only)
    Control-C              # stop target (user mode only)
    quit resume            # quit prex, continue target
    quit suspend           # quit prex, suspend target
    quit kill              # quit prex, kill target
    quit                   # quit prex, default action
# Note: pfilter commands apply only to kernel mode
    pfilter                # show pfilter status
    pfilter on             # turn on process filter mode
    pfilter off            # turn off process filter mode
    pfilter add 1234       # add to process filter pid list
    pfilter delete 1234    # delete from process filter pid list

end of help for topic processes
prex> help set_spec
Specifying Probe Sets

Prex provides the ability to define named sets of probes to simplify
commands operating on multiple probes.  The set "$all" is predefined,
as the set of all probes in the target.  A set is defined using the
"create" command, and can be used as an argument to the "list",
"enable", "disable", "trace", "untrace", "connect" and
"clear" commands.

Relevant Commands:
    create $myset name=/^thr/        # create a set
    list probes $myset               # list probes in a set
    list sets                        # list defined sets
    enable $myset                    # enable a set of probes
    trace $myset                     # trace a set of probes

end of help for topic set_spec
prex> help enable

Usage: enable <probe_spec>|<set_spec>

"enable" is used to specify that any activity associated with the probe
or probes will be performed when the probe is hit.  This includes connected
probe functions as well as the generation of trace records.  Note that in
order for a probe to generate a trace record, it must be "traced" as well
as enabled.

End of help for cmd enable
prex> help disable

Usage: disable <probe_spec>|<set_spec>

"disable" is used to to turn off all tracing activity associated with a
probe or a set of probes.

End of help for cmd disable
prex> help list

Usage: list probes <probe_spec>|<set_spec> 
       list <attrs> probes <probe_spec>|<set_spec>
       list sets
       list fcns
       list history
       list tracefile

"list" displays information about currently defined probes, sets, and
probe functions.  When listing probes, one can limit the output to a
desired set of attributes by specifying an attribute list as a set of
strings.  If an attribute is also a reserved word (such as "trace", it
must be enclosed in single quotes.  For example:

       list file 'trace' probes $all

"list" history lists the probe control commands history, and
"list" tracefile displays the current trace file name.

End of help for cmd list
prex> help source

Usage: source <filename>

The "source" command is used to invoke a set of prex commands stored in
a file.  A sourced file may in turn source other files.  The files
~/.prexrc and ./.prexrc are sourced automatically (in that order) when prex
is invoked, and may be used to store commonly used probe and set
specifications, or probe control directives.  Commands in sourced files
may override the effects of commands in previously sourced files.

End of help for cmd source
prex> help trace

Usage: trace <probe_spec>|<set_spec>

"trace" is used to turn on tracing for the specified probe or probes.
A "traced" probe that is also "enabled" will generate a trace record
when it is hit.

End of help for cmd trace
prex> help untrace

Usage: untrace <probe_spec>|<set_spec>

"untrace" turns tracing off for the specified probe or probes.  A probe
will not generate a trace record when it is not traced, although connected
probe functions will still be invoked as long as a probe is "enabled".

End of help for cmd untrace
prex> help help

Usage: help [<cmd>|<topic>]

"help" lists all available help topics when run without any arguments.
If a valid topic or command-name is supplied as an argument, help text for
the topic or command is displayed.

End of help for cmd help
prex> help quit

Usage: quit
       quit kill
       quit resume
       quit suspend

The "quit" command exits prex, leaving the target in a state specified
by the user, or taking a default action if no instructions are specified.
An optional argument may be used to indicated that the target should be
killed, resumed, or left suspended.  If no argument is supplied, then
prex's default behavior is to resume a process to which it had attached,
and to kill a process which it had invoked.

End of help for cmd quit
prex> help clear

Usage: clear <probe_spec>|<set_spec>

Note:  Not available in Kernel Mode

"clear" disconnects any probe functions that have previously been
connected to a probe or group of probes using the "connect" command.
The "clear" command cannot be used in kernel mode, since probe functions
are not available for kernel probes.

End of help for cmd clear
prex> help connect

Usage: connect <function> <probe_spec>|<set_spec>

Note:  Not available in Kernel Mode

"connect" connects a probe function to a probe or group of probes.
Currently, the only probe function available from prex is "&debug", which
prints out the arguments sent in to the probe, as well as the value (if
any) associated with the sunw%debug attribute in the detail field.
In order for a probe function to be invoked, the probe to which the
function is attached must be "enabled", but need not be "traced".

The "clear" command is available to disconnect a probe function from
a probe or group of probes.

End of help for cmd connect
prex> help continue

Usage:  continue

"continue" is used to resume execution of the target process.  This
command is not available in kernel mode, since the kernel is never stopped.

end of help for cmd continue
prex> help buffer

Usage: buffer alloc <size>
       buffer dealloc

Note:  Kernel Mode Only

"buffer" allocates or deallocates the in-core buffer used to hold
kernel trace records.  Size can be specified in kilobytes or megabytes,
by appending the character 'k' or 'm' to a numeric value (e.g. "2m").
A buffer must be allocated prior to a kernel tracing session.  Once
allocated, the buffer remains usable until deallocated, even through
multiple invocations of prex.

Before the buffer is deallocated, data may be extracted to an on-disk
tracefile using tnfxtract(1).

End of help for cmd buffer
prex> help ktrace

Usage: ktrace on
       ktrace off

Note:  Kernel Mode Only

"ktrace" toggles the master switch that indicates whether kernel
tracing is active or inactive.  Since the kernel cannot be stopped while
a tracing experiment is set up, "ktrace" is provided so that tracing
can be set up as desired before any trace records are generated

End of help for cmd ktrace
prex> help pfilter

Usage: pfilter
       pfilter on
       pfilter off
       pfilter add <pidlist>
       pfilter delete <pidlist>

Note:  Kernel Mode Only

"pfilter" controls process filtering by toggling process-filter mode,
and maintaining a list of process id's on which to filter.  When process
filtering mode is on, tracing is limited to the kernel events hit on behalf
of the processes in the pid list.  Process id 0 is used to represent all
threads not associated with a process.
When run without arguments, "pfilter" displays the current process-filter
mode and pid list.  A process filter pid list can be maintained whether
or not process filter mode is currently active.

A process must exist in order to be added to the pid list, and the pid list
is automatically updated to delete any processes that no longer exist.  If
the pid list becomes empty while process filtering is on, prex issues a
warning.  Process filtering mode is persistent between invocations of prex
so it should be turned off manually when a tracing experiment is complete.

End of help for cmd pfilter
