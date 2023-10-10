# EtherChannel
EtherChannel groups multiple physical interfaces together to act as a single interface. Spanning Tree Protocol treats this group as a single interface.

Some other names for EtherChannel are: Port-Channel or LAG (Link Aggregation Group)

## Load Balancing
EtherChannel load balances based on 'flows'. A flow is a communication between two nodes in the network. Frames in the same flow will be fowarded using the same physical interface. If frames in the same flow were forwarded using different physical interfaces, some frames may arrive at the destination out of order. You can change the inputs used in the interface selection calculation; source MAC AND/OR destination MAC address; source IP AND/OR destination IP.

## Why it is needed...
EtherChannel is needed for the following reasons:

**Increased bandwidth:** EtherChannel can be used to increase the bandwidth of a link between two devices. This can be useful for applications that require high bandwidth, such as streaming video or file sharing.
**Redundancy:** EtherChannel can be used to provide redundancy for a link between two devices. If one of the links in the EtherChannel fails, the other links will continue to carry traffic. This can help to improve the reliability of a network.
**Load balancing:** EtherChannel can be used to load balance traffic across multiple links. This can help to improve the performance of a network by reducing congestion on individual links.

## Configuration
Member interfaces must have matching configurations.
- same duplex (full/half)
- same speed
- same switchport mode (access/trunk)
- same allowed VLANs/native VLAN (for trunk interfaces)

If an interface's configurations do not match the others, it will be excluded from the EtherChannel.

There are three methods of EtherChannel configuration on Cisco switches:

### PAgP (Port Aggregation Protocol)
- Cisco Proprietary
- Dynamically negotiates the creation/maintenance of the EtherChannel

### LACP (Link Aggregation Control Protocol)
- Industry standard protocol (IEEE 802.3ad)
- Dynamically negotiates the creation/maintenance of the EtherChannel. (like DTP does for trunks)

### Static EtherChannel
- A protocol isn't used to determine if an EtherChannel should be formed.
- Interfaces are statically configured to form an EtherChannel.

Up to 8 interfaces can be formed into a single EtherChannel (LACP allows up to 16, but only 8 will be active, the other 8 will be in standby mode, waiting for an active interface to fail)

# Layer-3 EtherChannel
Layer-3 etherchannels can be created in multi-layer switches. The benefit of a layer-3 etherchannel is that ip addresses help to routing and thus we avoid STP from blocking ports to prevent layer 2 broadcast storms that would happen in a circular etherchannel network topology.
