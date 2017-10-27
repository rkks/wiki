% Zero Index - A Technology Retrospective
% Ravikiran K.S.
% January 1, 2006

## Zero Index - A Technology Retrospective

If you begin to count from 0 on, its very probable that you are a computer
programmer.

It is a common practice to have 0 as start index in computer world, works as
machine expects it. In computer terms, a base address is not only a pointer to
beginning of certain location in memory, but also a place holder for the first
element in that memory area. Hence to avoid any confusion, people have used
convention to begin with zero and count till (N - 1) elements. Sounds good.

Some people advocate similar approach to computer based product development
methodology. Top most argument is, "what you build, you have complete control
over it". But it need not be the case. At its extreme, you'll have to develop
your own hardware modules right from transistor on, and software right from
bootloader onwards. Hoping that you have all the time in the world for this job
alone, still It takes way too long a time for a person to understand all the
technologies, comprehend their interconnection, and use that knowledge to
develop something useful. Even if you take all that pain and develop something
over ages, it wouldn't be completely bug-free/perfect, not in-line with
industry best practices/standards, and certainly not competable with then
existing technologies and products in the field merely because world also would
have progressed with time.

In gist, it is counter-productive, time-consuming, and unrealistic to develop
something from ground zero. A simple tiny web-server on a mere small embedded
PSOC would have boatload of hardware and software elements like:
* hardware: processor, cache, RAM, ROM, PCB, buses, ISA, PCI, USB, UART, I/O
Peripherals, VGA, Power Rails, and so on
* software: bootloader, kernel, device drivers, file system, utility commands,
libraries, UIs, Web Server, DB, and so on.

Looks simple? Not quite. You begin to dig deeper in any of the top bullets,
there are several layers of complexity, information, and expertise involved in
it. For more on this, read Paul Jean's article.

I am sure those radicles don't mean by extreme as explained above. They are
dead against to using any framework, utility, middlewares atop of base system.
Few liberals extend it to allow anything that complies by the standards.
However, in reality it has got to be more liberal than that. Reasons are
simple:
* Standards are implemented only when there is a requirement for
inter-operability
* It takes time and effort to understand any technology considerably well and
incorporate best-practices
* Companies don't want to reinvent the wheel OR rely on single author of code
OR take ages to develop a product

Pros and Cons of using a framework/library/middleware are below.

Pros:
* Accelerates the development time
* Incorporates industry best practices to certain extent
* Provides security to certain extent

Cons:
* Provides lesser control to application
* Difficult to debug and modify on tougher issues
* Cost of migrating from one framework to another is high

All in all, I think, the balanced view would be to do the cost-benefit analysis
of framework/library/middleware options available at hand. Consider following
checklist (not comprehensive, add your own requirements to it) while evaluating
any option.

* Learning Curve - Availability of public information, Quality of documentation
and community support
* (De)Integration effort - Time-effort requirements in add/remove of framework
to existing code
* Debuggability - Ease of updating configuration, application monitoring,
runtime traces and logs

Only if something doesn't come in the way of development of actual application,
it is worth using.

For CodeIgniter, I found this. CodeIgniter is right for you if:
    You want a framework with a small footprint.
    You need exceptional performance.
    You need broad compatibility with standard hosting accounts that run a
variety of PHP versions and configurations.
    You want a framework that requires nearly zero configuration.
    You want a framework that does not require you to use the command line.
    You want a framework that does not require you to adhere to restrictive coding rules.
    You do not want to be forced to learn a templating language (although a
template parser is optionally available if you desire one).
    You eschew complexity, favoring simple solutions.
    You need clear, thorough documentation.
