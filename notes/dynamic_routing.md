# Dynamic Routing
Dynamic routing, also known as adaptive routing, is a method of routing network traffic that allows routers to automatically discover and maintain routes to destinations. This is in contrast to static routing, where routes are manually configured on each router.

In dynamic routing, routers use routing protocols to exchange information about the network topology and the availability of paths. This information is used to create routing tables that contain the best known paths to all destinations. When a router receives a packet, it looks up the destination address in its routing table and forwards the packet to the next router on the best path.

There are two main types of dynamic routing protocols: distance vector protocols and link state protocols.

## Dynamic Routing Algorithms
### Distance Vector Algorithm
The Distance Vector Algorithm use a metric, such as the number of hops or the total delay, to determine the best path to a destination. Routers using the distance vector algorithm exchange routing tables with their neighbors, and each router uses this information to update its own routing table. This is known as 'routing by rumor.' This is because the router doesn't know about the network beyond its neighbors. It only knows the information that its neighbors tell it.

Distance vector algorithms work by assigning a cost to each link in the network. The cost of a link is typically a measure of the delay or congestion on the link. Routers using distance vector algorithms exchange routing tables with their neighbors, and each router uses this information to update its own routing table. The routing table contains the best known path to each destination, and the best path is the path with the lowest cost.

### Link State Algorithm
Link state algorithms work by flooding the network with information about the state of each link, including whether the link is up or down and the cost of using the link. Routers using link state algorithms use this information to calculate the shortest path to each destination. The shortest path is the path with the fewest hops, and each hop is assumed to have the same cost.

When using this type of dynamic routing protocol, every router creates a 'connectivity map' of the network. To allow this, each router advertises information about its interfaces (connected networks) to its neighbors. These advertisements are passed along to other routers, until all routers in the network develop the same map of the network. Each router independently uses this map to calculate the best routes to each destination. Due to this, link state protocols use more resources (CPU) on the router, because more information is shared. However, they tend to be faster in reacting to changes in the network than distance vector protocols.

## Dynamic Routing Protocols
![image](https://github.com/jgome284/package-tracer/assets/30394024/fa51ef33-2076-461c-9589-95c484dbe539)

### IGP (Interior Gateway Protocol)
![image](https://github.com/jgome284/package-tracer/assets/30394024/bcc23fb8-a43e-458d-8dd8-d12ba24ec737)

An Interior Gateway Protocol (IGP) is a dynamic routing protocol that is used to exchange routing information within a single autonomous system (AS). An AS is a group of networks that are under the control of a single administrative entity. IGPs use a variety of algorithms to find the best path to a destination within the AS. Some common IGPs include:
- RIP (Routing Information Protocol), which uses the Distance Vector Algorithm type. 
- EIGRP (Enhanced Interior Gateway Routing Protocol), which uses the Distance Vector Algorithm type. (Cisco Proprietary)
- OSPF (Open Shortest Path First), which uses the Link State Algorithm type.
- IS-IS (Intermediate System to Intermediate System) which uses the Link State Algorithm type.

#### RIP
- Uses routing-by-rumor logic. (Distance vector IGP)
- Uses hop count as its metric. The maximum hop count is 15. (bandwidth is irrelevant)
- Has three versions 
    - RIPv1: used for IPv4, only support classful addresses, i.e. class A, B, C...
    - RIPv2: used for IPv4, supports VLSM, CIDR, and provides subnet mask info in its advertisements, messages are multicast to 224.0.0.9. Multicast messages are delivered to devices that have joined that specific multicast group.
    - RIPng: used for IPV6
- Has two message types, **Request**, to ask RIP-enabled neighbor routers to send their routing table, and **Response**, to send the local router's routing table to neighboring routers. 
- By default RIP-enabled routers will share their routing table every 30 seconds.

#### EIGRP
- Was Cisco proprietary, but Cisco has now published it openly so other vendors can implement it on their equipment.
- Considered an 'advanced' / 'hybrid' distance vector routing protocol.
- Much faster than RIP in reacting to changes in the network.
- Does not have the 15 'hop count' limit of RIP.
- Sends messages using mulicast address 224.0.0.10.
- is the only IGP that can perform unequal-cost load-balancing (by default it performs ECMP load-balancing over 4 paths like RIP)

By Default EIGRP uses **bandwidth** and **delay** to calculate metric. The formula is complex, including several K constants that are either 1 or 0 by default. That is K1 = 1, K2 = 0, K3 = 1, K4, = 0, and K5 = 0. The formula in simple terms is bandwidth (of the slowest link) + delay (of all links). The metric reported for EIGRP is provided as both **feasible distance** and **reported distance**. 
    - The **feasible distance** = the router's metric value to the route's destination.
    - The **reported distance** (aka *Advertised distance*) = the neighbor's metric value to the route's destination.

These terms are important because they are used to determine the best and alternate routes. 
    - The **successor** = the route with the lowest metric to the destination (the best route)
    - The **feasible successor** = an alternate route to the destination (not the best route) __which meets the feasibility condition__
    - The **feasibility condition**: is a route which is considered a **feasible successor** if its **reported distance** is lower than the successor route's **feasible distance**. If this condition is not met, the network is at risk for loops.

#### OSPF
- Open Shortest Path First (OSPF) is a link state routing protocol that support equal cost load balancing.
- Uses the shortest path first algorithm of Dutch computer scientist Edsger Dijkstra, thus OSPF is also known as Disjkstra's algorithm.
- There are three versions:
    - OSPFv1 (1989): OLD, not in use anymore...
    - OSPFv2 (1998): Used for IPv4
    - OSPFv3 (2008): Used for IPv6 (can also be used for IPv4, but usually v2 is used)
- Routers store information about the network in **LSAs** (link state advertisements), which are organized in a structure called the **LSDB** (link state database).
    - LSA's include: RID (router ID), IP (advertised network address), Cost (OSPF metric)
    - Each routers uses the SPF algorithm to calculate its best route to each LSA added to the LSDB.
    - Routers will **flood** LSAs until all routers in the OSPF area develop the same map of the network (LSDB).
    - Each LSA has an aging timer (30 minutes by default). The LSA will be flooed again after the timer expires.
- OSPF uses areas to divide up the network. An area is a set of routers and links that share the same LSDB. OSPF areas must be contiguous, devices in an area
    - There is a **backbone area** (Area 0) by which all other area's have to connect to, e.g. Area 1, 2, 3, etc...
    - OSPF interfaces in the same subnet must be in the same area.
    - Routers connected to the backbone area are called **backbone routers**
    - Routers with all interfaces in the same area are called **internal routers**
    - Routers with interfaces in multiple areas are called **area border routers** (ABRs)
        - They maintain a seperate LSDB for each area they are connected to.
        - It is recommended that you connect an ABR to a maximum of 2 areas. Otherwise 3+ areas can overburden the router.
    - An **autonomous system boundary router** (ASBR) is an OSPF router that connects the OSPF network to an external network.
    - An **intra-area route** is a route to a destination inside the same OSPF area.
    - An **interarea route** is a route to a destination in a different OSPF area.
    - small networks can be single-area without any negative effects on performance.
    - In larger networks, a single-area design can have negative effects:
        - the SPF algorithm takes more time to calculate routes
        - the SPF algorithm requires exponentially more processing power on the routers
        - The larger LSDB takes up more memory on the routers
        - any small change in the network causes every router to flood LSAs and run the SPF algorithm again
- OSPF's metric is called **cost**
    - it is automatically calculated based on the bandwidth (speed) of the interface.
    - It is calculated by dividing a **reference bandwidth** value by the interface's bandwidth. However, all values less than 1 will be converted to 1. By default FastEthernet, Gigabit Ethernet, 10Gig Ethernet, etc. are equal and all have a cost of 1 because the reference bandwidth is 100 mbps as shown below. But this is not ideal.
        **Reference:** 100 mbps / **Interface:** 10 mbps = cost of 10
        **Reference:** 100 mbps / **Interface:** 100 mbps = cost of 1     
        **Reference:** 100 mbps / **Interface:** 1000 mbps = cost of 1 (rounded up)
        **Reference:** 100 mbps / **Interface:** 10000 mbps = cost of 1 (rounded up)
    - Loopback interfaces have a cost of 1 by default.
    - The OSPF cost to a destination is the total cost of the 'outgoing/exit interfaces.' 
- When OSPF is activated on a interface, the router states sending OSPF **hello** messages out of the interface at regular intervals (determined by the **hello timer**). 
    - These are used to introduce the router to potential OSPF neighbors. 
    - The default hello timer is 10 seconds on an Ethernet connection.
    - Hello messages are multicast to 224.0.0.5 (multicast address for all OSPF routers)
    - OSPF messages are encapsulated in an IP header, with a value of 89 in the protocol field.
- OSPF neighbor states:
    - (**DOWN STATE**) When OSPF is activated on an interface, it sends an OSPF hello message with it's Router ID (RID), and a Neighbor RID of 0.0.0.0 in the down state, since it doesn't know about any OSPF neighbors yet. 
    - (**INIT STATE**) When a receiving router receives the OSPF Hello packet, but its own router ID is not in the Hello Packet, it adds the Router ID from the sending router to it's table and changes to the init state. 
    - (**2-WAY STATE**) When a router sends a hello packet containing the RID of both routers. 
        - If both routers reach the 2-way state, it means that all the conditions have been met for them to become OSPF neighbors. They are now ready to share LSAs to build a common LSDB.
        - In some network types, a DR (Designated Router) and BDR (Backup Designated Router) will be elected at this point.
    - (**EXSTART STATE**) To prepare exchanging information about their LSDB, both routers will have to select which one will start the exchange by sending DBD (Database Description) packets. They do this in the Exstart state, where the router with the higher RID will become the **master** and initiate the exchange. The router with the lower router RID will become the **slave**. 
    - (**EXCHANGE STATE**) The routers exchange DBDs which contain a list of the LSAs in their LSDB. These DBDs do not include detailed information about the LSAs, just basic information. The routers compare the information in the DBD they received to the information in their own LSDB to determine which LSAs they must receive from their neighbor.
    - (**LOADING STATE**) In the **Loading** state, routers send Link State Request (LSR) messages to request that their neighbors send them any LSAs they don't have. LSAs are sent in Link State Update (LSU) messages. The routers send LSAck messages to acknowledge that they received the LSAs.
    - (**FULL STATE**) In the **Full** state, the routers have full OSPF adjacency and identical LSDBs. They continue to send and listen to Hello packets (every 10 secs by default) to maintain neighbor adjacency. Every time a Hello packet is received, the 'Dead' timer (40 seconds by default) is reset. If the Dead counter goes to 0, the neighbor is removed. The routers will continue to share LSAs as the network changes to make sure that each router has a complete and accurate map of the network (LSDB).

### EGP (Exterior Gateway Protocol)
An Exterior Gateway Protocol (EGP) is a dynamic routing protocol that is used to exchange routing information between different ASes. EGPs use a variety of algorithms to find the best path to a destination outside of the AS. The most common EGP is BGP (Border Gateway Protocol) which uses the Path Vector algorithm.

## Routing Metrics
Routing metrics are numerical values that are used by routing protocols to determine the best path to a destination network. Metric is used to compare routes learned by the *same* routing protocol. Different routing protocols use different metrics, but some common metrics include:

- **Hop count:** The number of routers that a packet must pass through to reach its destination.
- **Bandwidth:** The amount of data that can be transmitted over a link in a given amount of time.
- **Delay:** The time it takes for a packet to travel from one router to the next.
- **Latency:** The total time it takes for a packet to travel from its source to its destination.
- **Cost:** A measure of the overall cost of using a particular path, which can take into account factors such as hop count, bandwidth, and delay.
- **Reliability:** The probability that a packet will be successfully delivered over a particular path.
- **Load:** The amount of traffic that is currently using a particular path.

### AD (Administrative Distance)
Different routing protocols use totally different metrics, so they cannot be compared. The administrative distance (AD) is used to determine which routing protocol is preferred. A lower AD is preferred, and indicateds that the routing protocol is considered more 'trustworthy' (more likely to select good routes). By default on Cisco devices, AD is measured for different protocols as follows. Though you can customize the AD of a static route so that is not preferred over the routes learned by dynamic routing protocol, for example. Static routes that are not preffered are called 'floating static routes'. They will be inactive unless the routes learned via dynamic routing protocol are removed.

![image](https://github.com/jgome284/package-tracer/assets/30394024/002d451a-4522-4c2c-89e0-3e0408294a80)


### ECMP (Equal Cost Multi-Path)
ECMP is a scenario where multiple paths in a network have the same cost between addresses. If a router learns two or more routes via the same routing protocol to the same destination (same network address, same subnet mask) with the same metric - or cost to reach the address - both routes will be added to the routing table. Traffic will be load balanced over both routes. ECMP can also be configured in static routes.

## Advantages & Disadvantages
### Advantages
- It is more scalable. As a network grows, it can be difficult to keep track of all the routes manually. Dynamic routing protocols automatically discover and maintain routes, which makes them more efficient for larger networks.
- It is more resilient to failures. The router will automatically remove invalid routes. If a link or router fails, dynamic routing protocols will automatically discover a new path to the destination. This helps to keep the network running even if there are problems.
- It is more adaptive to network changes. If the network topology changes, dynamic routing protocols will automatically update their routing tables to reflect the new topology. This helps to ensure that traffic is always sent over the best path.

### Disadvantages
- It can be more complex to configure than static routing. Dynamic routing protocols use complex algorithms to calculate the best path, and these algorithms can be difficult to understand and configure.
- It can be more susceptible to errors. If a routing protocol is not configured correctly, it can create routing loops or other problems that can disrupt network traffic.
- It can consume more network bandwidth than static routing. Dynamic routing protocols exchange routing information with their neighbors, and this can consume a significant amount of bandwidth on large networks.

Overall, dynamic routing is a more powerful and flexible routing method than static routing. However, it is also more complex and requires more careful configuration.
