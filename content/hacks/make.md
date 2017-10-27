% GNU Make Nits
% Ravikiran K.S.
% January 1, 2006

-n, --just-print, --dry-run, --recon  
Print the commands that would be executed, but do not execute them.  

-d  Print debugging information in addition to normal processing.  
The debugging information says  
which files are being considered for remaking,  
which file-times are being compared and with what results,  
which files actually need  to  be  remade,  
which implicit  rules are considered and which are applied---  
everything interesting about how make decides what to do.  

--debug[=FLAGS] Print debugging information in addition to normal processing.  
If the FLAGS are omitted, then the behaviour is the same as if -d was specified.  
FLAGS may be:  
'a' for all debugging output same as using -d,  
'b' for basic debugging,  
'v' for more verbose basic debugging,  
'i' for showing implicit rules,  
'j' for details on invocation of commands, and  
'm' for debugging while remaking makefiles.

make SHELL="/bin/bash -vx"
echo 'print: ; @echo "$(VAR)"' | make -f Makefile -f - print can be handy to examine the expanded value of variables.

``
$< - The source from which the target is to be made
$* - The base name of the target (no extensions or directory)
$@ - The full name of the target
--just-print/-n - A very good option for debugging makefiles
--print-data-base/-p - Prints macros extensions and interpretations by make
--warn-undefined-variables
-d - Prints debug info while processing the makefile
``

Passing params to makefiles.
* Values can come from command line parameters, assignment in the Makefile, from the environment, or be predefined.
* the precedence from highest to lowest follows that order
	- make CFLAGS=blah will override any CFLAGS assigned in the Makefile
	- if CC=gcc is hardcoded in Makefile, then any CC env var will be ignored which is often problematic
	- predefined variables can be see with make -p option

``
Makefile:
# Different types of variable assignments.
static := $(recurse) #empty
recurse = $(_recurse)
_recurse = recurse
condit ?= condit
append += append
override pappend += pappend #handle parameters to `make`

# Print variables
all:
	@echo $(CC) $(recurse) $(condit) $(append) $(pappend)
.PHONY: all
``

File has modification time in the future - This error is seen frequently on machines. My observation is, this error
pops up when time modification for given file hasn't completely reflected on file due to file-system processing
reasons. Ex. if you touch a file like:
``
[raviks@bng-lnx-shell3][~/work/test]$ touch new.*
[raviks@bng-lnx-shell3][~/work/test]$ ll new.c
-rw-r--r-- 1 raviks ipg  421 Mar  4  2013 new.c
We can see that time isn't properly reflected yet. At this moment if you try make, it complains. But little later,
[raviks@bng-lnx-shell3][~/work/test]$ ll new.c
-rw-r--r-- 1 raviks ipg 421 Mar  4 07:42 new.c
[raviks@bng-lnx-shell3][~/work/test]$ date
Mon Mar  4 07:49:20 IST 2013
``

Now date has properly reflected. Hence make runs smooth, without any errors. Problem mostly seen with NFS.
However not seen on local file systems like ext4, ufs. Hence looks to be filesystem sync timing issue.

``
#SUBDIRS = dir1 dir2 dir3
#for i in $(SUBDIRS); do $(MAKE) -C $$i clean; done
``

## Generating static linked binary in autotools integrated open source comps

While doing configure, adding -static flag to CFLAGS,LFLAGS,LDFLAGS generates static binary
./configure --prefix=/homes/raviks/tools/splint CFLAGS="${CFLAGS} -static"


