# The Transport Layer (Layer 4)
## Protocols
The transport layer is responsible for end-to-end communication and data flow control between devices across a network. Common transport layer protocols include **Transmission Control Protocol (TCP)** and **User Datagram Protocol (UDP)**. TCP provides a connection-oriented and reliable communication, while UDP is connectionless and is often used for real-time applications where some data loss is acceptable.

TCP provides more features than UDP, but at the cost of additional **overhead**. For applications that require reliable communications, for example, file transfer, TCP is preferred. For applications like real-time voice and video, UDP is preffered. There are applications that use UDP, but provide reliability within the application itself. Some applications use both TCP and UDP, for example, DNS (Domain Name System).

## Services
The various services that the transport layer may provide will depending on the protocol used. They are: 
1. **Segmentation and Reassembly:**
   - Large messages are divided into smaller segments for efficient transmission. The transport layer at the receiving end is responsible for reassembling these segments into the original message.

2. **Flow Control:**
   - Flow control mechanisms prevent congestion by regulating the flow of data between sender and receiver. This helps to ensure that a fast sender does not overwhelm a slow receiver.

3. **Error Detection and Correction:**
   - The transport layer may implement error detection and correction mechanisms to ensure the integrity of the data being transmitted. If errors are detected, the transport layer may request the retransmission of the corrupted data.

4. **Reliability:**
   - The transport layer provides reliable communication by using acknowledgment mechanisms. The sender is notified when data is successfully received, and retransmission is requested if data is lost or corrupted.

5. **Connection Establishment and Termination:**
   - In connection-oriented communication, the transport layer is responsible for establishing and terminating connections between devices. This involves a three-step process known as the "handshake" (connection establishment), data transfer, and connection termination.

6. **Port Numbers:**
   - Port numbers provide layer 4 addressing and identify the application layer protocol. They also provide **session multiplexing**. A **session** is an exchange of data between two or more communicating devices. 

## Ports
The transport layer uses port numbers to distinguish between different services or applications running on a device. Ports help direct incoming data to the appropriate application or service.
- TCP uses the source port number to identify the session. 
- The destination port number identifies the application layer protocol. For example, Dst:80 indicates Hyper Text Transfer Protocol (HTTP), Dst:21 indicates File Transfer Protocol (FTP).

The following ranges have been assigned by the **IANA** (Internet Assigned Numbers Authority)
- **Well-known** port numbers: **0 - 1023** (Reserved for major application protocols)
- **Registered** port numbers: **1024 - 49151** (Registration is required to use these but its not as strict as the *well-known* range)
- **Ephemeral** port numbers: **49152 - 65535** (Hosts use this range when selecting a random source port, AKA private or dynamic ports)


# Transmission Control Protocol (TCP)
## TCP Packet Header
The TCP Header includes several fields, for example:
    - Source & destination port
    - Sequence and Acknowledgement Number
    - Flag bits (like ACK, SYN, FIN) that are used to establish and terminate connections.
    - Window Size (Used for flow control to adjust the rate that data is sent)

## TCP Service
- TCP is connection oriented
    - Before actually sending data to the destination host, the two hosts communicate to establish a connection. Once the connection is established the data exchange begins.
    - To establish connections TCP uses the **Three-Way Handshake** as detailed below:
        1. The source sends a TCP segment with the SYN flag set to 1.
        2. The destination replies with a TCP segment with the SYN, and ACK flag set to 1.
        3. The source sends a TCP segment with the ACK bit set to 1.
    - To terminate connections TCP uses the **Four-Way Handshake** as detailed below:
        1. The source sends a TCP segment with the FIN flag set to 1.
        2. The destination replies with a TCP segment with the ACK flag set to 1.
        3. The destination replies once more with a TCP segment with the FIN flag set to 1.
        4. The source sends a TCP segment with the ACK flag set to 1.

- TCP provides a reliable communication.
    - The destination host must acknowledge that it received the TCP segment. 
    - If a segment isn't acknowledged within a specific time frame, it is sent again.

- TCP provides sequencing.
    - Sequence numbers in the TCP header allow destination hosts to put segments in the correct order, even if they arrive out of order.
    - When a connection is first established via the 3-way handshake, a random initial sequence number is sent by the source host, for example 10. Then, the destination host replies with its own random initial sequence number - 50 for example - and the acknowledgement number set to the next sequence number it expects to receive from the source host, in this case, 11. In the source hosts next message, a sequence of 11 is provided and an acknowledgement of 51 is sent.
    - **Forward Acknowledgement** is used to indicate the sequence number of the next segment the host expects to receive.
    - In real situations, the sequence numbers get much larger and do not increase by 1 with each message.

- TCP provides flow control.
    - The destination host can tell the source host to increase/decrease the rate that data is sent with the **window size** field.
    - Acknowledging every single segment, no matter what size, is inefficient. The TCP header's Window Size field allows more data to be sent before an acknowledgement is required.
    - A 'sliding window' can be used to dynamically adjust how large the window size is.

## TCP Ports
Here is a list of some major TCP port numbers and their associated functions:

1. **Port 20** : **FTP Data** AND **Port 21** : **FTP control** 
   - FTP is used for file transfers between a client and a server.

2. **Port 22** : **SSH (Secure Shell)**
   - SSH provides a secure channel over an unsecured network, allowing secure remote login and command execution.

3. **Port 23** : **Telnet**
   - Telnet is a protocol that provides command-line access to a remote device.

4. **Port 25** : **SMTP (Simple Mail Transfer Protocol)**
   - SMTP is used for sending email messages between servers.

5. **Port 53** : **DNS (Domain Name System)**
   - This port is used by both UDP and TCP protocols. DNS resolves domain names to IP addresses.

6. **Port 80** : **HTTP (Hypertext Transfer Protocol)**
   - HTTP is used for transmitting web pages over the internet.

7. **Port 110** : **POP3 (Post Office Protocol version 3)**
   - POP3 is an email retrieval protocol.

8. **Port 143** : **IMAP (Internet Message Access Protocol)**
   - IMAP is another email retrieval protocol that allows multiple devices to manage the same mailbox.

9. **Port 443** : **HTTPS (Hypertext Transfer Protocol Secure)**
   - HTTPS is a secure version of HTTP, commonly used for secure communication over the internet.

10. **Port 3389** : **RDP (Remote Desktop Protocol)**
    - RDP allows remote access to a computer or server.

11. **Port 3306** : **MySQL Database**
    - Used for communication with MySQL database servers.

12. **Port 8080** : **HTTP Alternate (commonly used for web proxy and caching)**
    - An alternative port for HTTP traffic.


# User Datagram Protocol (UDP)
## UDP Packet Header
The UDP datagram header includes 4 simple fields:
1. Source port
2. Destination port
3. Length
4. Checksum

## UDP Service
- UDP is **not** connection-oriented.
    - The sending host does not establish a connection with the destination host before sending data. The data is simply sent
- UDP **does not** provide reliable communication.
    - When UDP is used, acknowledgments are not sent for received segments. If a segment is lost, UDP has no mechanism to re-transmit it. Segments are sent 'best-effort'.
- UDP **does not** provide sequencing.
    - There is no sequence number field in the UDP header. If segments arrive out of order. UDP has no mechanism to put them back in order.
- UDP **does not** provide flow control.
    - UDP has no mechanism like TCP's window size to control the flow of data.

## UDP Ports
Here are some major UDP port numbers and their associated functions:

1. **Port 53** : **DNS (Domain Name System)**
   - This port is used by both UDP and TCP protocols. DNS resolves domain names to IP addresses.

2. **Port 67** : **DHCP server** AND **Port 68** : **DHCP client**
   - DHCP (Dynamic Host Configuration Protocol) is used for dynamically assigning IP addresses to devices on a network.

3. **Port 69** : **TFTP (Trivial File Transfer Protocol)**
   - TFTP is a simple file transfer protocol often used for bootstrapping devices.

4. **Port 123** : **NTP (Network Time Protocol)**
   - NTP is used for synchronizing the time on networked devices.

5. **Port 137-138** : **NetBIOS Name Service**
   - NetBIOS over UDP is used for name resolution in Windows networks.

6. **Port 161** : **SNMP agent** AND **Port 162** : **SNMP manager**
   - SNMP (Simple Network Management Protocol) is used for network management and monitoring.

7. **Port 500** : **ISAKMP/IKE (Internet Key Exchange)**
   - Used for setting up Virtual Private Network (VPN) connections.

8. **Port 514** : **Syslog**
   - Syslog is a protocol used for sending log messages between devices on a network.

9. **Port 1900** : **SSDP (Simple Service Discovery Protocol)**
    - SSDP is used for discovering and advertising network services.

10. **Port 5060 and 5061** : **SIP (Session Initiation Protocol)**
    - SIP is used for initiating, modifying, and terminating real-time sessions that involve video, voice, messaging, and other communications.

11. **Port 5353** : **mDNS (Multicast DNS)**
    - Used for local domain name resolution and service discovery in a small network.