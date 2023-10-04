# Classless Inter-Domain Routing (CIDR)
CIDR, which stands for Classless Inter-Domain Routing, was developed by the Internet Engineering Task Force (IETF) in 1993 to address limitations and inefficiencies in the original IP addressing scheme, which was based on classes; Class A, B, and C networks - recall that Classes D and E were reserved for special purposes.

The **IANA** assigns IPv4 addresses/networks to companies based on their size. In the past a large company might receive a class A network, while a medium sized company might receive a class B, and a small company might receive a class C address... however, this resulted in many wasted IP addresses.

Here are some of the key reasons why CIDR was developed:

1. **Efficient Use of IP Addresses:** In the original IP addressing scheme, IP addresses were allocated based on classes, and each class had a fixed number of network and host bits. This led to inefficient allocation of IP addresses, as organizations often received more IP addresses than they actually needed.

2. **Scarcity of IPv4 Addresses:** As the internet grew, the scarcity of available IPv4 addresses became a significant concern. The class-based addressing scheme exacerbated this problem by allocating IP addresses in large blocks, even if only a fraction of the addresses were used.

3. **Route Aggregation:** Class-based routing resulted in a large number of routing table entries in routers, which increased the complexity of internet routing and slowed down the routing process. CIDR introduced route aggregation, allowing multiple IP address prefixes to be represented as a single, more specific prefix. This reduced the size of routing tables and improved routing efficiency.

4. **Hierarchical Addressing:** CIDR introduced the concept of hierarchical addressing, where IP addresses are assigned in a hierarchical manner. This allows for more efficient routing by aggregating addresses based on their common prefixes.

5. **Flexibility in Address Assignment:** CIDR provides greater flexibility in allocating IP addresses, as organizations can request address blocks of varying sizes, rather than being constrained by class-based allocations.

6. **IPv6 Transition:** CIDR was introduced as a transitional measure to help extend the lifespan of IPv4 addresses while the transition to IPv6 (Internet Protocol version 6) was planned and implemented. IPv6 introduces a significantly larger address space to address the long-term issue of IP address exhaustion.

Overall, CIDR was developed to provide a more efficient, flexible, and scalable way of allocating and routing IP addresses, helping to extend the usability of IPv4 addresses until the adoption of IPv6 and addressing the challenges associated with the original class-based IP addressing scheme.

# Subnetting
With CIDR the requirements of Class A (/8), B (/16), and C(/24) were removed. This allowed larger networks to be split into smaller networks, called **subnetworks** or **subnets** allowing greater efficiency. Instead CIDR allows us to use different prefix lengths... something like /25, /26, /27! The use of / appended to ip addresses is actually CIDR notation.

    The /24 binary netmask is 11111111.11111111.11111111.00000000. In decimal it is 255.255.255.0 ... 2^8-2 = 252 usable addresses
    The /25 binary netmask is 11111111.11111111.11111111.10000000. In decimal it is 255.255.255.128 ... 2^7-2 = 126 usable addresses 
    The /26 binary netmask is 11111111.11111111.11111111.11000000. In decimal it is 255.255.255.192 ... 2^6-2 = 62 usable addresses
    The /27 binary netmask is 11111111.11111111.11111111.11100000. In decimal it is 255.255.255.224 ... 2^5-2 = 30 usable addresses
    The /28 binary netmask is 11111111.11111111.11111111.11110000. In decimal it is 255.255.255.240 ... 2^4-2 = 14 usable addresses
    The /29 binary netmask is 11111111.11111111.11111111.11111000. In decimal it is 255.255.255.248 ... 2^3-2 = 6 usable addresses
    The /30 binary netmask is 11111111.11111111.11111111.11111100. In decimal it is 255.255.255.252 ... 2^2-2 = 2 usable addresses
    The /31 binary netmask is 11111111.11111111.11111111.11111110. In decimal it is 255.255.255.254 ... 2^1-2 = 0 usable addresses

Typically there must be broadcast or network addresses, but in a point-to-point network, there is no need for a broadcast and network address. Therefore, the /31 netmask is the most efficient use of ip allocation for point-to-point networks.

The /32 netmask is used to create a static route to one specific host since there is not network portion of the address.

## Fixed Length Subnet Masks
LSM (Variable Length Subnet Masks) is the process of creating subnets of the same size.

## Variable Length Subnet Masks
VLSM (Variable Length Subnet Masks) is the process of creating subnets of different sizes, to make your use of network addresses more efficient.

# Practice Questions
## Fixed Length Subnetting
1. You have been given the 172.30.0.0/16 network. Your company requires 100 subnets with at least 500 hosts per subnet. What prefix length should you use?
    
    The /16 netmask is 11111111.11111111.00000000.00000000

    2^6 provides too little subnets but 2^7 borrowed host bits for the network portion provies 128 subnets. 
    
    Written in binary this is 11111111.11111111.11111110.00000000 which would result in using a /23 subnet...
    
    9 bits left for the host portion of the ip address equates to 2^9 - 2 = 510 usable addresses per subnet which meets the companies requirements

2. What subnet does 172.21.111.201/20 belong to?
    
    10101100.00010101.01101111.11001001 --> to find the subnet the host portion of the address is set to 0...

    10101100.00010101. 0110 | 0000.00000000 --> converted to decimal, this is the 172.21.96.0/20 subnet

3. What is the broadcast address of the 192.168.91.78/26 network?
    
    The address in binary is 11000000.10101000.01011011.01001110

    The broadcast address is when the host portion is all 1s... 11000000.10101000.01011011.01 | 111111

    Thus this back to decimal is 192.168.91.127/26

## Variable Length Subnetting
Imagine you are assigned the 192.168.1.0/24 (11000000.10101000.00000001.00000000) network and you need to subdivide it into 5 subnets of different sizes.
- **LAN A** = 110 hosts
- **LAN B** = 8 hosts
- **LAN C** = 29 hosts
- **LAN D** = 45 hosts
- **LAN E** = A point-to-point connection between two routers

To run VLSM, assign the largest subnet and work your way down... In this case, LAN A --> D --> C --> B...

**LAN A**

Needs at least 7 host bits (128 addresses) to have 110 hosts... 

    - Network Address: 11000000.10101000.00000001.0 | 0000000 --> 192.168.1.0/25
    - Broadcast Address: 11000000.10101000.00000001.0 | 1111111 --> 192.168.1.127/25
    - First Usable Address: 192.168.1.1/25
    - Last Usable Address: 192.168.1.126/25
    - Total Number of Usable Host Addresses: 2^7-2 = 126

**LAN D**

Adding 1 to the broadcast address of LAN A, we get the network address of LAN D... 192.168.1.128...

But we still need to determine the prefix length on this network.

It needs at least 6 host bits (64 addresses) to have 45 hosts... 

- Network Address: 11000000.10101000.00000001.10 | 000000 --> 192.168.1.128/26
- Broadcast Address: 11000000.10101000.00000001.10 | 111111 --> 192.168.1.191/26
- First Usable Address: 192.168.1.129/26
- Last Usable Address: 192.168.1.190/26
- Total Number of Usable Host Addresses: 2^6-2 = 62

**LAN C**

Adding 1 to the broadcast address of LAN D, we get the network address of LAN C... 192.168.1.192...

But we still need to determine the prefix length on this network.

It needs at least 5 host bits (32 addresses) to have 29 hosts... 

- Network Address: 11000000.10101000.00000001.110 | 00000 --> 192.168.1.192/27
- Broadcast Address: 11000000.10101000.00000001.110 | 11111 --> 192.168.1.223/27
- First Usable Address:192.168.1.193/27
- Last Usable Address: 192.168.1.222/27
- Total Number of Usable Host Addresses: 2^5-2 = 30

**LAN B**

Adding 1 to the broadcast address of LAN C, we get the network address of LAN B... 192.168.1.224...

But we still need to determine the prefix length on this network.

It needs at least 4 host bits (16 addresses) to have 8 hosts... 

- Network Address: 11000000.10101000.00000001.1110 | 0000 --> 192.168.1.224/28
- Broadcast Address: 11000000.10101000.00000001.1110 | 0000 --> 192.168.1.239/28
- First Usable Address: 192.168.1.225/28
- Last Usable Address: 192.168.1.238/28
- Total Number of Usable Host Addresses: 2^4-2 = 14

**LAN E**

Adding 1 to the broadcast address of LAN B, we get the next address available for the point-to-point connection... 192.168.1.240

But we still need to determine the prefix length on this connection. 

In this case, the most efficient use is to discard the need for a network and broadcast address so a /31 prefix does just that!

- Network Address: None
- Broadcast Address: None
- First Usable Address: 11000000.10101000.00000001.111100 | 0 --> 192.168.1.240/31
- Last Usable Address: 11000000.10101000.00000001.111100 | 1 --> 192.168.1.241/31
- Total Number of Usable Host Addresses: 2!

Imagine you are assigned the 192.168.5.0/24 (11000000.10101000.00000101.00000000) network and you need to subdivide it into 5 subnets of different sizes.
- **LAN 1** = 45 hosts
- **LAN 2** = 64 hosts
- **LAN 3** = 14 hosts
- **LAN 4** = 9 hosts
- **LAN 5** = A point-to-point connection between two routers

**LAN 2**
Needs 128 addresses to have 64 hosts... that is 7 borrowed bits
    - Network Address: 11000000.10101000.00000101.0 | 0000000 --> 192.168.5.0/25
    - Broadcast Address: 11000000.10101000.00000101.0 | 1111111 --> 192.168.5.127/25
    - First Usable Address: 192.168.5.1/25
    - Last Usable Address: 192.168.5.126/25
    - Total Number of Usable Host Addresses: 2^7-2 = 126

**LAN 1**
    - Network Address: 11000000.10101000.00000101.10 | 000000 --> 192.168.5.128/26
    - Broadcast Address: 11000000.10101000.00000101.10 | 111111 --> 192.168.5.191/26
    - First Usable Address: 192.168.5.129/26
    - Last Usable Address: 192.168.5.190/26
    - Total Number of Usable Host Addresses: 2^6-2 = 62

**LAN 3**
    - Network Address: 11000000.10101000.00000101.1100 | 0000 --> 192.168.5.192/28
    - Broadcast Address: 11000000.10101000.00000101.1100 | 1111 --> 192.168.5.207/28
    - First Usable Address: 192.168.5.193/28
    - Last Usable Address: 192.168.5.206/28
    - Total Number of Usable Host Addresses: 2^4-2 = 14

**LAN 4**
    - Network Address: 11000000.10101000.00000101.1101 | 0000 --> 192.168.5.208/28
    - Broadcast Address: 11000000.10101000.00000101.1101 | 1111 --> 192.168.5.223/28
    - First Usable Address: 192.168.5.209/28
    - Last Usable Address: 192.168.5.222/28
    - Total Number of Usable Host Addresses: 2^4-2 = 14

**LAN 5**
    - Network Address: 11000000.10101000.00000101.111000 | 00 --> 192.168.5.224/30 -- 255.255.255.252
    - Broadcast Address: 11000000.10101000.00000101.111000 | 11 --> 192.168.5.227/30
    - First Usable Address: 192.168.5.225/30
    - Last Usable Address: 192.168.5.226/30
    - Total Number of Usable Host Addresses: 2