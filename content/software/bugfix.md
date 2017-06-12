% Bug Fixing
% Ravikiran K.S.
% January 1, 2006


If you can't re-create the bug on demand, then your chances of fixing it
will be nil.

Step 1: Enter the bug in your case tracking system Record these three
things in each bug report:

  - What the user was doing

  - What they were expecting

  - What happened instead

Step 2: Search the error message Search for error phrases in
bug-tracking system

Step 3: Identify the immediate line of code where the bug occurs If its
a crash, its a boon. Logs are pristine.

Step 4: Identify the flow-of-code/code-sequence where the bug actually
occurs This is the most daunting part. Most of times, this isn't very
apparent.

Step 5: Identify the species of bug

  - Error in external/system library - Then most likely error is in your
    code, in the way that those routines are used. Look into manpages,
    documentation, etc.

  - Coding errors - Off-By-One, Unhandled return value, Unexpected NULL,
    Bad input, Assignments instead of comparisons, Buffer overflow &
    Index Out-of-range, Wrong variable type used (int instead of long),
    constants are wrong,

  - Design errors - Race condition, Invalid state, Certain assumptions
    don't match in field (ex, constant fields, etc)

Some of the debugging tricks don't always apply.

