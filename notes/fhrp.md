# First Hop Redundancy Protocols (FHRPs)
First Hop Redundancy Protocols (FHRPs) are networking protocols designed to provide high availability and fault tolerance for hosts on a local network. The primary purpose of FHRPs is to ensure continuous communication between devices on the same subnet in the event of a failure in the default gateway, which is the router that connects the local subnet to other networks or the Internet.

Here are three commonly used FHRPs: Hot Standby Router Protocol (HSRP), Virtual Router Redundancy Protocol (VRRP), and Gateway Load Balancing Protocol (GLBP).

The primary goal of these FHRPs is to ensure network resilience and minimize downtime in case of a router failure. They enable automatic failover so that if the primary router becomes unavailable, one of the backup routers can take over seamlessly, allowing devices on the local network to maintain connectivity without interruption. The choice between HSRP, VRRP, or GLBP often depends on factors like vendor preference, network requirements, and the need for additional features such as load balancing.

FHRPs are 'non-preemptive.' The current active router will not automatically give up its role, even if the former active router returns. However, you can change this default behavior to make R1 'preempt' R2 and take back its active role automatically.

- A **virtual IP** is configured on the two routers, and a **virtual MAC** is generated for the virtual IP (each FHRP uses a different format for the virtual MAC)
- An **active** router and a **standby** router are elected. (Different FHRPs use different terms)
- End hosts in the network are configured to use the virtual IP as their default gateway.
- The active router replies to ARP requests using the virtual MAC address, so traffic destined for other networks will be sent to it.
- If the active router fails, the standby becomes the next active router. The new active router will send **gratuitous ARP** messages so that switches will update their MAC address tables. It now functions as the default gateway.
- If the old active router comes back online, by default it won't take back its role as the active router. It will become the standby router.
- You can configure 'preemption', so that the old active router does take back its role as the active router.

## Hot Standby Router Protocol (HSRP)
   - HSRP is a Cisco proprietary protocol that allows for the automatic assignment of a backup router to take over in case the primary router fails.
   - It uses a virtual IP address and a virtual MAC address. The primary router sends periodic hello messages, and if the backup router doesn't receive them after a certain time, it assumes the role of the active router.
   - There are two versions: **version 1** and **version 2**. Version 2 adds IPv6 support and increases the number of groups that can be configured. Both routers must use the same HSRP version... v1 and v2 are not compatible.
   - Multicast IPv4 address: v1 = 224.0.0.2, v2 = 224.0.0.102
   - Virtual MAC address: v1 = 0000.0c07.ac**XX** (XX = HSRP group number), v2 = 0000.0c97.f**XXX** (XXX = HSRP group number)
   - In a situation with multiple subnets/VLANs, you can configure a different active router in each subnet/VLAN to load balance.
   - The **active** router is determined in this order:
        1. Highest priority (default 100)
        2. Highest IP address

### HSRP Example:

**1. Configuration:**
   - Two routers, Router A and Router B, are connected to the same local network.
   - HSRP is configured on both routers, and they are part of the same HSRP group.
   - Each router is assigned an IP address, and a virtual IP address is configured as the default gateway for devices on the local subnet.

**2. Active and Standby Roles:**
   - Initially, one router (let's say Router A) is elected as the active router, and the other (Router B) is the standby router.
   - The active router is responsible for forwarding traffic from the local network to other networks or the Internet.

**3. Virtual IP and MAC Address:**
   - A virtual IP address and a virtual MAC address are assigned to the HSRP group. This is the IP address that devices on the local subnet use as their default gateway.
   - The virtual IP and MAC addresses "float" between the routers, with the active router having the actual assignment at any given time.

**4. Hello Messages:**
   - Both routers periodically send hello messages to each other. If Router B stops receiving hello messages from Router A (indicating a failure), it assumes that Router A is no longer functioning correctly.

**5. Failover:**
   - If Router B determines that Router A is no longer active, it takes over the active role. The virtual IP and MAC addresses are then moved to Router B.
   - When Router A recovers, it can resume its role as the standby router.

**6. Seamless Connectivity:**
   - Throughout this process, devices on the local subnet continue to use the same virtual IP address as their default gateway. The change in active routers is transparent to these devices, ensuring uninterrupted connectivity.

This basic process ensures that if the active router fails, the standby router takes over without disruption to the network. The specifics may vary slightly depending on the FHRP used (VRRP, GLBP, etc.), but the fundamental idea of automatic failover and transparent continuity remains the same.

## Virtual Router Redundancy Protocol (VRRP)
   - VRRP is a standard-based protocol (defined in RFC 3768) similar to HSRP but is not vendor-specific, making it more interoperable across different devices.
   - Like HSRP, VRRP uses a virtual IP address and a virtual MAC address. Multiple routers participate in a VRRP group, with one router being elected as the virtual router master.

### VRRP Example:
- Open standard, very similar to HSRP...
- A **master** and a **backup** router are elected.
- Multicast IPv4 address: 224.0.0.18
- Virtual MAC address: 0000.5e00.01**XX** (XX = VRRP group number)
- In a situation with multiple subnets/VLANs, you can configure a different master router in each subnet/VLAN to load balance.

**1. Configuration:**
   - Consider two routers, Router A and Router B, connected to the same local network.
   - VRRP is configured on both routers, and they are part of the same VRRP group.
   - Each router is assigned an IP address, and a virtual IP address is configured as the default gateway for devices on the local subnet.

**2. Master and Backup Roles:**
   - Initially, one router (let's say Router A) is elected as the master router, and the other (Router B) is the backup router.
   - The master router is responsible for forwarding traffic from the local network to other networks or the Internet.

**3. Virtual IP and MAC Address:**
   - Similar to HSRP, a virtual IP address and a virtual MAC address are assigned to the VRRP group. Devices on the local subnet use this virtual IP address as their default gateway.
   - The virtual IP and MAC addresses are associated with the master router.

**4. Advertisement Messages:**
   - Both routers send VRRP advertisement messages to each other at regular intervals. These messages include information about the router's priority, state, and virtual IP address.
   - If Router B stops receiving advertisements from Router A, it assumes that Router A is no longer active.

**5. Priority and Election:**
   - Routers are assigned priorities, and the router with the highest priority becomes the master router.
   - If Router A (master) fails or is taken out of service, Router B (backup) with the next highest priority becomes the master.

**6. Failover:**
   - When failover occurs, the virtual IP and MAC addresses move from the master router to the backup router seamlessly.
   - If Router A recovers, it can become the master router again when conditions allow.

**7. Transparent Connectivity:**
   - Throughout this process, devices on the local subnet continue to use the same virtual IP address as their default gateway. The change in the master router is transparent to these devices, ensuring uninterrupted connectivity.

This example illustrates how VRRP provides redundancy and failover capabilities for the default gateway, ensuring that if the master router becomes unavailable, the backup router can take over, maintaining continuous communication for devices on the local subnet.

## Gateway Load Balancing Protocol (GLBP)
   - GLBP is another Cisco proprietary protocol that not only provides redundancy like HSRP and VRRP but also offers load balancing capabilities.
   - GLBP assigns a virtual IP address to a group of routers, and they share the traffic load. One router is elected as the active virtual gateway, and the others act as backup gateways. GLBP uses a weighting mechanism to distribute traffic among the routers in the group.
   - Load balancing amoung routers __within a single subnet__
   - An **AVG** (Active Virtual Gateway) is elected.
   - Up to four **AVFs** (Active Virtual Forwarders) are assigned by the AVG (the AVG itself can be an AVF, too)
   - Each AVF acts as the default gateway for a portion of the hosts in the subnet.
   - Multicast IPv4 address: 224.0.0.102
   - Virtual MAC address: 0007.b400.**XXYY** (XX = GLBP group number, YY = AVF number)

### GLBP Example:

**1. Configuration:**
   - Consider two routers, Router A and Router B, connected to the same local network.
   - GLBP is configured on both routers, and they are part of the same GLBP group.
   - Each router is assigned an IP address, and a virtual IP address is configured as the default gateway for devices on the local subnet.

**2. Virtual IP and Virtual MAC Address:**
   - GLBP uses a virtual IP address and a virtual MAC address similar to HSRP and VRRP.
   - However, in GLBP, multiple routers can share the virtual IP address simultaneously, allowing for load balancing.
   - Each router in the GLBP group is assigned a unique virtual MAC address.

**3. Load Balancing:**
   - GLBP uses a weighting mechanism to distribute traffic among the routers in the group. Each router is assigned a weight, and the router with the highest weight becomes the Active Virtual Gateway (AVG).
   - The AVG is responsible for responding to ARP requests for the virtual IP address.

**4. Forwarder Role:**
   - The other routers in the GLBP group become Active Virtual Forwarders (AVFs).
   - AVFs forward traffic on behalf of the AVG, providing load balancing.

**5. Advertisement Messages:**
   - Routers send hello messages to each other to communicate their state and availability.
   - If a router stops sending hello messages, other routers can consider it as a potential failure.

**6. Load Balancing in Action:**
   - Devices on the local subnet send traffic to the virtual IP address. The AVG determines which AVF should handle the traffic based on the configured load-balancing algorithm (such as round-robin or weighted).

**7. Failover and Redundancy:**
   - If the AVG becomes unavailable, another router with the next highest weight takes over.
   - If an AVF becomes unavailable, the AVG reassigns its responsibilities to another router.

**8. Transparent Connectivity:**
   - Throughout this process, devices on the local subnet continue to use the same virtual IP address as their default gateway. The change in the active router or load-balancing decisions is transparent to these devices, ensuring uninterrupted connectivity.

This example illustrates how GLBP provides not only redundancy but also load balancing capabilities for the default gateway, allowing multiple routers to share the traffic load and distribute it efficiently across the network.