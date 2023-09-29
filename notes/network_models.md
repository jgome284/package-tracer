# Networking Models
Networking models can be used to design, implement, and manage networks. A networking model is a conceptual framework for understanding how networks work. It categorizes and provides a structure for networking protocols and standards. Protocols describe a logical set of rules for how network devices and software should work. Standards describe how different components of a network should interact with each other.


## OSI Model
There are a number of different networking models, but the most common is the Open Systems Interconnection (OSI) model. The OSI model is a seven-layer model that describes the different functions that are involved in network communication. The seven layers of the OSI model are:

- Physical layer: The physical layer is responsible for transmitting and receiving raw data over a physical medium, such as a copper cable or a fiber optic cable.
- Data link layer: The data link layer is responsible for framing data into packets and transmitting those packets over a physical link. It also handles error detection and correction.
- Network layer: The network layer is responsible for routing packets between different networks. It also handles fragmentation and defragmentation of packets.
- Transport layer: The transport layer is responsible for providing reliable end-to-end communication between applications. It also handles flow control and congestion control.
- Session layer: The session layer is responsible for managing communication sessions between applications. It also handles authentication and authorization.
- Presentation layer: The presentation layer is responsible for formatting data so that it can be understood by the receiving application. It also handles encryption and decryption of data.
- Application layer: The application layer is responsible for providing network services to applications. It also handles user interaction with the network.
- The OSI model is a useful framework for understanding how networks work. It can be used to design, implement, and manage networks. However, it is important to note that the OSI model is a conceptual framework. It is not a strict set of rules that must be followed. In practice, many networks do not implement all seven layers of the OSI model.

### Protocol Data Units
Protocol Data Units (PDUs) are the basic units of information that are exchanged between communicating entities on a network. PDUs are typically encapsulated within each other as they move through the different layers of a network protocol stack.

Each layer of the network protocol stack adds its own header and trailer to the PDU before passing it on to the next layer. The header and trailer contain information that is specific to the layer, such as the source and destination addresses, the length of the PDU, and the type of PDU.

When a PDU reaches the destination device, the layers of the network protocol stack remove the headers and trailers in reverse order. The resulting data is then delivered to the application that requested it.

Here are some examples of PDUs at different layers of the OSI model:

- Layer 1 (Physical layer): Bit
- Layer 2 (Data link layer): Frame
- Layer 3 (Network layer): Packet
- Layer 4 (Transport layer): Segment or datagram
- Layer 5 (Session layer): Data
- Layer 6 (Presentation layer): Data
- Layer 7 (Application layer): Data

## TCP/IP Suite

The TCP/IP suite (Transmission Control Protocol/Internet Protocol suite) is a collection of communication protocols used to interconnect network devices on the internet. It is also used as a communications protocol in a private computer network (an intranet or extranet).

The TCP/IP suite is made up of four layers:

- Application layer: This layer provides services to network applications, such as web browsers, email clients, and file transfer programs.
- Transport layer: This layer provides reliable and unreliable end-to-end communication between applications.
- Internet layer: This layer is responsible for routing packets between networks.
- Link layer: This layer is responsible for transmitting packets over physical media, such as Ethernet cables or Wi-Fi networks.
- The TCP/IP suite is a very flexible and extensible protocol suite. It is used by a wide variety of devices and networks, including personal computers, servers, routers, and switches.

Here are some of the most common TCP/IP protocols:

- TCP (Transmission Control Protocol): TCP is a connection-oriented protocol that provides reliable end-to-end communication between applications. TCP is used by many common network applications, such as web browsers, email clients, and file transfer programs.
- UDP (User Datagram Protocol): UDP is a connectionless protocol that provides unreliable end-to-end communication between applications. UDP is used by many applications that require real-time performance, such as VoIP and online gaming.
- IP (Internet Protocol): IP is the protocol that is responsible for routing packets between networks. IP is used by all TCP/IP protocols.
- ICMP (Internet Control Message Protocol): ICMP is used to send error messages and other control messages between network devices.
- ARP (Address Resolution Protocol): ARP is used to map IP addresses to MAC addresses.

The TCP/IP suite is the foundation of the internet. It is the set of protocols that makes it possible for computers and other devices to communicate with each other across the globe.
