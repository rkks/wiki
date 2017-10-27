% MAC Layer Research Notes
% Ravikiran K.S.
% January 1, 2006

## Area of Study
1. More robust MAC layer to support better throughput and end-to-end delay.
2. More robust MAC layer to control broadcast and multicast traffic in WSNs and improved broadcast, multicast schemes for WSNs and MANETs.
3. Load-balancing in MAC layer to support realtime application traffic to increase the utility of network resources and network throughput, improve robustness and reliability of the transmission service, and reduce the average transmission delay.
4. Performance evaluation of existing MAC layer protocols and models for WSNs and Ad-hoc wireless networks.

## Details

As with all other protocols, MAC layer protocol layer also defines following
properties:
* Detection of the underlying physical connection (wired or wireless), or the
existence of the other endpoint or node
* Handshaking
* Negotiation of various connection characteristics
* How to start and end a message
* How to format a message
* What to do with corrupted or improperly formatted messages (error correction)
* How to detect unexpected loss of the connection, and what to do next
* Termination of the session and or connection.

It provides addressing and channel access control mechanisms for network nodes
to communicate within a multipoint network.  MAC layer emulates a full-duplex
logical communication channel in a multipoint network with support for unicast,
multicast or broadcast ``[http://en.wikipedia.org/wiki/Media_Access_Control]``. MAC
address makes it possible to deliver data packets to a destination within a
subnetwork (one or several network segments interconnected by repeaters, hubs,
bridges and switches, but not by IP routers; IP router may interconnect several
subnets). Error detection and correction is also taken care by MAC layer.

INPUTS:
    1. Frame from LLC, to be transmitted over the network using Physical Layer.
    2. Frame from PHY to be sent to LLC layer for further processing.
    3. Header and Data parts to be sent and received.
    4. Requests and Acks to be made to the network.

Outputs:
    1. Frame from PHY is sent to LLC layer for further processing
    2. Frame from LLC is transmitted over PHY.
    3. Header and Data parts to be sent are given to PHY and received are given to LLC.

Affecting Factors:
    1. Collisions in network.
    2. Corrupt data packets.
    3. Address conflict.
    4. Load distribution policy in diverse environment.

Objectives of MAC:
    * Efficiency (minimize collision, maximize throughput)
    * Fairness
    * QoS

Channel Access Control Mechanisms:
The channel access control mechanism relies on a physical layer multiplex
scheme. A multiple access protocol is not required in a switched full-duplex
point-to-point networks, such as today's switched Ethernet networks, but is
often available in the equipment for compatibility reasons. An Ethernet network
may be divided into several collision domains, interconnected by bridges and
switches. CSMA/CD is useful only in Ethernet bus network or a hub network.
    1. CSMA/CD
    2. CSMA/CA

Actual Protocols Studied:
    1. ARP
    2. RARP
    3. DARP

IEEE Standards:
    1. 802.2 - LLC
    2. 802.3 - MAC

The LLC sublayer is primarily concerned with:
    * Multiplexing protocols transmitted over the MAC layer (when transmitting)
and demultiplexing them (when receiving).
    * Providing flow and error control

HDLC specifies both MAC functions (framing of packets) and LLC functions
(protocol multiplexing, flow control, detection, and error control through a
retransmission of dropped packets when indicated)

Technical difficulties in MAC design:

    * Hidden terminal problem
    * Exposed node problem
    * Distributed control

MAC design can be formulated as a coloring problem, which is NP-complete.
However, if you can utilize the information that is unique to ad hoc networks,
(e.g., location info obtained by GPS), the complexity may be reduced to
polynomial time.

Transmission range: the range within which signal-to-noise-ratio (SNR) is
greater than or equal to a threshold ``gamma_t`` so that transmitted message can
be correctly received with high probability.  (interference free)

Sensing range: the range within which the signal-to-noise-ratio (SNR) is
greater than or equal to a threshold ``gamma_s`` (typically smaller than ``gamma_t``)
so that a transmitter can withhold its transmission and avoid interfering
ongoing transmissions between another pair of nodes.  Sensing is used in
CSMA/CD (802.11).

Interference range: the range within which transmission from an interferer
makes the signal-to-interference-and-noise-ratio (SINR) of the legitimate
receiver smaller than ``gamma_t``, so that the legitimate receiver cannot
correctly receive the message from the legitimate transmitter.

## MAC Protocols

A high-performance network is characterized by two performance measures
band-width and delay. Traditional network design focused mainly on bandwidth
planning; the solution to network problems was to add more bandwidth. Nowadays,
we have to consider message delay particularly for delay-sensitive applications
such as voice and real-time video. Both bandwidth and delay contribute to the
performance of the network.

MAC layer protocol such as ALOHA [1], SEEDEX [26], MACA [20], MACAW [8] and
IEEE 802.11 [19].

## MAC Papers to Study
1. Quality of Service Routing and Admission Control for Mobile Ad-hoc Networks
with a Contention-based MAC Layer.
2. An improvement to the reliability of IEEE 802.11 broadcast scheme for
multicasting in mobile ad networks.
3. Cost-Aware Capacity Optimization in Dynamic Multi-Hop WSNs
4. MAC-Layer Capture: A Problem in Wireless Mesh Networks using Beamforming
Antennas
5. OCARI: Optimization of communication for Ad hoc reliable industrial networks
6. A Case for an Overlay Routing on Top of MAC Layer for WSN
7. DS/CDMA throughput of multi-hop sensor network in a Rayleigh fading
underwater acoustic channel

1. Quality of Service Routing and Admission Control for Mobile Ad-hoc Networks with a Contention-based MAC Layer
Hanzo,II, L.; Tafazolli, R.

2. An improvement to the reliability of IEEE 802.11 broadcast scheme for multicasting in mobile ad networks
Jiawei Xie; Das, A.; Nandi, S.

3.  MAC-Layer Capture: A Problem in Wireless Mesh Networks using Beamforming Antennas
Choudhury, R.R.   Vaidya, N.
Duke Univ., Durham

4.  OCARI: Optimization of communication for Ad hoc reliable industrial networks
Tuan Dang,   Devic, Catherine

5.  A Case for an Overlay Routing on Top of MAC Layer for WSN
Almamou, Abd al basset   Schiller, Jochen   Labiod, Houda Gnes, Mesut

6.  SOTP: A Self-Organized TDMA Protocol for Wireless Sensor Networks
Yu Wang   Henning, I.   Xiaoyun Li   Hunter, D.

7.  Optimizing Routing Protocols for Ad Hoc Network
Rastogi, S.

8.  Label routing protocol: a new cross-layer protocol for multi-hop ad hoc wireless networks
Yu Wang   Jie Wu

9.  Routing in ZigBee: Benefits from Exploiting the IEEE 802.15.4 Association Tree
Cuomo, F.   Della Luna, S.   Monaco, U.   Melodia, F.

10.  A cross-layer design for passive forwarding node selection in wireless ad hoc networks
Tarique, M.   Tepe, K.E.   Naserian, M.

11.  Cost-Aware Capacity Optimization in Dynamic Multi-Hop WSNs
Suhonen, J.   Kohvakka, M.   Kuorilehto, M.   Hannikainen, M.   Hamalainen, T.D.

12.  DS/CDMA throughput of multi-hop sensor network in a Rayleigh fading underwater acoustic channel
Mar, C.H.   Seah, W.K.G.

13.  DiffQ: Differential Backlog Congestion Control for Wireless Multi-hop Networks
Warrier, A.   Sangtae Ha   Wason, P.   Injong Rhee   Kim, J.H.

14.  TCP support for sensor networks
Braun, T.   Voigt, T.   Dunkels, A.

15.  A SiFT: an efficient method for trajectory based forwarding
Capone, A.   Pizziniaco, L.   Filippini, I.   de la Fuente, M.A.G.

16.  Quality of Service Routing and Admission Control for Mobile Ad-hoc Networks with a Contention-based MAC Layer
Hanzo,II, L.   Tafazolli, R.

17.  Optimal Transmit Power in Wireless Sensor Networks
Panichpapiboon, S.   Ferrari, G.   Tonguz, O.K.

18.  Optimal cross-layer designs for energy-efficient wireless ad hoc and sensor networks
Safwati, A.   Hassanein, H.   Mouftah, H.

19. ECPS and E2LA: new paradigms for energy efficiency in wireless ad hoc and sensor networks
Safwat, A.   Hassanein, H.   Mouftah, H.

20. RWPS: a low computation routing algorithm for sensor networks
Bergamo, P.   Maniezzo, D.   Mazzini, G.

21.  Link layer measurements in sensor networks
Reijers, N.   Halkes, G.   Langendoen, K.

22.  Challenges in multi-hop networks
Rosenberg, C.

23.  A Multiple Disjoint Paths Routing Protocol for Ad Hoc Sensor Networks
Xun Zhou   Yu Lu   Yu Gao

24.  Hop count optimal position based packet routing algorithms for ad hoc wireless networks with a realistic physical layer
Kuruvila, J.   Nayak, A.   Stojmenovic, I.

25.  Real Time Traffic Support in Wireless Sensor Networks
Channa, M.I.   Memon, I.

analytical aspects such as probability, random processes, queueing,

CDMA - Code Division Multiple Access
TDMA - Time Division Multiple Access
OFDMA - Orthogonal Frequency Division Multiple Access

Papers to be Requested:
1. Wireless LAN Comes of Age: Understanding the IEEE 802.11n Amendment
Paul, T.K.; Ogunfunmi, T.
Circuits and Systems Magazine, IEEE
Volume 8, Issue 1, First Quarter 2008 Page(s):28 - 54
Digital Object Identifier   10.1109/MCAS.2008.915504

2. Mobility using IEEE 802.21 in a heterogeneous IEEE 802.16/802.11-based, IMT-advanced (4g) network
Eastwood, L.; Migaldi, S.; Qiaobing Xie; Gupta, V.
Wireless Communications, IEEE
Volume 15, Issue 2, April 2008 Page(s):26 - 34
Digital Object Identifier   10.1109/MWC.2008.4492975

3. IEEE 802.11s: WLAN mesh standardization and high performance extensions
Hiertz, G.R.; Yunpeng Zang; Max, S.; Junge, T.; Weiss, E.; Wolz, B.
Network, IEEE
Volume 22, Issue 3, May-June 2008 Page(s):12 - 19
Digital Object Identifier   10.1109/MNET.2008.4519960

4. Cognitive Wireless Mesh Networks with Dynamic Spectrum Access
Chowdhury, K.R.; Akyildiz, I.F.
Selected Areas in Communications, IEEE Journal on
Volume 26, Issue 1, Jan. 2008 Page(s):168 - 181
Digital Object Identifier   10.1109/JSAC.2008.080115

5. Cognitive Wireless Mesh Networks with Dynamic Spectrum Access
Chowdhury, K.R.; Akyildiz, I.F.
Selected Areas in Communications, IEEE Journal on
Volume 26, Issue 1, Jan. 2008 Page(s):168 - 181
Digital Object Identifier   10.1109/JSAC.2008.080115

## Questions Addressed in Distributed Computing:
1. Scheduling and Resource Management in Realtime Distributed Computing.
2. Service Oriented Systems for Dynamic Distributed Computing Environments - Security issues (Information Transfer).
3. Dynamic Distributed Computing for devices and Applications - Optical Networks.
4. Self Organizing and Stabilizing systems - Virtualization, Collaborative Applications, Grids.

Networking:
1. Scaling, reliability, throughput and better quality in Wireless Networks - Ad-hoc Wireless networks.
2. Cooperative Spectrum sharing, 4G networks, Next Generation Reconfigurable Wireless networks - OFDMA.
3. MAC Protocol for Ad-hoc Wireless and Sensor Networks.
4. QoS, Resource management, Cross layer wireless network adaption - Heterogeneous Wireless networks.

5. Position independent and reliable routing for Wireless Sensor Networks. - QoS & Energy Aware
