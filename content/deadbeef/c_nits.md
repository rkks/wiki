% C Programming Nits
% Ravikiran K.S.
% January 1, 2006

1. Machine dependency
Word length is machine dependent, C supports several data types with different word lengths for each format.
Integer formats (one's complement or two's complement) and floating point formats are machine dependent.

2. Two basic function usage rules to remember:
    a. The compiler does not convert function parameters from floating point to integer and vice versa.
    b. If a function returns a data type other than an integer, you must inform the compiler of the return data type before you call it (by including appropriate header files and function prototypes).

For Ex,
sqrt() expects double input, but this is 'const int'. hence asm conversion doesn't yield correct results.
printf("%.8g\n", sqrt(25));

a call with floating pt. input, but not including correct header file makes compiler to interpret result incorrectly.
printf("%.8g\n", sqrt(25.));

a call with floating pt. input and inclusion of correct header file and link by -lm option gives correct output.
#include <math.h>
printf("%.8g\n", sqrt(25.));

The way that in absence of declared return value of a function, compiler assumes it to be an int.
Same way, it also assumes type as int for input params too.

3. While using macros, the recommended approach by C standard is to guard them with do{ }while(0);
``
#define X(x)  do {   \
  _tmp = (x);           \
  (y) = (_tmp);         \
  (z) = (_tmp)           \
} while (0)
``

4. Using GNU indent, we can significantly reduce the time required to indent any C
source code. We can make it automated.

4. Return values of processes on Unix systems are mostly governed by shell. The
shell dictates convention by which a child process must return an 8-bit number
representing it status: 0 for normal termination, 0 > n < 128 for process
detected abnormal termination, and n > 128 for signal induced termination.

5. Watch for Interrupted calls: A number of API functions return an error code
if the call was interrupted by a signal.  Usually this is not an error by
itself and the call should be restarted. For instance:
``
int raccept( int s, struct sockaddr *addr, socklen_t *addrlen )
{
    int rc;
    do {
        rc = accept( s, addr, addrlen );
    } while ( rc == -1 && errno == EINTR );

    return rc;
}
``

The list of interruptible function differs from Unix-like platform to platform.

6. Watch for Spurious wake-ups: Threads waiting on a pthreads condition
variable can be waken up even if the condition has not been met. Upon waking up,
the condition should be explicitly checked and return waiting if it is not met.

7. Everything is a file descriptor: As just about anything is a file (folders,
sockets, pipes, etc...), just about anything can result in a file descriptor
that needs to be closed. /proc file system can provide info on open FD.

# tree /proc/26041
/proc/26041
...
|-- fd                  # Open files descriptors
|   |-- 0 -> /dev/pts/21
|   |-- 1 -> /dev/pts/21
|   |-- 2 -> /dev/pts/21
|   `-- 3 -> socket:[113497835]
|-- fdinfo
|   |-- 0
|   |-- 1
|   |-- 2
|   `-- 3
...

8. Interposition libraries to intercept for open, close, bind, socket, etc.
syscalls can be written to trace these FDs.

9. How can a C program produce a core dump of itself without terminating?
``
void create_dump(void)
{
    if(!fork()) {
        // Crash the app in your favorite way here
        abort() || (*((void*)0) = 42);
    }
}
``

Note: Remember, this wouldn't give full snapshot of all threads in original
process. It only gives state of first thread. That is because fork() creates
only the first thread layout at child. Use google core-dumper for realistic
working.

Even GNU libc provides api to print callstack at runtime. And gcore can be used
to take snapshot of process at runtime.

## Integer Overflows

Integer Overflows - They are handled differently for signed and unsigned
integers. This is because C standard states signed integer overflow as an
undefined behavior. For unsigned integers, following gcc options would catch
most of the errors:

    gcc -Wtype-limits -Wstrict-overflow=5 -fstrict-overflow -Wsign-compare

Some of the signed integer overflows can be caught using '-fwrapv' option of
gcc, but not all. '-fwrapv' tells the compiler to treat signed overflow as
wrapping. That is how Java defines signed overflow. The disadvantage of
'-fwrapv' is that it inhibits optimizations.

int f(int i) { int j, k = 0; for (j = i; j < i + 10; ++j) ++k; return k; }

In example above, if compiler can assume that signed overflow is undefined,
function can simply return 10. With '-fwrapv', it has to treat a value like
0x7ffffff8 which will overflow during the loop. Hence gcc has provided 2
options:

* -fno-strict-overflow - tells the compiler that it may not assume that signed
overflow is undefined. 
* -Wstrict-overflow - tells compiler to warn about cases where it uses 'signed
overflow is undefined' to optimize code.

Difference between -fwrapv and -Wstrict-overflow is seen only on processors
that do not use the ordinary twos-complement arithmetic: -fwrapv requires
twos-complement overflow, and -fno-strict-overflow does not. On common
processors, they behave same, however, affect optimizers differently. The
difference occurs because -fwrapv precisely specifies how the overflow should
behave. -fno-strict-overflow merely says that the compiler should not optimize
away the overflow.

One way to handle these signed integer overflows is by reducing values of both
sides like:

``
size_t alloc = SIZE_MAX;
if (alloc < SIZE_MAX - BUFSIZ)
............
``

Or else use explicit/implicit casting to wider types like below:

``
/* Explicit casting to wider types.  */
off_t chunk_size (off_t file_size, int n)
{
    assert (n && n <= file_size);
    /* Given the above assertion, the largest 'file_size%n' can be
     * 'file_size-n' as n will divide file_size at least once */
    return ((uintmax_t) file_size + n/2) / n; /* round() */
}
``
``
8 bits for a char isn't guaranteed by C standard; you can always override value
by writing CHAR_BIT from limits.h. Neither does it guarantee that an integer
arithmentic will use the 2's complement method.
``

For an unsigned 32-bit wide integer 0xffffffff if you add 1, it wraps back to 0
again. For a signed 32-bit integer minimum value is 0x80000000 and maximum is
0x7fffffff. Given above, the easiest way to check if overflow has happened is
to check:

if ((a+b) < a)  // OR a+b < b; whichever suits.

This may not always work because, when adding a number to a pointer or to a
signed integer, overflow is undefined.

Couple of pitfalls here:

1: abs(-2147483648) < 0
abs() returns the argument if it is positive, or the negated argument if it is
negative. But unfortunately, negative of -2147483648 viz. -(-2147483648) is
still -2147483648 (on a 32-bit platform). So, don't rely on abs() to provide
non-negative number.

2: adding a positive number to an integer might make it smaller
if you add two 32-bit numbers, the result can have 33 bits. On the CPU level,
this is usually in form of a 'carry flag'.  But unfortunately, there is no way
to access this carry bit from C directly. Storing result in a bigger data type,
OR checking MAX of variable type and comparing result to it doesn't work
always. Hence it is good to do the checking of result before doing operation.
But it has its own flaws.  For unsigned data types, minimum value is 0, and the
maximum value is (type) -1 (that is, -1 cast to the type we want) But this
doesn't work for signed data types. Fortunately, for both signed and unsigned
var, the minimum value is the bitwise NOT of the maximum value (the ~ operator
in C). That is: ~0x80000000 is equal to 0x7fffffff.

So, simple macros to calculate these min and max would be:
#define __HALF_MAX_SIGNED(type) ((type)1 << (sizeof(type)*8-2))
#define __MAX_SIGNED(type) (__HALF_MAX_SIGNED(type) - 1 + __HALF_MAX_SIGNED(type))
#define __MIN_SIGNED(type) (-1 - __MAX_SIGNED(type))

#define __MIN(type) ((type)-1 < 1?__MIN_SIGNED(type):(type)0)
#define __MAX(type) ((type)~__MIN(type))

Next question would be to findout whether we can assign a value to a variable
without truncation/loosing precision.  For ex, if (1 + -5) is assigned to an
unsigned int, result will still overflow. Macro is:

#define assign(dest,src) ({ \
    typeof(src) __x=(src); \
    typeof(dest) __y=__x; \
    (__x==__y && ((__x<1) == (__y<1))?(void)((dest)=__y),0:1); \
})

Finally, Add and subtract macros are:
#define add_of(c,a,b) ({ \
    typeof(a) __a=a; \
    typeof(b) __b=b; \
    (__b)<1 ? \
        ((__MIN(typeof(c))-(__b)<=(__a))?assign(c,__a+__b):1) : \
        ((__MAX(typeof(c))-(__b)>=(__a))?assign(c,__a+__b):1); \
})

#define sub_of(c,a,b) ({ \
  typeof(a) __a=a; \
    typeof(b) __b=b; \
      (__b)<1 ? \
          ((__MAX(typeof(c))+(__b)>=(__a))?assign(c,__a-__b):1) : \
              ((__MIN(typeof(c))+(__b)<=(__a))?assign(c,__a-__b):1); \
})

3: Multiplication can overflow, too
On most platforms, the CPU has a multiply instruction that returns a double
width integer. Ex, multiply of two 32-bit integers results into a 64-bit
integer. But in C, if you multiply two 32-bit integers, you get a 32-bit
integer again, losing the upper 32 bits of the result. Fortunately, if you cast
the first integer to a 64-bit number, and keep the destination as a 64-bit
number, then result will be 64-bit wide.

int umult32(uint32 a,uint32 b,uint32* c) {
    unsigned long long x=(unsigned long long)a*b;
    if (x>0xffffffff) return 0;
    *c=x&0xffffffff;
    return 1;
}

In C99, we can use __VA_ARGS__ in macro
For example, to print twice
#define printf(...) printf(__VA_ARGS__); printf(__VA_ARGS__)
