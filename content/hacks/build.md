% Build Systems
% Ravikiran K.S.
% January 1, 2006

## Make-based Build system maintenance guidelines

* Each directory should produce only single output - .so, .a, binary, .dll, etc.
* Each top level directory should leverage its sub-directories only.
* All dependencies should be part of project and referenced no-where else. If reference is inevitable, it should be explicit.
* All 3rd-party dependencies should be placed in single local shared "library" directory.
* This 3rd-party "library" directory should be named after complete version detail. ex. lib-1.2.3.4
* Each project should have a single root folder from where build is kicked in.
* All deliverables should be placed in a single local shared target output directory.
* An automated build script should be built from scratch and have no dependencies on IDE, etc.
* Build script should reference its build-tools by a configurable and fully versioned absolute path.
* Build script should reference 3rd-party "library" directory to link any dependencies.
* Build script should reference all source code by absolute path relativa to project base directory.
* Build script should not reference any object in project without using project base directory as reference.
* In complete project, no object should be referenced by relative path. Always absolute path should be used.
* For project root directory, define an environment variable that can always be referred to.
* Require build script to validate all required environment variables and exit with meaningful error if not found.
* Require build script to check all dependent build tools, libraries, 3rd-party dependencies and exit with meaningful error.
* Anything that could be rebuilt using project contents shoudln't be stored. viz, binaries, generated code, generated docs, etc.
* Keep a storage/server for all external dependencies that are backed up regularly. Maintain version names.
* Establish a continuous integration server (build machine) with NO development tools at all.

Guidelines on creating project directory structure:

## Ant vs. Make comparison (OR Why Make is bad for Java?)

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

