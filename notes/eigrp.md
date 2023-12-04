# EIGRP
## About EIGRP
- Was Cisco proprietary, but Cisco has now published it openly so other vendors can implement it on their equipment.
- Considered an 'advanced' / 'hybrid' distance vector routing protocol.
- Much faster than RIP in reacting to changes in the network.
- Does not have the 15 'hop count' limit of RIP.
- Sends messages using mulicast address 224.0.0.10.
- is the only IGP that can perform unequal-cost load-balancing (by default it performs ECMP load-balancing over 4 paths like RIP)

## EIGRP Metric
By Default EIGRP uses **bandwidth** and **delay** to calculate metric. The formula is complex, including several K constants that are either 1 or 0 by default. That is K1 = 1, K2 = 0, K3 = 1, K4, = 0, and K5 = 0. The formula in simple terms is bandwidth (of the slowest link) + delay (of all links). The metric reported for EIGRP is provided as both **feasible distance** and **reported distance**. 
    - The **feasible distance** = the router's metric value to the route's destination.
    - The **reported distance** (aka *Advertised distance*) = the neighbor's metric value to the route's destination.

## Key Terms
*Feasible distance* and *reported distance* metrics are important because they are used to determine the best and alternate routes. 
- The **successor** = the route with the lowest metric to the destination (the best route)
- The **feasible successor** = an alternate route to the destination (not the best route) __which meets the feasibility condition__
- The **feasibility condition**: is a route which is considered a **feasible successor** if its **reported distance** is lower than the successor route's **feasible distance**. If this condition is not met, the network is at risk for loops.