# IPv4 Header

The IPv4 header is a 20-byte header that is used to encapsulate IP packets. It contains information about the packet, such as the source and destination IP addresses, the length of the packet, and the type of service.

The IPv4 header is divided into the following fields:

- Version: This field indicates the version of the IP protocol.
- Internet Header Length (IHL): This field indicates the length of the IPv4 header in 32-bit words.
- Differentiated Services Code Point (DSCP): used for quality of services (QOS)... used to prioritize delay-sensitive data (streaming, voice, video, etc.)
- Explicit Congestion Notification (ECN): provides end-to-end notification of network congestion without dropping packets.
- Total Length: This field indicates the length of the IP packet in bytes, including the L3 header and L4 segment.
- Identification: If a packet is fragmented due to being too large, this field is used to identify which packet the fragment belongs to.
- Flags: Used to control/identify fragments. This field indicates whether the packet is fragmented and how to reassemble it.
- Fragment Offset: This field indicates the position of the fragment in the original unfragmented IP packet. Allows fragmented packets to be reassembled even if they arrive out of order.
- Time to Live (TTL): This field indicates how long the packet should be allowed to live on the network. Each time a packet arrives at a router, the router decreases the TTL by 1. The recommended TTL is 64. It's used to prevent infinite loops
- Protocol: This field indicates the protocol of the encapsulated L4 PDU in the IP packet, whether TCP, UDP, ICMP, OSPF (dynamic routing protocol), etc.
- Header Checksum: This field is used to verify the integrity of the IP header. Not the encapsulated data.
- Source IP Address: This field contains the IP address of the sender of the IP packet.
- Destination IP Address: This field contains the IP address of the recipient of the IP packet.
- Options: rarely used, however, if the IHL field is greater than 5, it means that options are present.

The IPv4 header is essential for the routing and delivery of IP packets. It contains the information that routers need to determine how to forward the packet to its destination.

# IPv4 Address Classes
IPv4 address classes are a way of classifying IPv4 addresses based on their first few bits. This classification is used to determine how the address can be used. There are five IPv4 address classes, the prefix length for each network class is as follows:

| Class   |	Prefix length | Description                                                                          |
| --------|---------------| ------------------------------------------------------------------------------------ |
| Class A |	8 bits        | have the first bit set to 0. They can be used to represent up to 16,777,216 hosts.   |
| Class B |	16 bits       | have the first two bits set to 10. They can be used to represent up to 65,536 hosts. |
| Class C |	24 bits       | have the first three bits set to 110. They can be used to represent up to 256 hosts. |
| Class D |	Multicast     | are multicast addresses and are used to send data to a group of hosts.               |
| Class E |	Experimental  | are reserved for experimental purposes.                                              |

Class A IP address have 0, and 127 reserved. 

IPv4 address classes are no longer used in modern networks, as they are not efficient for allocating and managing addresses. Instead, CIDR (Classless Inter-Domain Routing) is used to allocate IPv4 addresses. CIDR allows for more flexibility in allocating addresses and can help to reduce the number of wasted addresses.

# Loopback Addesses
Loopback addresses are special IPv4 addresses that are used to test and troubleshoot network connectivity. The Loopback Address range is 127.0.0.0 - 127.255.255.255. Loopback addresses are not routed on the network, so they can be used to test the internal components of a device, such as the network interface card (NIC) and the TCP/IP stack.

The loopback address for IPv4 is 127.0.0.1. Any packet sent to the loopback address will be looped back to the device that sent the packet.

# Netmask
A netmask is a 32-bit number that is used to identify the network portion of an IP address. It is used by routers to determine how to route packets to their destination. Netmask is another way to write the prefix of an ip address... for example, Class A or /8 is 255.0.0.0, Class B or /16 is 255.255.0.0, and Class C or /24 is 255.255.255.0

The netmask is a combination of 1s and 0s, where 1s represent the network bits and 0s represent the host bits. The number of 1s in the netmask determines the prefix length.

For example, a netmask of 255.255.255.0 has a prefix length of 24, which means that the first 24 bits of the IP address identify the network and the remaining 8 bits identify the host.

Netmasks are used in conjunction with IP addresses to determine whether two devices are on the same network. If two devices have the same network address and netmask, then they are considered to be on the same network.

Netmasks are also used in conjunction with subnetting to create smaller networks within a larger network. Subnetting allows for more efficient use of IP addresses and can help to improve network security.

A network address is a unique identifier for a group of devices that are connected to a network. It is used by routers to route packets to the correct network.

# Network Address
If the host portion of an IP address is all 0's, (binary)... decimal 0, then it is a network address. The network address cannot be assigned to a host. For example, 192.168.1.0/24.

# Broadcast Address
If the host portion of an IP address is all 1's (binary)... decimal 255, then it is a broadcast address. The broadcast address cannot be assigned to a host. For example, 192.168.1.255/24.