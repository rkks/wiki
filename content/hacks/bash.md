% Bash Tips & Tricks
% Ravikiran K.S.
% January 1, 2006

## Less in More - Bash

I have lost countless hours tracking different issues in my bash
settings. Either vim doesn't work OR screen doesn't work OR some other
platform has an issue. Being an enthusiast to I keep on updating my
configurations regularly with some good stuff. But, each time that I
make a change, I do not try to test it in every way, as in restarting
every application that is dependent on these configuration file and
ensuring that everything works fine. Simply because there are way too
many applications that depend on environment settings and it is
impossible to test every god-damn application.

Overtime, these changes accumulate and one day nothing works. And I end
up debugging the problem in expectation to find something useful that
can fix the problem. This pattern of timely updates and troubles is
counter productive and has very less advantage. Thus, I have learned
something today:

“Don't have more configuration than what is necessary” - period.

\* be judicious in taking new changes to your configuration. Fancy
doesn't mean working and/or correct. \* if an application needs some
configuration, write a wrapper script for it. \* if some option is
working in some platform, keep it guarded in flags until you verify that
it works on other platforms too.

This much is enough for this notes.

