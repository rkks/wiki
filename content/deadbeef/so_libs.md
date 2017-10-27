% C Library Creation
% Ravikiran K.S.
% January 1, 2006

# How to generate a library:
    * Compile: cc -c ctest1.c ctest2.c
    * Create library "ctest.a": ar -cvq ctest.a ctest1.o ctest2.o
    * List files in library: ar -t ctest.a
    * Using library: cc -o executable-name prog.c ctest.a
    * Example files:
          o ctest1.c

            ctest1(int *i)
            {
             *i=5;
            }

          o ctest1.c

            ctest1(int *i)
            {
             *i=100;
            }

          o prog.c

            #include <stdio.h>
            main()
            {
               int x;
               ctest1(&x);
               printf("Valx=%d\n",x);
            }

Historical note: After creating the library it was once necessary to run the command: ranlib ctest.a. This created a symbol table within the archive. Ranlib is now embedded into the "ar" command.

# How to generate a shared object: (Dynamically linked object library file.)
        gcc -fPIC -c *.c
        gcc -shared -Wl,-soname,libfoo.so.1 -o libfoo.so.1.0   *.o

Notes:
    * Option -fPIC: Compiler directive to output position independent code suitable for dynamic linking.
    * Pass options to linker as -Wl. In example above options to be passed on to linker are: "-soname libfoo.so.1".
    * Option -shared: Produce a shared object which can then be linked with other objects to form an executable.
    * Option -o: Output of operation. In this case the name of the shared object to be output will be "libfoo.so.1.0

Man Pages:
    * gcc - GNU C compiler
    * ld - The GNU Linker

Multiple Static Declarations:
Multiple Static declarations.

[root@xe30-1][~/rkks/test/lib]$ gcc -o prog prog.c test.a
prog.c:3: error: multiple storage classes in declaration of `a'
[root@xe30-1][~/rkks/test/lib]$ gcc -o prog prog.c -L. -ltest
prog.c:3: error: multiple storage classes in declaration of `a'

## Useful Libraries
general : libglib / libgobject / libpthread/libevent
console : libncurses
2D graphics : libX11 / libSDL
3D graphics : libGL / libGLU / libGLUT
GUI toolkits : libgtk / libQT
Images : libjpeg / libpng / libgif
text rendering : libpango / libfreetype
sound : libasound / libSDL
compression : libz (zlib) / libgzip / libbz2
encryption : libcrypt / libssl / libgssapi / libkrb5
XML : libxml2, Xerces XML parser
web : libcurl
debug: libbfd, libiberty, libopcodes
Documentation: doxygen
C Frameworks - apr
PHP Frameworks - CodeIgnitor, Yii, Zend, Symfony
Java Frameworks - ant, cmake
config file parser: libconfuse

apptrace
diffstat
-xprofile - 

## Local/Non-root installation advisory

If you ever happen to want to link against installed libraries in a local
directory, LIBDIR, you must either use libtool, and specify the full pathname
of the library, or use the `-LLIBDIR' flag during linking and do at least one
of the following:
    - add LIBDIR to the `LD_LIBRARY_PATH' environment variable during execution
    - add LIBDIR to the `LD_RUN_PATH' environment variable during linking
    - use the `-Wl,-rpath -Wl,LIBDIR' linker flag
    - have your system administrator add LIBDIR to `/etc/ld.so.conf'
