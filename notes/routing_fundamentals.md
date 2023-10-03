# Routing

Routing is the process of forwarding packets from one network to another. Routers are devices that are responsible for routing packets. Routers use routing tables to determine the best path for a packet to take to reach its destination.

A **route** tells the router: to send a packet to destination X, you should send the packet to next-hop Y. Or, if the destination is the router's own IP address, receive the packet for yourself (don't forward it).

A **connected route** is a route to the network the interface is connected to. It provides a route to all hosts in that network.

A **local route** is a route to the exact IP address configured on the interface. A /32 netmask is used to specify the exact IP address of the interface. 

The **most specific** route is the matching route with the longest prefix length, for example /32. The local route tends to be the most specific.

## Dynamic Routing
Dynamic routing is a type of routing that automatically updates routing tables based on network topology changes. Dynamic routing protocols use algorithms to exchange routing information between routers. This allows routers to quickly and efficiently update their routing tables when the network topology changes.

## Static Routing
Static routing is a type of routing that manually configures routing tables. Static routes are typically used for networks that are relatively static, such as networks that do not change frequently.

## Default Gateway
A default gateway is a router that is used to route traffic to networks that are outside of the local network. When a device on a network needs to send traffic to a destination network that is not directly connected to the local network, the device will send the traffic to the default gateway. The default gateway will then forward the traffic to the destination network.

Default gateways are typically configured on devices such as routers, switches, and servers. The default gateway configuration specifies the IP address of the router that should be used to route traffic to networks that are outside of the local network.

To configure a default gateway on a device, you need to specify the IP address of the router that you want to use as the default gateway. You can do this using the device's configuration interface.

## Default Route
A default route is a route to 0.0.0.0/0, the least specific route possible. It includes every possible destination IP address.

 To set a default route to the internet you can use the `ip route` command in the Cisco CLI with the least specific destination ip address, i.e. `ip route 0.0.0.0 0.0.0.0 <SELECT EXIT-INTERFACE or NEXT-HOP IP or both>`