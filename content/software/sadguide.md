% System Analysis Design Guidelines
% Ravikiran K.S.
% January 1, 2006


  - System should be asynchronous which is well guarded using
    state-machine(s).

  - Different parts of the system should be independently update-able.

  - Different features of the system should be well documented and
    searchable.

  - Design of the system should make access to different features be
    very intuitive.

  - System should have no more than what is necessary for it to work.
    Rest all could be optional bundles.

  - System should have statistics about different parts of the system
    that it deals with.

  - Tracing, logging, monitoring, and debugging should be painless in
    the system.

Basics of Unix Philosophy 18 Rules of Success:  

1.  Rule of Modularity: Write simple parts connected by clean
    interfaces.

2.  Rule of Clarity: Clarity is better than cleverness.

3.  Rule of Composition: Design programs to be connected to other
    programs.

4.  Rule of Separation: Separate policy from mechanism; separate
    interfaces from engines.

5.  Rule of Simplicity: Design for simplicity; add complexity only where
    you must.

6.  Rule of Parsimony: Write a big program only when it is clear by
    demonstration that nothing else will do.

7.  Rule of Transparency: Design for visibility to make inspection and
    debugging easier.

8.  Rule of Robustness: Robustness is the child of transparency and
    simplicity.

9.  Rule of Representation: Fold knowledge into data so program logic
    can be stupid and robust.

10. Rule of Least Surprise: In interface design, always do the least
    surprising thing.

11. Rule of Silence: When a program has nothing surprising to say, it
    should say nothing.

12. Rule of Repair: When you must fail, fail noisily and as soon as
    possible.

13. Rule of Economy: Programmer time is expensive; conserve it in
    preference to machine time.

14. Rule of Generation: Avoid hand-hacking; write programs to write
    programs when you can.

15. Rule of Optimization: Prototype before polishing. Get it working
    before you optimize it.

16. Rule of Diversity: Distrust all claims for “one true way”.

17. Rule of Extensibility: Design for the future, because it will be
    here sooner than you think.

18. Rule of Open Systems: Use open standards and specifications wherever
    possible.

\<note important\>PS: Write programs that do one thing and do it well.
Write programs to work together.\</note\>

