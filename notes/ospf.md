# OSPF
## About
- Open Shortest Path First (OSPF) is a link state routing protocol that support equal cost load balancing.
- Uses the shortest path first algorithm of Dutch computer scientist Edsger Dijkstra, thus OSPF is also known as Disjkstra's algorithm.

## Versions
- There are three versions:
    - OSPFv1 (1989): OLD, not in use anymore...
    - OSPFv2 (1998): Used for IPv4
    - OSPFv3 (2008): Used for IPv6 (can also be used for IPv4, but usually v2 is used)

## Link State Information
- Routers store information about the network in **LSAs** (link state advertisements), which are organized in a structure called the **LSDB** (link state database).
    - LSA's include: RID (router ID), IP (advertised network address), Cost (OSPF metric)
    - Each routers uses the SPF algorithm to calculate its best route to each LSA added to the LSDB.
    - Routers will **flood** LSAs until all routers in the OSPF area develop the same map of the network (LSDB).
    - Each LSA has an aging timer (30 minutes by default). The LSA will be flooed again after the timer expires.

## OSPF Areas
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

## OSPF Cost
- OSPF's metric is called **cost**
    - it is automatically calculated based on the bandwidth (speed) of the interface.
    - It is calculated by dividing a **reference bandwidth** value by the interface's bandwidth. However, all values less than 1 will be converted to 1. By default FastEthernet, Gigabit Ethernet, 10Gig Ethernet, etc. are equal and all have a cost of 1 because the reference bandwidth is 100 mbps as shown below. But this is not ideal.
        **Reference:** 100 mbps / **Interface:** 10 mbps = cost of 10
        **Reference:** 100 mbps / **Interface:** 100 mbps = cost of 1     
        **Reference:** 100 mbps / **Interface:** 1000 mbps = cost of 1 (rounded up)
        **Reference:** 100 mbps / **Interface:** 10000 mbps = cost of 1 (rounded up)
    - Loopback interfaces have a cost of 1 by default.
    - The OSPF cost to a destination is the total cost of the 'outgoing/exit interfaces.' 
## Messages
- When OSPF is activated on a interface, the router states sending OSPF **hello** messages out of the interface at regular intervals (determined by the **hello timer**). 
    - These are used to introduce the router to potential OSPF neighbors. 
    - The default hello timer is 10 seconds on an Ethernet connection.
    - Hello messages are multicast to 224.0.0.5 (multicast address for all OSPF routers)
    - OSPF messages are encapsulated in an IP header, with a value of 89 in the protocol field.
## OSPF States
- (**DOWN STATE**) When OSPF is activated on an interface, it sends an OSPF hello message with it's Router ID (RID), and a Neighbor RID of 0.0.0.0 in the down state, since it doesn't know about any OSPF neighbors yet. 
- (**INIT STATE**) When a receiving router receives the OSPF Hello packet, but its own router ID is not in the Hello Packet, it adds the Router ID from the sending router to it's table and changes to the init state. 
- (**2-WAY STATE**) When a router sends a hello packet containing the RID of both routers. 
    - If both routers reach the 2-way state, it means that all the conditions have been met for them to become OSPF neighbors. They are now ready to share LSAs to build a common LSDB.
    - In some network types, a DR (Designated Router) and BDR (Backup Designated Router) will be elected at this point.
- (**EXSTART STATE**) To prepare exchanging information about their LSDB, both routers will have to select which one will start the exchange by sending DBD (Database Description) packets. They do this in the Exstart state, where the router with the higher RID will become the **master** and initiate the exchange. The router with the lower router RID will become the **slave**. 
- (**EXCHANGE STATE**) The routers exchange DBDs which contain a list of the LSAs in their LSDB. These DBDs do not include detailed information about the LSAs, just basic information. The routers compare the information in the DBD they received to the information in their own LSDB to determine which LSAs they must receive from their neighbor.
- (**LOADING STATE**) In the **Loading** state, routers send Link State Request (LSR) messages to request that their neighbors send them any LSAs they don't have. LSAs are sent in Link State Update (LSU) messages. The routers send LSAck messages to acknowledge that they received the LSAs.
- (**FULL STATE**) In the **Full** state, the routers have full OSPF adjacency and identical LSDBs. They continue to send and listen to Hello packets (every 10 secs by default) to maintain neighbor adjacency. Every time a Hello packet is received, the 'Dead' timer (40 seconds by default) is reset. If the Dead counter goes to 0, the neighbor is removed. The routers will continue to share LSAs as the network changes to make sure that each router has a complete and accurate map of the network (LSDB).