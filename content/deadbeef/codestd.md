% C Coding Standard Rules -- Barr Group
% Ravikiran K.S.
% January 1, 2006


### Rule \#1

Braces - Braces ({ }) shall always surround the blocks of code (also
known as compound statements) following if, else, switch, while, do, and
for keywords. Single statements and empty statements following these
keywords shall also always be surrounded by braces.

**Reasoning:** Considerable risk is associated with the presence of
empty statements and single statements that are not surrounded by
braces. Code constructs of this type are often associated with bugs when
nearby code is changed or commented out. This type of bug is entirely
eliminated by the consistent use of braces.

### Rule \#2

Keyword const - The const keyword shall be used whenever possible,
including:

  - To declare variables that should not be changed after
    initialization,

  - To define call-by-reference function parameters that should not be
    modified (for example, char const \* p\_data),

  - To define fields in structs and unions that cannot be modified (such
    as in a struct overlay for memory-mapped I/O peripheral registers),
    and

  - As a strongly typed alternative to \#define for numerical constants.

**Reasoning:** The upside of using const as much as possible is
compiler-enforced protection from unintended writes to data that should
be read-only. NOTE: A const definition requires memory space to store
the value, and processor cyles to fetch it from memory - whereas a
\#define may be coded as an immediate value. This is the downside of
defining numerical constants as 'const'. Instead 'enums' are better.

### Rule \#3

Keyword static - The static keyword shall be used to declare all
functions and variables that do not need to be visible outside of the
module in which they are declared.

**Reasoning:** C's static keyword has several meanings. At the
module-level, global variables and functions declared static are
protected from inadvertent access from other modules. Heavy-handed use
of static in this way thus decreases coupling and furthers
encapsulation.

### Rule \#4

Keyword volatile - The volatile keyword shall be used whenever
appropriate, including:

  - To declare a global variable accessible (by current use or scope) by
    any interrupt service routine,

  - To declare a global variable accessible (by current use or scope) by
    two or more tasks, and

  - To declare a pointer to a memory-mapped I/O peripheral register set
    (for example, timer\_t volatile \* const p\_timer).

**Reasoning:** Proper use of volatile eliminates a whole class of
difficult-to-detect bugs by preventing the compiler from making
optimizations that would eliminate requested reads or writes to
variables or registers that may be changed at any time by a
parallel-running entity.

### Rule \#5

Comments - Comments shall neither be nested nor used to disable a block
of code, even temporarily. To temporarily disable a block of code, use
the pre-processor's conditional compilation feature (for example, \#if 0
 \#endif).

**Reasoning:** Nested comments and commented-out code both run the risk
of allowing unexpected snippets of code to be compiled into the final
executable.

### Rule \#6

Fixed-width data types - Whenever the width, in bits or bytes, of an
integer value matters in the program, a fixed-width data type shall be
used in place of char, short, int, long, or long long. The signed and
unsigned fixed-width integer types shall be as shown in Table 1.

``` code
  Table 1
  Integer Width   Signed  Unsigned
  8 bits/1 byte   int8_t  uint8_t
  16 bits/2 bytes int16_t uint16_t
  32 bits/4 bytes int32_t uint32_t
  64 bits/8 bytes int64_t uint64_t
```

**Reasoning:** The ISO C standard allows implementation-defined widths
for char, short, int, long, and long long types, which leads to
portability problems. Though the 1999 standard did not change this
underlying issue, it did introduce the uniform type names shown in the
table, which are defined in the new header file \<stdint.h\>. These are
the names to use even if you have to create the typedefs by hand.

### Rule \#7

Bit-wise operators - None of the bit-wise operators (in other words, &,
|, ~, ^, «, and ») shall be used to manipulate signed integer data.

**Reasoning:** The C standard does not specify the underlying format of
signed data (for example, 2's complement) and leaves the effect of some
bit-wise operators to be defined by the compiler author.

### Rule \#8

Signed and unsigned integers - Signed integers shall not be combined
with unsigned integers in comparisons or expressions. In support of
this, decimal constants meant to be unsigned should be declared with a
'u' at the end.

**Reasoning:** Several details of the manipulation of binary data within
signed integer containers are implementation-defined behaviors of the C
standard. Additionally, the results of mixing signed and unsigned data
can lead to data-dependent bugs.

### Rule \#9

Parametrized macros vs. inline functions - Parameterized macros shall
not be used if an inline function can be written to accomplish the same
task.

**Reasoning:** There are a lot of risks associated with the use of
preprocessor \#defines, and many of them relate to the creation of
parameterized macros. The extensive use of parentheses (as shown in the
example) is important, but doesn't eliminate the unintended double
increment possibility of a call such as MAX(i++, j++). Other risks of
macro misuse include comparison of signed and unsigned data or any test
of floating-point data.

### Rule \#10

Comma operator - The comma (,) operator shall not be used within
variable declarations.

**Reasoning:** The cost of placing each declaration on a line of its own
is low. By contrast, the risk that either the compiler or a maintainer
will misunderstand your intentions is high.

\<note tip\>Complex variable declarations are common, but can be
difficult to comprehend. Reading declaration from right-to-left
simplifies the translation into English. Eg. timer\_t volatile \* const
p\_timer; p\_timer is a constant pointer to a volatile timer\_t
register set. i.e., address of timer registers is fixed while the
contents of those registers may change at any time.\</note\>

### Rule \#11

Keywords short and long - The keywords short and long shall not be
used.

**Reasoning:** The ANSI/ISO C standard contains a set of strict
requirements plus mere guidelines for the authors of C compilers. Among
the guidelines (also known as implementation-defined behaviors) are
the precise width, in bits, of the char, short, int, and long data
types. ISO C mandates that variables of type short must hold at least 16
bits and long at least 32 bits, across all platforms. Other than that,
the only strict requirement is that sizeof(char) < sizeof(short) <
sizeof(int) < sizeof(long). There are times when you must use char (see
Rule \#12). In other places (e.g., loop counter declarations), it makes
sense to allow the compiler to choose the best/fastest width using int.
But in keeping with Rule \#6 concerning adoption of the C99 fixed-width
integer types/names, it is appropriate to ban use of the short and long
types altogether. Like the semi-colon followed by left-brace sequence
above, an automated tool can be used to enforce this rule.

### Rule \#12

Keyword char - Use of the keyword char shall be restricted to the
declaration of and operations concerning strings.

**Reasoning:** Among the implementation-defined behaviors of C is the
signed-ness of a char data type. One compiler may treat your char
variables as unsigned, another as signed and yet both are technically
ISO C compliant\! This introduces subtle and potentially hidden risks
related to using the bit-wise operators on signed char data (Rule \#7)
or mixing signed and unsigned data (Rule \#8). The risk of bugs derived
from this subtlety of C are entirely eliminated by choosing int8\_t or
uint8\_t explicitly whenever the data is other than part or all of a
string.

### Rule \#13

Initialization of variables - All variables shall be initialized before
use.

**Reasoning:** Too many C programmers assume the C run-time will watch
out for them. This is a very bad assumption, which can prove dangerous
in a real-time system. We may sometimes get lucky with C startup code
that zeros all uninitialized data or the stack. But lucky is not
acceptable in a medical device or any other safety-critical product. By
making initialization before use a hard rule on your project, you
elevate the warning a static-analysis tool can raise on this to an error
that must be dealt with by the team.

### Rule \#14

Global variable names - The names of all global variables shall begin
with the letter 'g'. For example, g\_zero\_offset.

**Reasoning:** Eschew them all you like, but global variables are a fact
of life in some parts of embedded software. There are two specific risks
associated with their use. The first risk of global variables is that a
race condition between two or more asynchronous entities (be they CPUs,
peripherals, ISRs, tasks, or a combination) will corrupt the data stored
there. To prevent this, exclusive access must be established
programmatically, such as via use of interrupt disables or mutex
acquisition. The second risk is that the compiler will optimize away one
entity's read or write because it cannot see the other user during
compilation. Both risks can be eliminated, but only if everyone on the
team sees that they are global variables. By naming all globals in an
obvious way, code reviews become that much more effective all the way
down to the individual line of code or function.

### Rule \#15

Constants in comparisons - When evaluating the equality or inequality of
a variable with a constant value, always place the constant value on the
left side of the comparison operator.

**Reasoning:** It is always desirable to detect possible typos and as
many other bugs as possible at compile-time; run-time discovery may be
dangerous to the user of the product and require significant effort to
locate. By following this rule, the compiler can detect erroneous
attempts to assign (i.e., = instead of == ) a new value to a constant.

