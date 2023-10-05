# LAN
The poor-man's definition of a Local Area Network (LAN) is a group of devices (PCs, servers, routers, switches, etc.) in a single location (home, office, etc.). But a more specific definition for a LAN is a single **broadcast domain**, including all devices in that broadcast domain.

# Broadcast Domains
A group of devices which will receive a broadcast frame (destination MAC FFFF.FFFF.FFFF) sent by any one of the members.

# VLAN
**VLANs (Virtual Local Area Networks)** are a technology used in network management to logically segment a physical network into multiple virtual networks. These virtual networks operate as if they were entirely independent, even though they share the same physical infrastructure. VLANs serve several purposes, including enhancing network security, improving network performance, and simplifying network management.

Here's a breakdown of VLANs, their purpose, and how they are configured:

**Purpose of VLANs:**

1. **Network Segmentation:** VLANs allow network administrators to logically divide a physical network into multiple isolated broadcast domains. This segmentation can be based on departments, teams, or other organizational criteria.

2. **Improved Security:** By segregating network traffic into different VLANs, administrators can enhance network security. Devices in one VLAN typically cannot communicate directly with devices in another VLAN without routing through a firewall or router, adding a layer of security.

3. **Optimized Network Performance:** VLANs can be used to group devices with similar traffic patterns together. For example, high-bandwidth devices like servers can be placed in one VLAN, while lower-priority devices like guest Wi-Fi can be placed in another. This helps prioritize network traffic and optimize performance.

4. **Broadcast Control:** VLANs reduce broadcast traffic by isolating broadcast domains. In traditional networks, broadcast traffic can negatively impact network performance, especially as the network grows.

5. **Network Flexibility:** VLANs offer flexibility by allowing changes to the logical network without requiring physical changes to the network infrastructure. You can move devices between VLANs or reconfigure VLANs as needed.

**Configuring VLANs:**

Configuring VLANs involves several steps, including:

1. **VLAN Creation:** Create the VLANs you need on network devices that support VLANs, such as managed switches or routers. You'll assign each VLAN a unique VLAN ID (VLAN number).

2. **Port Assignment:** Associate specific switch ports with the appropriate VLANs. Devices connected to these ports will be members of the corresponding VLAN.

3. **VLAN Tagging:** If you have multiple VLANs traversing a single network link (trunk link), you'll need to configure VLAN tagging. VLAN tagging adds a VLAN identifier to Ethernet frames, allowing switches and routers to distinguish traffic for different VLANs on the same link.

4. **Router Configuration:** To enable communication between devices in different VLANs, you'll need a router or a Layer 3 switch. Configure the router interfaces or subinterfaces with IP addresses for each VLAN and set up routing between them.

5. **Access Control:** Implement access control policies, firewall rules, or other security measures to control traffic between VLANs, as needed for your network's security requirements.

6. **Monitoring and Management:** Continuously monitor and manage VLANs to ensure they meet your network's requirements and adjust configurations as necessary.

VLANs are a powerful tool for network segmentation and management, and they play a crucial role in modern network architectures. Properly configured VLANs enhance network security, performance, and flexibility, making them a valuable resource for network administrators.

# Trunk Ports
In a small network with few VLANS, it is possible to use a separate interface for each VLAN when connecting switches, and switches to routers. However, when the number of VLANs increases, this is not viable. It will result in wasted interfaces, and often routers won't have enough interfaces for each VLAN.

Switches will 'tag' all frames that they send over a trunk link. This allows the receiving switch or router to know which VLAN the frame belongs to.

Trunk ports = 'tagged' ports
Access ports = 'untagged' ports

## Inter-Switch Link
Inter-Switch Link is an old proprietary Cisco protocol, created before IEEE 802.1Q. Modern Cisco equipment no longer supports ISL.
## IEEE 802.1Q Encapsulation
IEEE 802.1Q is an industry standard protocol created by the IEEE.

An Ethernet Frame includes the IEEE 802.1Q tag after the source MAC Address and the Type/Length field. It includes a Tag Protocol Identifier (TPID) and Tag Control Information (TCI). 

The TPID is always set to a value of 0x8100 when the frame is tagged. (0x = hexadecimal)

The TCI can be subdivided into 3 fields: 

- PCP: Priority Code Point, 3 bits - Used for Class of Service (COS), which prioritizes important traffic in congested networks.
- DEI: Drop Eligible Indicator, 1 bit - Used to indicate frames that can be dropped if the network is congested.
- VID: VLAN ID, 12 bits = 4096 total VLANS (2^12), actual range of 1-4094, since 0 & 4095 are reserved.

## VLAN Ranges
Normal VLANS: 1-1005
Extended VLANS: 1006-4094

**Note:** Some older devices cannot use the extended VLAN range...

## Native VLAN
The native VLAN is VLAN 1 by default on all trunk ports, however this can be manually configured on each trunk port. The switch does not add an 802.1Q tag to frames in the native VLAN. When a switch receivecs an untagged frame on a trunk port, it assumes the frame belongs to the native VLAN. **it's very important that the native VLAN matches between switches!**

For security purposes, it is best to change the native (default) VLAN to an unused VLAN.

# Router on a Stick (ROAS)
ROAS is used to route between multiple VLANs using a single interface on the router and 1 switch. It divides one physical interface between a router and switch into n logical subinterfaces

The switch interface is configured as a regular trunk.

The router interface is configured using **subinterfaces**, which means you configure the VLAN and IP address on each subinterface.