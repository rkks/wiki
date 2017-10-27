% Common FOSS Tools build options
% Ravikiran K.S.
% January 1, 2006

Installation Options:
Resin:
tar xvzf resin-*.gz
cd resin-*
./configure --prefix=$HOME/tools/linux/resin
make install

Compilation Options:
********************

Git:
----
tar xvzf git-1.8.1.2.tar.gz && cd git-1.8.1.2
Edit Makefile. Add '-static' option to CFLAGS
make prefix=/homes/raviks/tools/freebsd/git NO_OPENSSL=YesPlease NO_CURL=YesPlease NO_EXPAT=YesPlease NO_TCLTK=YesPlease NO_GETTEXT=YesPlease all
make prefix=/homes/raviks/tools/freebsd/git NO_OPENSSL=YesPlease NO_CURL=YesPlease NO_EXPAT=YesPlease NO_TCLTK=YesPlease NO_GETTEXT=YesPlease install

Splint
------
./configure --prefix=/homes/raviks/tools/splint CFLAGS="${CFLAGS} -static"
make && make install

Ctags
-----
./configure --prefix=/homes/raviks/tools/ctags --disable-etags CFLAGS="${CFLAGS} -static"
Edit Makefile. Add -static flag to both CFLAGS and LDFLAGS
make && make install

Ncurses
-------
./configure --prefix=/homes/raviks/tools/freebsd/ncurses CFLAGS="${CFLAGS} -static"
make && make install

Vim (Ncurses is required by Cscope)
---
./configure --prefix=/homes/raviks/tools/freebsd/vim CFLAGS="${CFLAGS} -static" --with-features=normal --disable-gui --without-x --without-gnome --enable-cscope LDFLAGS="${LDFLAGS} -static"

./configure --prefix=/homes/raviks/tools/linux/vim CFLAGS="${CFLAGS} -static -I$HOME/tools/linux/libevent/include -I$HOME/tools/linux/ncurses/include -I$HOME/tools/linux/ncurses/include/ncurses" LDFLAGS="${LDFLAGS} -static -L$HOME/tools/linux/libevent/lib -L$HOME/tools/linux/ncurses/lib -L$HOME/tools/linux/ncurses/include -L$HOME/tools/linux/ncurses/include/ncurses -L$HOME/tools/linux/libevent/include" CPPFLAGS="${CPPFLAGS} -static -I$HOME/tools/linux/ncurses/include -I$HOME/tools/linux/ncurses/include/ncurses -I$HOME/tools/linux/libevent/include" --with-features=normal --disable-gui --without-x --without-gnome --enable-cscope

make && make install

Cscope (Ncurses is required by Cscope)
------
./configure --prefix=/homes/raviks/tools/cscope CFLAGS="${CFLAGS} -static"
For Linux, additional steps had to be done. Open Makefile and src/Makefile. Change CURSES_INCLUDEDIR from /homes/raviks/tools/linux/ncurses/include to /homes/raviks/tools/linux/ncurses/include/ncurses/. And provide this option in configure --with-ncurses=/homes/raviks/tools/linux/ncurses  
make && make install

Global
------
./configure --prefix=/homes/raviks/tools/freebsd/global CFLAGS="${CFLAGS} -static" LDFLAGS="${LDFLAGS} -static" LFLAGS="${LFLAGS} -static"
make && make install

Doxygen
-------
./configure --prefix /homes/raviks/tools/linux/doxygen --static
On FreeBSD, this doesn't compile due to curses errors.

Httpd
-----
Download apr and-x.y.z.tar.gz extract to ./srclib/apr
Download apr-util-x.y.z.tar.gz and extract to ./srclib/apr-util
Download pcre-8.32.tar.bz2 and install to /homes/raviks/tools/freebsd/pcre
cd pcre-8.32/ && ./configure --prefix=/homes/raviks/tools/freebsd/pcre && make && make install && cd -
cd httpd-2.4.3 && ./configure --prefix=/homes/raviks/tools/freebsd/httpd CFLAGS="${CFLAGS} -static" LDFLAGS="${LDFLAGS} -static" --with-included-apr --with-pcre=/homes/raviks/tools/freebsd/pcre --enable-mods-static="reallyall" --enable-pie --enable-example-ipc --enable-example-hooks --enable-echo --enable-so --enable-static-fcgistarter --enable-static-httxt2dbm --enable-static-htcacheclean --enable-static-checkgid --enable-static-ab --enable-static-htdbm --enable-static-logresolve --enable-static-rotatelogs --enable-static-htdigest --enable-static-htpasswd --enable-static-support --enable-cgi --with-port=8080
make && make install
Alternate config is: Configure apache-httpd as (look at options enabled):
./configure --enable-file-cache --enable-cache --enable-disk-cache --enable-mem-cache --enable-deflate --enable-expires --enable-headers --enable-usertrack --enable-cgi --enable-vhost-alias --enable-rewrite --enable-so --with-apr=/home/username/apache/httpd-2.4.2/srclib/apr --prefix=/home/username/apache/httpd-2.4.2/ --with-included-apr --with-pcre=/home/username/apache/pcre Note: When configuring apache-httpd, use option "--enable-ssl" ONLY if OpenSSL is installed otherwise DON'T enable it.
$ vi PREFIX/conf/httpd.conf
$ PREFIX/bin/apachectl -k start
$ PREFIX/bin/apachectl -k stop
$ ./config.nice --prefix=/home/test/apache --with-port=90

Lighttpd
--------
 ./configure --prefix=/homes/raviks/tools/linux/lighttpd --enable-static
 make && make install

Logrotate
---------
make POPT_DIR="/homes/raviks/tmp/popt-1.14/bin" CFLAGS="${CFLAGS} -D__NetBSD__ -I /homes/raviks/tmp/popt-1.14/bin/include" LD_LIBRARY_PATH="/homes/raviks/tmp/popt-1.14/bin/lib" LD_RUN_PATH="/homes/raviks/tmp/popt-1.14/bin/lib"
Other options are hardcoded into Makefile of logrotate. Source preserved.

Ncdu
----
PATH=/homes/raviks/tools/linux/ncurses/bin:$PATH ./configure --prefix=/homes/raviks/tools/linux/ncdu --with-ncurses=/homes/raviks/tools/linux/ncurses/ CFLAGS="${CFLAGS} -static -I/homes/raviks/tools/linux/ncurses/include/ncurses/ -I/homes/raviks/tools/linux/ncurses/include/" LDFLAGS="${LDFLAGS} -static -L/homes/raviks/tools/linux/ncurses/lib/"

Netcat
------
./configure --prefix=/homes/raviks/tools/freebsd/netcat --disable-nls CFLAGS="${CFLAGS} -static" LDFLAGS="${LDFLAGS} -static" LFLAGS="${LFLAGS} -static" && make && make install

Libevent
--------
./configure --prefix=/homes/raviks/tools/linux/libevent --disable-shared && make && make install

Tmux
----
./configure --prefix=/homes/raviks/tools/linux/tmux CFLAGS="-I$HOME/tools/linux/libevent/include -I$HOME/tools/linux/ncurses/include -I$HOME/tools/linux/ncurses/include/ncurses" LDFLAGS="-static -L$HOME/tools/linux/libevent/lib -L$HOME/tools/linux/ncurses/lib -L$HOME/tools/linux/ncurses/include -L$HOME/tools/linux/ncurses/include/ncurses -L$HOME/tools/linux/libevent/include" CPPFLAGS="-I$HOME/tools/linux/ncurses/include -I$HOME/tools/linux/ncurses/include/ncurses -I$HOME/tools/linux/libevent/include" && make && make install

Wget
----
./configure --disable-nls --disable-iri --disable-rpath --without-ssl --prefix=/homes/raviks/tools/freebsd/wget
make && make install

Installation Options:
*********************

Perl
----
Download ActivePerl from ActiveState website, then do:
./install.sh --prefix=/homes/raviks/tools/perl --license-accepted
