# Classic Spanning Tree Protocol (STP)
The Spanning Tree Protocol (STP), aka "Classic Spanning Tree Protocol", is a network protocol used to prevent infinite loops in Ethernet networks, which can lead to broadcast storms and network instability. At the same time, STP is important to enable network redundancy in layer two devices. STP is part of the IEEE 802.1D standard and is designed to ensure that a single, loop-free path exists in a network, while blocking redundant paths.

![Spanning Tree Protocol](images\spanning-tree-protocol.png)

Here are the key points about the Spanning Tree Protocol:

**1. Loop Prevention:**
   - Ethernet networks are susceptible to loops when multiple paths exist between switches. Loops can lead to broadcast storms, excessive traffic, and network downtime. STP's primary purpose is to detect and prevent these loops. STP prevents layer 2 loops by placing redundant ports in a blocking state, essentially disabling the interface. These interface act as backups that can enter a forwarding state if an active (=currently forwarding) interface fails. STP-enabled switches send/receive Hello BPDUs out of all interfaces, the default timer is 2 seconds. If a switch receives a Hello BPDU on an interface, it knows that interface is connected to another switch (routers, PCs, etc. do not use STP, so they do not send Hello BPDUs).

**2. Electing a Root Bridge:**
   - In an STP-enabled network, one switch is elected as the "Root Bridge." The Root Bridge serves as the reference point for all other switches in the network. It's responsible for calculating the optimal path to reach all other switches (designated as Bridge Ports) and, in turn, the end-user devices connected to those switches (designated as Port Access Devices).

**3. Bridge Protocol Data Units (BPDU):**
   - STP relies on Bridge Protocol Data Units (BPDUs) to exchange information among switches. BPDUs contain information about the sender's identity, priority, and the cost of the path to the Root Bridge. Interfaces in a forwarding state behave normally. They send and receive all normal traffic. However, interfaces in a blocking state only send or receive STP messages (called BPDUs = Bridge Protocol Units)

**4. Bridge ID:**
   - Each switch in the network has a unique Bridge ID, consisting of a priority value and a MAC address. The Bridge ID is used to determine the Root Bridge. 
   
   - Bridge ID is split into three fields, Bridge Priority, Extended System ID (=VLAN ID), and MAC Address. Extended System ID is used on cisco switches to use a version of STP called Per-VLAN Spanning Tree (PVST)
   
   - The Bridge Priority is compared first, and the switch with the lowest Bridge Priority becomes the root bridge. However, the default bridge priority is 32769 on all switches, so if that is not changed, the MAC address is used as the tie-breaker (lowest MAC address becomes the root bridge).
   ![Bridge Priority](images/bridge-id.png)
   
   - To change bridge priority without changing the VLAN, you must change the total bridge priority by the least significant bit of the bridge priority field, that is 4096. 
   ![Changing Bridge Priority](images/adjusting-bridge-priority.png)

   - ALL ports on the root bridge are put in a forwarding state, and other switches in the topology must have a path to reach the root bridge. Forwarding ports on the root bridge have an STP cost of 0.

   - Each remaining switch will select ONE of its interfaces to be its root port. The interface with the lowest root cost will be the root port. Root ports are also in a forwarding state. port with 10 Mbps have an STP cost of 100, 100 Mbps have a cost of 19, 1 Gbps have a cost of 4, and 10 Gbps have a cost of 2.

   - If there is a tie in STP cost, Bridge ID is the tie breaker.
   - If there is a tie in Bridge ID because two physical connections exits between switches, the neighboring switches port ID is the tie breaker.
   - Every collision domain has a single STP designated port so for collision domains that are not tied to Root ports, the switch with the lowest Bridge ID will become the port in the forwarding state and the other will be in the non-designated (blocking) state.

**5. Path Cost:**
   - The cost of a path between two switches is calculated based on the link's bandwidth. Higher bandwidth links have lower costs. STP chooses the path with the lowest cumulative cost to reach the Root Bridge.

**6. Port States:** (*Classic Spanning Tree Protocol*)
   - STP switches have different port states, including:
     - **Blocking:** Ports in this state do not forward traffic but listen to BPDUs to detect loops. They do NOT learn MAC addresses.
     - **Listening:** Ports in this state prepare to forward regular traffic but still do not. They only forward/receive STP BPDUs. Only designated or root ports enter the listening state. It is 15 seconds long by default and set by the forward delay timer. Ports do NOT learn MAC addresses from regular traffic while in this state.
     - **Learning:** Ports in this state prepare to forward traffic while learning MAC addresses. Ports enter the learning state after the listening state. This state is 15 seconds long by default and set by the forward delay timer. An interface in the learning state only sends/receives BPDUs, not regular traffic.
     - **Forwarding:** Ports in this state actively forward traffic.

     Listening and Learning are transitional states which are passed through when an interface is activated, or when a Blocking port must transition to a forwarding state due to a change in the network topology.
**7. Port Roles:**
**Root Port:**
   - A Root Port is the switch port on a non-root switch that provides the best path to reach the Root Bridge.
   - Each non-root switch has exactly one Root Port, which is the port with the lowest cost to the Root Bridge.
   - The Root Port is in the Forwarding state, allowing traffic to flow through it.

**Designated Port:**
   - A Designated Port is a switch port that is part of the Root Bridge's forwarding path for a specific segment or LAN.
   - On each network segment (e.g., an Ethernet segment), one switch is elected as the Designated Bridge (usually the switch with the lowest Bridge ID).
   - The switch with the Designated Port on that segment sends and receives traffic for that segment.
   - Designated Ports are also in the Forwarding state.

**Blocking Port:**
   - A Blocking Port is a switch port that is not forwarding traffic but is actively listening to Bridge Protocol Data Units (BPDUs).
   - Blocking Ports are part of the loop prevention mechanism in STP and are used to prevent loops from forming.
   - They are essentially ports that are turned off for data traffic but are on for STP-related communication.
   - The Blocking state is a transitional state that ports go through before moving to the Forwarding state.

**7. Convergence:**
   - STP is designed to converge quickly when the network topology changes. When a link failure occurs or a switch is added or removed, STP recalculates the topology to establish a new loop-free path.

**8. STP Variants:**
   - Several STP variants and enhancements have been developed over time, including Rapid Spanning Tree Protocol (RSTP) and Multiple Spanning Tree Protocol (MSTP). RSTP offers faster convergence, and MSTP allows for multiple STP instances to run on a single network.

**9. Cisco's PVST and PVST+:**
   - Cisco developed its own versions of STP, called Per-VLAN Spanning Tree (PVST) and Per-VLAN Spanning Tree Plus (PVST+). These versions extend STP to support VLAN-specific spanning trees, allowing for better load balancing and redundancy in networks with multiple VLANs.

STP is an essential protocol for network stability in Ethernet-based LANs. It ensures that network loops are avoided, and a single active path is established, preventing issues like broadcast storms and packet flooding. Network administrators should be familiar with STP and its variants to configure and manage networks effectively.

# Broadcast Storms
A broadcast storm is an abnormally high number of broadcast and multicast packets within a short period of time. Broadcast storms can occur when a device on a network sends a broadcast packet and all of the other devices on the network receive and forward the packet. This can cause a chain reaction, with each device forwarding the packet to all of the other devices on the network. This can quickly overwhelm the network and cause performance problems or even network outages.

There are a number of things that can cause broadcast storms, including:

**Misconfigured devices:** Devices that are misconfigured can send out too many broadcast packets, or they may forward broadcast packets that they should not forward.

**Network loops:** A network loop is a situation where a packet can travel around the same loop of devices indefinitely. This can cause broadcast packets to be forwarded repeatedly, which can lead to a broadcast storm.

**Malicious actors:** Malicious actors can launch denial-of-service attacks by sending out large numbers of broadcast packets. This can cause broadcast storms that can overwhelm the network and make it unusable for legitimate users.

Broadcast storms can have a number of negative consequences, including:

**Performance problems:** Broadcast storms can cause performance problems by consuming bandwidth and CPU resources. This can make it difficult for devices on the network to communicate with each other.

**Network outages:** In severe cases, broadcast storms can cause network outages by overwhelming the network and making it unusable.

**Security risks:** Broadcast storms can also increase the risk of security attacks, as attackers can use broadcast storms to spread malware or launch other types of attacks.
There are a number of things that can be done to prevent and mitigate broadcast storms, including:

**Configuring devices correctly:** Make sure that all devices on the network are configured correctly. This includes disabling unnecessary broadcast and multicast traffic.

**Eliminating network loops:** Make sure that there are no network loops on the network. This can be done using network monitoring tools or by manually checking the network configuration.

**Using broadcast storm prevention features:** Many routers and switches have features that can help to prevent and mitigate broadcast storms. These features can be used to limit the number of broadcast packets that are forwarded and to detect and isolate broadcast storms.

# Network Redundancy
Network redundancy is a design approach in networking that involves creating backup or redundant components, paths, or systems within a network to ensure continuous network availability, minimize downtime, and enhance fault tolerance. The goal of network redundancy is to maintain network connectivity and reliability even in the face of component failures, network congestion, or other unexpected issues. Here are some key aspects of network redundancy:

**1. Redundant Components:**
   - Network redundancy often involves duplicating critical components, such as switches, routers, servers, power supplies, and network cables. Redundant components are usually placed in such a way that if one fails, another can take over seamlessly.

**2. Redundant Paths:**
   - Redundant paths involve having multiple physical or logical connections between network devices. In case one path becomes unavailable due to link failure or congestion, traffic can be rerouted through an alternative path.

**3. Load Balancing:**
   - Redundancy can be combined with load balancing to distribute network traffic across multiple paths or components. Load balancers can monitor the health of network components and direct traffic to the most optimal path.

**4. High Availability:**
   - The primary goal of network redundancy is to ensure high availability. This means that network services and resources remain accessible to users and applications even when certain components or connections experience issues.

**5. Failover Mechanisms:**
   - Redundant systems often employ failover mechanisms that automatically switch to the backup component when a failure is detected. This can be achieved through technologies like Virtual IP addresses, Heartbeat monitoring, or protocols like HSRP (Hot Standby Router Protocol) and VRRP (Virtual Router Redundancy Protocol).

**6. Geographic Redundancy:**
   - Geographic redundancy involves replicating network infrastructure and data centers in different geographic locations. This approach provides protection against regional disasters or outages that could impact a single location.

**7. Redundant Power Supplies:**
   - Redundant power supplies in network devices ensure that the equipment remains operational even if one power supply fails. This is especially important in data centers and critical network infrastructure.

**8. Redundant Internet Connections:**
   - Organizations often have multiple internet connections from different providers to ensure internet access remains available, even if one provider experiences issues.

**9. Backup Data and Disaster Recovery:**
   - Network redundancy should be complemented by data redundancy and disaster recovery plans. Regularly backing up data and having a plan to recover from data loss or catastrophic events is crucial for overall network resilience.

**10. Complexity and Cost:**
    - Implementing network redundancy can be complex and may involve additional costs due to the purchase of redundant hardware and increased maintenance efforts. However, the benefits of improved reliability and availability often outweigh these costs.

Network redundancy is essential for ensuring business continuity, particularly in environments where network downtime can result in financial losses or significant disruptions. It is a critical component of designing robust and resilient network architectures.

# STP Timers
Spanning Tree Protocol (STP) uses several timers to control various aspects of its operation. These timers help ensure network stability, rapid convergence, and loop prevention. Here are the key STP timers and their functions:

1. **Hello Time (2 seconds by default):**
   - The Hello Time is the interval at which Bridge Protocol Data Units (BPDUs) are sent out on a bridge's designated ports. BPDUs contain information about the bridge's identity and the topology of the network.
   - BPDUs help switches communicate with each other to detect changes in the network topology and initiate the spanning tree algorithm if necessary.
   - A lower Hello Time can result in faster network convergence but may increase network overhead.

2. **Forward Delay (15 seconds by default):**
   - The Forward Delay timer is the time a port spends in the Listening and Learning states before transitioning to the Forwarding state.
   - It helps prevent temporary loops during network topology changes by allowing time for BPDUs to propagate and bridges to converge.
   - Reducing the Forward Delay timer can lead to faster convergence but may increase the risk of temporary loops.

3. **Max Age (20 seconds by default):**
   - The Max Age timer defines the maximum time a switch will wait for a BPDU to arrive before it considers the information about a topology change outdated.
   - If a switch does not receive BPDUs from a designated bridge within the Max Age interval, it assumes that the designated bridge is no longer operational, and it takes corrective action.
   - Max Age helps prevent stale information from causing network instability.

4. **Bridge Forward Delay (0 seconds by default):**
   - Bridge Forward Delay is the delay a bridge introduces before transitioning from the Listening state to the Forwarding state. This timer is not standard in all versions of STP but is used in some variants.
   - It can be used to stagger the Forwarding state transitions of multiple switches, further reducing the risk of temporary loops during network convergence.

5. **Aging Time (Max Age + Forward Delay by default):**
   - Aging Time is the total time it takes for a bridge to age out outdated information about network topology changes.
   - Aging Time is typically set to Max Age plus Forward Delay. When this timer expires, the bridge assumes that topology information is no longer reliable and initiates the spanning tree algorithm if necessary.

These timers collectively contribute to the stability and efficiency of STP by ensuring that network devices communicate and converge in a coordinated manner. Modifying these timers should be done with caution, as it can affect network performance and stability. Generally, the default timer settings are suitable for most network environments, but adjustments may be necessary in specific situations to optimize network behavior.

# STP BPDU
Bridge Protocol Data Units (BPDUs) are fundamental components of the Spanning Tree Protocol (STP) and its variants. BPDUs are data frames exchanged between network switches to establish and maintain a loop-free topology in Ethernet networks. Here's a detailed explanation of BPDUs:

**1. Purpose of BPDUs:**
   - BPDUs serve two primary purposes within the context of STP:
     - To elect a Root Bridge: BPDUs are used to elect a Root Bridge in the network. The Root Bridge serves as the reference point for the STP calculations.
     - To determine the best path to the Root Bridge: BPDUs contain information about the sender's identity, the path cost to reach the Root Bridge, and the state of the sending switch's ports. This information helps switches calculate the optimal path to reach the Root Bridge and avoid network loops.

**2. BPDU Format:**
   - BPDUs are encapsulated within Ethernet frames and use the IEEE 802.1Q standard for VLAN tagging. The IEEE 802.1D standard defines the BPDU frame format, which includes fields for the Root Bridge ID, Sender Bridge ID, Root Path Cost, Sender Port ID, and a few others.

   PVST+ Mac address is 01:00:0c:cc:cc:cd
   Regular STP Mac address is 0180.c200.0000

**3. BPDU Exchange Process:**
   - BPDUs are exchanged between switches in the following manner:
     - When a switch is initially powered on or detects a topology change, it begins sending BPDUs out of all its ports (known as the Listening state).
     - BPDUs are received and forwarded by neighboring switches, which then add their own information to the BPDUs and forward them further.
     - As BPDUs traverse the network, switches use the information in the BPDUs to determine the Root Bridge, calculate their own path cost to the Root Bridge, and determine which ports should be in the Forwarding state (forwarding traffic) and which should be in the Blocking state (blocking traffic to prevent loops).
     - The process continues until all switches have received and processed BPDUs, and the network topology converges to a loop-free state.

**4. BPDU Timers:**
   - BPDUs are sent periodically by switches to ensure that the network topology remains up to date. The Hello Time timer determines the interval at which BPDUs are sent (usually every 2 seconds by default).
   - If a switch stops receiving BPDUs from a neighboring switch for an extended period (as determined by the Max Age timer), it assumes that the neighboring switch has failed or the network topology has changed.

**5. VLAN-Specific BPDUs (PVST and PVST+):**
   - In Cisco's Per-VLAN Spanning Tree (PVST) and Per-VLAN Spanning Tree Plus (PVST+), separate BPDUs are generated and processed for each VLAN. This allows switches to calculate a spanning tree topology per VLAN, optimizing network resource usage.

In summary, BPDUs are the heart of the Spanning Tree Protocol. They carry critical information about the network's topology, including the identity of the Root Bridge, path costs, and port states. By exchanging BPDUs and making decisions based on their content, switches collaboratively create a loop-free Ethernet network that ensures network stability and prevents broadcast storms and packet flooding caused by loops.

# STP Optional Features
## Portfast
allows a port to move immediately to the forwarding state, bypassing listening and learning. If used, it must be enabled only on ports connected to end hosts. If enabled on a port connected to another switch it could cause a layer 2 loop.

## Root Guard
If you enable root guard on an interface, even if it receives a superior BPDU (lower bridge ID) on that interface, the switch will not accept the new switch as the root bridge. The interface will be disabled.

## Loop Guard
If you enable loop guard on an interface, even if the interface stops receiving BPDUs, it will not start forwarding. The interface will be disabled.

# STP Load-Balancing
There are two main ways to configure STP load-balancing:

**VLAN-based STP load-balancing:** This type of STP load-balancing uses VLANs to distribute traffic across multiple links. Each VLAN is assigned to a different spanning tree, and each spanning tree uses a different set of links. This allows traffic from different VLANs to be distributed across different links.

**Port-based STP load-balancing:** This type of STP load-balancing uses port priorities to distribute traffic across multiple links. Ports with higher priorities are more likely to be selected for the spanning tree. This allows traffic to be distributed across the links with the highest priorities.

# Rapid Spanning-Tree Protocol
Rapid Spanning Tree Protocol (RSTP) is an improvement over the Spanning Tree Protocol (STP) and provides faster convergence time, enhanced network stability, and reduced downtime. Spanning Tree Costs were updated with RSTP to include up to 1Tbps speeds. Additionally, in rapid STP, ALL switches originate and send their own BPDUs from their designated ports. Switches also age the BPDU information much more quickly; if a neighboring connection is lost, and the switch misses 3 BPDUs (6 seconds). It will flush all MAC addresses learned on that interface.

RSTP convergence time is much because RSTP uses a number of features to accelerate convergence, such as:

**Proposal/agreement:** RSTP uses a proposal/agreement mechanism to quickly negotiate the spanning tree topology between bridges. The new bridge-bridge handshake mechanism, allows ports to move directly to forwarding as opposed to waiting on the timers in classic STP.

**Port states:** RSTP uses a different set of port states than STP. These port states allow bridges to quickly transition to the forwarding state after a topology change. Rapid Spanning Tree Protocol (RSTP) has three port states:

- **Discarding:** This is the initial state of all ports. In this state, the port does not send or receive any traffic.
- **Learning:** In this state, the port is learning the MAC addresses of the devices on the connected network segment. The port does not forward any traffic during this state.
- **Forwarding:** In this state, the port is forwarding traffic between the connected devices.

**Link Types**
- **point-to-point:** 
    - link type between switches
    - full duplex connection
- **edge**
    - on connections to end host
    - type of connection with portfast enabled so there is no negotiation to move to the forwarding state.
- **shared**
    - for half duplex connections
    - typically used for connections to hubs

**Port Roles**
The Rapid Spanning Tree Protocol (RSTP), defined by IEEE 802.1w, simplifies and accelerates the Spanning Tree Protocol (STP) convergence process. While RSTP retains some of the classic STP port roles, it introduces a few new ones to improve network convergence and efficiency. Here are the port roles in RSTP:

**1. Root Port:**
   - Similar to classic STP, the Root Port is the port on a non-root switch that provides the best path to reach the Root Bridge.
   - Each non-root switch has exactly one Root Port, which is the port with the lowest cost to the Root Bridge.
   - The Root Port is in the Forwarding state, allowing traffic to flow through it.

**2. Designated Port:**
   - Like classic STP, a Designated Port is a switch port that is part of the Root Bridge's forwarding path for a specific segment or LAN.
   - On each network segment (e.g., an Ethernet segment), one switch is elected as the Designated Bridge (usually the switch with the lowest Bridge ID).
   - The switch with the Designated Port on that segment sends and receives traffic for that segment.
   - Designated Ports are also in the Forwarding state.

**3. Alternate Port (RSTP-Specific):**
   - An Alternate Port is a port that provides a backup path to reach the Root Bridge.
   - Alternate Ports are introduced in RSTP to speed up network convergence by having a port that is ready to transition to the Forwarding state if the Root Port fails.
   - When the Root Port fails, the Alternate Port transitions to the Forwarding state without waiting for timers to expire, reducing downtime.

**4. Backup Port (RSTP-Specific):**
   - A Backup Port is similar to an Alternate Port, providing a backup path to the Root Bridge.
   - However, Backup Ports are used in cases where multiple switches are connected in a ring topology, and one switch is designated as the "backup" for the ring.
   - If a failure occurs in the ring, the Backup Port becomes a part of the new active path, ensuring loop-free operation.
   
**5. Edge ports:** 
   - RSTP edge ports can quickly transition to the forwarding state without the need for any negotiation with other bridges.

RSTP is a widely supported protocol and is used by most Ethernet switches. It is a good choice for networks that require fast convergence time and high availability.


# Multiple Spanning Tree Protocol
- Uses modified RSTP mechanics
- Can group multiple VLANs into different instances (i.e. VLANS 1-5 in instance 1, VLANs 6-10 in instance 2) to perform load balancing.