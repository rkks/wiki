% D-left hashing
% Ravikiran K.S.
% January 1, 2006

# D-left Hashing Algo
===================
Hashing function does basic task of mapping a universe of numbers into a fixed
range of buckets with minimum/no collision. Regardless of the quality of hashing
function used, there will be collisions. Collisions are handled by having linked
list at each hash bucket. Higher the collision rate, larger the size of linked
list. And correspondingly larger latency in retrieval time it will be, because 
linked lists do not exhibit good locality for cache. Additionally, the overhead
of maintaining linked list pointers is incurred.

Open Address hashing has been long used in routers to do IP/MAC address lookups
at much high speeds. While performing at line-rate, the above said latencies can
cause issues.

Alternatively, fixed-size buckets holding assumed max elements per bucket can
be devised. The size of such memory could also be aligned to size of cache. But
it has 2 problems
- It fails if any bucket has more hits than accounted MAX elements. Leads to the
bucket over-flow. Re-hashing the key can be a solution, though not optimal.
- Another issue is, if only fewer buckets are populated, a lot of memory goes
wasted.

Two Hash Functions
------------------
Better alternative is to have 2 hash functions. Run key through 2 hash functions
to obtain 2 indexes. Then examine both buckets to see how many keys are stored
in each. Put the new key into a bucket with lower number of keys. Tie-breaking
can be done by choosing left/first bucket always.

This decreases the likelihood of bucket overflow as it increases the number of
element holders available for particular hash index by 2.

http://www.drdobbs.com/architecture-and-design/good-hash-tables-multiple-hash-function/184405046
