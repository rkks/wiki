% Writing Debuggable Code
% Ravikiran K.S.
% January 1, 2006

## Writing Debuggable Code

* Every function that has a return value, should be inspected.
* Debug trace logs at entry and exit of function to dump input/outputs.
* Debug trace at system call, library calls to dump the return values.
* Logs at important stages to identify flow of code.
It should be possible to get some sense of where the problem is just from the
logs, without need for recompile.

* Protect code from invalid data at entry points like cmdline args, UI, system
call, DB, network, file, etc.
* Catch exceptions, fail with big splash, highlight the error, print error
condition as specifically as possible.
* Validate output of library calls, return values, and other artifcats.
* Have consistent method of handling, expected errors and exception errors.
* Protect code from other people's mistakes, from your own mistakes. Use
assertions in place.
* Instead of too many checks, use type safety, scope localization methods to
avoid errors.

http://swreflections.blogspot.in/2012/03/defensive-programming-being-just-enough.html
