% Working on Large Codebase
% Ravikiran K.S.
% January 1, 2006

## Conquering Large Legacy Software
It's not the big code-base, which is a problem. Rather, it's multiple things
associated to it that cause the problem like:
* Code itself is so big, it takes hours to fetch it, index it, modify it, and
build it.
* Loading it on machine takes long time, setting-up pre-conditions to test it
is tedious.
* Documentation isn't easily accessible, not complete, sprayed in multiple
places.
* Tests aren't readily accessible, they are not enough to verify if anything
is broken.
* After making changes, it's not easy to commit. Processes, approvers,
timezones, etc.
* Each team has it's own goals, timelines, development processes

Idea is not to tackle entire source code, or make it good. Idea is to remove
the donkey work. So that engineers can focus on useful work at hand, than
getting frustrated by other things.

Re-write an entire soure code is bad idea. What's the assurance that it
wouldn't end up same as current code?    - Joel Spolskey

We often over-estimate the time it takes to refactor code, and under-estimate
what it takes to develop new code.

An approach to tackle unknown source code is:
1. Take a look at directory structure, file organization.
2. Go through naming, calling, logging, build conventions.
3. Read all documentation that exists, no matter how bad.
4. Run code as-is, collect logs, compare them to code flow.
5. Step through code using debugger, inspect data & flows.
6. Keep test-suits/test-cases handy to verify if you break.
7. Make small changes to code, build+run, see if it works.
8. Reduce code clutter, refactor, indent, make it readable.
    - add comments, no magic numbers, DRY, no dead code
9. Find new ways to test it, automate builds/tests/reports.
10. Understand patterns used, idioms in code, and language.

Step through code in debug mode to see how it works Pair up with someone
more familiar with the code base than you, taking turns to be the person
coding and the person watching/discussing. Rotate partners amongst team
members so knowledge gets spread around. Write unit tests. Start with an
assertion of how you think code will work. If it turns out as you
expected, you've probably understood the code. If not, you've got a
puzzle to solve and or an enquiry to make. (Thanks Donal, this is a
great answer) Go through existing unit tests for functional code, in a
similar fashion to above Read UML, Doxygen generated class diagrams and
other documentation to get a broad feel of the code. Make small edits or
bug fixes, then gradually build up Keep notes, and don't jump in and
start developing; it's more valuable to spend time understanding than to
generate messy or inappropriate code.

  - Check the unit tests, they often contain some simplified example of
    how code is used.

  - Check the database schema or something equivalent to it.

  - Get a local version of the system running as early as possible. This
    way you can test the impact of small changes on the code base.

  - At the beginning ask the code owners as many insightful questions
    about the code as you can.

  - Start adding functionality or do some bug fixes.

Identify use cases of your product - I have found that it's easy to look
get a high-level feel of a large codebase if you know what use cases
your product serves. If it's a UI application, sit with the user of that
system and go through 4-5 use cases of the system. Then you can use do a
code walkthrough by putting breakpoints and see the flow.

Identify the design patterns used in the code - Often, the problems
solved by your product are well studied problems with well suited design
patterns backing their solution. Figure them out. The design pattern
might specify some standard interfaces/methods (e.g. execute() method in
Command pattern). Seeing a lot of code using the pattern without knowing
about the pattern itself can get frustrating.

Interacting with code owners - Sit with the main developers of the
product and spend some time with them asking good questions. Don't just
go and eat someone's head. Nobody likes to spoon-feed in fast-paced
engineering organization. Read through the code and try to figure out
few things on your own. There might be a common piece of code used by
many modules which you are not able to understand. You can go and ask
the developer in somewhat this fashion, “I have been reading the code
and understood many parts of this module but I am not able to figure out
what this particular piece of code does”. Most probably, the dev person
should help you with that.

Check out the bugs history of the product - This point is somewhat
related to first point. Bugs help you understand what use cases bug was
filed for. If the bug also has code file changes present against it, it
will help you identify patterns of what kind of bugs have been fixed
with what kind of code file changes. If a code file is present for lot
of bugs, it's mostly the case that the file contains critical code
piece. Read well through that code file.

Think how would you have done it - if you have figured out what is the
exact problem/use-case a code module is solving, you can hold
code-reading for sometime and think in your head how would you have
solved the problem. Then you can go ahead and read the code. What would
happen is now when you are reading the code, you are not just reading
but looking for classes/methods which does what you already have in
mind. Instead of code reading, the process becomes code discovery which
is actually exciting. I have personally found this technique to be very
useful.

Read Code Comments/Documentation - Good programmers know that a well
documented code will serve them as well as others later to figure out
what some piece of code is responsible for. Reading through code
comments will help you understand code/logic/approach better.

\* … but, Don't worry too much about the class-level design except in
the area you're working on. It's way too big to ever get your head
around more than just your part. And it changes so fast that it would be
a waste of time to try. Instead just try to keep up to date with the
big-picture structure. This changes much less rapidly.

\* Ask someone familiar with the code for examples of common patterns
used in the system. In Chrome, I'd suggest chrome/browser/bookmarks and
chrome/browser/history as examples. Both these systems are split into a
frontend and a backend which live on separate threads, with a message
passing interface between them. This turns out to be a common pattern in
Chrome, motivated by a the desires to not do any IO on the UI thread and
to avoid explicit locks.

\* Exploit module boundaries. It's nice to be able to eliminate code
that \*cannot\* be the problem. For example, if you're debugging a
problem in the browser process, nothing \[2\] in chrome/renderer can be
the culprit. In Chrome's case, modularization is done using the DEPS
\[3\] system. Spend some time reading the DEPS files for the areas
you're working on (and cross-referencing with the big picture design
docs above), to deeply understand what code is involved.

Some good answers, I'll try to add some of my own thoughts:

Tools:

First make sure you are setup with a good tool to browse code - I can't
emphasize this enough. This is your critical path when browsing code,
and its up to you to optimize the crap out of it as you will be reading
code more than writing it.

You will be jumping around code a lot, and if you are sitting around
grepping which takes O(files in the code) vs. a quick lookup in an
indexed data structure for browsing code in an appropriate tool, it can
make a huge difference to your personal productivity.

I'm somewhat old school and personally tend pass on IDEs over using
cscope and ctags, they have mods for just about every language. Cscope /
ctags let you jump around large codebases very, very quickly, as well as
really quickly query for global codebase wide assignments,
callers/calles, and references.

On reading code

Generally speaking, you want to take a divide and conquer approach, with
the key part is figuring out what are the right planes to split the
codebase across, and study each one individually.

Always strive to learn exponentially Done right, at each level you are
building upon the sum total of all knowledge that you have built reading
the code you have already learnt, and in doing so you are building up
your knowledge of code exponentially rather than linearly.

Identify the critical paths of the codebase. Generally speaking, the
critical codepaths of the product are likely to be the most important.
This lets you assimilate the slice with the highest density of valuable
information about a codebase that you can use to build up a knowledge
base about the supporting codepaths.

On a related note, looking at profiles of code is helpful here - a
profile will at a glance get you really good information about
relationships between different parts of the code and ties in well with
getting a solid feel for the critical path.

Try to also get an understanding of the performance invariants of the
code - what is the code complexity of various subsections of the
critical path, what runs in constant time, what runs in linear time with
respect to the size of certain structures, and so on.

Understand the codepaths that deal with user interaction The codepaths
that have user interaction are likely to have a lot of breadth, and this
will let you very quickly assimilate information that will be highly
relevant to understanding how various user inputs affect the code flow.

This will also be highly useful to understanding in depth some of the
features of the product you are working on as well as dealing with
understanding bug reports.

Read the unit tests and asserts I find reading unit tests ( assuming
they are respected and executed often as they should be ) and asserts to
be a great source of figuring out the thought process of what people
were thinking of when the wrote the code initially.

Have a healthy disrespect for documentation and similar non formal
descriptions I have a slight disdain for documentation as often it can
convey intent though not actual current behavior when it becomes
obsolete. The code is the authoritative documentation, and the beauty of
programs is you can figure out exact behavior by reading code and
applying yourself.

Always check yourself before asking someone else a question if you are
better served by reading the code thoroughly than bothering someone else
to do your job for you.

The idea here is that you treat asking someone else about code like a
page fault - assuming you have information cached in your head reading
the code thoroughly, you can be much faster by internalizing the code
than asking someone else. This can be expensive when you are new to the
code and are taking lots of page faults, but the goal here is unless you
actually spend this time diving into the code, you will always be the
person searching for answers instead of providing them.

( You also get lots of kudos from other people if they are in the
situation of asking how something works and you consistently solid,
helpful answers to their questions about their code )

Read historical patches and commit messages Look around and identify the
best engineers around you working on the codebase, and start reading
their historical commits and how the code as evolved over time. This
works particularly well for the point I mentioned above about
understanding the critical path.

In particular, really old versions of the code can give you a picture of
what the code looked like when it was younger and simpler, and how it
has grown to be as complex as it is today.

Observe runtime behavior of the code Often the debug / user facing log
files and statistics gathering code are good landmarks of the codebase.
Like unit tests, they are often better documentation than comments.

Likewise, stepping through the code with a debugger will help you
understand a lot about how the run time state of various variables looks
like.

Pay attention to the build output The build is also part and part of the
product. Don't treat it like a separate entity - learning how the build
works and in particular reading Makefiles will give you a good idea of
some code dependencies. Bonus points if you can actually look at the
build and identify build bugs with dependencies

Debugging Large Codebases

There are a few tricks I use, though I don't know that they are best
practices, per se:

  - Index the code your responsible for. Having a pre-built search index
    in any decent code searching tool is critical to locating code you
    haven't dealt with before, and without it, the other tricks become
    more difficult.

  - Use unique strings as a starting point. I am often debugging GUI
    components with no idea where the code behind them lives, let alone
    any of the function names relevant to a screenshot I'm presented.
    Using my index, I can usually find the strings in a resource file,
    look inside, find the variable for that string, then find where it
    is used in source. This is a great way to get into the neighborhood
    of where your relevant breakpoints will be.

  - Document as you go. As I'm breaking down a problem, I'll keep notes
    of which functions I've looked at, and how they tie back to the
    problem I'm trying to solve. This prevents me from going in circles
    and makes it easy to explain the problem to others. For complex
    issues, I like a tool like Visio to map things out. This will help
    you see the 'big picture' of how different components come together.

  - Read the comments. While they aren't always up-to-date or correct,
    if someone spent the time to comment the purpose of a function, read
    it. It will help you understand the original intent of the code,
    even if the developer didn't quite hit their mark. This may even
    make a bug obvious (the comment said this code never returns 0, but
    I just got one…what did the developer overlook?) To a lesser extent,
    this also applies to any available architecture docs.

  - Keep an internal wiki. When you've fully broken down and figured out
    a component, its good to keep this somewhere where you and your team
    can reference it later. This can be anything from diagrams to useful
    breakpoints.

  - Use a real debugger. Printfs are a poor substitute for a full
    powered debugger with the ability to set breakpoints, examine
    memory, collect call stacks, single step, and look at registers.

