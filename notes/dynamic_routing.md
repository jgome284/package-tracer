# Dynamic Routing
Dynamic routing, also known as adaptive routing, is a method of routing network traffic that allows routers to automatically discover and maintain routes to destinations. This is in contrast to static routing, where routes are manually configured on each router.

In dynamic routing, routers use routing protocols to exchange information about the network topology and the availability of paths. This information is used to create routing tables that contain the best known paths to all destinations. When a router receives a packet, it looks up the destination address in its routing table and forwards the packet to the next router on the best path.

There are two main types of dynamic routing protocols: distance vector protocols and link state protocols.

## Dynamic Routing Algorithms
### Distance Vector Algorithm
The Distance Vector Algorithm use a metric, such as the number of hops or the total delay, to determine the best path to a destination. Routers using the distance vector algorithm exchange routing tables with their neighbors, and each router uses this information to update its own routing table. This is known as 'routing by rumor.' This is because the router doesn't know about the network beyond its neighbors. It only knows the information that its neighbors tell it.

Distance vector algorithms work by assigning a cost to each link in the network. The cost of a link is typically a measure of the delay or congestion on the link. Routers using distance vector algorithms exchange routing tables with their neighbors, and each router uses this information to update its own routing table. The routing table contains the best known path to each destination, and the best path is the path with the lowest cost.

### Link State Algorithm
The Link state algorithm floods the network with information about the state of each link, including whether the link is up or down and the cost of using the link. Routers using link state protocols use this information to calculate the shortest path to each destination.

Link state algorithms work by flooding the network with information about the state of each link, including whether the link is up or down and the cost of using the link. Routers using link state algorithms use this information to calculate the shortest path to each destination. The shortest path is the path with the fewest hops, and each hop is assumed to have the same cost. Link state protocols tend to be faster in reacting to changes in the network than distance vector protocols. They also use more CPU on the router because more information is shared.

## Dynamic Routing Protocols
![image](https://github.com/jgome284/package-tracer/assets/30394024/fa51ef33-2076-461c-9589-95c484dbe539)

### IGP (Interior Gateway Protocol)
![image](https://github.com/jgome284/package-tracer/assets/30394024/bcc23fb8-a43e-458d-8dd8-d12ba24ec737)

An Interior Gateway Protocol (IGP) is a dynamic routing protocol that is used to exchange routing information within a single autonomous system (AS). An AS is a group of networks that are under the control of a single administrative entity. IGPs use a variety of algorithms to find the best path to a destination within the AS. Some common IGPs include:
- RIP (Routing Information Protocol), which uses the Distance Vector Algorithm type.
- EIGRP (Enhanced Interior Gateway Routing Protocol), which uses the Distance Vector Algorithm type. (Cisco Proprietary)
- OSPF (Open Shortest Path First), which uses the Link State Algorithm type.
- IS-IS (Intermediate System to Intermediate System) which uses the Link State Algorithm type.

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
