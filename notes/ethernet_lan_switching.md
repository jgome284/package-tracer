# Ethernet Frames

An Ethernet frame is the basic unit of data transmission on an Ethernet network. It is a packet of data that is sent from one device to another device on the network.

An Ethernet frame consists of an Header, Payload, and Trailer. The Header is made up of the Preamble, SFD, Destination, Source, and Type/Length fields. The Trailor is a Frame Check Sequence or FCS. More details are shown below:

- Preamble: This is a 7-byte field that is used to synchronize the sender and receiver clocks.
- Start frame delimiter (SFD): This is a 1-byte field that marks the beginning of the frame.
- Destination MAC address: This is a 6-byte field that contains the MAC address of the device that the frame is being sent to.
- Source MAC address: This is a 6-byte field that contains the MAC address of the device that is sending the frame.
- Type / Length: This is a 2-byte field. A value of 1500 or less indicates the LENGTH of the encapsulated packet. 1536 or greater indicates the TYPE of the encapsulated packet (usually IPv4 or IPv6) and length is determined via other methods.
- Payload: This is the data that is being transmitted.
- Frame Check Sequence (FCS): This is a 4-byte field that is used to detect errors in the frame. It runs a a Cyclic Redundancy Check (CRC) algorithm


The Ethernet frame is encapsulated in a physical layer frame, which is made up of the preamble, SFD, and destination and source MAC addresses. The physical layer frame is then transmitted over the network medium.

When a frame arrives at a destination device, the physical layer frame is stripped off and the Ethernet frame is processed. The Ethernet frame is then passed up to the network layer, where it is processed by the network protocol that is being used.

Ethernet frames are used to transmit a variety of data types, including IP packets, TCP segments, and UDP datagrams. Ethernet frames are also used to transmit control messages, such as ARP requests and responses.

Ethernet frames are an essential part of Ethernet networks. They provide a reliable and efficient way to transmit data between devices on the network.

# MAC Address
A MAC address (Media Access Control address) is a unique identifier assigned to a network interface controller (NIC) for use in communications on a network. It is also known as a Burned-In Address (BIA). MAC addresses are 48-bit (6-byte) identifiers that are burned into the NIC's hardware at the time of manufacture.

MAC addresses are used in Ethernet networks to identify individual devices on the network. When a device wants to send data to another device on the network, it uses the MAC address of the destination device to direct the data to the correct destination.

MAC addresses are also used in other types of networks, such as Wi-Fi networks and Bluetooth networks.

MAC addresses are important because they allow devices on a network to communicate with each other efficiently. Without MAC addresses, devices would not be able to identify each other and would not be able to send data to each other.

Here are some of the uses of MAC addresses:

- Identifying devices on a network: MAC addresses are used to uniquely identify devices on a network. This allows devices to communicate with each other directly, without the need for a central server.
- iltering and forwarding traffic: MAC addresses can be used to filter and forward traffic on a network. For example, a router can use MAC addresses to forward traffic to the correct destination network.
- Security: MAC addresses can be used to implement security measures on a network. For example, a network administrator can use MAC addresses to restrict access to the network to authorized devices.

MAC addresses are an essential part of modern networking. They allow devices to communicate with each other efficiently and securely.


## MAC Address Tables

A MAC address table is a database that stores the MAC addresses of devices on a network and their corresponding ports. MAC address tables are used by network devices to forward traffic to the correct destination device.

When a device sends a frame on a network, the receiving device checks its MAC address table to see if it knows the MAC address of the destination device. If the receiving device knows the MAC address of the destination device, it can forward the frame directly to the destination device.

If the receiving device does not know the MAC address of the destination device, it will broadcast the frame to all devices on the network. The destination device will then respond to the broadcast frame with its MAC address.

MAC address tables are updated periodically as devices come and go on the network.

## Dynamic MAC Addresses

When a new device connects to a network device, it sends its MAC address. The network device then checks its MAC address table to see if the MAC address to see if the device is know. Any device whos MAC address is not configured on the MAC address table prior to connecting is said to be dynamically learned. If the MAC address is not authorized, the network will deny the connection. 

Dynamic MAC Addresses are typically dropped from the MAC address table after 5 minutes of inactivity.

# Communication Types

| Feature                | Unicast                              | Multicast                      |
|------------------------|--------------------------------------|--------------------------------|
| Number of destinations | One	                                | Many                           |
| Addressing	         | MAC address	                        | Multicast address              |
| Common uses	         | Web browsing, email, file transfer	| Streaming video, audio, gaming |

## Unicast Frames
Unicast communication is used to send data to a single device on the network. When a device wants to send a unicast message, it uses the MAC address of the destination device to direct the message to the correct destination.

### Unknown Unicast Frames
An unknown unicast frame is a frame that is addressed to a MAC address that the receiving device does not know. The receiving device will typically drop the frame. However, the receiving device can also *flood the frame*, that is, send it to all ports. 

Unknown unicast frames can occur for a number of reasons, such as:

- A new device has joined the network and the receiving device has not yet updated its MAC address table.
- A device has moved to a different port on the network and the receiving device has not yet updated its MAC address table.
- A device has been spoofed and is using a MAC address that the receiving device does not know.

### Known Unicast Frame
 
A known unicast frame is a frame that is addressed to a MAC address that the receiving device knows. The receiving device will typically forward the frame to the destination device.

Known unicast frames are essential for efficient and reliable network communication. They allow devices to communicate directly with each other, without having to broadcast their frames to all devices on the network.

## Multicast Frames
Multicast communication is used to send data to a group of devices on the network. When a device wants to send a multicast message, it uses a multicast address to direct the message to all devices that are subscribed to that multicast address.