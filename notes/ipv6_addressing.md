# About IPv6
## Why IPv6?
- The main reason is that there simply aren't enough IPv4 address available.
- There are 2^32 (4,294,967,296) IPv4 addresses available.
- VLSM, private IPv4 addresses, and NAT have been used to conserve the use of IPv4 address space.
- IANA (Internet Assigned Numbers Authority) distributes IPv4 address space to various RIRs (Regional Internet Registries), which then assign them to companies that need them.

![RIR Map](images/Regional_Internet_Registries_world_map.svg.png)

## Hexadecimal review
The following prefixes denote the type of numbering system
- binary : **0b** - 0, 1
- decimal: **0d** - 0, 1, 2, 3, 4, 5, 6, 7, 8, 9
- hexadecimal: **0x** - 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, A, B, C, D, E, F

Note that each hexadecimal digit includes 4 bits of information, that is, hexadecimal 0 is 0000 in binary, and hexadecimal F is 1111 in binary.
## What about IPv5?
- 'Internet Stream Protocol' was developed in the late 1970s, but it was never actually introduced for public use.
- It was never called 'IPv5', but it used a value of 5 in the version field of the IP header.
- So, when the successor to IPv4 was being developed, it was named IPv6

# IPv6 Addressing
- An IPv6 address is 128 bits. Every additional bit doubles the number of possible addresses, and in IPv6 this amounts to about 3.4 x 10^38 possible addresses.
- The address is written in hexadecimal, for example... 2001:0DB8:0000:0000:0000:0000:C92D:09BD

## IPv6 Shorthand Notation
- To abbreviate ipv6 addresses, leading zeros can be removed as such... 2001:DB8:0:0:0:0:C92D:9BD
- Additionally, consecutive quartets of all 0s can be replaced with a double colon... 2001:0DB8:0000:0000:0000:0000:C92D:09BD, can be written as 2001:DB8::C92D:9BD
- consecutive quartets of 0s can only be abbreviated once in an IPv6 address.
- In modern IPv6 Address representation
   - leading 0s MUST be removed.
   - double colon (::) MUST be used to shorten the longest string of all-0 quartets.
   - If there are two equal-length choices for the ::, use the :: to shorten the left-most quartet of zeros
   - Hexadecimal characters 'a', 'b', 'c', 'd', 'e', and 'f' MUST be written in lowe-case, NOT upper-cast A B C D E F

## IPv6 Configuration
Typically, an enterprise requesting IPv6 addresses from their ISP will receive an /48 block.
- Typically, IPv6 subnets use a /64 prefix length. That means that an enterprise has 16 bits to use to make subnets.
    
    For example: 2001:0DB8:8B00:AAAA:0000:0000:0000:0000:0001/64
    - 2001:0DB8:8B00 is the 48-bit 'global routing prefix'
    - AAAA is the 16-bit 'subnet identifier
    - The remaining 0000:0000:0000:0000:0001 is the 64-bit host portion of the address.
    
    To find the IPv6 prefix, change all bits in the host portion of the address to 0. 
    - In the case above 2001:0DB8:8B00:AAAA::/64

## Modified EUI-64
The Modified EUI-64 (Extended Unique Identifier 64) is a method used to create IPv6 interface identifiers (host portions of IPv6 addresses) from the 48-bit Media Access Control (MAC) addresses used in Ethernet networks. It is primarily used for stateless address autoconfiguration in IPv6.

### **The Modified EUI-64 Process:**

1. **MAC Address:**
   - The MAC address is a 48-bit (6-byte) identifier assigned to network interfaces for communication on a physical network. It is usually represented as 12 hexadecimal digits. 

2. **Insertion of FF:FE:**
   - The 48-bit MAC address is extended to 64 bits by inserting the hexadecimal value "FF:FE" in the middle of the MAC address. 

3. **Bit Manipulation:**
   - The seventh bit (the universal/local bit) in the MAC address is inverted. This means if it is 0, it becomes 1, and if it is 1, it becomes 0. 
   - MAC addresses can be divided into two types:
      - **UAA** (Universally Administered Address) Uniquely assigned to the device by the manufacturer
      - **LAA** (Locally Administered Address) Manually assigned by an admin, it doesn't have to be globally unique. 
   - You can idenitfy a UAA or LLA by the 7th bit of the MAC address, called the U/L bit (Universal/Local)
      - U/L set to **0 = UAA**, 
      - U/L set to **1 = LAA**.
   - In the context of IPv6 addresses / EUI-64, the meaning of the U/L bit is reversed: 
      - U/L bit set to **0** = The MAC address the EUI-64 int ID was made from was an LAA 
      - U/L bit set to **1** = The MAC address the EUI-64 int ID was made from was an UAA

4. **Creation of EUI-64:**
   - The result is the Modified EUI-64, a 64-bit interface identifier that can be combined with the network prefix to form a complete 128-bit IPv6 address.

### **EUI-64 Examples**
For example, let's say we have a MAC address like `001A.2B3C.4D5E`. Applying the Modified EUI-64 process:

1. Insert FF:FE: `001A:2BFF:FE3C:4D5E`
2. Invert the seventh bit: `021A:2BFF:FE3C:4D5E`
3. Assuming '2001:db8::/64' is the network prefix, combine to complete the entire IPv6 address: `2001:DB8::021A:2BFF:FE3C:4D5E`

Alternatively, let's say we have a MAC address like `1234.5678.90AB.`

1. Insert FF:FE: `1234:56FF:FE78:90AB`
2. Invert the seventh bit: `1034:56FF:FE78:90AB`
3. Assuming '2001:db8::/64' is the network prefix, combine to complete the entire IPv6 address:  `2001:db8::1034:56FF:FE78:90AB`

### **EUI-64 Summary**
The Modified EUI-64 method is commonly used for automatically generating host identifiers in Stateless Address Autoconfiguration (SLAAC) without the need for DHCPv6. It simplifies the configuration process by allowing hosts to create their interface identifiers based on the MAC address, maintaining a level of familiarity for network administrators accustomed to working with MAC addresses. However, it's important to note that privacy concerns have led to the development of alternative methods, such as IPv6 Stateless Address Autoconfiguration with Privacy Extensions (RFC 4941), to enhance the privacy of IPv6 addresses generated by hosts.

## IPv6 Header
 The IPv6 Header has a fixed length, unlike IPv4. It is always 40 bites. The processing of IPv6 headers is easier for routers so performance is generally improved. It contains various fields that provide information about the packet and guide its routing and delivery. Here is a brief description of each field in the IPv6 header:

1. **Version (4 bits):**
   - Indicates the version of the Internet Protocol. For IPv6, this field is set to 6.

2. **Traffic Class (8 bits):**
   - Originally called the "Type of Service" (TOS) field in IPv4, it is now called the "Traffic Class" in IPv6. It is used to classify and prioritize different types of traffic.

3. **Flow Label (20 bits):**
   - Intended for flow control and specifying particular flows of data between hosts. It is used to identify specific traffic 'flows' (communmications between a specified source and destination).

4. **Payload Length (16 bits):**
   - Indicates the length of the IPv6 payload (data) in octets (8-bit bytes), excluding the length of the IPv6 header.

5. **Next Header (8 bits):**
   - Identifies the type of the next header in the packet after the IPv6 header. For example, UDP or TDP. It is similar to the IPv4 Protocol field.

6. **Hop Limit (8 bits):**
   - Replaces the Time-to-Live (TTL) field in IPv4. It limits the number of hops (routers) a packet can traverse before being discarded. If the it reaches 0, the packet is discarded.

7. **Source Address (128 bits):**
   - The IPv6 address of the sender (source) of the packet.

8. **Destination Address (128 bits):**
   - The IPv6 address of the intended recipient (destination) of the packet.

The structure and organization of the IPv6 header are streamlined compared to IPv4, with a fixed-size header and additional extension headers used for optional features. The mandatory fields listed above provide essential information for routing and delivering IPv6 packets across networks. Extensions headers, when present, follow the main IPv6 header and provide additional information or features such as fragmentation, security, and mobility. The use of extension headers allows for greater flexibility and scalability in the IPv6 protocol.

# IPv6 Address Types
IPv6 (Internet Protocol version 6) introduces several address types to accommodate the increased address space and to support various functionalities. 

These address types provide flexibility and support for various communication scenarios in the IPv6 protocol. Global unicast addresses are similar to public IPv4 addresses and are routable on the internet, while link-local addresses are used for communication within the same network segment. Multicast and Anycast addresses enable efficient and specialized communication patterns. Additionally, IPv6 introduces improved mechanisms for address configuration, such as Stateless Address Autoconfiguration (SLAAC) and DHCPv6.

Here are the main IPv6 address types:

## Unicast Addresses:
### Global Unicast Address:
 Similar to public IPv4 addresses, **global unicast addresses** are routable on the IPv6 internet. They are globally unique and used for communication between devices across the internet.
- Originally they were assigned from the range `2000::/3`, but now they are defined as all addresses which aren't reserved for other purposes. 
- They have to be registered for use.
- 2001:0DB8:8B00:AAAA:0000:0000:0000:0001/64 is sectioned as follows:
   - 2001:0DB8:8B00: --> 48bit 'global routing prefix' assigned by the ISP
   - AAAA: --> 16-bit 'subnet identifier' used by the enterprise to make various subnets
   - 0000:0000:0000:0001 --> 64-bit host portion of the address

### Link-Local Address:
Used for communication within the same network segment (link) or subnet. These addresses are automatically configured by devices when they are connected to a network. 
- They are assigned from the range `fe80::/10`
- The standard states that the 54 bits after FE80/10 should all be 0, so you won't see link local addresses beginning with FE9, FEA, FEB, Only FE8. 
- The Interface ID is generated using EUI-64 rules.
- Common uses of link-local addresses:
   - routing protocol peerings (OSPFv3) uses LLA for neighbor adjacencies
   - Next-hop addresses for static routes
   - Neighbor Discovery Protocol (NDP), IPv6's replacement for ARP, uses link-local addresses to function.

### Unique Local Address (ULA):
Similar to IPv4 private addresses, ULA addresses are used for private networks. They are not routable on the global internet and are designed for communication within an organization. 
- You don't need to register to use them.
- They don't need to be globally unique (Though best practice is to make them so). 
- They are assigned from the range `fc00::/7`, however a later update requires the 8th bit set to 1, so the first two digitst must be **fd**.
- As an example, FD45:93AC:8A8F:AAAA:0000:0000:0000:0001/64 is sectioned as follows:
   - FD --> Indicates a unique local address
   - 45:93AC:8A8F: --> 40-bit 'global ID', It should be randomly generated to prevent issues when companies merge
   - AAAA: --> 16-bit 'subnet identifier' used by the enterprise to make various subnets
   - 0000:0000:0000:0001 --> 64-bit host portion of the address


## Multicast Addresses:
- Multicast addresses are used to send traffic to multiple devices simultaneously.
- IPv6 uses range `ff00::/8` for multicast.
- IPv6 doesn't use broadcast, there is no 'broadcast address' in IPv6! But the Multicast address intended for end hosts serves the same basic function.

### Multicast Scopes
- IPv6 defines multiple multicast 'scopes' which indicate how far the packet should be forwarded. 
![Multicast Scopes](images/ipv6scope.png)

- **Interface-Local Multicast Scope:** The packet doesn't leave the local device. Can be used to send traffic to a service within the local device. They are assigned from the range `ff01::/16`

- **Link-Local Multicast Scope:** Used for communication within the same network segment. The packet remains in the local subnet. Routers will not route the packet between subnets. They are assigned from the range `ff02::/16`

The addresses in the image below use the 'link-local scope, for example.
![IPv6 Multicast Link-Local Addresses](images/IPv6-Multicast-Well-Known-Addresses.png)

- **Site-Local Multicast Scope:** The packet can be forwarded by routers. Should be limited to a single physical location (not forwarded over a WAN - Wide Area Network). They are assigned from the range `ff05:/16`

- **Organization-Local Multicast Scope:** Wider in scope than site-local (an entire company/organization). They are assigned from the range `ff08:/16`

- **Global Multicast:** Used for communication across the global internet. No boundaries, it is possible to be routed over the internet. They are assigned from the range `ff0e::/16`

### Solicited-Node Multicast Address
A Solicited-Node Multicast Address is a special type of IPv6 multicast address used in the context of **Neighbor Discovery Protocol (NDP)**, which is part of the IPv6 suite of protocols. The purpose of the Solicited-Node Multicast Address is to efficiently resolve the layer-2 (link-layer) address of a neighbor on the same network segment when performing address resolution.

An IPv6 solicited-node multicast address is calculated from a unicast address as follows...
- if unicast address = 2001:db8:0:1:f2a:4fff:fea3:00b1
- solicited-note multicast address = ff02::1:ff + last 6 hex digits of unicast address (**a3:00b1**)
- solicited-note multicast address = ff02:1:ff**a3:b1**

## Anycast Addresses:
- Anycast is **'one-to-one-of-many**' routing. It is a new feature of IPv6.
- Anycast addresses are used for load balancing and high availability.
- Anycast addresses are assigned to multiple devices, and traffic is routed to the nearest (in terms of network topology / routing metric) device with that address.
- There is no specified address range for anycast addresses. Use a regular unicast address (global unicast, unique local) and specify it as an anycast address.

## Special Addresses:
- **Loopback Address (::1):** Similar to the IPv4 loopback address (127.0.0.1), the IPv6 loopback address is used for communication with the same device.

- **Unspecified Address (::):** Equivalent to the IPv4 address 0.0.0.0. 
   - The unspecified address is used as a placeholder when an IPv6 address is not yet assigned.
   - IPv6 default routes are configured to ::/0

- **IPv4-Compatible IPv6 Address:** Deprecated and no longer used.

- **IPv4-Mapped IPv6 Address:** Deprecated and no longer used.

# IPv6 Functions
## Neighbor Discover Protocol (NDP)
Neighbor Discovery Protocol (NDP) is a protocol used with IPv6. Devices keep an IPv6 neighbor table. It has various functions...
- One of those functions is to replace ARP, which is no longer used in IPv6. The ARP-like funciton of NDP uses ICMPv6 and solicited-node multicast addresses to learn the MAC address of other hosts. (ARP in IPv4 uses broadcast messages). Two message types are used:
   1. Neighbor Solicitation (NS) = ICMPv6 Type 135
      - This is where the **solicited-node multicast address** is used in the IPv6 destination field to solicite the MAC address of a neighboring network device
      - The destination MAC address is based on the  solicited-node multicast address as well.
   2. Neighbor Advertisement (NA) = ICMPv6 Type 136 
      - This is where the network device responds with its MAC address
- Another function of NDP allows hosts to automatically discover routers on the local network. Two messages are used for this process:
   1. Router Solicitation (RS) = ICMPv6 Type 133
      - Sent to multicast address FF02::2 (all routers).
      - Asks all routers on the local link to identify themselves
      - Sent when an interface is enabled/host is connected to the network.
   2. Router Advertisement (RA) = ICMPv6 Type 134
      - Sent to multicast address FF02::1 (all nodes) so all hosts on local link not just routers
      - The router annouces its presence, as well as other infromation about the link.
      - These messages are sent in reponse to RS messages.
      - They are sent periodically, even if the router hasn't received an RS.

## Stateless Address Auto-Configuration (SLAAC)
SLAAC is a process where Hosts use the RS/RA messages to learn the IPv6 prefix of the local link (ie 2001:db8::/64), and then automatically generate an IPv6 address.
- Using the `ipv6 address <prefix/prefix-length> eui-64` command, you need to manually enter the prefix.
- Using the `ipv6 address autoconfig` command, you don't need to enter the prefix. The device uses NDP to learn the prefix used on the local link via RS and RA messages, the device will then use EUI-64 to generate the interface ID, or it will be randomly generated (depending on the device/maker)

## Duplicate Address Detection (DAD)
Duplicate Addres Detection (DAD) allows hosts to check if other devices on the local link are using the same IPv6 address.
Any time an IPv6-enabled interface initializes (**no shutdown** command), or an IPv6 address is configured on an interface (by any method: manual, SLAAC, etc.), it performs DAD.

DAD uses two messages: Neighbor Solicitation and Neighbor Advertisement.
   - The host will send an NS to its own IPv6 address. If it doesn't get a reply, it knows the address is unique.
   - If it gets a reply, it means another host on the network is already using the address.

# IPv6 Routing
- IPv6 routing works the same as IPv4 routing. However, the two processes are separate on the router, and the two routing tables are seperate as well.
- IPv6 routing is disabled by default, and must be enabled by `ipv6 unicast-routing`
- If IPv6 routing is disabled, the router will be able to send and receive IPv6 traffic, but will not route IPv6 traffic, i.e.it will not forward it between networks.
- A connected network route is automatically added for each connected network.
- A local *host route* is automatically added for each address configured on the router.
- Routes for link-local addresses are not added to the routing table
## Static Route Types
1. **Directly attached static route:** Only the exit interface is specified.
   - For example, `ipv6 route 2001:db8:0:3::/64 s0/0`
   - Note: In IPv6, you can't use directly attached static routes if the interface is an Ethernet interface.
2. **Recursive static route:** Only the next hop is specified.
   - For example, `ipv6 route 2001:db8:0:3::/64 2001:db8:0:12::2`
3. **Fully specified static route:** Both the exit interface and next hop are specified.
   - For example, `ipv6 route 2001:db8:0:3::/64 g0/0 2001:db8:0:12::2`
4. **Floating static route:** A floating static route is a routing configuration that involves the use of a static route with a higher administrative distance than another route of the same destination. 


