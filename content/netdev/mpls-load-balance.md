% Parsing MPLS packets for Load Balancing
% Ravikiran K.S.
% January 1, 2006

Parsing MPLS packets is not known to Switch, so we get in first 32 bytes of the
packet and parse it ourselves to identify if the packet has relevant data. For
an MPLS. We parse MPLS packet till the end of stack bit is found. This is to
identify how deep the IP address can be found. Then, depending on depth of MPLS
tags, we parse IP address field and load balance packet based on IP address.
If number of MPLS labels exceeds 3, we cannot support it because Fulcrum can
read only 32 bytes of a packet. If there are more than 3 labels in MPLS, the
packet doesn't hit any of the criteria, so it is left untouched and is not
load balanced.
