% Automation using Expect
% Ravikiran K.S.
% January 1, 2006

## Pexpect on Mac

``
$ sudo easy_install pexpect
$ sudo easy_install ptyprocess
``

$ python -c "import pexpect"
$ echo $?                           // to check if pexpect module is installed

## Tips n Tricks

1. Command line input using -c option
$ expect -c 'expect "\n" {send "pressed enter\n"}'

2. Execute expect script interactively using -i option
$ expect -i arg1 arg2 arg3

3. Print debug messages while executing expect script
$ expect -d sample.exp

4. Enable expect debugger using -D
$ expect -D 1 sample.exp

5. Execute expect script line by line
$ expect -b sample.exp

6. To provide options to expect script (not to be interpreted by expect itself)
$ expect sample.exp -d -c

7. Reading command line arguments by indexes. lindex can be used for this. Ex.
set arg0 [lindex $argv 0]

8. Reading command line arguments by range. lrange is used. It specifies start
arg index and stop arg index.  Hence to read same arg0 using lrange, it is
set arg0 [lrange $argv 0 0]

9. Printing logs to user console. Use ``'send_user'`` instead of 'puts'. For ex.
``send_user "string"``

10. Square brackets [] have different meaning in Expect (used to execute
commands). Hence, when using them in strings, they should be escaped using \
character.

11. In order to provide default values to some arguments of a function, use
proc justdoit {a {b 1} {c -1}} {
}

12. To pass variable number of arguments to a proc/function, 'args' is used.
proc justdoit {args} {
}

This can be combined with variable names to get proc arguments in which first
few arguments are fixed and remaining ones are variable.
proc justdoit {firstarg secondarg args} {
}

13. Pre-defined command line arguments
$argc - number items of arguments passed to a script.
$argv - list of the arguments.
$argv0 - name of the script.

14. Few more debugging pearls:
To see where the error was triggered in interactive mode, use
    set errorInfo

This variable shows index of the faulty line inside the procedure body - FP (for the beginners)
    info body _the_name_of_the_procedure_

15. These seem to be the major issues people hit:
    a. expecting less than is advisable and too much in other places:
            "Funny Users Password:" in contrast to "assword: "
    b. underestimating the greediness of REs
            ".*"  in contrast to ".*?"
            or building really brilliant and convoluted REs
            that break in really brilliant and convoluted ways.
    c. using spawn where exp_send would be the proper command:
            spawn telnet
            .. login stuff ..
            exp_send "MySuperDuperRemoteScript with some args\r"
    d. being too optimistic and constructing scripts that continue unfazed due
        to not handling  "timeout" and "eof"
    e. Would making un"expect"ed timeout or eof a catchable error help?

16. 'autoexpect' creates script.exp by recording different commands entered on
command prompt.

http://www.thegeekstuff.com/2010/12/5-expect-script-command-line-argument-examples/
