% Debugging Tips Tricks
% Ravikiran K.S.
% January 1, 2006

## Some crazy issues that are hard to debug

* Not using a global allocation policy for data management.
    Problem - Ex. If you have made calculations of handling data using some
global #define, whereas allocating memory at a different place using local
hardcode values, the out-of-bound array access are common.
    Best Practice - Allocate all required memory at a single place (mostly
during init) And use one-to-one mapping of allocation space versus
global/module scoped macro to operate on that memory. Red zones, 0xDEADBEEF,
and other patterns can be used to find out-of-bound accesses, but better to
prevent it in first place.

* Stack data defined and spread into multiple places, multiple global variable, multiple static variables.
    Problem - Ex. If there are two variables with identical names but different
scopes are defined. Then functions may access wrong values leading to problem.
Scoping errors are difficult to trace as there are no compiler warnings raised.
    Best Practice - Allocate the static data into a small number of Control
Blocks. These are structures holding different memory elements. It helps to
keep a structure as it gives a human understandable shape to data, can be
synchronized easily using minimum set of locks, provide caching benefits by
loading related data to cache in one go.

* Single thread/interrupt routine hogging maximum CPU resource.
    Problem - Ex. Since a single thread consumes all CPU bandwidth, other
threads will end up starving. Becomes a major bottleneck in case of
time-sensitive protocols and processing requirements.
    Best Practice - Keep an accounting thread/scheduler that accounts for
On-CPU time of each thread. If a thread consumes more than specified amount of
time, raise alarms. Alternatively, register a software watchdog timer
registered at a higher interrupt level that identifies the thread hog and
restarts the machine. One more approach is to have a hardware watchdog timer
that regularly kicks off CPLD to verify certain memory area/register. If CPLD
finds a positive value in that area, it will trigger a system reboot. Software
threads in this case will be responsible for periodically erasing the data.

## DeadLocks

One creative way to deadlock two threads without involving any userland locking
mechanism is to have each thread join the other.

Another design approach is to have contending threads acquire lock in same order.
For example, say thread A and thread B are contending for resources X, Y, and Z
locks. Then, if the lock order is specified as X -> Y -> Z, then there can never
be a deadlock in that application. The lock order means, if a Thread wants lock
for Y, they it should have acquired lock for X as well. Similarly, locking Z
would require thread to have lock for X and Y as well. But this is not scalable.

 A       B
---     ---
X
Y        X
Z        Y
         Z

Searching for a deadlock means reconstructing the graph of dependencies between
threads and resources (mutexes, semaphores, condition variables, etc.) - who
owns what and who wants to acquire what. A typical deadlock would look like a
loop in that graph. The task is tedious, as some of the parameters we are
looking for have been optimized by the compiler into registers.

pthreads has some built-in support for certain synchronization mechanisms (e.g.
``PTHREAD_MUTEX_ERRORCHECK`` mutexes).  Also, there's a POSIX mutex construction
attribute for robust mutexes (a recoverable mutex that was held by a thread
whose process has died): an error return code indicates the earlier owner died;
and lock acquisition implies acquired responsibility for dealing with any
cleanup.

Dynamic Linker
--------------
To find out which dynamic library is a symbol coming from:
``
$ LD_DEBUG_OUTPUT=sym.log LD_DEBUG=bindings /bin/ls
$ cat sym.log.7688 | grep malloc
$ LD_DEBUG=help /bin/ls
Valid options for the LD_DEBUG environment variable are:
  libs        display library search paths
  reloc       display relocation processing
  files       display progress for input file
  symbols     display symbol table processing
  bindings    display information about symbol binding
  versions    display version dependencies
  all         all previous options combined
  statistics  display relocation statistics
  unused      determined unused DSOs
  help        display this help message and exit
To direct the debugging output into a file instead of standard output
a filename can be specified using the LD_DEBUG_OUTPUT environment variable.
``

## Memory Issues

- Memory can be allocated through many API calls:
    malloc()/memalign()/realloc()/mmap()/brk()/sbrk()
- To return memory to the OS: free()/munmap()

GlibC provides built-in functionality to help detect memory issues: mtrace()

Steps to use Valgrind:
``
LD_LIBRARY_PATH=/path/to/valgrind/libs:$LD_LIBRARY_PATH /path/to/valgrind \
-v --error-limit=no --num-callers=40 --fullpath-after= --track-origins=yes \
--log-file=/path/to/valgrind.log --leak-check=full --show-reachable=yes \
--vex-iropt-precise-memory-exns=yes /path/to/program program-args
``
