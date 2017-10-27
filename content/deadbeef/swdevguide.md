% Development Practices
% Ravikiran K.S.
% January 1, 2006

Quick note of items that come to mind for handling software development artifacts.

General Guidelines:
    * Separate location for 3rd party code
    * Proper Copyright notice, Creation, Modification dates
    * Change history saved in version control
    * Do one thing - properly. Applies to functions, files,...
    * All work is in human readable form. Easy to extract
    * All Build, Test, and Deploy/sync work is automated
    * Everything is testable except for test code itself
    * Easy to trace, easy to debug, easy to change
    * Inline documentation and automated docs extraction

Right tools for task:
* Code
    * 3 Scripts (bash, expect, perl) and 2 Languages (C and Java).
        - Library: Common routines for accessing resources and performing common actions
        - Utils: Automation utils, handy utils.
* Docs - Doxygen
* Version control - Git (Source), SVN (Binary)
* Static Analysis
* Automated Testing
* Issue Tracking
* Customer Trials

Automated Systems:

  - Revision Control System - Git

  - Continuous Integration - RCS update, Nightly/Hourly build, Sanity
    test, Coding standards checks, Tagging

  - Bug Tracking System - What features are being built, tested,
    approved, troublesome? What outstanding issues are there? How
    critical?

  - Patching System - How new changes are propagated to nightly tester
    hardware and/or software? You don't want to uninstall-install each
    time.

  - Release Qualification System - Pulling out test branch, generating
    builds for SysTest, tracking changes to it, stabilizing it.

Specification Guidelines:

  - Clarity than Intelligence - Keep it concise, precise, and
    comprehensive. No ambiguous, unquantified, unqualified requirements.

  - Keep it rigorous - It should address almost all aspects of the
    product. If something isn't addressed, it wouldn't be done.

  - Document corner cases - Identify technology corner cases, system
    issues, dependencies, bottlenecks early on.

  - Have enough eye balls - Involve all stake holders to review it. More
    the review, better the clarity.

  - Say what not how - A spec should only address WHAT needs to be done
    and possibly WHY, but certainly not HOW. Give developers freedom.

  - Outcomes should be tangible - Specify the outcome that is
    verifiable, document-able, testable, and that can be mapped to
    requirements.

User Interface Guidelines:

  - Set user's expectations right - Make it clear of what he can expect
    from product.

  - Less is more - More options you provide to user, more confused he
    will be. Keep interface slim.

  - Consistency is the key - Consistent user interface and organized
    information is user friendly and clean.

  - Keep noise to minimum - Only throw messages, prompts and information
    that user must know.

  - Avoid acronyms - WTF could mean 'What The Fun' to you, but not
    necessarily same to your user.

  - Feedback is friend - Keep feedback providing as easy as possible for
    user.

Product Guidelines:

  - System works if everything works - Test every possible combination
    of usage that is expected to work. Be explicit on what doesn't work.

  - Initial expression has great impression - Product should work when
    run first time, keep configurables to minimum. Keep it intelligent.

Coding Guidelines:

  - Write readable code - Consistency, Readability, Well formatted code
    is not an option, but a necessity. Coding conventions are for
    everyone.

  - Input sanitization is must - Put rigorous checks on all input
    interfaces. They are main entry points for exploits.

  - Data structures are key - Better data organization simplifies code
    logic. Use tried and tested structures.

  - Variables - Require careful scoping, ease of access, synchronized,
    and friendly naming conventions.

  - Global variables - Should be minimum, all in one place, and
    modularized (to avoid synchronization bottlenecks).

  - Locks - Must be handled automatically by pre-defined functions and
    trackers. Keep reference counting.

  - Functions - Keep them thread-safe, reentrant, input sanitized,
    output validated, well defined, traceable, and with user friendly
    naming.

  - Comments - At a ratio of 30% comment to 70% code. Not a story, not a
    dictation of program, only 'logic, assumptions, constraints'.

  - Logging - Must for all softwares. Should be enough to trace any
    basic faults in system.

  - Tracing - Must for run-time analysis of software. Trace points
    should include key data structure values at trace junctions.

  - Memory accounting - Necessary to track number of allocations, frees,
    used memory, free memory, control requests, responses, etc.

  - CPU accounting - Necessary on per thread basis to avoid CPU
    thrashing and to allow time critical things to be run smoothly.

Debugging Guidelines:

  - Check for memory errors - Off-by-one errors, Stack overflow errors,

Interview Guidelines:

  - Do new candidates write code during their interview?

  - Do you do hallway usability testing?

  - Do programmers have quiet working conditions?

  - Do you use the best tools money can buy?

  - Do you have
testers?

<!-- end list -->

``` code
  Help strengthen your team by surveying your computing neighborhood. Choose two or three broken windows and discuss with your colleagues what the problems are and what could be done to fix them.
```

``` code
  Can you tell when a window first gets broken? What is your reaction? If it was the result of someone else's decision, or a management edict, what can you do about it?
```

Why is it worthy to look into code that is already working?

  - Working doesn't really mean correct. It may not really be working,
    just working due to undiscovered bugs or boundary conditions or just
    lucky.

  - Undocumented behavior that you rely upon may change with the next
    release of the library. Try not to use undocumented api's. If you
    must, then document your assumptions.

  - Additional and unnecessary calls make code slower and also increase
    the risk of introducing new bugs of their own.

How to program deliberately?

  - Always be aware of what you are doing.

  - Attempting to build an application you don't fully understand OR to
    use a technology you aren't familiar with, is an invitation to be
    misled by coincidences.

  - Proceed from a plan, whether that plan is in your head, on the back
    of a cocktail napkin, or on a wall-sized printout from a CASE tool.

  - Rely only on reliable things. Don't depend on accidents or
    assumptions. If you can't tell the difference in particular
    circumstances, assume the worst.

  - Document your assumptions. Design by Contract, can help clarify your
    assumptions in your own mind, as well as help communicate them to
    others.

  - Don't just test your code, but test your assumptions as well. Don't
    guess; actually try it. Write an assertion to test your assumptions.
    If your assertion is right, you have improved the documentation in
    your code. If you discover your assumption is wrong, then count
    yourself lucky.

  - Prioritize your effort. Spend time on the important aspects; more
    than likely, these are the hard parts. If you don't have
    fundamentals or infrastructure correct, brilliant bells and whistles
    will be irrelevant.

  - Don't be a slave to history. Don't let existing code dictate future
    code. All code can be replaced if it is no longer appropriate. Even
    within one program, don't let what you've already done constrain
    what you do next, be ready to refactor. This decision may impact the
    project schedule. The assumption is that the impact will be less
    than the cost of not making the change.

## Fundamental Principles of Software Engineering

Apply and use quantitative measurements in decision-making (A)
Build with and for reuse (B)
Control complexity with multiple perspectives and multiple levels of abstraction (C)
Define software artifacts rigorously (D)
Establish a software process that provides flexibility (E)
Implement a disciplined approach and improve it continuously (F)
Invest in the understanding of the problem (G)
Manage quality throughout the life cycle as formally as possible (H)
Minimize software component interaction (I)
Produce software in a stepwise fashion (J)
Set quality objectives for each deliverable product (K)
Since change is inherent to software, plan for it and manage it (L)
Since tradeoffs are inherent to software engineering, make them explicit and document them (M)
To improve design, study previous solutions to similar problems (N)
Uncertainty is unavoidable in software engineering. Identify and manage it (O)

Derived from paper: "Fundamental principles of software engineering a journey"
