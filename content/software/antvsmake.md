% Ant Vs. Make
% Ravikiran K.S.
% January 1, 2006


The fundamental issue with Make and Java is that Make works on the
premise that you have specify a dependency, and then a rule to resolve
that dependency.

With basic C, that typically “to convert a main.c file to a main.o file,
run “cc main.c”. You can do that in java, but you quickly learn
something. Mostly that the javac compiler is slow to start up. The
difference between:

`$ javac Main.java $ javac This.java $ javac That.java $ javac
Other.java`

and

`$ javac Main.java This.java That.java Other.java`

is night and day.

Exacerbate that with hundreds of classes, and it just becomes untenable.

Then you combine that with the fact that java tends to be organized as
groups of files in directories, vs C and others which tend towards a
flatter structure. Make doesn't have much direct support to working with
hierarchies of files.

Make also isn't very good at determining what files are out of date, at
a collection level.

With Ant, it will go through and sum up all of the files that are out of
date, and then compile them in one go. Make will simply call the java
compiler on each individual compiler. Having make NOT do this requires
enough external tooling to really show that Make is not quite up to the
task.

That's why alternatives like Ant and
[Maven](http://kent.spillner.org/blog/work/2009/11/14/java-build-tools.html "http://kent.spillner.org/blog/work/2009/11/14/java-build-tools.html")
rose up.

Derived from
[here](http://stackoverflow.com/questions/2209827/why-is-no-one-using-make-for-java "http://stackoverflow.com/questions/2209827/why-is-no-one-using-make-for-java").

