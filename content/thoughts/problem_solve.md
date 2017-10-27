% How do you approach a new problem?
% Ravikiran K.S.
% January 1, 2006

## Problem vs. Exercise

Problem is a puzzle, to which you don't know how to approach it to start with.
Once you solve a problem, it becomes a repeated excercise from next time on,
whenever you attempt at solving it again. Hence, someone's problem could very
well be an exercise for someone else, depending on whether they've faced similar
issue in the past.

## Approaching a new problem

By its very nature, problem determination is about dealing with the unknown and
the unexpected. If you knew in advance everything about all the problems that
you could encounter and exactly how they manifest themselves, then you would've
take measures to prevent them and wouldn't have to investigate them. You cannot
expect that problem determination should be a perfectly predictable process,
but there are a number of common approaches that can make the process go
smoother and be more effective.

Common challenges that can make problem determination exercises difficult to
resolve:

  - Need for direction or What do I do next? - Problems can be complex
    and it is not always obvious how to approach them.

  - Need for information, or What does it mean? - Sometimes what's
    missing is simply information: you have data, but you don't know how
    to interpret it or can't understand how it relates to the problem at
    hand.

  - Miscommunication and lack of organization, or 'What was I doing?
    What were you doing' - Sometimes time is wasted or important clues
    are lost because of miscommunication, or because the investigation
    has dragged on for so long, making the collected information more
    difficult to manage.

  - Dealing with multiple unrelated problems or symptoms, or What are
    we looking for? - A particular challenge in complex situations is
    not knowing whether you are dealing with a single problem or with
    multiple independent problems that happen to occur at the same time.
    Being able to distinguish between the noise and the real problems
    can go a long way to an timely resolution.

## Characterizing the problem

Thumb rules of every news story - Who, What, When, Where, and Why.

  - What happened?

    1.  What are the main symptoms that determine the problem? How would
        the system behave if the problem is not present?

    2.  How would you recognize that the same problem happened again?
        How can you recreate it?

    3.  What information is required to analyze this problem? What are
        unknowns?

  - Where did it happen?

    1.  Precisely which machine, which application, which processes, and
        so on, the problem was observed on.

    2.  Which logs and which screens should you look at to see the
        problems?

    3.  What is network topology, system topology, software version,
        configurations, and other related factors.

  - When did it happen?

    1.  Note time stamps of when problem happened, to look in the logs.
        Calculate time zone offsets. Check if dependent systems have
        clock synchronized.

    2.  Are there any special timing circumstances? Is the problem
        repeatable? Is problem

    3.  When does it needs to be fixed? What is the priority and whether
        that is valid?

  - Why did it happen?

    1.  Why did the problem happen now, and not earlier? What has
        changed? Has it ever worked correctly in the past?

    2.  Why does the problem happen only on this system, and not on
        other similar systems? What is different?

    3.  Why should this problem be fixed? What will happen if we don't
        fix it?

  - Who is appropriate?

    1.  Which technology area does it come into? Who is better person to
        analyze this problem?

\<note warning\>Be cautious about

  - using vague terms like hang, crash, and fail, which are often
    generalizations, inaccurate, and distract attention away from important
    symptoms.

  - in real-world situations, there can be several independent problems
    rather than just one. You need to recognize them and prioritize
    them.

  - tangential problems that are consistent, well-known, and incidental.
    While source of problem lies elsewhere, these are mere consequences.

\</note\>

## Not every problem is difficult

Before jumping into complex analysis tasks:

  - Phase 1 - perform a broad scan of the entire system to find any
    major error messages or dump files that have been generated in the
    recent past. Search those errors, dump traces, and initial symptoms
    across one or more knowledge bases of known problems.

  - Phase 2 - do everything else

## Balancing resolution and relief

  - Resolution: Finding the root cause and ensuring that it will not
    happen again.

  - Relief: Enabling your users to resume productive work.

Unfortunately, these two goals are not always simultaneously attainable.
A simple reboot is often enough to bring the system back, but it can
destroy information needed to debug the problem. In some situations,
like system testing, driving the problem to full resolution is required
and relief is not an issue. Whereas in business-critical production
systems, immediate relief is paramount. Once the system is back up,
investigation can begin with whatever data is available/salvaged.

\<note tip\>Person/team working on project must be clear about the
business priorities and tradeoffs involved. While finding a frequently
recurring problem might raise the importance of full resolution;
however, finding a problem with too limited scope may tilt the scales
toward simple relief.\</note\>

Hence, before launching an in-depth investigation, you need to consider
the business context in which the problems and your investigation occur.

## Fundamental approaches: analysis and isolation

While each problem and hence its investigation could be different, all
troubleshooting exercises boil down to this: you watch a system that exhibits
an abnormal or undesirable behavior, make observations and perform a sequence
of steps to obtain more information to understand the causing factors, then
devise and test a solution

Two distinct but complementary approaches:

  - **analysis approach** - you pick one or more specific symptoms or anomalies
    observed in system, and then drill down to gain more detailed information
    about these items and their causes. These, in turn, might lead to new, more
    specific symptoms or anomalies, which you further analyze until you reach a
    point where they are so simple or fundamental that it is clear what caused
    them.

Techniques involved are reading source code/knowledge base, finding more
specific symptoms, and research further.

  - **isolation approach** - rather than focusing on one particular
    symptom and analyzing it in greater detail, you look at the context
    in which each symptom occurs within the overall system and its
    relation to other symptoms, and then attempt to simplify and
    eliminate factors until you are left with a set of factors so small
    and simple that, it is clear what caused the problem.

Techniques involved are performing specific experiments to observe
system behavior for different inputs, narrow down the problem to few
specific factors, reduce number of variables and inter-module
dependencies in an attempt to see which parts are exactly contributing
to the problem.

Although a clear distinction is made here between the analysis and
isolation approaches, in practice they are not mutually exclusive. In
the course of a complex investigation, you will often take steps from
both approaches, either in succession or in parallel, to pursue multiple
avenues of investigation at the same time.

## Organizing the investigation

Four types of information are generally required:

### Executive summary

Summarizes the status of highest priority issues – what has been done,
top action items, owners, approx. dates being chased, etc. This enables
all stakeholders (not just executives) to understand the current status,
helps focus the team, explains progress and next steps, and highlights
any important discoveries, dependencies, and constraints.

### Table of problems, symptoms, and actions

Often problems are interlinked. While investigating one problem, several
additional problems might surface. Record-keeping of these additional
problems pays-off, especially when things are much more complicated than
originally believed. Regardless of the exact format, this table should
contain:

  - Problems: One entry for each problem that you are attempting to
    resolve (OR each thing that needs to be changed).

  - Symptoms: External manifestations of the underlying problem and
    anomalies that might provide a clue about the underlying problem.
    For ex. an error message observed in a log, or a particular pattern
    noticed in a system dump; the error condition or crash itself. They
    can be used as guide to verify the fix. Sometimes after fixing a
    problem, old symptoms go away, but new symptoms appear during an
    investigation.

  - Actions: Tasks to be performed that may or may not be directly
    related to a particular symptom or problem, e.g. upgrading the
    software or preparing a new test environment.

  - Fixes: Alternatives to be tried to achieve a resolution or
    workaround.

  - Theories: It is useful to track ideas about why the problem is
    occurring or how it might be fixed. Noting which symptoms the
    theories are derived from can help rule out theories or draft new
    ones for the investigation.

These tables should be constantly reviewed and updated to reflect
current status of investigation. When there are multiple problems, it is
also important to group symptoms with their corresponding problem, and
to review these relationships frequently. Finally, each entry should be
prioritized.

``` code
  +------+----------+-------+-------------+-------------+---------------+
  |Prior | Problem  | #PR   | Symptoms    |  Status     | Actions       |
  +------+----------|-------|-------------|-------------|---------------|
  | #1   | App Crash|  123  |On Peak CPU  |Log Collected|Analyze logs   |
  +------+----------+-------+-------------+-------------+---------------+
```

IMP: Do not attempt to use the table as a historical record of the
progress and activities in the investigation. This table will typically
be complex enough just keeping track of the current state of things. The
timeline (covered next) will contain the historical information needed
for most investigations.

### Timeline of events

In any investigation that lasts more than a few days, or that involves
more than a few individuals, there will invariably be questions about
the results of some earlier experiment or trace, where some file was
saved, and so on. Keep a log book in which you record a timeline of all
major events that occurred during the investigation. A timeline will
typically contain:

  - One entry for each occurrence of any problem being investigated.

  - One entry for each significant change made to the system (such as
    software upgrades, reinstalled applications, and so on).

  - One entry for each major diagnostic step taken (such as a test to
    reproduce the problem or experiment with a solution, a trace, and so
    on).

  - A precise date and time stamp.

  - A note of the systems (machines, servers, and so on) that were
    involved.

  - A note of where any diagnostic artifacts (logs, traces, dumps, and
    so on) were
saved.

<!-- end list -->

``` code
  +------+----------+-------+-------------+-------------+---------------+
  |Event |Date/Time |      Details        |  Results    | Location      |
  +------+----------+---------------------+-------------|---------------|
  |Test#1|02/Sep 1PM|Analyze systest setup|Log Collected|/path/to/logs  |
  +------+----------+---------------------+-------------+---------------+
  |Test#2|4/Oct 5:00|Reproduce problem    |Register trac|               |
  +------+----------+---------------------+-------------+---------------+
```

### Inventory of the diagnostic artifacts

Over the course of an investigation, one ends up collecting a large
number of diagnostic artifacts and files, some could be result of
multiple spontaneous occurrences of the problem over time, and others
could be the result of specific experiments conducted to try to solve
the problem or to gather additional information. It is very important to
manage various diagnostic artifacts collected during these experiments
so that they can be consulted when additional info is required to pursue
a line of investigation. For each artifact, you should be able to tell:

  - Which event in the timeline does it correspond with?

  - Which specific machine or process (among all of the machines and
    processes involved in a given event) did the artifact come from?

  - What system or process configuration was in effect at the time the
    artifact was generated?

  - What options were used specifically to generate that artifact, if
    appropriate (for example, which trace settings)?

  - If the artifact is a log or trace file that could contain entries
    covering a long period of time, exactly at which time stamp(s) did
    something noteworthy happen in the overall system, that you might
    wish to correlate with entries in this log or trace file?

Organize all artifacts into subdirectories, with one subdirectory
corresponding to each major event from the timeline, and to give each
artifact within a directory a meaningful file name that reflects its
type and source (machine, process, and so on). When necessary, also
create a small README file associated with each artifact or directory
that provides additional details about the circumstances when that
artifact was generated (for example, configuration, options, detailed
timestamps, and so on).

## Avoiding tunnel vision

Failure to see the whole picture and not methodically evaluating all
potential solutions can result in prolonged relief and recovery times.
Here are some ideas to help you avoid this condition:

  - Utilize checkpoints: Checkpoints are time-to-time team syncups
    required during the investigation. Create checkpoints for all team
    members to share all relevant findings since the last checkpoint.
    With this all team members will be in sync.

  - Work in parallel: Different team members can work parallely on
    alternative theories to the root cause, it can benefit the
    investigation by expediting it.

  - Regularly (re)ask the big picture questions: Keep asking 'what is
    the problem?' and 'are we solving the right problem?'. One classic
    example of tunnel vision is thinking that you have reproduced a
    problem in a test environment only to discover later that it was
    actually a slight permutation of a production problem. Asking big
    picture questions can help contextualize problems and avoid chasing
    those that are of lesser priority.

## Peeling the onion: The nature of complex problems

Often problems are relatively straightforward, with a single/small
cluster of symptom(s) that lead more or less directly to an
understanding of a single problem which, when fixed, resolves the entire
situation. Sometimes, with more complex situations, a series of related
symptoms or problems must be peeled back one-by-one to get to the root
cause. The phenomenon of peeling the onion might manifest itself in a
few different variations:

  - Multiple problems or symptoms can be linked by a cause-and-effect
    relationship. Ex, web server is hung because it can't create more
    connections, that is because of application server busy handling
    existing connections, that is because database server is slow, that
    is because network connection to db is slow, that is because an
    file-system replication is ongoing over that link.

  - Encountering one problem might cause the system to enter a
    particular error recovery path, and during the execution of that
    path another problem might manifest itself.

  - In other cases, one problem might simply mask another; the first
    problem does not let the system proceed past a certain point. But
    once that first problem is resolved, you get further into the
    processing and encounter a second independent problem.

In all these cases, there is no choice but to address one problem at a
time, in the order that they are encountered, while observing the
operation of the system. Each problem could itself require a lengthy
sequence of investigative steps before it is understood. Once one
problem is understood, you can proceed to the next problem in the
sequence, and so on. This method can be very frustrating, especially to
those unfamiliar with the troubleshooting process, but effective
communication can help keep morale high and build trust in the process
by showing concrete progress and minimizing confusion.

\<note important\>Maintaining and publishing a clear executive summary
helps set the context of the overall situation, helps highlight each
specific problem when it has been resolved so that progress is evident,
and helps identify new (major) problems as they are discovered. The
table of problems, symptoms and actions helps to keep track of the
various layers and clarify the relationship between similar problems and
symptoms.\</note\>

Problem determination is about dealing with the unknown and unexpected,
and so it will probably never be an exact science, but it's also not
rocket science. By following more organized and systematic way to
problem determination, in end it can be more effective and rewarding.

References:
[IBM](http://www.ibm.com/developerworks/websphere/techjournal/0806_supauth/0806_supauth.html)

