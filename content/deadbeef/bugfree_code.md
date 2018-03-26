% Writing Bug-Free C Code - Review
% Ravikiran K.S.
% January 1, 2006

## Writing Bug-Free C Code - Review (http://www.duckware.com/bugfreec/preface.html)

http://www.duckware.com/bugfreec/index.html

The key to eliminating bugs from your code is learning from your mistakes.

A rock-solid function must first of all be bug-free itself.
- The function must provide a clean, intuitive interface.
- Function names must clearly state what the function does.
- Function must detect and report invalid function arguments.
- The function should be fault-tolerant.
Not all C libary functions and/or system calls are rock-solid. Hence, A layer
that is rock-solid needs to be coded. For ex, malloc()/free()

1. Programming methodologies must make it easy for new programmers to jump into
a project without introducing a slew of new bugs.

2. One major problem in large projects is that there is too much information to
become familiar with in a short period of time.

3. A well-defined, small, and isolated problem is easier to solve. Real work is
to breakdown and define large complex problems into small simpler problems.

4. A goal is achievable if you know end result, and can describe very next step

5. Eliminating data structures from include files make code safer by compile and
link time type-checking of pointers.

6. Too much global data (that too scattered) is root of all evil. Limit scope
of data-structure by providing helper APIs. But remember get/set are evil too.
    - expose API's to achieve abstract goal, not var names
    - any change in data structure should require at-most one file change

7. For global API's (exported functions), define signature based on What, leave
the How.

8. Sanitize API inputs, assert API outputs. Keep fewer checks in util functions.

9. Always validate function/syscall return values, have a method to propragate
error/return info into top level API/abstraction layer or business logic.

10. When fixing a bug, fix the underlying problem or cause of the bug and not
just the bug itself.

11. Add instruments to detect and repair/report the runtime errors. Like memory,
locks, timeouts, cpu hog, and so on.

12. Use kernel provided debugging aids like Linux KConfig, MS Win debug kernel
while doing development.

13. Array reference E1[E2] is identical to ``*((E1)+(E2))``. Hence arr[idx] is
same as idx[arr].

14. If p is a pointer to a structure, the structure reference p->member is
identical to ``(*p).member``.

15. Use CompilerAssert() to verify design-time assumptions at compile-time. Ex,
``#define ct_assert(e) extern char (*ct_assert(void)) [sizeof(char[1 - 2*!(e)])]``
``
ct_assert(sizeof(my_struct)==512);
ct_assert(sizeof(int)==4);              // check size
ct_assert(M_PI/2);                      // compile time check, divisible by 2
``

16. Keep all names in a given program/lib unique to that by adding a prefix. For
example, module xyz will have prefix ``xyz_``, libabc has prefix ``libabc_``

17. Make it easy to identify non-elementary data-types by adding suffix and
identify elementary data-types by adding prefix.
``
_t : typedef, _cb : control block, _e : enum, _s : struct, _u : union
c_ : char, i_ : int, l_ : long, f_ : float, s_ : short, d_ : double, p_ : ptr
``

18. Initial any variable, of any type, without using memset(&var,0,sizeof(var))
``type_t variable = {0,};``

NOTE: gcc -Wmissing-field-initializers option (which is included in -Wextra)
issues a warning for this useful construct in versions before 4.7

19. Check gcc compiler supported options, all adhering to C99 standard
echo "void f(int i) {i++;int j;}" |
${CC:-gcc} -xc -c -o /dev/null - 2>/dev/null && echo c99

20. Constants in C programs are bundled together with text segment to provide
read-only protection for same.
``
$ size test.o  # compiled from the code in the question
   text    data     bss     dec     hex filename
     58       0       0      58      3a test.o
     |
     +------------- This is sum of .text, .rodata, and .eh_frame below.

$ size -A test.o
test.o  :
section           size   addr
.text                6      0
.data                0      0
.bss                 0      0
.rodata              4      0
.comment            29      0
.note.GNU-stack      0      0
.eh_frame           48      0
Total               87

$ objdump -t test.o | grep -w global
0000000000000000 g     O .rodata    0000000000000004 global
                 |     |
                 |     +---------- variable is of 'Object' type, not 'Function'
                 +---------------- variable 'global' is part of .rodata section
``

21. Print datatypes through string concatenation features in compilers
#define PRINTV(var, fmt) printf( "Variable " #var "= %" #fmt "\n", var )
#define PRINTINT(var) PRINTV(var, d)
#define PRINTFLOAT(var) PRINTV(var, f)
#define PRINTLONG(var) PRINTV(var, l)
#define PRINTUINT(var) PRINTV(var, u)
#define PRINTCHAR(var) PRINTV(var, c)
#define PRINTHEX(var) PRINTV(var, x)

22. Each if/else/for/while/do-while should have opening and closing braces

23. A misspelled default in a switch is considered a label by compiler, careful

24. Use 'De Morgans' laws to simplify the nested condition checks
- !(a && b) == (!a || !b)
- !(a || b) == (!a && !b)

25. Always hide large macros in their own scope by do-while(0) enclosure

26. Avoid 'else' in if condition checks as long as possible.

27. Some may or may not need conditional code-block to be evaluated if certain
condition is hit. For example, assert().

WinAssert(expression);          // just an assert, no further dependency
WinAssert(expression) {         // tries to execute code if assert() was PASS
    (block of code)
}

#define WinAssert(exp) if (!(exp)) {AssertError;} else

28. Useful Macros
    - for calculating total number of elements placeable in a static array
``#define sizeof_arr(arr) (sizeof(arr)/sizeof(arr[0]))``
    - stringify an input variable name
#define STRING(x) #x
    - concatenate an input variable name to a constant prefix or suffix
#define EV(n) {0x##n,szPE##n}
static struct {
    WORD wError;
    LPSTR lpError;
} BASEDCS ErrorToTextMap[]={ EV(7008), EV(700A), EV(7060) };
    - determine if input is a power of 2
#define ISPOWER2(x) (!((x)&((x)-1)))
    - swap 2 integers without a temporary space. XOR method
#define SWAP(x, y)     do { \
    (x) ^= (y);             \
    (y) ^= (x);             \
    (x) ^= (y);             \
while(0)
    - Turn on all bits in a word (0xFFFF equivalent)
#define ALLONES     (~0)

29. In signed numbers, zero is represented by 2 numbers, all 1's and all 0's.
Thus to negate a value, say get -x value for x.

#define NEGATE(x) (((x)^~0)+1)

30. For any construct that's compiler or platform specific, hide it behind an
abstraction of a macro or a function.

31. Macros in UPPERCASE, everything else in lowercase, no underscore as prefix
References:

32. Absolutely no repetition of code or data in programs. Refactor continuously
to achieve optimum code.

33. Turn C into object-oriented code by having 4 things and a compiler check.
- objects (private): any new data-type declaration, scope local to a file.
- methods (public) : a way for other files to operate on this opaque data.
- handle : abstract object identifier. must be type-checkable by compiler.
- instance : global/static var of object-type occupying memory space.
It is possible to create a pointer in C that points to an unknown object, and
yet it's type-checkable by compiler. It's also impossible to perform indirection
on this pointer in all source files, except the owner source file.

``
typedef struct tagNODEA *PNODEA;
typedef struct tagNODEB *PNODEB;
``
typedef struct tagNODEA {
    PNODEB pNodeB;
    ...
    } NODEA;

typedef struct tagNODEB {
    PNODEA pNodeA;
    ....
    } NODEB;

Creating a pointer-type to a structure-type as a typedef allows compiler to do
the type-checking on pointers. Adding this capability to create handles as macro

``#define NEWHANDLE(hdl) typedef struct hdl##_s *p_##hdl`` // in handler.h

``
// random number generator interfaces in header file (using above macro)
NEWHANDLE(hrand);   // create a new handler/typef for random number generator
.
EXTERNC p_hrand APIENTRY RandCreate  ( int );
EXTERNC p_hrand APIENTRY RandDestroy ( p_hrand );
EXTERNC int   APIENTRY RandNext    ( p_hrand );
``

// random number generator implementation (only file to know structure)
typedef struct hrand_s {
    long lRand;
} hrand_t;

HRAND RandCreate( int nSeed )
{
    HRAND hRand;
    (allocate memory);
    hRand->lRand = nSeed;
    return (hRand);
}

HRAND RandDestroy( HRAND hRand )
{
    (free hRand memory)
    return (NULL);
}

int RandNext( HRAND hRand )
{
    hRand->lRand = NEXTRAND(hRand->lRand);
    return(FINALRAND(hRand->lRand));

}

// random number generator user/test program (doesn't know what struct t
void Testing( void )
{
    p_hrand hRand = RandCreate(0);
    LOOP(100) {
        printf( "Number %d is %d\n", loop, RandNext(hRand) );
        } ENDLOOP
    hRand = RandDestroy( hRand );
}

This typedef can't be dereferenced in other files as struct-type isn't yet known
All the xyzDestroy() API's will return NULL as a rule, thereby resetting caller.

http://www.pixelbeat.org/programming/gcc/
