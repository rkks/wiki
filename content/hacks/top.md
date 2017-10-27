% Top command tricks
% Ravikiran K.S.
% January 1, 2006

## top command on-screen options

1 - displays all CPU details (SMP view)
m - displays memory usage details
u - for selection of users
l - load averages
t - (toggle) task/cpu stats
z - toggle color display
O - sort by any field
T - sort tasks by time/cumulative time.
o - change display order
f - add/remove field
> - move sort field to right
< - move sort filed to left
c - (toggle) show command line for each process
i - (toggle) show idle tasks
W - write configuration file
q - quit
k - kill a process
r - change priority of process
s - set update interval
A - display multiple views of top output (each view has different combination of columns)
G - change the display to one of the given view
= - remove all current applied restrictions
- - Show or hide current window
_ - Show all invisible windows.

.toprc
------
It should consist of two lines. The default may be something like:

AbcDgHIjklMnoTP|qrsuzyV{EFWX
5ci

- In the first line you specify what is displayed and the letters are the same
as explained in man page and in help
* A - PID - process id
* D - USER username
* H - PRI piority
* I - NI nice value
* M - SIZE size, virtual image
* T - RSS Resident set size
* P - SHARE share pages
* V - process status
* W - TIME
* E - CPU? (not sure)
* F - Mem? (not sure)

- In the second line you specify the behavior of top. In the example

* [number] - how many seconds for refresh, standard is 5
* c - show command line
* i ignore zombie processes

## command line arguments

-p - given as: -pN1 -pN2 ... or -pN1, N2 [,...] up to 20 pids. Monitors only mentioned processes.
