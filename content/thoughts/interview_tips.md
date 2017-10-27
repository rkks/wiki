% Interview Questions
% Ravikiran K.S.
% January 1, 2006


## Where do you see yourself in 5 years? (Awesome)

“That depends on where this company will be in four.”

It's honest, and it's an opener to further questions about the company.
you should come out of an interview knowing more about the company's
trajectory than when you entered. It's also confronting. If they've
given their response and still want you to answer theirs, then your
final answer is inevitable:

“Well, now it depends on whether you've just given me the job…”

“If you can't dazzle them with brilliance, baffle them with bullshit.” -
W. C. Fields

## Technical Questions

### 1\. Check integer overflow

If adding two positive values yields a negative output or adding two
negative values yields a positive output, then it is an overflow.

  - Time Complexity : O(1)

  - Space Complexity: O(1)

If one of the components (let's say 'a') in addition is greater than
INT\_MAX - b, then it will be an overflow.

  - Time Complexity : O(1)

  - Space Complexity: O(1)

### 2\. Compute integer absolute value

(( n \>\> 31 ) + n ) ^ ( n \>\> 31 ) OR (( n \>\> 31 ) ^ n) - ( n \>\>
31 )

### 3\. Count the number of bits in integer

Simple method:  
`for (; num; num »= 1) { count += (num & 1); }`

Brian Kernighan’s Algorithm:  
`for (; num; num &= (num - 1)) { count++; }`

### 4\. Find intersection two linked lists

Given number of elements in first linked list be m and in second linked
list be n, and m \> n. Then on first linked list start from (m - n)th
element and on second linked list, from first element. If any of the
pointers match during traversing, except NULL, then linked list
intersects.

### 5\. Reverse a linked list

``` code
Node *reverse(Node *x) {
   Node *current = x;
   Node *previous = NULL;
   while (current != NULL) {
      Node *next = current->next;
      current->next = previous;
      /* switch to new node */
      previous = current;
      current = next;
   }
   return previous;
}
```

### 6\. Reverse the string

``` code
void reverseString(char* str, int len)
{
    int i, j;
    char temp;
    i=j=temp=0;
    j=len-1;
    for (i=0; i<j; i++, j--)
    {
        temp=str[i];
        str[i]=str[j];
        str[j]=temp;
    }
}
```

### 7\. Find loop in singly linked list

Keep two pointers, first offsets by one and second offsets by two. If
there is a loop, they two will intersect.

``` code
bool hasLoop( Node *startNode )
{
    Node *slowNode, *fastNode;
    slowNode = fastNode = startNode;
    while( slowNode && fastNode && fastNode->next )
    {
        if( slowNode == fastNode->next || slowNode == fastNode->next->next )
        {
            return true;
        }
        slowNode = slowNode->next;
        fastNode = fastNode->next->next;
    }
    return false;
}
```

### 8\. Find and return linked list from Nth element till last element

``` code
Node * findNToLastNode( Node *head, int N )
{
    int i = 0;
    Node *current, *behind;
    current = head;
    for( i = 0; i < N; i++ ) {
        if( current->next ) {
            current = current->next;
        } else {
            return NULL;                  // Length of the list is less than N
        }
    }
    behind = head;
    while( current->next ) {
        current = current->next;
        behind = behind->next;
    }
    return behind;
}
```

### 9\. Dangling Pointer

A pointer that either hasn't been allocated any memory or allocated
memory is freed while the pointer still points to same address.

### 10\. Memory leak

The pointer pointing to allocated memory region has got corrupt or
mis-assigned to some other location, and there is no way to recover back
or free the previously allocated memory.

### 11\. Opaque pointer

It is a pointer that doesn't have associated type definition in the
translation unit that it is used.

### 12\. Reverse the bits in integer

Simple:

``` code
for (i = 0; i < (sizeof(num) * 8); i++)
{
    if((num & (1 << i))) {
        reverse_num |= (1 << ((NO_OF_BITS - 1) - i));
    }
}
```

  - Time Complexity: O(log n)

  - Space Complexity: O(1)

Standard:

``` code
unsigned int count = sizeof(num) * 8 - 1;
unsigned int reverse_num = num;
for (num >>= 1; num; num >>= 1, reverse_num <<= 1, count--)
{
    reverse_num |= num & 1; 
}
reverse_num <<= count;
```

  - Time Complexity: O(log n)

  - Space Complexity: O(1)

### 13\. Check endianness of machine

``` code
unsigned int i = 1;
char *c = (char*)&i;
if (*c)
    printf("Little endian");
else
    printf("Big endian");
```

### 14\. Turn off the rightmost set bit

``` code
n = n & (n - 1);
```

### 15\. Add 1 to given number without using +, -, ++ and other operators

Simple: To add 1 to a number x (say 0011000111), we need to flip all the
bits after the rightmost 0 bit (we get 0011000000). Finally, flip the
rightmost 0 bit also (we get 0011001000) and we are done.

``` code
int m = 1;
/* Flip all the set bits until we find a 0 */
while( x & m )
{
  x = x^m;
  m <<= 1;
}
/* flip the rightmost 0 bit */
x = x^m;
```

### 16\. Implement spin lock

Simple:

``` code
lock(l)
{
    lock:
    while(l);
    atomic_add(l);
    if (l > 1) {
        atomic_sub(l);
        goto lock;
    }
}
unlock(l)
{
   atomic_sub(l);
}
```

Intel:

``` code
lock:                        ; The lock variable. 1 = locked, 0 = unlocked.
   dd      0
spin_lock:
   mov     eax, 1          ; Set the EAX register to 1.
   xchg    eax, [lock]     ; Atomically swap EAX register with lock variable. This will always store 1 to the lock, leaving previous value in the EAX 
   test    eax, eax        ; Test EAX with itself. Among other things, this will set the processor's Zero Flag if EAX is 0. If EAX is 0, then the lock was 
                           ; unlocked and we just locked it. Otherwise, EAX is 1 and we didn't acquire the lock.
   jnz     spin_lock       ; Jump back to the XCHG instruction if the Zero Flag is not set; the lock was previously locked, and so
                           ; we need to spin until it becomes unlocked.
   ret                     ; The lock has been acquired, return to the calling function.
```

spin\_unlock:

``` code
   mov     eax, 0          ; Set the EAX register to 0.
   xchg    eax, [lock]     ; Atomically swap the EAX register with the lock variable.
   ret                     ; The lock has been released.
```

Other:

``` code
wait(s)
{
   while (atomic_add(s) != 1)
   {
      atomic_sub(s);
   }
}
```

``` code
signal(s)
{
   atomic_zero(s); // release the lock
}
```

### 17\. Swap an integer in place

``` code
void xorSwap (int *x, int *y) {
   if (x != y) {
       *x ^= *y;
       *y ^= *x;
       *x ^= *y;
   }
}
```

Since XOR is an expensive operation, using temp register is far more
efficient. These days processors provide native instructions (for eg.
XCHG in x86) for swapping.

``` code
void addSwap (unsigned int *x, unsigned int *y)
{
   if (x != y) {
       *x = *x + *y;
       *y = *x - *y;
       *x = *x - *y;
   }
}
```

### 18\. Modulus operation without modulus operator

Works only when divider is a power of 2.

``` code
N % 2 = N & (2 - 1)
N % 4 = N & (4 - 1)
N % 64 = N & (64 - 1)
```

### 19\. Find smallest of three integers without comparison operators

``` code
while ( x && y && z )
{
    x--;  y--; z--; count++;
}
```

### 20\. Given an array arr\[\] of two elements having value 0 and 1, make both elements 0

Catch is, though it is guaranteed that one element is 0 but we do not
know its position, can only complement array elements, no other
operation like and, or, multi, division, …. etc. allowed, can’t use if,
else and loop constructs, and obviously can’t directly assign 0 to array
elements

``` code
arr[ arr[1] ] = arr[ !arr[1] ];
Or
arr[ !arr[0] ] = arr[ !arr[1] ];
Or
arr[ arr[1] ] = arr[ arr[0] ];
```

### 21\. Free a memory allocated using malloc() without using free()

``` code
int *ptr = (int*)malloc(10);
/* we are calling realloc with size = 0 */
realloc(ptr, 0);
```

### 22\. Cause a segfault in a program

\*(int \*) 0 = 0; \#define CRASH() do { \\\\\\ ((void(\*)())0)(); \\\\\\
}
while(false)

### 23\. Bootstrap code before and after main() function in GCC

``` code
/* Apply the constructor attribute to startup() so that it is executed before main() */
void startup (void) __attribute__ ((constructor));

/* Apply the destructor attribute to cleanup() so that it is executed after main() */
void cleanup (void) __attribute__ ((destructor));
```

There can be more than one functions with constructor attribute. But
then order of execution of those functions is not defined.

### 24\. Calculate log n in one line

``` code
unsigned int logn(unsigned int n)
{
    return (n > 1)? 1 + logn(n/2): 0;
}
```

### 25\. Remove duplicates from a sorted linked list

Traverse the list from the head (or start) node. While traversing,
compare each node with its next node. If data of next node is same as
current node then delete the next node. Before we delete a node, we need
to store next pointer of the node

### 26\. Remove duplicates from an unsorted linked list

Two loops are used. Outer loop is used to pick the elements one by one
and inner loop compares the picked element with rest of the elements.

### 27\. We traverse the link list from head to end.

For the newly encountered element, we check whether it is in the hash
table: if yes, we remove it; otherwise we put it in the hash table.

### 28\. Volatile keyword usage

  - a. For data accessed by different hardware threads or software
    threads locked onto different hardware threads.

  - b. For device registers accessed by kernel and subsystems.

### 29\. Const keyword usage

a. Normal pointer - Both pointer can be modified to point to different
object, and the object pointed to by the pointer can also be modified.

``` code
int *ptr, i;
ptr = &i;
```

b. Constant pointer - The pointer itself can't be modified to point to
any other object once assigned, though the object pointed by pointer
could be modified.

``` code
const int *ptr;
Or
int const *ptr;
```

c. Pointer to constant object - Pointer can be modified to point to any
of the objects, but the object pointed to by the pointer can't be
modified.

``` code
int *ptr;
const int i;
ptr = &i;
```

d. Constant pointer to constant object - Both the pointer and as well
the object it is pointing to, can't be modified.

``` code
const int *const ptr;
int i;
ptr = &i;
Or
const int *ptr;
const int i;
ptr = &i;
```

### 30\. Is given integer power of 2?

``` code
bool isPowerOfTwo (int x)
{
  /* First x in the below expression is for the case when x is 0 */
  return x && (!(x & (x - 1)));
}
```

### 31\. Find next power of 2

``` code
unsigned int nextPowerOf2(unsigned int n)
{
  unsigned count = 0;
  /* First n in the below condition is for the case where n is 0*/
  if (n & !(n&(n-1)))
    return n;
  for(;n != 0; n >>= 1, count += 1);
  return 1<<count;
}
```

### 32\. Rotate bits in given integer by number of bits specified towards left

``` code
int leftRotate(int num, unsigned int num_bits)
{
   return (num << num_bits)|(num >> ((sizeof(int) * 8) - d));
}
```

### 33\. Rotate bits in given integer by number of bits specified towards right

``` code
int rightRotate(int num, unsigned int num_bits)
{
   return (num >> num_bits)|(num << ((sizeof(int) * 8) - d));
}
```

### 34\. Given only a pointer to a node to be deleted in a singly linked list, how do you delete it?

Copy the data from the next node to the node to be deleted and delete
the next node. This solution doesn’t work if the node to be deleted is
the last node of the list.

### 35\. Count number of bits to be flipped to convert A to B

``` code
C = A ^ B;
```

count number of bits in C

### 36\. Multiply a number by 7

``` code
((n<<3) - n)
```

### 37\. Find parity of given integer

An integer is of odd parity if it has odd number of bits set, and is of
even parity if even number of bits are set. Used in cryptography.

``` code
bool getParity(unsigned int n)
{
   bool parity = 0;
   for (;n; parity = !parity, n = n & (n - 1));
   return parity;
}
```

### 38\. Return height of a binary tree

``` code
int height (node *tree)
{
   if (tree == NULL)
      return 0;
   else
      return (1 + max(height(tree->left), height(tree->right)));
}
```

### 39\. Number of elements in the tree

``` code
int elems (node *tree)
{
   if (tree == NULL)
      return 0;
   else
      return (1 + elems(tree->left) + elems(tree->right)));
}
```

### 40\. Count non-leaf nodes in tree

``` code
int nonleaf-elems (node *tree)
{
   if (tree) {
      return (!tree->lptr && !tree->rptr)? 0 : (1 + nonleaf-elems(tree->left) + nonleaf-elems(tree->right));
   else
      return 0;
}
```

### 41\. Tree traversal

  - a. Preorder - root, left, then right.

  - b. Postorder - left, right, then root.

  - c. Inorder - left, root, then right.

### 42\. Shallow copy versus Deep copy

If only another pointer is updated to point to the given memory
location, then it is shallow copy. If contents are all copied, then it
is deep copy. Array copy is always a deep copy. For ex.

``` code
char a[20] = "hello world";
char b[20];
b = a;  /* deep copy */
```

### 43\. Memory layout of C programs

1.  **Text segment** - Often read only to prevent programs from
    accidentally modifying it.

2.  **Initialized data segment** - Global variables, static variables,
    etc. that are initialized by programmer. Further divided into
    'initialized read-only area' and 'initialized read-write area'. For
    ex. 'char s\[\] = “hello world”;' will be kept in read-only area,
    and 'int i = 10;' will be in read-write area.

3.  **Uninitialized data segment** - Also called 'bss' segment. Contains
    all the global variables, static variables, etc. not explicitly
    initialized by programmer. Generally set to 0 (zero) by kernel while
    loading.

4.  **Stack Segment** - Traditionally adjoined to heap area, and grows
    in the opposite direction. Contains at minimum a return address,
    optionally automatic variables, local variables, etc. When stack
    meets heap, corruption happens. 'stack pointer' always points to top
    frame of the stack.

5.  **Heap Segment** - Shared by all shared libraries and dynamically
    loaded modules in the process. Used by malloc()/free() and other
    routines. brk()/sbrk() syscalls used to change size of this area.

### 44\. size command

'size' command in Linux reports sizes (in bytes) of text, data and bss
segments.

### 45\. How many number of processes will be spawned by this program

``` code
#include <stdio.h>
#include <unistd.h>
int main()
{
   fork();
   fork() && fork() || fork();
   fork();
   printf("forked\n");
   return 0;
}
```

``` code
(main)
(main, fork)
(main, fork), (fork, fork)
(main, && fork), (fork, && fork), (fork, && fork), (fork, && fork)
(main, || fork), (&& fork, || fork), (fork, || fork), (&& fork, || fork), (fork, || fork), (&& fork, || fork), (fork, || fork), (&& fork, || fork), 
(main, fork), (|| fork, fork), (&& fork, fork), (|| fork, fork), (fork, fork), (|| fork, fork), (&& fork, fork), (|| fork, fork), (fork, fork), (|| fork, fork), (&& fork, fork), (|| fork, fork), (fork, fork), (|| fork, fork), (&& fork, fork), (|| fork, fork), (fork, fork), (|| fork, fork), (&& fork, fork), (|| fork, fork)
```

### 46\. IBM Questions

1.  How Debugger works ? When you put a breakpoint, how is it enabled?

2.  How a packet type is registered with the kernel?

3.  How a skb is maintained and kept in kernel?

4.  How check for particular bit to be set in \#define without
    arithmetic operators?

5.  When two threads are trying to use two pieces of data with seperate
    locks, how can you ensure that they won't deadlock? – Ans: You need
    to put a constraint that they will alway take a lock on
    lowest/highest address first. For ex: If A & B are 2 threads trying
    to use lock on the data regions C & D. C is at lower address and D
    at higher address. Now if we put the constraint that One thread
    should always try to take a lock in order C-then-D, then the threads
    would never go into deadlock kind of a situation. Or even the oder
    of D-then-C will also work.

6.  How does a packet get transmitted between different layers of
    kernel? How can you send a packet to upper layer? What is
    netif\_rx\_schedule()?

Answer Following:

1.  Memory map of kernel ? No

2.  Modules load and place to see the module parameters - where can I
    see? No

3.  dev\_register() .. what does it do ? No

4.  Interrupt sharing and how is it implemented ? Yes/No

5.  normal lock and spin lock - what is difference ? Yes/No

6.  how can you avoid deadlock ? Yes

7.  calloc for 1 GB ? No

8.  From where does kernel allocate memory for a module ? No

Amazon Questions
================

Explain before coding

Solution: 
	root
left

Binary tree - level
struct node {
	struct node *left;
	struct node *right;
	int val;
};

void print_level(struct node *root, int level, int clevel) {
	if (!root || (clevel + 1) > level) {
		return;
	}

	clevel++;
	if (clevel == level) {
		printf("%d", root->val);
		return;
	}
	
	print_level(root->left, level, clevel);
	print_level(root->right, level, clevel);
	return;
}

http://www.codetohack.com/2013/01/interviewstreet-solutions-pairs.html
http://technicalshowoff.blogspot.in/2012/01/interview-streetstring-similarity.html
http://justprogrammng.blogspot.in/2012/06/interviewstreet-median-challenge.html
http://justprogrammng.blogspot.in/2012/06/interviewstreet-median-challenge-part-2.html
http://maheshmalka.wordpress.com/category/solutions-for-interviewstreet-problem/
http://www.geeksforgeeks.org/forums/topic/a-puzzle-from-interview-street/
http://www.c-program-example.com/2011/10/c-program-to-implement-breadth-first.html

## Trevor questions (Cambium)

File Server
Multiple ~25
Bandwidth ~10 Mbps

Desktop -> Wifi Router -> ISP
Wireshark

telnet abc.com

## Infinera Questions

const int *p;
int const *p;

struct p {
    char a;
    unsigned int b;
    uint32_t *arr;
};

