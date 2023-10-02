# Interface Configuration
As opposed to routers, which are by default in an *administratively down*/down state, cisco switches will be in the down/down state for layer 1 and 2 connectivity. Alternatively, if ports have a connection, the interface will be in the up/up state by default.

You can view port configurations on a switch the same as you would on a router with the `sh ip int br` command. Of note, since switch ports are layer 2 devices, they do not need an ip address assigned. However, the concecpt of multilayer switching does involve ip address configuration.

# Interface Speed
Interface speed refers to the maximum rate at which data can be transferred between a network interface card (NIC) and a network medium. It is measured in bits per second (bps). The interface speed of a NIC is determined by its hardware capabilities and the type of network medium it is connected to.

Common interface speeds include:

- 10 Mbps: This is the slowest common interface speed and is typically used for legacy networks.
- 100 Mbps: This is a faster interface speed that is commonly used for LANs and small businesses.
- 1 Gbps: This is a very fast interface speed that is commonly used for WANs and enterprise networks.
- 10 Gbps: This is an even faster interface speed that is becoming increasingly common for high-performance networks.
- 40 Gbps: This is a very fast interface speed that is typically used for data centers and other high-traffic environments.
- 100 Gbps: This is the fastest common interface speed and is typically used for the most demanding data center applications.

The interface speed of a NIC is important because it determines the maximum throughput of the network connection. If the interface speed is too slow, it can bottleneck the network and prevent data from flowing at its full potential.

# Interface Duplex

Network interface duplex refers to the ability of two network devices to transmit and receive data simultaneously. There are two types of duplex:

> **Half duplex:** Half duplex allows devices to transmit or receive data, but not both at the same time. This is the simplest type of duplex and is typically used for legacy networks.
> **Full duplex:** Full duplex allows devices to transmit and receive data simultaneously. This is the most common type of duplex and is typically used for modern networks.

The duplex setting of a network interface can be configured manually or automatically. Automatic duplex detection (**Auto MDI/MDIX**) is a feature that allows network interfaces to automatically negotiate the duplex setting with the other device on the link.

## Ethernet Hubs
These oldschool network devices function as repeaters. They are layer one devices. They broadcast the frames they receive to all network devices connected. This can lead to collisions where two network devices try to send information at the same time. To deal with these collisions in half-duplex situations like those in an Ethernet Hub, devices use CSMA/CD or Carrier Sense Multiple Access with Collision Detection.

## CSMA/CD
CSMA/CD (Carrier Sense Multiple Access with Collision Detection) is a media access control (MAC) protocol that is used to regulate access to a shared medium, such as a network cable. CSMA/CD works by allowing devices to transmit data only when the medium is idle. CSMA/CD works as follows:

1. A device that wants to transmit data first listens to the medium to see if it is idle.
1. If the medium is idle, the device transmits its data.
1. If another device is transmitting data at the same time, a collision occurs.
1. When a collision occurs, both devices stop transmitting and wait a random amount of time before trying to transmit again.

# Interface Autonegotiation
Speed/duplex autonegotiation is a feature that allows network interfaces to automatically negotiate the speed and duplex settings with the other device on the link. This eliminates the need for manual configuration and ensures that the two devices are communicating at the same speed and duplex.

Speed/duplex autonegotiation works by sending and receiving link pulses. The devices on the link then compare the pulses to determine the fastest speed and duplex setting that they both support. Once the devices have agreed on a speed and duplex setting, they will begin communicating at that setting.

Speed/duplex autonegotiation is supported by most modern network interfaces. It is enabled by default on most devices. However, it is important to verify that both devices on the link support autonegotiation before enabling it.