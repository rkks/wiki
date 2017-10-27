% Bash Tips & Tricks
% Ravikiran K.S.
% January 1, 2006

## Less in More - Bash

I have lost countless hours tracking different issues in my bash settings.
Either vim doesn't work OR screen doesn't work OR some other platform has an
issue. Being an enthusiast to I keep on updating my configurations regularly
with some good stuff. But, each time that I make a change, I do not try to test
it in every way, as in restarting every application that is dependent on these
configuration file and ensuring that everything works fine. Simply because
there are way too many applications that depend on environment settings and it
is impossible to test every god-damn application.

Overtime, these changes accumulate and one day nothing works. And I end up
debugging the problem in expectation to find something useful that can fix the
problem. This pattern of timely updates and troubles is counter productive and
has very less advantage. Thus, I have learned something today:

"Don't have more configuration than what is necessary" - period.

* be judicious in taking new changes to your configuration. Fancy doesn't mean
working and/or correct.
* if an application needs some configuration, write a wrapper script for it.
* if some option is working in some platform, keep it guarded in flags until
you verify that it works on other platforms too.

# Tricks in bash

() - creates sub-shell. hence any command run within sub-shell wouldn't apply
to the parent shell.
{} - is for command grouping. if commands are grouped, then there must be a
semicolon before the closing.
((...)) - is for arithmetic and evaluating any flag to be positive. same as ()
operator in C. also supports tertiary operator.
unset -f myfunction - remove function
unset -v 'myArray[2]' - unset element 2 of myArray. The quoting is important to
prevent globbing.
unalias rm

special built-in command "command" tells the function not to call itself
recursively. Ex:
function ls() {
    command ls --color=auto "$@";   # 'ls' command would be called, not a recursive function call to itself (ls())
}

Some facts about alias:
    * Aliases will not invoke themselves recursively
    * Aliases cannot have local variables.
    * Aliases cannot take arguments.
    * Aliases do not work in scripts, at all. They only work in interactive shells.

http://mywiki.wooledge.org/BashGuide/CompoundCommands#preview

at command syntax -- $ at <time> <command> <options>
time format -- now|HH:MM + count <time-units> [today|tomorrow]
time-units can be minutes, hours, days, weeks, months, years.

## TERM Settings

if [ "$TERM" = "xterm" ] ; then
    if [ -z "$COLORTERM" ] ; then
        if [ -z "$XTERM_VERSION" ] ; then
            echo "Warning: Terminal wrongly calling itself 'xterm'."
        else
            case "$XTERM_VERSION" in
            "XTerm(256)") TERM="xterm-256color" ;;
            "XTerm(88)") TERM="xterm-88color" ;;
            "XTerm") ;;
            *)
                echo "Warning: Unrecognized XTERM_VERSION: $XTERM_VERSION"
                ;;
            esac
        fi
    else
        case "$COLORTERM" in
            gnome-terminal)
                # Those crafty Gnome folks require you to check COLORTERM,
                # but don't allow you to just *favor* the setting over TERM.
                # Instead you need to compare it and perform some guesses
                # based upon the value. This is, perhaps, too simplistic.
                TERM="gnome-256color"
                ;;
            *)
                echo "Warning: Unrecognized COLORTERM: $COLORTERM"
                ;;
        esac
    fi
fi
``
SCREEN_COLORS="`tput colors`"
if [ -z "$SCREEN_COLORS" ] ; then
    case "$TERM" in
        screen-*color-bce)
            echo "Unknown terminal $TERM. Falling back to 'screen-bce'."
            export TERM=screen-bce
            ;;
        *-88color)
            echo "Unknown terminal $TERM. Falling back to 'xterm-88color'."
            export TERM=xterm-88color
            ;;
        *-256color)
            echo "Unknown terminal $TERM. Falling back to 'xterm-256color'."
            export TERM=xterm-256color
            ;;
    esac
    SCREEN_COLORS=`tput colors`
fi
if [ -z "$SCREEN_COLORS" ] ; then
    case "$TERM" in
        gnome*|xterm*|konsole*|aterm|[Ee]term)
            echo "Unknown terminal $TERM. Falling back to 'xterm'."
            export TERM=xterm
            ;;
        rxvt*)
            echo "Unknown terminal $TERM. Falling back to 'rxvt'."
            export TERM=rxvt
            ;;
        screen*)
            echo "Unknown terminal $TERM. Falling back to 'screen'."
            export TERM=screen
            ;;
    esac
    SCREEN_COLORS=`tput colors`
fi
``

## String Manipulation

Find length of a String: ${#string}
Extract sub-string from certain position: ${string:position}
Extract sub-string from certain position for certain length: ${string:position:length}
Longest Substring Match from Front of String: ${string##substring}
Longest Substring Match from Back of String: ${string%%substring}
Shortest Substring Match from Front of String: ${string#substring}
Shortest Substring Match from Back of String: ${string%substring}
Replace first match of pattern with new string: ${string/pattern/replacement}
Replace all matches of pattern with new string: ${string//pattern/replacement}
Replace pattern only if it is in beginning of string: ${string/#pattern/replacement
Replace pattern only if it is in end of string: ${string/%pattern/replacement

## Some tips and tricks for bash

### Output your microphone to a remote computer's speaker using SSH
$ dd if=/dev/dsp | ssh -c arcfour -C username@host dd of=/dev/dsp

Then you can play anything. Testing can just be done by
$ cat /bin/bash > /dev/dsp

### Compare a remote file with a local file
$ ssh user@host cat /path/to/remotefile | diff /path/to/localfile -

### Execute a command at a given time
$ echo "ls -l" | at midnight

### Shutdown a Windows machine from Linux
$ net rpc shutdown -I ipAddressOfWindowsPC -U username%password

### A fun thing to do with ram is actually open it up and take a peek.
This command will show you all the string (plain text) values in ram
$ sudo dd if=/dev/mem | cat | strings

### Execute a command without saving it in the history
$ <space>command

### To check the TTY settings, use:
# stty -a

