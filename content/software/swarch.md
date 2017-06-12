% Software Development Best Practices - Architecture
% Ravikiran K.S.
% January 1, 2006


1.  Basic function of an architecture needs to be well understand and
    well stated.

2.  Time To Market (TTM) is critical for any business to operate and get
    an edge over competition.

3.  The simplest design that meets the requirements is the best and most
    desirable design. Simple means, nothing more (over-engineer),
    nothing less (under-engineer).

4.  The architectural limit of any hardware is generally the maximum
    load that the said hardware could carry in a stable fashion given a
    “perfect” implementation of the software. The design goal for all
    softwares is to reach this architectural limit. However, perfection
    in every possible dimension is not a goal because it is by
    definition unattainable.

5.  Decreased inter-card, inter-node, inter-process communication to
    improve the performance. This can be realized by minimum
    inter-dependency. But this does not dismiss the modularity as a
    goal. Rather that the performance takes a precedence whenever
    benefits of modularity are not clearly and greatly exceeding the
    promise of a better performance. At the end of the day, customer
    pays for the performance.

6.  Fewer number of processes and threads, but large enough to cater to
    all the processing needs. Each process to have single, well defined
    responsibility. All the waiting/blocking calls summed up into a
    single thread. Minimum code on critical path.

7.  No duplication of state/data, except for redundancy. However, in the
    choice of memory versus processing speed in fastpath, processing
    speed takes preference.

8.  Avoid dynamic memory allocation at run-time. Take statistical
    feedback's and feedback's from user to tune the memory
    configurations. User feedback takes precedence.

9.  Architecture needs to support variety of hardware and operating
    systems. Abstractions and plug-gable software architecture can be
    used to achieve this. Portability is to be provided without
    sacrificing on the performance.

10. High availability is not a mandate, but a feature that can be turned
    off and on. HA should not cost performance. System needs to be
    highly available, at NIC, node, and system levels. Flexibility in
    design is required to provide limiting impact on network planning,
    however performance impact takes precedence.

11. Designs should be made extensible to accommodate the easy addition
    of the new features. Create extensible API's using flags to be
    passed to each API. This will minimize the need for api\_func(),
    api\_func2(), api\_fun3(), etc.

12. Data structures design defines the limit of software performance.
    Couple the information that are naturally accessed together,
    seperate those that aren't dependent.

13. Have minimum locks. Allow atomic locks on smallest region, big
    enough to protect data. Nested locks are evil, when used in
    out-of-order can cause deadlocks. Use spin-locks on when required.

14. A good and complete state machine provides the best (and earliest)
    insight into component and system behavior and performance.

15. A good machine provides best responsiveness to its user. Minimum
    startup time, maximum responsiveness are keys to easy system
    adoption by users.

16. Dynamic restart-ability of the process becomes a big plus in
    providing software watchdog and self-healing capabilities to the
    system.

17. Visibility into the system at run-time is the most desired at
    deployments. Visibility at runtime configuration, cpu utilization,
    core allocation, memory usage, memory consumption statistics,
    network usage statistics, memory allocation error statistics,
    network error statistics, overall system state, centralized logs,
    debug logs, and ability to logon into and inspect any hardware is
    key to diagnostics.

18. Debugg-ability of the system defines its robustness. Debugger
    availability, coverage tools, core file generation facility, memory
    profiling tools, call flow analysis tools, in-circuit tests,
    register dumping tools, register configuration tools, hardware
    access tools, diagnostic self-tests are required to ease the mass
    production of the system.

19. Keeping the system cost to minimum without sacrificing on the
    performance and quality is the goal of every product development. A
    system is both hardware and supporting software. So, due
    considerations must be given to identify the hardware components
    that are not required, the optimizations in hardware + software that
    can together reduce the need for extra hardware components, etc.

20. Performance Per Watt is the industry standard to measure
    performance. So, the electrical qualities of the system should meet
    the EC, WEEE and other standards.

21. The network connects for data traffic, management traffic, and
    replication traffic must be independent of each other so that they
    do not affect the performance of each other.

22. Some components like Fabric Interface Cards, TCAM cards, etc. can be
    made as add-on cards so that in future the component could be
    upgraded easily to latest technology.

