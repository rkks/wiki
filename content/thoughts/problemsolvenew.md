% How do you approach a new problem?
% Ravikiran K.S.
% January 1, 2006


By its very nature, problem determination is about dealing with the
unknown and the unexpected. If you knew in advance everything about all
the problems that you could encounter and exactly how they manifest
themselves, then you would take measures to prevent them and wouldn’t
have to investigate them. You cannot expect that problem determination
should be a perfectly predictable process, but there are a number of
common approaches that can make the process go smoother and be more
effective.

Common challenges that can make problem determination exercises
difficult to resolve:

  - Need for direction or What do I do next? - Problems can be complex
    and it is not always obvious how to approach them.

  - Need for information, or What does it mean? - Sometimes what’s
    missing is simply information: you have data, but you don’t know how
    to interpret it or can’t understand how it relates to the problem at
    hand.

  - Miscommunication and lack of organization, or “What was I doing?
    What were you doing” - Sometimes time is wasted or important clues
    are lost because of miscommunication, or because the investigation
    has dragged on for so long, making the collected information more
    difficult to manage.

  - Dealing with multiple unrelated problems or symptoms, or “What are
    we looking for?” - A particular challenge in complex situations is
    not knowing whether you are dealing with a single problem or with
    multiple independent problems that happen to occur at the same time.
    Being able to distinguish between the “noise” and the real problems
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

  - using vague terms like “hang,” “crash,” and “fail,” which are often
    generalizations, inaccurate, and distract attention away from
    important symptoms.

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
the business context in which the problems and your investigation –
occur.

## Fundamental approaches: analysis and isolation

While each problem and hence its investigation could be different, all
troubleshooting exercises boil down to this: “you watch a system that
exhibits an abnormal or undesirable behavior, make observations and
perform a sequence of steps to obtain more information to understand the
causing factors, then devise and test a solution”

Two distinct but complementary approaches:

  - **analysis approach** - you pick one or more specific symptoms or
    anomalies observed in the system, and then drill down to gain more
    detailed information about these items and their causes. These, in
    turn, might lead to new, more specific symptoms or anomalies, which
    you further analyze until you reach a point where they are so simple
    or fundamental that it is clear what caused them.

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

  - Regularly (re)ask the “big picture” questions: Keep asking “what is
    the problem?” and “are we solving the right problem?” One classic
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
symptoms or problems must be “peeled back” one-by-one to get to the root
cause. The phenomenon of “peeling the onion” might manifest itself in a
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
and so it will probably never be an exact science – but it’s also not
rocket science. By following more organized and systematic way to
problem determination, in end it can be more effective and rewarding.

Derived from
[here](http://www.ibm.com/developerworks/websphere/techjournal/0806_supauth/0806_supauth.html "http://www.ibm.com/developerworks/websphere/techjournal/0806_supauth/0806_supauth.html").

# 12 ways to prepare for effective troubleshooting

Preparing the environment so that troubleshooting can be performed more
quickly and effectively if and when problems eventually do occur is key
to quick diagnosis.

## 1\. Create and maintain a system architecture diagram

Create an architecture diagram that shows all major components of the
overall system, how they communicate, what is exchanged, and main/major
flows for requests, data, info, being processed through the system.
Diagram helps you to:

  - Identify various points in system where to find information or clues
    about the cause of a problem.

  - Clearly communicate (even complex) problems to various parties
    involved in troubleshooting (both inside & outside org).

  - Answer and verify a favorite question of all troubleshooters: What
    has changed
recently?

## 2\. Create and track an inventory of all problem determination artifacts

Make an inventory of all the important problem determination artifacts
(log files, coredumps, conf files, etc.) in the system:

  - Note what each file is for, its name, location, purpose, typical
    contents, and its typical size.

  - Use architecture diagram to review all useful problem determination
    artifacts.

  - Simply “knowing” that the artifact exists doesn't suffice. Though
    everything may be set-up & documented perfectly in theory, check
    live system periodically to verify that all real-life artifacts are
    getting generated as expected. And that there are enough resources
    to hold artifacts like enough disk space, etc.

  - Make sure that articfacts are not purged/overwritten too quickly,
    and are not accidentally deleted/overwritten if system is restarted
    after an incident.

While having artifacts is useful. Having quality artifacts without much
noise and useless/distracting info can ease the process
considerably.

## 3\. Pay special attention to dumps and other artifacts that are only generated when a problem occurs

There are generally a variety of configuration options that control how
and when these artifacts are generated:

  - For any file that can be generated automatically when a problem is
    detected: carefully consider the potential benefit – and impact – of
    having this file produced automatically, and set up the
    configuration accordingly. If the potential benefit is high and the
    impact is low, make sure that this feature is enabled.

  - Make sure that these configurations work as expected by testing
    them.

## 4\. Review and optimize the level of diagnostics during normal operation

Effective serviceability is always a trade-off:

  - On one hand, to maximize the chances of being able to determine the
    cause of a problem upon its first occurrence, one wants to gather
    maximum amount of diagnostic data from system at all times.

  - But gathering very detailed diagnostics can cause a substantial
    performance overhead. Therefore, for performance reasons, one might
    be tempted to disable all diagnostics during normal operation of
    system.

Its important to find the right balance between these two conflicting
goals. By default, most products and environments tend to err on the
conservative side with a relatively small set of diagnostics enabled at
all times. However, it is quite worthwhile to examine the specific
constraints of particular production environment, the likelihood of
specific problems, and specific performance requirements, and then
enable as many additional diagnostics as you can afford during normal
steady-state operation.

## 5\. Watch low-level operating system and network metrics

Such metrics are often overlooked, or considered only late in the course
of a complex problem investigation, yet many of them are relatively easy
and cheap to capture. In some particularly difficult cases, especially
network-related problems, this information often plays a key role in
tracking down the source. System level metrics are like:

  - Overall CPU & memory usage for the entire machine, CPU & memory
    usage of individual processes, Paging and disk I/O activity

  - Rate of network traffic between various components,
    reduction/total-loss of network connectivity between various
    components

While it's not practical to monitor every single system-level metric on
a permanent basis, but where possible, pick a lightweight set of
system-level metrics to monitor regularly, so that you have data both
before a problem occurs, and when a problem does occur. It pays-off to
write a few simple command scripts to run periodically and collect the
most useful
statistics.

## 6\. Be prepared to actively generate additional diagnostics when a problem occurs

In addition to normal artifacts present at time of an incident, the
troubleshooting plan should consider any additional explicit actions
that should be performed to obtain additional information as soon as an
incident is detected – before the data disappears or the system is
restarted.

  - Actively trigger various system dumps, if they have not been
    generated automatically - heap dump, system dump, etc.

  - Take a snapshot of key Operating system metrics, such as process
    states, sizes, CPU usage, and so on.

  - Dynamically enable a specific trace, and collect that trace for a
    given interval while the system is in the current unhealthy state.

  - Actively test or “ping” various aspects of the system to see how
    their behavior has changed compared to normal conditions, to try to
    isolate the source of the problem in a multi-component system. – How
    can you independently verify if some sub-module is working fine?

Clearly, there are potentially infinite variety of such actions, and
it's not possible to perform them all. A careful review of the
application and the system architecture can help to anticipate the most
likely failure modes, and decide which actions are very likely to yield
most useful information on case-by-case basis.

## 7\. Define a diagnostic collection plan -- and practice it

When a problem happens, too often there is confusion, along with great
pressure to restore the system to normal operation, causing mistakes
that lead to unnecessary delays or general difficulties in
troubleshooting. Having a plan of action, ensuring that everyone is
aware of the plan of action – and rehearsing the execution of the plan
ahead of time – are critical. While simplest diagnostic collection plan
can be a plain, written documentation that lists all the detailed manual
steps that must be taken. To be more effective, try to automate as much
as this plan as possible, by providing one or more command scripts that
can be invoked to perform a complex set of actions, or by using more
sophisticated system management tools

## 8\. Establish baselines

“What's different now compared to yesterday when the problem was not
occurring?” To answer this question, you must actively collect and
maintain a baseline – an extensive info about state of system at a time
when that system is operating normally. It includes:

  - Copies of the various log files, trace files, etc., over a full day
    period of time in normal operation of the system.

  - Copies of few heap dumps, system dumps, or other types of artifacts
    that are normally generated “on demand.”

IMP: This activity can be combined with the earlier recommendation to
test the generation of these artifacts on a healthy system before a
problem occurs.

  - Info about the normal transaction rates in the system, response
    times, and so on.

  - Various operating system level stats on a healthy system, such as
    CPU usage for all processes, memory usage, network traffic, and so
    on.

  - Copies of other artifacts of normal expected results from the
    special diagnostic collection actions, recommended earlier, for each
    anticipated type of problem.

Systems evolve, the load changes, and so what is representative of the
“normal” state will likely not remain constant over time. So, remember
to refresh this baseline info periodically. If there is a pattern of
changes in load throughout the day, keep different baselines for
different times.

## 9\. Periodically purge, archive, or clean-up old logs and dumps

Quantity is not always a good thing. This can actually hamper the
troubleshooting process. Having too much logs will slow down artifact
collection scripts, and thereby the entire troubleshooting process. In
extreme cases, the system could run out of disk space and other system
resources due to sheer volume of collected artifacts. So, the main
objective should be to gather a maximum amount of diagnostic information
just before and around the time of a problem. And keep or archive only
sufficient amount of historical diagnostic information to serve as a
baseline for comparison purposes.

## 10\. Eliminate spurious errors and other "noise" in the logs

If system generates a large volume of error messages, even during normal
operation; then such benign or common errors are clearly not
significant, but they make it more difficult to spot unusual errors
among all the noise. To simplify future troubleshooting, either
eliminate all such common errors, Or find a way to filter them out.

## 11\. Keep a change log

While using a baseline is certainly one way of identifying the recent
changes, keeping a rigorous log of all changes that have been applied to
the system over time can simplify the process of identifying the
difference. When a problem occurs, you can look back through the log for
any recent changes that might have contributed to the problem. You can
also map these changes to the various baselines that have been collected
in the past to ascertain how to interpret differences in these
baselines. Your log should at least track all upgrades and software
fixes applied in every software component in the system, including both
infrastructure products and application code. It should also track every
configuration change in any component. Ideally, it should also track any
known changes in the pattern of usage of the system; for example,
expected increases in load, a different mix of operations invoked by
users, and so on. IMP: Be aware that the concept of change control, and
keeping a change log, is generally broader than the troubleshooting
arena. It is also considered one of the key best practices for managing
complex systems to prevent problems, as opposed to troubleshooting them.

## 12\. Setup ongoing system health monitoring

In a surprising number of real-life cases, the overall health or
performance of the system might degrade slowly over a long period before
it finally leads to a serious problem. Therefore, having a good policy
for continuous monitoring of the overall health of the system is an
important part of an overall troubleshooting plan. Rather than wait for
a problem to be reported externally, system could be scanned for
potential problems. Again, simple command scripts or sophisticated
system management tools, if available, can be used to facilitate this
monitoring. Things that might be monitored are:

  - Significant errors in the logs emitted by the various components.

  - Metrics produced by each component should remain within acceptable
    norms (Ex, CPU and memory stats, transaction rate, and so on).

  - Spontaneous appearance of special artifacts that only get generated
    when a problem occurs, such as system dumps, heap dumps, and so on.

  - Periodically send a “ping” through various system components or the
    application, verifying that it continues to respond as expected.

Derived from
[here](http://www.ibm.com/developerworks/websphere/techjournal/0708_supauth/0708_supauth.html "http://www.ibm.com/developerworks/websphere/techjournal/0708_supauth/0708_supauth.html").

