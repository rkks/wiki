% Code Review CheckList
% Ravikiran K.S.
% January 1, 2006

## Code review checklist

* Functional impact
    * Which modules are affected; which functionality is impacted.
    * Which modules & functionality is consumed by the changed code.
    * Which modules/functionality is using it.
    * Side effects.

* Architecture
    * Scalability
    * Stability
    * Functionality
    * Robustness

* Technicalities
    * Generic
        * First pass: logic. Correctness. What does it (tries to) do.
            * Side effects.
            * Why is code there?
        * Second pass: error handling & odd conditions.
            * Side effects.
            * Are returned codes checked. Can the code handle them.
            * Arguments:
                * Border/out of bounds/strange values handled.
                * Arg type: convenient?
                * Where is the data coming from? Three levels upstream.
                * Where are the outputs going? Three levels downstream.
                    * What are the possible/permitted outputs? Are all handled downstream?
            * Variables: initialized?
            * (over/under)flows:
                * buffers.
                * integral types.
            * Asserted invariants & assumptions.
            * Loops: off-by-one? infinite?
            * Recursion: base case?
    * Third pass: thread safety.
        * Localization?
        * Duplicate information?
    * Math:
        * Small values processed first?
        * NaN checks/std::isnan & friends.

    * C
        * return: resource handling.
        * Use the N functions (ex. s'n'printf vs. sprintf).
        * printf & friends: do formatters match type and number of arguments?
        * Non re-entrant/non thread-safe API
        * Unsafe/race condition afflicted API ( mktemp() )
        * enums:
            * all cases handled?
            * default returns/logs/throws error
        * Unspecified/undefined/implementation-defined/non-standard behavior or extensions

    C++
        * Fourth pass: exception handling:
            * In destructors.
            * Exception catching boundaries?
            * Are resources RAII guarded.
        * Fifth pass: performance.
            * C++11: perfect forwarding & temporaries.
            * How does it scale?
        * Initialized data members.
        * The big 6 (default constructor, destructor, copy/move constructors/operators).
        * Virtual destructor?
        * Resource handling: RAII.
        * Where is created & destructed? Lifetime management?
        * Thorough const & volatile.
        * Are predicates stateless.
        * Use size_t for std::string positions.
        * Why casts?

    * Platform specific
        * POSIX/SUS
            * Interrupted calls.
            * Spurious wake-ups.
            * Calls known to have race conditions.
