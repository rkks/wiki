% Network as a Service
% Ravikiran K.S.
% January 1, 2006

# Network as a service (NaaS) - Cisco way

Today internet is offered as a generic service for which people have to pay a
rental regardless of type of service/application they require. People also need
to compromise on quality of their service because internet is not tailored to
their usage requirements. Usage requirement of an online gamer is quite different
from that of a stock broker using internet. On demand video throws different
challenges than uninterrupted tunnel over WAN.

Instead, internet could be offered as value packs of different tailored services
User can pick & choose based on his requirements.

Implementation would require:
* Not every stream type is channelized through same path of connectivity
    - Today each router decides best path regardless of traffic type
* Tailored virtual paths that enhance catered service quality
    - Specialized hardware to guarantee data flow requirements
* Central orchestration entity to direct traffic flows appropriately

Today's implementations of NaaS are not standardized.

## Details
Today, ISPs offer internet as a simple connectivity, where one gets hooked onto
internet back-bone where they can access various services provided regardless.

With advance of application delivery mechanism, current applications have varied
demands. Some applications merely consume more bandwidth (ex. p2p), whereas video
applications require continuity along with bandwidth, some applications do not
consume bandwidth rather need quality of connection to be good (vpn).

As SPs do not distinguish and tailor network according to application needs,
they are depending on external mechanisms of throttling certain types of traffic
to certain QoS/bandwidth. These CoS/QoS policies are static in several networks
and dynamic at different times of day with large SPs.

Enterprises are following a different route, of putting intelligence in the wifi
router, which allows/denies certain type of traffic into the enterprise network

Analogous, earlier mobile operators used to provide monthly packs:
200 sms, 60min national/local calls, 500mb internet, 1 caller tune, all for INR 999

A person had to pay for each service, regardless whether they use it or not.
Now they are providing packs: sms pack, internet pack, caller pack. You select
package you want and only pay for that.

Similarly, today people need to pay single internet bill, regardless of their
usage pattern. Instead internet can be offered as value packs, where network
is tailored to the needs of customer. For example, if traffic is catagorized as
audio/video, games, p2p, vpn, regular internet, the requirements would be:

p2p: high bandwidth only
audio/video: high bandwidth with need for continuity
games: high bandwidth with need for p2p relays
vpn: low bandwidth with need for continuous connectivity

There needs to be a central entity that orchastrates the network requirements
given the requirement of customer. This entity can configure various network
attributes dynamically based on user profile. Certain requirements may need the
specialized hardware to provide quality in those areas like video/games. These
will fuel the need for specialized hardware and its adoptability.


Other Implementations
Aryaka NaaS : http://www.aryaka.com/products/wan-solutions/
    - Different. Tries to have private lease lines to ensure quality
Aerohive: http://www.aerohive.com/products/cloud-services-platform/network-service-naas-subscription
    - Different. VPN and cloud based virtualized services
