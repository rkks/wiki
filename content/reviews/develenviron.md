% Practical Development Environments by Matthew B. Doar (Category J)
% Ravikiran K.S.
% January 1, 2006

*“Projects have Environments in which People produce Products.”*

## Dirty Secrets of Software Projects

1.  We can't rebuild the same product that we shipped to the customer,
    so we'll ship her the latest version, just to be safe.

2.  Building from scratch (also known as a clean build) each time is
    often the only way to get the product to build reliably. This takes
    so long that the developer goes off and does something else.
    Consequently, every developer thinks that his build tool is too
    slow.

3.  Testers, technical writers, and managers can't build the latest
    version of the product. Even if it has already been built for them
    by someone else, it's unclear where to find the latest version.

4.  Fixes for known bugs are not released, because it's too hard to
    properly test the required changes and they may break other parts of
    the product.

5.  Everyone finds the bug tracking tool awkward to use.

6.  The environment used by developers and the one in which the product
    is used by customers are different in some important ways, so the
    developers never actually see problems the same way that a customer
    does.

7.  Getting a product ready to release takes so long that any
    late-arriving changes don't get tested or documented.

8.  The wrong set of bits gets shipped to the customer. This one should
    make anyone involved in developing software wince, but it does
    happen.

9.  Nobody knows when to remove a feature from a product, because no one
    knows whether the feature is actually being used.

10. Communication between groups happens by reading printouts left at
    the printer nearest to the coffee machine.

## Three Strikes And You Automate

1.  The first time you do something, you just do it manually.

2.  The second time you do something similar, you wince at the
    repetition, but you do it anyway.

3.  The third time you do something similar, you automate.

(Martin Fowler, Refactoring: Improving the Design of Existing Code,
Addison-Wesley, 1999)

### Some of the Automation Environment Tasks include

1.  Checking out the latest versions of source files

2.  Calculating the appropriate build or release number

3.  Tagging the source files

4.  Building one or more products from the virgin source code

5.  Testing the products with both unit tests and system tests

6.  Moving the generated release packages to a suitable location for
    other people's use

7.  Marking certain bugs as having potential fixes available in this
    release

8.  Creating change logs and release notes about what changed in this
    release

9.  Notifying people when a release becomes available, and also
    notifying the responsible individuals when a build or test fails

10. Publishing test reports and build logs to a web site

11. Collecting project data and running static analyzers on the source
    code

12. Regenerating the dynamic parts of a project web site

**Some of public Automation Environments**: Tinderbox (Mozilla), and
CruiseControl.

## what's there in a name

### A build label should include

1.  **Build type** - Was this build for internal testing (QA) or release
    (REL)?

2.  **Version** - The major, minor, and patch numbers.

3.  **Build number** - A number that uniquely identifies each build.

4.  **Date** - The year, month, and day when the build was started, in
    that order so that builds can be sorted on this field.

5.  **Special** - If the build is otherwise significant, add an optional
    field at the end. An example is BETA for a beta release.

**Example**: QA\#1\_3\_0\#129\#2005\_07\_09\#ALPHA A release number of
the form major.minor.patch uses three separate integers; the major,
minor, and patch release numbers. This scheme communicates something
about the degree of change and maturity of each release. Generally, the
patch number is changed for bug-fix releases, the minor number is
changed for releases with new features, and the major number is changed
for releases that break compatibility with prior releases in some
significant manner.

Another requirement for a build label is that it should not contain
characters that are illegal in any of the contexts that the label will
be used in-that is, SCM labels, file and directory names, field values
in your bug tracking system, web pages, and documentation. Characters to
avoid include $, \<, \>, |, /, \\, spaces, and tabs. Underscores don't
show up in underlined links on web pages. Some SCM tools have their own
restrictions as well. Build labels don't need to have a branch name in
them, since the version number and build number can tell you whether a
branch was used for the source code.

### Choosing Project Names

Project names are usually chosen by engineering groups, with one name
for each significantly different version of the products that they are
working on. Product names, on the other hand, are the names that
customers see, and these names are usually chosen to help a product sell
or to become popular. Some general guidelines for choosing names for
projects are:

1.  Keep it short

2.  Use distinctive sounds

3.  Use low-frequency letters (the ones not common in normal english
    words)

4.  Make it unmarketable

### Choosing Machine Names

1.  Don't overload other terms already in common use

2.  Don't use your own name

3.  Don't use long names (anything longer than 8 character irritates)

4.  Don't use digits at the beginning of the name

5.  Don't expect case to be preserved

### Naming Branches and Tags

1.  All branch names end in \_branch or \_b. Tag names do not.

2.  Private branches and tags should have \_private in their name.

3.  Tag names that are connected to points where branches occurred
    should have \_bp (for “branch point”) in their name. Another idea is
    to start the names of branch point tags with Root-of.

4.  Tag names that are connected to points where merges occurred should
    have \_mp (for “merge point”) in their name.

5.  Non-private branch names created by users should be collection of
    username\#feature\#rfeId in case of feature development,
    username\#issue\#bugId in case of bugfix, and username\#int\#intId
    for integration branches. Any main branches created for product
    should have product\#main for main branch, and product\#test for
    test
branches.

### Some more general advice about the naming of files and directories in a project

1.  When naming directories, make sure their names start with different
    characters. Then completing their names will be easier when using a
    shell prompt at the command line.

2.  Use common prefixes for the names of files within the same
    directory. The extra information can give you more of an idea about
    where to find the file.

3.  Don't reuse directory names that are significant in your operating
    system (e.g., sys in Unix and system in Windows). It's confusing,
    and one day some tool will pick up files from the wrong sys
    directory and you may not even realize it.

4.  Avoid embedding version numbers into the name of a file or directory
    that's managed using an SCM tooltracking versions is what the SCM
    tool is for\! Put a version into a filename only if there is an
    occasion when multiple versions might be used at the same time.

## Right Tool for Right Use

First, other members of the project are likely to have suggestions for
different tools. Send out a request for information, making sure it has
an obvious deadline for replies. Specify what the tool must do and what
you would merely like it to do. Be sure to ask people about whether they
have actually used their recommended tool, not just evaluated it. Make
sure you ask them whether they have administered the tool since using
and administering tools can be very different experiences.

### Steps When Changing Tools

1.  It is helpful to start with a clear and broadly agreed-on
    understanding of what the tool must do and not do, and what features
    would be useful but are not essential. These requirements will
    probably change as you investigate the available options, but
    they're a good place from which to start.

2.  Create a document that summarizes the different choices and their
    perceived advantages and disadvantages, or rate them in a number of
    different areas. Make sure you know which requirements are mandatory
    and which are merely desirable. Propose some tentative schedules for
    how the tool could be introduced.

3.  Evaluate the two or three leading choices, ideally by installing
    them locally on a development machine and using them with your own
    data or files. Record all the key information about installing,
    configuring, and using a new tool while you are doing it and it is
    fresh in your mind. Other ways of evaluating an open source tool are
    reading the source code and looking at the number of bugs, the
    number of messages on mailing lists, the amount of recent SCM
    activity, and the download statistics, if available.

4.  Talk with senior developers on your project about their impressions
    of each tool. Ask the tool's vendor or project administrators for
    names of people who already use the tool that you can talk to. Add
    all these comments to the document, but keep them separate from each
    other, since they come from different perspectives.

5.  Discuss the document with the senior members of the group and come
    to a decision. It helps if you can make a business case for the cost
    of each tool and the expected return on investment. Add people's
    names and a date, and save this document for revisiting in the
    future when people ask why a particular tool was chosen.

6.  Document how you expect the new tool to be used and send this out to
    the whole project. Offer to describe it in person if your numbers
    and locations permit.

7.  If at all possible, bring up the new tool in parallel with an
    existing tool and migrate people to the new tool gradually. If a
    switchover date is necessary, publicize it well in advance and make
    sure that extra support is available for the tool when it goes live.

8.  Archive and move the old tool aside so that it isn't used by mistake
    or ingrained habit. It may still occasionally need to be used in the
    future, it may still be required for generating patches for older
    releases. If the tool is installed on peoples' individual machines,
    send detailed instructions about how to migrate safely from the old
    tool to the new one.

9.  Update the documents about the development environment, especially
    the documents for new developers.

### Authentication, Authorization, and Accounting

Changes to a project should be made only by the people who have been
both authenticated and authorized to make them.

### Some practical suggestions for securing your SCM tool

(CVS in particular)

1.  Use separate and well-secured machines as SCM servers, which few or
    no developers can log in to directly. If you have secure server
    rooms, keep your SCM machines in there. Emergency power is often
    available in server rooms, which helps keep your filesystem intact,
    as do redundant disks.

2.  Use encrypted connections from SCM clients to SCM servers,
    especially if there is a wireless connection involved anywhere in
    the network. If people have to have accounts on the SCM server, use
    a secure shell such as smrsh to limit the commands that they are
    allowed to execute.

3.  Carefully guard the physical security of your backups of the
    repository. Destroy the physical media of outdated backups.\[2\]

4.  Track each change in the repository using notifications of commits
    and inspection of diffs. Train developers to expect to see email
    when they make changes and to occasionally confirm that the
    information in the email is also appearing in any change logs.

### What should you look for in an SCM tool?

1.  Confidence in the integrity of your data

2.  Fast and simple creation of tags, extraction of tagged files, and
    generation of diffs

3.  Good support for branching and merging, ideally with both
    command-line and graphical interfaces

4.  Integration with other existing tools such as bug tracking systems

5.  A good web interface to let people browse the different versions of
    their files and also to search through earlier versions of the files

6.  Good support from the tool vendor or the tool's community

### What should you look for in a build tool?

1.  It should have accurate dependency checking.

2.  Startup and dependency checking should be fast. The build tool
    should use tools such as compilers intelligently.

3.  Builds should be independent from the local user environment in
    which the build tool was started. This makes it easier to reproduce
    builds on different machines.

4.  Variant builds for debugging or with extra optimization should be
    easy to specify, preferably with a single argument to the build tool
    on the command line.

5.  It should include support for many platforms and languages,
    particularly if the product is open source. The ability to build
    using the same source tree on multiple platforms at the same time is
    helpful.

6.  It should be easy to write and read the build files, and the build
    tool should already be understood by many of the project members.

7.  It should be scalable, with support for parallel builds.

8.  It should have support for debugging builds and build file problems.
    Graphical display of the dependencies and changes in dependencies is
    a bonus. Clear output from a build tool about what command-line
    arguments were used and which user started the build is helpful.
    Minimizing the number of complete commands that are displayed can
    also make build logs easier to read, though the complete commands
    and their output should also be logged somewhere for each build.

**Advised Tools**: Ant or Jam, Autotools or SCons.

### Changing Your Build Tool

1.  **Search for advice** - Search carefully for examples of how other
    people have used the new build tool. Create a few local prototypes
    before going to the effort of creating all of the new build files.

2.  **Start with a working build** - Make sure that the existing build
    is working, at least for the major targets that are used by the
    project.

3.  **Capture all the output from a working build** - Generate a text
    logfile that shows the complete details of every command that the
    build currently executes, including any warnings from compilers and
    other tools. Now make your new build tool do what the old one did,
    as closely as possible.

4.  **Use the existing directories** - If at all possible, create the
    new build files for the new build tool using the existing directory
    structure. This is one less variable changing at the same time.

5.  **Use temporary scripts** - If there are parts of the original build
    files that the new build tool doesn't support until you customize it
    in some way, you can still make progress by hardcoding the old build
    commands into a script file and calling that with your new build
    tool.

6.  **Compare the results** - If you run both the old and new build tool
    in parallel for a few days or weeks, then you can compare every
    generated file.

## Backups and SCM

1.  Disaster recovery

2.  Corruption detection

3.  Intrusion detection

### SCM Checklist

1.  What is saved in your SCM system? What is not in your SCM system?
    Why?

2.  What have you overlooked? Often the only time this question is
    carefully answered is when a hard disk dies and you try to recreate
    your environment. List all the files, tools, and other pieces of
    your environment that you use to build a release.

3.  Can you still recreate older releases if a file is renamed?

4.  Can you still recreate older releases if a directory is renamed?

5.  How do you know the date on which a file was branched?

6.  How do you know the intended purpose of each branch?

7.  Who can change permissions for write and read access to the SCM
    tool?

8.  What happens with your SCM tool if two files in the same directory
    have the same name, but one is uppercase and one is lowercase? What
    happens if a filename has spaces in it?

9.  How does the backup size of your SCM tool's files change over time?
    When will you next fill up a key disk, CD, DVD, or tape?

10. Can you develop on a laptop on an airplane? How much of your SCM
    tool still works, and how do you resynchronize when you reconnect
    later on?

11. How would you add a process to your SCM toolfor example, requiring
    each change to be reviewed by other people?

12. Do you have good integration between your SCM tool and your bug
    tracking system?

13. How do you decide when to upgrade your SCM tool, whether it's to fix
    bugs in the tool or for extra functionality?

14. What is the most common mistake that people using your SCM tool
    make? How could you help them to avoid doing that?

## Build System

### The Different Stages of a Build

1.  **Define the targets** - What would you like to build? Everything?
    Just one file? A subset of files?

2.  **Read the build files** - The names of the executables and the
    source files for each executable are defined explicitly somewhere,
    often in build files.

3.  **Configuration** - The build tool discovers which platform and
    tools are to be used.

4.  **Calculate the dependencies** - The build tool scans the build
    files and source files to work out which parts of the program depend
    on which other parts. This stage also reports any errors such as
    circular dependencies, where the chain of dependencies has a loop in
    it.

5.  **Determine what to build** - Using the dependencies, the build tool
    works out which files need to be updated or generated. It reports
    errors such as nonexistent source files or files that couldn't be
    generated.

6.  **Construct the build commands** - The build tool assembles the
    appropriate commands to update the out-of-date parts of the program.

7.  **Execute the build commands** - The build tool runs the commands to
    update the files that need updating and reports any errors returned
    by the commands.

### Build States

The build state is the state of the source files used by the build tool
when it starts a build.

1.  **Virgin** - A completely fresh set of source code files, never
    before used in any build.

2.  **Up-to-date** - No changes have been made in the source code files
    (or in any generated files) since the last build.

3.  **Changed** - Changes have been made in the set of source code files
    or generated files since the last time the build process was
    started.

4.  **Interrupted** - The last time that the build tool was run it was
    interrupted, so some files may be incomplete or have unexpected
    contents.

5.  **Clean** - All the files that were generated by a previous build
    have been deleted from the source files.

### Common Build Problems

1.  **Command-line length** - Some reasons for this are absolute
    pathnames being used for all files, or long file lists being used to
    maximize the number of files processed each time that a
    slow-to-start compiler is invoked. Build tools can sometimes avoid
    this problem by using a temporary file containing the list of files
    to be used by another tool.

2.  **Filename formats** - If you want to build your project on multiple
    platforms, then a build tool with good support for avoiding
    filesystem-specific names is helpful. The easiest build tools to use
    are ones that allow you to specify a filename in one format and then
    automatically convert the filename to the appropriate format for the
    intended platform.

3.  **Unit tests for build tools** - A collection of small test projects
    provides an excellent place to develop new techniques for using and
    testing your build tool.

4.  **Identifying builds** - Unique identifiers for all builds that are
    to be used by other people helps clear communication within a
    project. A label such as QA\#1\_3\_0\#129\#2005\_07\_09 can identify
    a build as build 129, for internal testing (QA), Version 1.3.0,
    which was built on July 9, 2005. This kind of build label has many
    uses for SCM tools, bug tracking systems, and file and directory
    names.

### Slow Builds

1.  **Profile the build** - The first thing to do with a slow build is
    find out where most of the time is being spent. You can do something
    as simple as logging the current time at various interesting points
    in your build files. Some build tools such as SCons can provide more
    complete profiling information by treating the build tools as an
    application to be profiled just like any other application.

2.  **Build only once** - Incorrect dependencies in build files may mean
    that files are built more than once or that other files that aren't
    needed are being built. To make sure this isn't happening to you,
    extract filenames from the build log and make sure each one is
    expected and occurs only once. Profiling can sometimes help here
    too.

3.  **Use a build server** - If developers' desktop machines seem
    underpowered for the size of the project, you can try using shared
    build machines with more disk space and RAM. If there are a lot of
    disk accesses during a build, then a RAM disk cache (a ramdisk) may
    help. Of course, using a single build server makes your development
    environment more vulnerable to a single point of failure.

4.  **Create staged builds** - If the build for your product is taking
    more than an hour or two, it may be time to consider splitting it up
    into multiple builds, where each one builds a subsection of the
    product. The results of the builds can then be staged that is, made
    available for other people in the project to use as starting points
    for their own builds.

5.  **Use ccache** - When you compile a C file, the C pre-processor
    first creates an expanded source file, which is what is actually
    compiled into an object file. ccache
    ([http://ccache.samba.org](http://ccache.samba.org "http://ccache.samba.org"))
    is an application that keeps a copy of the generated object file,
    and if the expanded source file hasn't changed the next time that it
    needs to be compiled, then ccache just returns the saved copy
    instead of wasting time recompiling the file. If the file has
    changed, then ccache compiles it as usual. To do all this
    accurately, ccache has to also record which compiler flags were used
    when the saved object file was created. This idea is great for
    builds where you rebuild lots of files the same way every time (for
    instance, virgin builds), or if a build tool is rebuilding files by
    mistake.

6.  **Use parallel builds** - Many build tools have arguments to run
    different parts of a build in parallel. Depending on how good the
    build tool is at separating the build into different parts, this may
    improve your build times, or it may not do much. The only way to
    find out is to try it, which is usually a straightforward matter of
    passing another command-line argument to your build tool. Another
    approach is to use a distributed compiler such as distcc instead of
    expecting the build tool to handle distributing the work for the
    build.

7.  **Use distributed compilation** - Using a cluster of machines to
    build a product is sometimes helpful. One tool that does this is
    distcc
    ([http://distcc.samba.org](http://distcc.samba.org "http://distcc.samba.org")),
    an open source front-end to the GNU gcc compiler that distributes
    preprocessed files to remote machines for compilation. After being
    compiled remotely, the object files are then returned to the main
    build machine. distcc assumes that the build dependencies calculated
    by your build tool are correct and can be cleanly partitioned for a
    distributed compilation. Electric Make is another high-end
    distributed build tool.

## Building Software Checklist

1.  When you build the product, do the generated files end up mixed in
    with the source files, or are they in a separate location?

2.  You made a small change to a few files, but then rebuilding the
    product took much longer than you expected. Can you find out why it
    took so long? Exactly which files were rebuilt, and why was each one
    rebuilt?

3.  How long does an up-to-date build take with your build tool? (As
    explained in the earlier Section 5.2, an up-to-date build is one
    where nothing has changed since the last build and so the generated
    files are already up-to-date.)

4.  How long do the other kinds of build (virgin, changed, interrupted,
    and clean) typically take for your product?

5.  How do you cause just a subset of your product to be rebuilt? Do you
    have to delete a magic file? Specify some target name to the build
    tool? Change to a special location in your source tree?

6.  How do you debug a build process? Can you debug where incorrect
    dependencies come from?

7.  Can you use different versions of your build tools on the same
    machine with confidence?

8.  Can you list all the other tools (and their versions) that your
    build tool depends on? Are they safely stored somewhere in such a
    way that you could recover them after a disaster? When was this
    backup last updated?

9.  Can you list all the operating systems (and their versions) that
    your build tool depends on? Are they safely stored somewhere in such
    a way you could recover them after a disaster? When was this backup
    last updated?

10. How long did all the different kinds of builds take in the past? Can
    you predict when your build will become too slow?

11. Do you think that a parallel build would improve your build times?
    How do you configure your build tool to run parallel builds?

12. If you want to produce a different product using the same build tool
    and build files, in how many places do you have to add information
    about the new product?

13. If you want to add a new type of build (e.g., with a different set
    of debugging arguments to a compiler or to generate PDF files
    instead of HTML), how many of the build files do you have to change?
    How many places do you have to make these changes in each build
    file?

14. If you had to change the name of a product or a project, or the name
    of the company, how many build files would you have to change?

15. Finally, the real motivation for this chapter: how much time per
    week do the project's developers spend wrestling with the build
    tool, waiting for slow builds, and investigating phantom bugs that
    are due to inconsistent builds?

## Test Environment

### At a minimum, a test environment should allow you to

1.  Run a number of tests.

2.  Decide whether the tests ran properly and whether any tests hung. A
    hung test should not prevent other tests from being run.

3.  Determine what the result of each test waserror (in the test),
    failure, or successand also explain why.

4.  Summarize the results of the tests in a
report.

### A good test environment can help with the following before any tests are run

1.  **Checking resources** - If an external resource (such as a database
    or a web site) is a prerequisite for running a group of tests, then
    it often makes sense to check that the resource is available just
    once, not before every one of the tests.

2.  **Defining expected results** - Defining the expected results is
    part of defining a test. Some tests may be expected to fail. This is
    true when a bug has been found and a test has been written to
    demonstrate this failure, but the bug has not yet been fixed. Other
    failing tests may be tests that are themselves waiting to be fixed.

3.  **Tracking primary owners** - Test environments can make it easy to
    change who owns each test, ideally by using a UI or a simple text
    search and replace. It should always be easy to identify who will be
    asked to make a broken test work again. With bugs, even if it's not
    clear until the bug has been investigated more thoroughly who really
    owns the faulty code, at least there's a name to start from. If the
    same test keeps failing due to changes in an unrelated area, then
    the owner of the test can offer to let the owner of the other area
    own the test, without having to change who owns the source code.

4.  **Tracking secondary owners** - Providing a secondary owner of a
    test in case the primary owner leaves the project is another useful
    idea. That way, email messages about failed tests should always
    reach someone, who may well feel motivated to reassign the tests to
    a new primary owner, if only to stop the email.

5.  **Generating test data** - Some edge cases or boundary conditions
    can be tested for automatically. For instance, if a method accepts
    an integer as an argument, then values of 0, -1, and 1 are probably
    worth using in a unit test. Similarly, very short and very long
    strings, strings with spaces and punctuation, and non-ASCII strings
    are all good candidates for testing APIs that have string
    parameters. If your product uses text files, try passing an
    executable file to it as an input test.

6.  **Handling input data** - If you are using fixed data, make sure it
    is stored in such a way that it is easy to change, such as in a file
    that's used for input by the test, rather than being compiled into
    the test executable. This also implies that test environments should
    be able to read from data files.

7.  **Generating random numbers** - A test environment should have a
    good random number generator for generating data and for making
    choices in tests. Good ones are hard to find, and they're easy to
    use in ways that make the results decidedly nonrandom, so guidelines
    about using them properly are helpful. Somewhat paradoxically, if
    you do need a random number generator, you should make sure that its
    output can be precisely repeated, so that you can debug your tests.
    This is usually done by passing in a “seed value” to the function
    that initializes the random number generator. Using the same seed
    value will produce the same sequence of random numbers it's where
    you start in the sequence that isn't random.

### Some aspects of a test environment that help when running tests are

1.  **Single tests** - The ability to run just one test quickly
    dramatically improves the turnaround time for retesting small parts
    of a product.

2.  **Groups of tests** - Being able to refer to tests in groups and to
    have the same test be part of multiple groups means that you can run
    test groups such as “the ones that failed last time” or “just the
    tests for Windows.”

3.  **Independent, idempotent tests** - Ideally, tests should have no
    side effects and should leave the environment in a known state. This
    means that you should be able to run the same test over and over
    again, or run the tests in any order. Test environments that help
    you get this right will save you hours of time trying to debug tests
    that failed only because the test that was run just before them
    failed. Running the tests in a different order also simulates more
    closely how customers may use the product.

4.  **Parallel processes** - Tests that have to be run at the same time
    and be synchronized with each other are hard to run in most test
    environments. Most testers simply try to avoid these tests or design
    the product so that it can be put into states where it will wait for
    certain inputs for synchronization. The useful software clock
    resolution on many machines without real-time operating systems is
    only tens of milliseconds, so microsecond synchronization of
    software is most unlikely to be supported by test environments.

5.  **Background processes** - Even if parallel processes don't have to
    be synchronized with each other, some tests will need processes to
    run in the background, and output logging and deadlock detection
    should work for these processes just as they do for other tests. A
    test environment should also provide a clean way to check for
    orphaned background processes before the next test starts.

6.  **Multiple machines** - The use of multiple machines for a test
    means that the test environment has to provide a way of executing
    commands on remote machines, and also a way to collect the results
    of the commands for processing as part of creating a report. The
    test itself may have to be written so that it can be started and
    then wait for a signal to proceed.

7.  **Multiple platforms** - Tests that run on many different platforms
    (for example, both Unix and Windows) benefit greatly from a test
    environment that hides the differences between the platforms from
    the person writing tests.

8.  **Capturing output** - Good test environments can make capturing
    both the output and any errors from each test much easier. Keeping
    textual output and errors in the same place makes it easier to see
    when errors occurred relative to other output. If they have to
    remain separate, then timestamps are required, with as much
    resolution as the platform will provide. It's also helpful to be
    able to tee the output from each test to be able to monitor the
    progress of each test.

9.  **Scanning output** - When the output of a test has been captured,
    usually into a file, the result of a test is often determined by
    searching for certain strings in the output. A good test environment
    can help by making the common cases simple to implement, while also
    supporting complex regular expressions or even state machines driven
    by the content of the output file. If there is some part of the
    output (such as the starting time or memory addresses) that changes
    every time a test is run, make sure that this output either is
    separate from other output or can be filtered by the comparison
    tools that are being
used.

### Some features of a test environment that are helpful after tests have been executed include

1.  **Distinguishing between failures and test errors** - Tests can
    fail; this is expected when a product isn't behaving as expected.
    Tests can also have errors that is, when the tests themselves fail
    to run correctly. Test environments that make a clear distinction in
    their test definitions and test reports between these two cases help
    reduce confusion. Test reports also have to make it clear when a
    test failure was expected. Of course, tests that fail intermittently
    without a clear explanation eventually tend to not get run.

2.  **Tracking which files were tested** - Since tests may have names
    like test019 that are unrelated to the source code that they are
    testing, all other clues about which files, classes, methods, and
    functions a particular test was exercising are helpful. For
    instance, you could look at the definition of a test to see which
    methods it called; or your test environment could provide you with
    that information and maybe add links to those methods to the
    detailed parts of test reports.

3.  **Flexible report formats** - A wide variety of ways of
    communicating results is useful. Generating a set of HTML web pages
    is one common way to present reports, but text-based email, PDF,
    Word, or spreadsheet files are sometimes more appropriate. To make
    generating test reports in a variety of formats easier, it's often
    useful to store the results in a structured file format such as XML.
    Some test environments use databases to store their test results.

4.  **Storing test information** - Once tests have become stable enough
    to be run automatically as part of a build, it's a good idea to use
    an SCM tool to store both the source code for the tests and the test
    results. The tests and their results should be tagged with the same
    build label that the build was tagged with. That way, you know which
    tests were run against a particular build, and also which versions
    of those tests were used.

5.  **Historical reports** - Some test environments provide tools to
    create historical reports about test results. Graphs showing the
    total number and proportion of tests that pass or fail against time
    can provide a sense of a product's stability. Like any project
    statistic, these figures are useful only if you understand how the
    information was gathered and what kinds of errors exist in the data.

6.  **Creating bugs** - Once a test has failed, it should be easy to
    create a bug about the failure. Creating a bug should always have a
    human involved to act as a filter for what is added to the bug
    tracking system. However, once the failure has been identified as a
    genuine bug, filling in the fields of the bug and attaching logfiles
    should be handled as automatically as possible.

### Testing Software Checklist

1.  Can you use your test environment to run just one test?

2.  How do you decide when to automate a test?

3.  Do you ever discard tests? If so, how do you decide when? If not, do
    all of your tests actually help you to test the product?

4.  When are automated tests run? Is someone automatically informed
    about failed tests?

5.  Can your test environment detect when a test has become deadlocked?
    Can you kill it, and can the other tests still be executed?

6.  How do you know who to contact about a particular test when it stops
    working?

7.  How do you know which parts of your product are the least tested?

8.  When was each aspect of the product last tested, and where are the
    results?

9.  What resources do each of your tests require?

10. How could you change your product to make it easier to test?

11. How much work would it be to test your product on a new platform?
    Does your test environment help you with this?

12. When a bug is fixed, do you know which precise test will confirm
    that it is fixed?

13. Which versions of the tests and test environment tools were used
    with which release?

14. If your product uses color anywhere in it at all, have you had a
    color-blind person test those parts?

15. Most importantly, does your test environment make testing your
    product easier?

## Tracking Bugs

### A number of characteristics to look for in any bug tracking system

1.  Easy to enter new bugs - There should be a minimum number of fields
    that have to be filled in, just enough so that if any of them were
    empty, then the entire bug would be useless.

2.  Easy to change the state of a bug - The different states that a bug
    can be in varies widely with different products and projects, so a
    convenient state diagram that shows how bug states are expected (and
    permitted) to change is very useful.

3.  Easy to change many bugs - The ability to change a number of bugs
    with a single operation can save a lot of time, especially if the
    list of bugs is the result of running a report.

4.  Easy to review a particular bug - Given a particular bug identifier,
    you should be able to view and edit that bug quickly and
    intuitively.

5.  Easy to watch a bug - It should be easy to become aware that
    something in a bug has changed. Email is the usual way to notify
    people about this.

6.  Easy to search using brute force - When all other ways of finding a
    particular bug fail, you should be able to search all the text
    fields for a given string. It may not be efficient, but when you are
    stuck, you really need to be able to do this.

7.  Easy to trace the history of a bug - You should be able to see how a
    particular bug has changed over timewhat changed, who made the
    change, and when.

8.  Easy to connect to source code
changes

### Characteristics to look for related to using the bug tracking system to manage a project are

1.  Easy to generate reports

2.  Easy to produce historical reports - Not all bug tracking systems
    let you produce reports about how the bugs have changed over time.

3.  Easy to connect to a schedule - The list of which bugs should be
    fixed for a particular release should be straightforward to enter
    and to create reports about. If one bug depends on another, then
    that also should be made very
obvious.

### Desirable features that are more closely related to administering a bug tracking system include

1.  Easy to modify the definition of a bug -

2.  Easy to modify the workflow -

3.  Easy to manage the list of users -

4.  Scalable - As the number of bugs, groups, and users in a bug
    tracking system grows, the interface design must also scale well.

5.  Accessible through external clients; good API - You should be able
    to read and modify data in the bug tracking system from outside the
    GUI or browser.

6.  Easy to change the text displayed - It should be easy to change the
    text that is displayed onscreen for usernames, release numbers, and
    other field names.

7.  Easy to back up different parts of the bug tracking system - You
    should be able to save copies of reports and UI screens before
    modifying them, in case of errors.

### Bugs Tracker Checklist

1.  How long does it take to enter a new bug? (More than a minute for a
    simple bug can seem like an awfully long time when you have do it
    over and over again.)

2.  Can you find the definitions of the reports that you regularly run?

3.  Can you make copies of these reports, edit the copies, or back up
    the definitions of the reports to somewhere outside the bug tracking
    system?

4.  How are bugs expected to progress through different states? Is there
    a diagram of this anywhere that users of the bug tracking system can
    easily find?

5.  How do you make sure that the correct people are aware of all
    changes to a bug?

6.  If you wanted to reassign 20 bugs to someone, would you have to do
    this one bug at a time, or is there an easier way?

7.  Can you change a comment field in a bug after the fact?

8.  How do you create historical reports using your bug tracking system?

9.  How do you add or remove values such as release names from a field?

10. How do you modify the definition of a bug?

11. When is the data in the bug tracking system backed up? When was the
    backup last tested by recovering it into a standby system?

12. How big an attachment is deemed reasonable to add to a bug? 100KB?
    1MB? 10MB?

13. How regularly is your bug tracking system cleaned up?

14. How will you move your current data to your next bug tracking
    system?

15. Most importantly, does your current bug tracking system really help
    or hinder your project overall?

## Documentation Environments

### File Formats for Documentation

Some common requirements of a file format and the tools that support it
are:

1.  Typeset printing, often using different file formats, sizes, and
    layouts

2.  Online viewing, often with hyperlinks

3.  Images interleaved with text

4.  Searching documents for text or formatting

5.  Support for non-English languages and characters

6.  Comments that can be mixed with text for reviews but that don't
    appear in the final product

7.  Joining and splitting files

8.  Generating lists of the differences between versions of the
    document, or diffability

### Documentation Checklist

1.  How do you tell someone that this error exists? Do you file a bug?
    Send her email? Annotate a printout?

2.  Can you easily find out who is responsible for the particular
    document?

3.  Where do you find the version number of each document?

4.  How do you know when to check for a corrected version?

5.  Where would you go to find the corrected version?

6.  Can you view and print the released documents from all the
    environments you work in?

7.  When do updates to the documentation appear?

8.  How is feedback from reviewers incorporated into your documentation?
    What sort of information do you imagine is lost or garbled during
    this process?

9.  When you want to resolve conflicting input from different reviewers,
    is there a record of who approved each change to the document? This
    is often available either as part of the document itself or by using
    an SCM tool.

10. What file formats do you deliver to customers, and why?

11. How long does it take to convert all the source files for your
    documentation to the formats that are delivered to customers?

12. How much of the time generating the documentation is spent doing
    things manually that have to be repeated every time the
    documentation is generated?

13. On how many different machines can you create the released versions
    of the documentation?

## Releasing Product

### Before the Release

1.  **What are the user System Requirements**? - Hardware, Operating
    system, Other software dependencies, License keys, Legal (GPL,
    Commercial, etc), Effort (Some indication of how long a simple
    installation should take, post install operations if any, like
    reboot etc.)

2.  **Build Numbers** - major.minor.patch. Adding a suffix of “rcn” for
    “release candidate n.” Ex. 4.0.0-rc1

3.  **Release Information** -
    
    1.  **Project name** - The internal project name for this build.
    
    2.  **Build number** - The build number that makes each build unique
        for the same release number.
    
    3.  **Build timestamps** - The times the build started and finished,
        including the time zone.
    
    4.  **User** - The username of the account that was used for the
        build. If the build tool is configured correctly, the build
        should be independent of the user's particular environment, so
        this is a safety check.
    
    5.  **Build type** - Was this build intended for internal testing or
        for customer use? Is it an official build by a release team, or
        an unofficial build? Is it for an alpha or a beta release, or
        some other kind of release-candidate build? Was it built with
        profiling, debugging, or optimizing enabled?
    
    6.  **Build log** - A link or location for the commands used during
        this build.
    
    7.  **SCM details** - The build label used to tag the source files
        for this build, and the name of an SCM branch if used. If
        confirmation of the SCM tool is required, then a manifest or BOM
        that lists all the source files and their versions can be added.
    
    8.  **Bugs** - Identifiers for any bugs that are related to this
        build.
    
    9.  **Operating system and tools** - If some parts of a release,
        such as a compiler or the operating system, are not controlled
        using the SCM tool, then you can at least record information
        about them here. Most tools will generate a version number if
        prompted correctly, and this can be generated as part of the
        build tool's configuration stage, when it is making sure that it
        has the required tools for the build.
    
    10. **Size** - Recording the size of a build is useful for tracking
        changes over time or checking that a build seems superficially
        complete.
    
    11. **Test results** - The versions and results of all automated
        tests that have been run on this build should be easily
        available given a particular build label.

### Quick Fixes and Engineering Specials

A minimal procedure for unsupported releases has the following
requirements:

1.  You should explain to the customer that this release is unsupported
    and what that means. Documenting the agreement in writing will help
    everyone later on.

2.  All source files involved in an unsupported release must be checked
    into the SCM tool and labeled before release. This is an appropriate
    case for using a suitably named private branch of just a small
    number of files.

3.  The labels for each unsupported release should be recorded
    somewhere, along with the customer's name, the date, and any related
    bug numbers.

4.  The build information associated with the unsupported release should
    show who built it. This is useful for finding holes in the process
    if an unaccounted-for release turns up later on in the field.

5.  Once the customer is satisfied, you should tell her which official
    release will include the changes that she helped to test.

### Creating the Release

When a build is converted to a release for internal testing, or an
internal release is converted into a customer release, the following
steps are commonly followed:

1.  Obtain virgin copies of the correct files using the local SCM tool.
    The version of the product that uses these files has already been
    built and tested.

2.  Set the release number and other build information, either in a
    file, a database, or on the command line.

3.  Build the product for each platform.

4.  Build the packages that are released to customers.

5.  Update the release notes as part of the packages.

6.  Retest the different packages prior to releasing them. Don't just
    check that the installer worksrun as many of the unit and system
    tests as possible against the installed product.

7.  Use the SCM tool to tag all of the files that are part of the
    release, including all automated tests and test results.

8.  Archive all customer releases and any other important builds.

**Packaging Formats**: They should be a commonly available format on
that platform. Windows - Zip, Unix - Tar.gz, or Tar.bz2

### Installation Tool requirements

When evaluating an installation tool, be careful to distinguish between
what the installation tool should do and what the installers that it
produces should be able to do. For instance, you may want to produce
installers that have Japanese text, but do you also need the
installation tool to be localized for Japanese?

1.  **Small, fast, repeatable** - Just like a compiler, an installation
    tool should not produce overly large installers; it should run as
    fast as possible.

2.  **Portable** - The configuration files for the installation tool
    should be portable to other machines, and ideally to other
    platforms.

3.  **Debuggable** - Debugging how an installation tool creates an
    installer and debugging what an installer did when it ran are both
    tasks to consider when deciding how easy it is to work with each
    installation tool.

4.  **Automated** - Integrating the installation tool with an automated
    build process should be possible, by using a command-line version of
    the tool, for example. This includes signing packages.

5.  **Multilingual** - An installation tool should be able to produce
    installers that are localized for different languages while running
    on a non-localized machine.

6.  **Produces installers for updates** - The installation tool should
    be able to produce installers to update products that have already
    been installed.

7.  **Supports different media** - You should be able to create an
    installer suitable for download from a web site, as well as an
    installer for releasing on CDs or DVDs. The installation tool should
    be able to put the different files onto multiple CDs or DVDs in such
    an order that the most common installation sequences never require
    the customer to reinsert a disk.

8.  **Flexible** - A good installation tool can be extended to do things
    during an install that no one imagined when the tool was designed.

9.  **Stable** -

### Installer requirements

1.  **Simple** - The main purpose of an installer is to make a product
    work correctly on a customer's machine and to do this as simply as
    possible.

2.  **Atomic** - A failed installation should remove all traces that it
    was ever there. Leftover files can make subsequent installations
    fail in obscure and hard-to-debug ways.

3.  **Provides an installation summary** - Before making any changes to
    the machine, the installer should show at least the product's name
    and version, where it will be installed, how much space it will take
    up, and any other useful configuration details.

4.  **Supports unattended installs** - System administrators installing
    your product on numerous machines will want two things: the ability
    to install it with no input, and the ability to customize the
    installer

**Windows** - Windows Installer, Installshield, InstallAnywhere,
InnoSetup, NSIS. Unix - rpm,
apt.

### Some common issues with installations from the project's point of view worth taking care are

1.  **Testing installs** - Testing installs is hard since a single
    installer can change the entire environment of the machine on which
    it was run, and can do this in ways that are hard to revert.

2.  **Testing localization** - Ideally, you want to test every localized
    installer on an appropriately localized machine.

3.  **Automating installer creation** - Automation of the installation
    tool as part of a release process, to create the installers for the
    product, is also tricky. Command-line use of installation tools
    often doesn't seem to have had the same amount of testing as the GUI
    versions of the same tools.

4.  **Installing third-party products** - Bundling other products with
    your own product in the same installer is a common requirement,
    since it makes installations simpler for customers.

5.  **Installation chains** - Identifying dependency chains is
    difficulty especially on Unix and Linux machines.

6.  **Inappropriate privileges** - Why should a user have to be root to
    install a package to her own Unix home directory? Why should your
    child need to have Windows administrator privileges just to install
    a simple game? Too many products assume they have to be installed as
    the superuser on a machine when they don't really need to be.

7.  **Corrupted installers** - Adding a Virus or Trojan horse to a
    publicly available installer is a classic approach for infecting
    other machines. Consequently, making sure the installer you released
    is what customers actually download and run is going to become even
    more important.

8.  **Uninstallers** - How does a customer know that uninstalling your
    product won't damage some other vital part of his system? Just as
    installers describe what they are going to do before they do it,
    thus giving customers a chance to cancel the installation if they're
    not comfortable about something, so uninstallers should provide
    detailed information about what they are going to change. Cryptic
    messages like “Couldn't remove all the files” are really irritating
    they could at least say which files, and why they couldn't be
    removed\!

9.  **Lost data** - No data generated by someone using your product
    should ever be deleted, even if she unwisely stored it in the same
    directory where the product was installed; all customer data is
    sacred.

10. **Multiple versions** - Make it very clear before an installer
    actually changes anything whether two versions of the same product
    can coexist on the same machine. If not, then explain how the
    customer would be able to restore the previous version if there are
    problems with the version currently being installed.

### Release Tools Checklist

1.  What's the next version number that your product will use? Who
    decides when it's time to make a change to each part of the version
    number?

2.  Where do you go to find the latest public release of your product?

3.  How large is the latest release of your product? Which part of the
    product creates most of the files that you deliver to your
    customers?

4.  What other software does your product depend on, and which versions?

5.  If your product needs license keys, where do you obtain these? How
    do customers obtain them? How long does it take for a customer to
    obtain a new license key?

6.  What kinds of changes are allowed in a patch release? In a minor
    release? In a major release?

7.  Which versions of your product will operate correctly with each
    previous version of your product? How does a customer know this?

8.  Are a customer's data and configuration choices preserved when she
    upgrades your product?

9.  Can a customer downgrade your product to an earlier version?

10. What's different between how developers run the product, how testers
    run the product, and how customers actually install and use the
    product? Have these differences been the root cause of any recent
    bugs?

11. If your product is available on the Internet, how does a customer
    prove that his downloaded package contains the same bits that you
    released?

12. What help do you have in your product for supporting it once it has
    been released? Can a customer easily display installation and
    configuration information, along with the results of any diagnostic
    tests? Can you use this as part of a phone call or in email?

13. How many releases can your group really work on at the same time?

## Developing for Easier Maintenance

1.  Remember that source code is a communication from human to human, as
    well as from human to machine.

2.  The less code there is, the less there is to maintain. Eliminate all
    commented-out code and code surrounded with the ugly \#ifdef NOTDEF
    directive. It's still in the SCM tool if you need it later. If it's
    code for logging or debugging, use a proper framework for these, not
    ad hoc commented-out code.

3.  Avoid or clearly document functions and methods that need to be
    called in a certain order or that should not be called more than
    once. Treat functions with side effects with great caution; avoid
    writing them if at all possible.

4.  Describe the current implementation before you change it. This helps
    to make the changes clear for you, and it's useful as a reminder
    later on of how it all used to work.

## Maintenance Checklist

1.  Who is responsible for deciding when tools or operating systems need
    to be upgraded or replaced?

2.  Who makes sure that new licenses are acquired for tools before the
    existing licenses expire or are exceeded?

3.  How much of your development environment can be controlled using
    your SCM tool? Can your SCM tool track changes to itself?

4.  How long does each release of your product last? How long is it
    supported by your project or company, and how long is it actually
    used by customers?

5.  Which areas of your product are the hardest ones to work on? Why?
    What could make them easier to maintain?

6.  Which parts of your product would you most like to refactor or
    rewrite? How do you know whether the time spent refactoring or
    rewriting them would actually make them easier for anyone else to
    maintain?

7.  What finite resources are used by your development environment? How
    long could you use your environment if nothing were ever discarded?

8.  Do you know who is permitted to make changes in product releases
    that are in maintenance mode?

9.  How confident are you that your unit and system tests let you know
    if your maintenance changes have broken something unexpectedly?

10. How do you decide what to test in a maintenance release of your
    product?

11. How do you know who to contact about each part of the product in
    earlier releases?

12. Do you have processes defined for removing files, SCM tags, builds,
    test definitions and results, bug tracking information, and releases
    from your project?

## Tools for Communication

Some of the most helpful tools for project communication have common
characteristics. These characteristics are:

1.  **Easy to use** - If a tool is hard to use, people will use other
    tools instead. Obvious, but often overlooked.

2.  **Context** - Who is speaking? When did they speak? Who were they
    talking to? Without this kind of context, all communication becomes
    harder to understand.

3.  **Active and passive modes** - Sometimes you want to find
    information or actively seek someone out. Other times you want the
    information to be sent to you, with no action needed on your part. A
    good environment can provide both kinds of communication.

4.  **Archives** - Communications that can be stored, printed out, and
    searched are more useful than those that are ephemeral or just a
    mass of
data.

### Some communication tools that are commonly used in software projects are

1.  **Web sites** - Web sites, Wikis (which are web sites that anyone
    can edit, with the editing being done via a web page) and weblogs
    (online diaries, also known as blogs). These are all simple to
    browse actively (though they are not always easy to follow) and they
    can sometimes be searched.

2.  **Email** - Email, mailing lists, web-based groups such as Yahoo\!
    Groups, and newsgroups. Email is the common denominator, and
    everyone on the project already uses it. It's easy to be a “lurker”
    and passively receive information. Messages are often archived, and
    following a conversation by subject or thread usually works well
    enough.

3.  **RSS feeds** - RSS feeds are places that RSS reader applications
    can regularly poll for information. Most weblogs and many news sites
    already have RSS feeds. Some automation environments can provide RSS
    feeds about the state of the build. Many web browsers also have RSS
    readers already built in.

4.  **Messaging** - Instant messaging (IM) and SMS text messages on
    mobile telephones are used by some groups for short conversations.
    Some IM conversations are archived for later reference.

5.  **Telephone** - Telephones are easy enough to use, but they actively
    interrupt other people, and voicemail messages are tedious to search
    through.

6.  **Office navigation aids** - The basics for finding people in an
    office are a list of telephone numbers that is regularly updated and
    name tags on cubicles and offices. A map of where people sit and a
    web page with photographs of people's faces are even better.

## Different Areas for the Project Web Site

1.  **Home Page**
    
    1.  Messages about server outages and other current project-related
        news. When news items become old, move them to a separate
        location; don't delete them.
    
    2.  A link to FAQs.
    
    3.  The current status of various important builds.
    
    4.  A search box for different parts of the web site.

2.  **Getting Started**
    
    1.  Names, email addresses, and locations of IT people and
        toolsmiths
    
    2.  A step-by-step guide to setting up an account on a machine and
        accessing email
    
    3.  Names of important servers, what accounts or permissions to ask
        for, and a basic overview of the local network
    
    4.  Which directories and files are backed up, and when
    
    5.  How to find a list of contacts for each project
    
    6.  A link to a map of where people are located, with their time
        zones if the project is global

3.  **Specifications**

4.  **SCM**
    
    1.  Files - Graphical browsing of the hierarchy of files managed by
        the SCM tool.
    
    2.  Change logs -
    
    3.  Branches - Change logs for the mainline and other branches, also
        arranged by project, showing the names, versions, commit
        messages, and usernames, grouped and sorted by time.
    
    4.  A list of major branches, their names, purpose, and creation
        dates.
    
    5.  SCM tool - More information about the SCM tool, including
        details about how to configure it for each project.

5.  Building
    
    1.  Current state - If automated builds are running, a web page is a
        great place to display the results, especially if there are many
        different platforms and products being built continuously.
    
    2.  “How Not to Break the Build” - A document describing the minimal
        process to follow when committing changes to the source code.
    
    3.  “How to Fix the Build” - The counterpart of “How Not to Break
        the Build,” this document describes where to find information
        about when the build broke, which files or tools changed since
        the build last succeeded, and who made those changes. This
        document should also describe how to rerun the unit tests.
    
    4.  Supported environments - A single, authoritative place for
        project members to find out which machines, operating systems,
        and tools, and their versions, are supported for the project.
    
    5.  Build tool - More information about the build tool and the
        specific details about how it is used in various projects goes
        here.
    
    6.  Historical information - Tracking the size of the product and
        how long builds take can be useful information for finding out
        what's making the product so large, or why builds take so long
        now.

6.  Testing
    
    1.  Current state - The latest results of running the system tests
        on different releases, including releases internal to the
        project and releases for customers. Test reports should include
        links to the definition of each test.
    
    2.  “How to Fix a Test” - This document describes the processes for
        confirming that a test is failing and for escalating the problem
        appropriately.
    
    3.  Test tools - A description of the tools used for testing, along
        with more information about using and modifying the tools.

7.  **Bug Tracking**
    
    1.  Bugs - Access to the bug tracking system.
    
    2.  Common reports - Links to common reports for developers,
        testers, writers, and managers.
    
    3.  Submitting a bug - Some guidelines for how to submit a bug for
        each project.
    
    4.  Bug tracking system - More information about the bug tracking
        system and how it is configured locally for your projects.

8.  **Documentation**
    
    1.  Documents - Links to both released and latest versions of the
        product documentation. The release process should automatically
        update the links to the latest versions.
    
    2.  Review process - A description of how the project usually
        reviews documents and provides feedback.
    
    3.  Building the documentation - The tools required and the process
        to create the documentation as it is distributed to reviewers
        and customers.
    
    4.  Documentation tools - More information about the documentation
        tools themselves.

9.  **Releases**
    
    1.  Releases - Instructions for finding and downloading internal and
        external releases, often using their build labels. The release
        process should automatically update this page as releases appear
        and are approved.
    
    2.  Creating a release - A step-by-step description of the process
        of creating a release.
    
    3.  Roadmap - A link to a description of the features planned for
        each future release.
    
    4.  Release tools - More information about the release tools
        themselves.

10. **Maintenance**
    
    1.  Design - A description of the architecture of the product,
        ideally including both the original design and subsequent
        modifications.
    
    2.  Implementation - Notes about the intentions of various
        developers who have worked on each part of the product. Previous
        maintainers may have kept notes about their understanding of
        each part, too. A good description of the directory hierarchy
        for the source code and the intended use of each directory.
    
    3.  Coding standards - Coding standards for the project, to be
        followed by future maintainers.
    
    4.  Deprecation - A document describing how to deprecate parts of a
        product's public API.

11. **Support**
    
    1.  Support tools - Links to the main tools used for tracking
        customer issues, such as ticket systems, knowledge bases, and
        other informative articles. Also links to tools developed by
        support for their own use when diagnosing customer problems.
    
    2.  Customer information - A document describing what information to
        gather from a customer with a problem and, more importantly, why
        the information is useful and to whom.
    
    3.  Privacy - A document about the privacy guidelines for customer
        information and data.
    
    4.  Triage - A description of the basic steps for investigating a
        problem with the product.

12. **Project Management**
    
    1.  Projects - A list of all the projects, past and present, and
        email aliases for contacting the members of that project.
    
    2.  People - The people in each project and contact information for
        them.
    
    3.  Resources - A list of the key machines used in the project.
    
    4.  Email archives - An interface for browsing and searching a
        centralized archive of the messages to email aliases for each
        project.
    
    5.  Statistics - Project statistics can include the number of lines
        of source code, the top ten committers, the complexity of the
        code, and the estimated value of the code.

13. **About the Web Site**
    
    1.  Sitemap - What information is available on the web site. This
        page should be automatically generated.
    
    2.  Changing pages - The instructions for how to modify a page.
    
    3.  Changing the web site - A description of how the web site is
        structured and how to modify the structure of the web site.
    
    4.  Usage statistics - Statistics about how the web site is used.
    
    5.  Web tools - Information about how to run a link checker for the
        web site and about other web-related tools such as HTML Tidy.
    
    6.  Contact details - An email alias for contacting the people
        responsible for running the project web site.

