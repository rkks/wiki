% Network Analytics
% Ravikiran K.S.
% January 1, 2006

## Network Analytics

Communication Network is a means to connect two end-points. From Telco-to-ISP,
packet-to-circuit switched, goal is to transfers bits from one node to another.
Analytics is the discovery and communication of meaningful patterns in data.
Simultaneous application of statistics, operations research and computer
programming.

Network Analytics is ability to look inside network, collect data, retrieve
meaningful patterns, and present them for guided decision making.

Overlay network is a virtual network built atop of another network. Example,
Internet was initially built as an overlay over telecom networks. Other
examples are
    - iSCSI (SCSI over TCP/IP), FCoE (FC over Ethernet), FCIP (FC over IP),
MPLS (Eth over ATM), P2P networks (connected over Internet)

Old days, Human were primary users of network. They connected to each other and
to storage arrays using PCs for exchange of info. So, network elements provided
- Physical port level statistics for utilization levels, etc.
- Router/switch level statistics for macro utilization information.

New technologies: Virtualzation, Cloud, Application Migration

Main questions:
* Who is talking to who? How much bandwidth are they taking? Is it consistent?
* Is there a pattern in network usage among user groups? (timed/burst/pkt-size)
* Is there a way to know if we're running out of resources on network?(predict)
* Is there a way to correlate different incidents to triage system easily?
* Is there a way to profile resource usage per Application, per VM? 
* How can I do End-to-end debugging/troubleshooting of switch fabric with ease
* How can I do proactive diagnosis and prevention of network slowdown (due to
congestion/slow-drain/storm/etc)

Mutliple types of data can be collected from network:
- Statistics: Switch/LC/Port (physical), Application/Algo/Layer level (logical)
- Flow info: L4 flows, overlay flows, application flows
- Health info: health of switch/lc/port, health of connected nodes

- Logical flow level statiscs for resource utilization graph per connection
- Logical stats per App, per VM (Apps can migrate from one VM to other easily)

Methods to collect data:
* SNMP
* CLI
* WEB (DMTF REST API's, VMWare vCenter)
* Bump-on-Wire traffic analysis
* Probe (health check of links, adapters, conf conflicts, login/logouts,
utilization water-mark, under-utilized hardware)

Digital Analytics with Google: Internet available at click of button, Mobile to
connect each and everyone, Cloud provides cheap/infinite hardware


Conclusion:
Harsha's analytics design guide caters to most of these requirements. An NPU
will be added as a co-processor to both LC and SUP cards. Any interested data
(SCSI controls, FC_OTHER header) will be forwarded to this AXE NPU (from LSI)
using ACL rules for further processing. To keep the load of processing packets
limited, these packets are stripped after header length and forwarded to AXE
NPU. Accumulation of such data happens at LC levels and polled by SUP at
regular intervals. On SUP, intelligent algorithm consolidates these inputs and
provides as meaningful information to the DCNM tool. The job of querying
servers/VMs/racks is to be taken care of by DCNM tool. DCNM is also responsible
for pictorial representation of such data to the end user.

