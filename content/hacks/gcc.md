% GCC Tips and Tricks
% Ravikiran K.S.
% January 1, 2006

## GCC Tips and Tricks

1. To get complete listing of internal invocations of gcc, pass -v flag as below
$ gcc -v -o ldwrap ldwrap.c  > gcc.out 2>&1
gcc.out has all information on different binaries that gcc internall invoked to get the job done.

2. To only pre-process the C file: gcc -E
3. To only generate assembly file: gcc -S

4. Signed Integer Overflow:
Use -Wstrict-overflow=N with N>=3 to signal cases where code is optimized away.
Use -fwrapv

5. Unsigned Wrap Around:
Compiler assumes unsigned integers do not wrap. Keep the unsigned variables in
the range ``[INT_MIN/2, INT_MAX/2]`` to avoid surprises.

6. "Dead Code" Removed/Elimination:
If compiler deems some data location is unused at that point in code and after,
it could remove that code altogether.
Use #pragma optimize() directives to force the code in.
Use volatile.

6. volatile Pitfalls:
Code can be moved around in unexpected ways. A flag that was supposed to be set
after certain operations, might be moved to a place prior to operation.
Use compiler intrinsic barriers to prevent the flag moving.

7. Loops could be optimized into one read call only:
``
void *ptr = address;
while ( *((volatile int*)ptr) & flag ) {}
``

8. Pointers:
Use restrict
Use -fno-delete-null-pointer-checks. Null pointer checks are deleted if placed
after the first use of the pointer.

9. Stack Corruption
gcc 4.x can instrument the code to check for stack corruption. gcc will add
guard variables and code to check for buffer overflows upon exiting a function.
Use -fstack-protector and -fstack-protector-all

10. Memory leak check.
With Glibc2.x and Linux libc-5.4.23 onwards, setting env var ``MALLOC_CHECK_``
would use a special implementation which is designed to be tolerant to simple
errors like double calls of free() with the same argument, or overruns of a
single byte (off-by-one bugs). if set to 1, the error message is printed on
stderr, but the program is not aborted; if set to 2, abort() is called
immediately, but the error message is not generated; if set to 3, the error
message is printed on stderr and program is aborted.

11. Multiple Static declarations.
It is observed that compilation goes fine even if same global symbol is defined
multiple times. The easy way is to define global variable with same name in two
different C files. Compile those files separately. Then put them into one single
static archive .a file using 'ar'. Then link another program using this library
to make a binary. Now the value of global variable accessed depends on the order
in which the object files are put into the archive. However, the same is not true
in case of 'static' declarations. In which case, you'll get following error:

[root@xe30-1][~/rkks/test/lib]$ gcc -o prog prog.c test.a
prog.c:3: error: multiple storage classes in declaration of `a'
[root@xe30-1][~/rkks/test/lib]$ gcc -o prog prog.c -L. -ltest
prog.c:3: error: multiple storage classes in declaration of `a'

12. To make gcc not to pick header files from standard system directories like:
/usr/include/,/usr/local/include/,/usr/X11R6/include/,then use -nostdinc option.
Then only the directories you have specified with -I options (and the directory
of the current file, if appropriate) are searched.

GCC Standard Include directory:
/usr/include/
/usr/local/include/
/usr/X11R6/include/

gcc -nostdinc
Do not search the standard system directories for header files. Only the
directories you have specified with -I options (and the directory of the
current file, if appropriate) are searched.

13. Both -O3 and -g3 options can not be given together. It confuses gcc.

14. Use -E option in gcc when trying to debug the compiler related issue.

15. Know compilation flags enabled in GCC
$ echo 'main(){printf("hello world\n");}' | gcc -E -v -

## Stripping debug symbols from binary (strip cheatsheet)

$ strip -s <bin>                # strip symbol table
$ strip --strip-debug <bin>     # strip only debug symbols
$ strip -R <sect> <bin>         # strip a section
$ strip --strip-unneeded <bin>  # strip unneeded symbols
$ strip -K<sym-name> ...        # retain a symbol while stripping
$ strip -N<sym-name> ...        # strip particular symbol
$ strip -o <new-bin> ...        # produce new binary on strip
$ strip @<file>                 # read options from filename

## Objdump

$ objdump -DRTgrstx OR -xDrtWGgs
$ objdump -d -S --file-start-context --prefix-addresses -l fp-xlr
        |   |       |                   |              |
        |   |       |                   |              +-- print line numbers
        |   |       |                   +----------------- print complete address on each line
        |   |       +------------------------------------- in interlisted disassembly (-S) give file info
        |   +--------------------------------------------- display source code intermixed with assembly
        +------------------------------------------------- display assembly instructions for machine code in objfile

## readelf cheatsheet

$ readelf -s <bin>          # show sections
$ readelf -a <bin>          # show all ELF info detailed
$ readelf -S <bin>          # show section header
