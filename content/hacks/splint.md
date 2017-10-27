% Lint Code Inspection Tool
% Ravikiran K.S.
% January 1, 2006

## Very Good Lint Example

[root@xe30-1][~/tmp]$ cat lint.c
main(void)
{
        int i;

        for (i = 0; i < 10l; i++)
        {
                printf("%d\n",i);
        }
}

===================================
(gdb) disassemble
Dump of assembler code for function main:
0x08048368 <main+0>:    push   %ebp
0x08048369 <main+1>:    mov    %esp,%ebp
0x0804836b <main+3>:    sub    $0x8,%esp
0x0804836e <main+6>:    and    $0xfffffff0,%esp
0x08048371 <main+9>:    mov    $0x0,%eax
0x08048376 <main+14>:   add    $0xf,%eax
0x08048379 <main+17>:   add    $0xf,%eax
0x0804837c <main+20>:   shr    $0x4,%eax
0x0804837f <main+23>:   shl    $0x4,%eax
0x08048382 <main+26>:   sub    %eax,%esp
0x08048384 <main+28>:   movl   $0x0,0xfffffffc(%ebp)
0x0804838b <main+35>:   cmpl   $0x64,0xfffffffc(%ebp)
0x0804838f <main+39>:   jg     0x80483ab <main+67>
0x08048391 <main+41>:   sub    $0x8,%esp
0x08048394 <main+44>:   pushl  0xfffffffc(%ebp)
0x08048397 <main+47>:   push   $0x8048494
0x0804839c <main+52>:   call   0x80482b0
0x080483a1 <main+57>:   add    $0x10,%esp
0x080483a4 <main+60>:   lea    0xfffffffc(%ebp),%eax
0x080483a7 <main+63>:   incl   (%eax)
0x080483a9 <main+65>:   jmp    0x804838b <main+35>
0x080483ab <main+67>:   mov    $0x0,%eax
---Type <return> to continue, or q <return> to quit---
0x080483b0 <main+72>:   leave
0x080483b1 <main+73>:   ret
End of assembler dump.

=================================
Splint 3.1.1 --- 15 Jun 2004

lint.c: (in function main)
lint.c:19:14: Operands of < have incompatible types (int, long int): i < 10l
  To ignore type qualifiers in type comparisons use +ignorequals.
lint.c:23:2: Path with no return in function declared to return int
  There is a path through a function declared to return a value on which there
  is no return statement. This means the execution may fall through without
  returning a meaningful result to the caller. (Use -noret to inhibit warning)

Finished checking --- 2 code warnings
