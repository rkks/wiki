% Garbage Collection
% Ravikiran K.S.
% January 1, 2006

# Garbage Collection Algo
1. Although the programmer still allocates data structures, they are never
explicitly freed. Instead, they are "garbage collected" when no live references
to them are detected. This avoids the problem of having a live pointer to a
dead object. So, GC "keeps" eye on amount of memory allocated by program and
tries to free memory of unreferenced objects in "good" (when your program does
not consume much CPU) time. You do not need to do free() operation like in C++.
GC does it for you. Most of memory leaks happen due to bugs in Java itself
rather than bad programming (happens also :-)) In a large application, a good
garbage collector is more efficient than malloc/free

2. GC makes heap defragmentation (merges the small pieces of free memory into
one big piece) that increases performance on the fly. It is difficult to do
such thing easy in most of programs written on C++.

3. Your time! If you have fast enough CPU and good GC you will save a lot of
time. Manual tuning of a millions pieces of code like malloc/free will take so
much time that increases the cost of project dramatically! Let say like this
if you have slow CPU and small program then C++ with malloc/free is more
efficient. If you have big one - rely on GC!

4. Security issue: Java programmers cannot crash the JVM by incorrectly freeing
memory Main disadvantage is that GC adds overhead that can affect performance.
In some real time it is critically important that no GC-ing will run in
definite periods of time.
