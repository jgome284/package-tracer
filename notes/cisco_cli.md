# Connecting to a Cisco Device
 Connnect via console port. On a RJ-45 port, a rollover cable is used.
 Make a serial connection with the Putty terminal emulator. 

# CLI Modes
## User Exec Mode
Default mode with limited options...
Enter `exit` to leave mode

## Priveledged Exec Mode
 Enter with the `enable` | `en` command.
 Enter `exit` to leave mode

 Priviledged Exec Commands Include:
 - `show running-config` | `sh run` - exactly what you would expect
 - `show startup-config` | `sh start` - exactly what you would expect
 - `write` | `write memory` | `copy running-config startup-config` - all commands are used to save the running configuration as the startup configuration
 - `show arp` - used to display the ARP cache on a device. The ARP cache is a database that stores the IP addresses and MAC addresses of devices on the network.
 - `show mac address-table` - used to display the MAC address table on a device. The MAC address table is a database that stores the MAC addresses and ports of devices on the network.
 - `clear mac address-table dynamic` - used to clear all dynamic MAC addresses from network device.
 - `clear mac address-table dynamic address <INSERT DYNAMIC MAC ADDRESS HERE>` - used to clear specific dynamic MAC address.
 - `clear mac address-table dynamic interface <INSERT PORT ID HERE>` - used to clear all dynamic MAC addresses from network device.
 - `show ip interface brief` | `sh ip int br` - shows ip status of router interfaces... information includes interface name, IP-Address assigned, layer 1 status - whether the connection is up or down, and the layer 2 - protocol - status 
 - `show interfaces` - shows information about all interfaces
 - `show interfaces <INTERFACE PORT HERE>` - shows interface information for specified port. For example, `show interfaces g0/0`
 - `show interfaces <INTERFACE PORT HERE> switchport` - shows switchport administrative and operational modes.
 - `show interfaces decription` - Provides table of interfaces with decription included.
 - `show interfaces status` - shows interface information like port, name, status, Vlan, duplex, speed, and type.
 - `show ip route` - shows the routers ip routing table. If prompt state *Gateway of last resort is not set* then a default route has not been configured yet.
 - `show vlan brief` - shows VLANs setup on a switch with associated ports. VLANs 1, 1002-1005 exist by default and cannot be deleted.
- `show interfaces trunk` - shows vlans allowed on trunk port
- `show vtp status` - displays vtp information
- `show spanning-tree` - displays STP information
- `show spanning-tree detail` - displays STP information with more detail, including total root cost
- `show spanning-tree summary` - display summary of STP info
- `show etherchannel load-balance` - display the etherchannel load-balance configuration
- `show etherchannel port-channel` - display port-channel/etherchannel information including number of ports, protocol, channel group mode, etc.
- `show etherchannel summary` - displays information about etherchannels on switch.
- `show ip protocols` - shows the dynamic routing protocol currently in place, its message timers, version, the maximum number of paths allowed between destinations for load balancing.
- `show ip eigrp neighbors` - displays eigrp neighbors 
- `show ip eigrp topology` - displays detailed information about eigrp topology

## Global Config Mode
 Enter with `configure terminal`
 Enter `exit` to leave mode

 If you are in another mode, you can elevate commands global config modeby typing `do` infront of the command of interest. 

 Global Commands include:
 - `enable password <YOUR PASSWORD HERE>` - enables `<YOUR PASSWORD HERE>` as the password
 - `service password-encryption` - used to encrypt password so that it does not display on `show running-config` command.
 - `enable secret <YOUR SECRET HERE>` enables `<YOUR SECRET HERE>` as a secret which offers more robust encryption
 - `hostname` - change hostname
 - `ip route <DESTINATION IP-ADDRESS> <NETMASK> <NEXT-HOP>` - used to configure a static route on a router. For example, `ip route 192.168.1.0 255.255.255.0 192.168.34.3` would configure the router's 192.168.34.3 port interface as the next hop for all traffic with a destination address of 192.168.1.0/24.
 - `ip route <DESTINATION IP-ADDRESS> <NETMASK> <EXIT-INTERFACE>` - used to configure a static route on the router by pointing to a port interface.
 - `vlan <VLAN #>` - creates a vlan and enables vlan configuration.
 - `ip routing` - enables layer 3 routing on a multilayer switch.
  - `vtp domain <NAME>` - sets vtp domain name on switch. If a switch with no VTP domain (domain NULL) receives a VTP advertisement with a VTP domain name, it will automatically join that VTP domain. Changing the VTP domain to an unused domain will reset the revision number to 0.
 - `vtp mode ?` - sets vtp mode on switch... valid commands are `server`, `client`, and `transparent`. If `transparent` is selected, the revision number is set back to 0.
 - `spanning-tree portfast default` - enables portfast on all access ports (not trunk ports), if used enabling bpduguard on interfaces is also recommended.
 - `spanning-tree portfast bpduguard default` - enables BPDU guard on all portfast-enabled interfaces.
 - `spanning-tree mode ?` - can set STP mode to either `mst` (multiple spanning tree), `pvst` (per vlan spanning tree), or `rapid-pvst` (per vlan rapid spanning tree).
 - `spanning-tree vlan <vlan-number> root primary` - sets the STP priority to 24576 on the vlan number selected. If another switch has a priority lower than 24576, it sets this switch's priority to 4096 less than the other switch's priority.
 - `spanning-tree vlan <vlan-number> root secondary` - sets the STP priority to secondary on the vlan number selected.
 - `spanning-tree vlan <vlan-number> cost` - change an interface's per VLAN spanning tree path cost
 - `spanning-tree vlan <vlan-number> port-priority` - change an interface's spanning tree port priority.
 - `interface port-channel <#>` - creates etherchannel/portchannel interface.
 - `port-channel load-balance ?` - change the etherchannel load-balance configuration to the displayed options.
 - `router rip`
 - `version 2`
 To set a default route to the internet you can use the `ip route` command with the least specific destination ip address, i.e. `ip route 0.0.0.0 0.0.0.0 <SELECT EXIT-INTERFACE or NEXT-HOP IP or both>`

To cancel commands, use `no` before the command of interest. For example, to avoid future passwords from automatically being encrypted, run `no service password-encryption`. 

## Interface Config Mode
To enter interface configuration mode, you must be in global config mode, then enter `interface <INTERFACE NAME>` | `in <INTERFACE NAME>`. For example, `interface gigabitethernet 0/0` | `in g0/0`.

You can also create loop back interfaces. For example `interface loopback <number` | `int l <#>`, this creates a loop back interface of the specified number that can be configured like any other interface.

To configure several interfaces all at once, type `interface range <INTERFACE START> - <INTERFACE END>`

- `ip address <IP ADDRESS> <NETMASK>` | `ip add <IP ADDRESS> <NETMASK>` - used to set IP address. For example, `ip address 10.255.255.254 255.0.0.0` for an ip address that is equal to the Class A address, 10.255.255.254/8.
- `shutdown` - used to disable interface on network device.
- `no shutdown` | `no shut` - used to enable the interface on the network device. Note: Cisco router interface have the `shutdown` command applied to them by default.
- `description <YOUR DECRIPTION> | desc <YOUR DECRIPTION>` - used to add interface description.
- `speed <SPEED>` - sets interface speed: 10, 100, auto, etc.
- `duplex <DUPLEX>` - sets interface duplex: auto, full, half.
- `no switchport` - configures the interface on a multilayer switch as a layer 3 routed port, i.e. not a switchport.
- `switchport mode access` - sets interface as an access port, a switchport which belongs to a single VLAN, and usually connects to end hosts like PCs.
- `switchport mode trunk` - sets interface as an trunk port, a switchport which belongs to multiple VLANs. Before this works, the trunk encapsulation must be set to 802.1Q or ISL. However, on switches that only support 802.1Q, this is not necessary. By default all VLANs are allowed on the trunk.
- `switchport mode dynamic` - sets mode to dynamically negotiate access or trunk mode
- `switchport mode dynamic ?` - give option to set `auto` or `desirable`
    - `switchport mode dynamic auto` - will NOT actively try to form a trunk with other Cisco switches, however it will form a trunk if the switch connected to it is actively trying to form a trunk. It will for a trunk with a switchport in the following modes:
        - switchport mode trunk
        - switchport mode dynamic desirable
    - `switchport mode dynamic desirable` - will actively try to form a trunk with other Cisco switches. It will form a trunk if connected to another switchport in the following modes:
        - switchport mode trunk
        - switchport mode dynamic desirable
        - switchport mode dynamic auto
- `switchport nonegotiate` - Used to disable DTP negotiation on an interface.
- `switchport trunk encapsulation dot1q` - sets trunk encapsulation mode to 802.1Q on **switch** interface. 
- `switchport access vlan <VLAN #>` - sets switchport to vlan number specified. 
- `switchport trunk allowed vlan ?` - sets VLANs allowed on trunk port. input`?` to see all options
- `switchport trunk native vlan <VLAN #>` - changes the native VLAN on the switch. Make sure the native VLAN matches between switches in your network.
- `default interface <interface number>` - resets setting on the interface.
- `interface vlan<vlan#>` - for example, `interface vlan10`, sets vlan on SVIs or switch virtual interfaces. 
    - SVIs are shutdown by default so remember to use `no shutdown` to enable the interface. 
    - The VLAN must exist on the switch to work. Remember to add the VLAN in global config mode.
    - The switch must have at least one access port in the VLAN in an up/up state, AND/OR one trunk port that allows the VLAN that is in an up/up state.
    - The VLAN must not be shutdown (you can use the `shutdown` command to disable a VLAN)
- `span vlan <vlan-number> cost <cost>` - adjust vlan cost of interface.
- `spanning-tree portfast` - enables portfast on the interface (recommended only for end hosts)
- `spanning-tree bpduguard enable` - if an interface with BPDU guard enabled receives a BPDU from another switch, the interface will be shut down to prevent a loop from forming.
- `channel-group <#> mode ?` - provides configuration options for an etherchannel. The channel-group number has to match for member interfaces on the same switch. However, it doesn't have to match the channel-group number on the other switch
- `channel-protocol ?` - provides manual configuration options for etherchannel protocol... this command is not very useful considering this is already done automatically when you select the `channel-group <#> mode`. 
  
## Subinterface Config Mode
To create a *Router on a Stick* (ROAS) you can create subinterfaces by entering subinterface configuration mode. For example, `interface g0/0.10` creates a subinterface 10 on port g0/0... it is highly recommended for the subinterface number to match the VLAN number!
- `encapsulation dot1q <vlan-id>` - sets encapsulation mode to 802.1Q on the vlan id provided.
- `encapsulation dot1q <vlan-id> native` - tells the router that the subinterface belongs to the native VLAN. It will assume that untagged frames belong to the native VLAN. Alternatively, you can also just set the native VLAN on the router's physical interface.
- `ip address <ADDRESS> <NETMASK>` - sets ip address and net mask on sub interface.

## VLAN Config Mode
To enter VLAN configuration mode, you must be in global config mode, then enter `vlan <VLAN #>`. Fo,. example, `vlan 1`.
- `name` - provides name to vlan.
- `shutdown` - disables the VLAN.

## RIP Config Mode
To enter RIP configuration mode, you must be in global config mode, then enter `router rip`. 
- `version 2` - configures to router to use RIPv2
- `no auto-summary` - auto-summary is on by default, and it automatically turns the networks the router is connected with to classful-networks, which are used on RIPv1... this is not something you want so turning it off is necessary to use RIPv2 which is typical in modern networks.
- `network <IP address>` - The RIP 'network' command is classful, it will automatically convert to classful networks. For example, even if you enter the command network 10.0.12.0, it will be converted to network 10.0.0.0 (a class A network). Because of this behavior, there is no need to enter the network mask. The network command tells the router to:
    - look for interfaces with an IP address that is in the specified range.
    - activate RIP on the interfaces that fall into that range.
    - Form adjacencies with connected RIP neighbors
    - advertise the network prefix of the interface (NOT the prefix in the network command)
    - note: the OSPF and EIGRP network commands operate in the same way.
    - if there are no RIP neighbors connected to an interface, the router will still continue to send RIP advertisements. This is unnecessary traffic, that is unless the interface is configured as a **passive-interface**.
- `passive-interface <interface id>` - configures interface as passive, for example 'passive-interface g2/0'. It tells the router to stop sending RIP advertisements out of the specified interface. However, the router will still continue to advertise the network prefix of the interface to its RIP neighbors on active RIP interfaces. EIGRP and OSPF both have the same passive interface functionality, using the same command.
- `default-information originate` - advertises default gateway information to other routers, for example when you want a router to advertise its access point to the internet.
- `maximum-paths <number>` - sets the maximum number of paths allowed between two destinations for load balancing, for example 'maximum-paths 4' sets a maximum number of 4 paths available between routers for load balancing. 
- `distance <number>` - overrides the default administrative distance for RIP configuration mode.

## EIGRP Config Mode
To enter EIGRP configuration mode, you must be in global config mode, then enter `router eigrp <Autonomous System #>`. For example, 'router eigrp 1'. The AS (autonomous system) number must match between routers, or they will not form an adjacency and share route information.
- `no auto-summary` - auto-summary may or may not be on by default, but it automatically turns the networks the router is connected with to classful-networks... this is not something you want on modern networks so making sure to turn it off is typical.
- `network <IP address> <wildcard mask>` - The EIGRP 'network' command is classful unless you provide a wildcard mask that enables EIGRP on a specific interface. A wildcard mask is basically an 'inverted' subnet mask. All 1s in the subnet mask are 0 in the equivalent wildcard mask. All 0s in the subnet mask are 1 in the equivalent wildcard mask. 
    - A '0' in the wildcard mask = must match. 
    - A '1' in the wildcard mask = don't have to match.
    - Note: to enable EIGRP on all interfaces, you can run `network 0.0.0.0 255.255.255.255`

If you enter the command 'network 10.0.12.0', it will be converted to network 10.0.0.0 (a class A network) and grab interfaces with ip addresses like 10.0.12.0 and 10.0.7.0 if they exist, for example. The network command tells the router to:
    - look for interfaces with an IP address that is in the specified range (without a mask) or the specific IP address (with mask).
    - activate EIGRP on the interface with the specified IP (with mask) or interfaces that fall into the classful range (without mask).
    - Form adjacencies with connected EIGRP neighbors
    - advertise the network prefix of the interface (NOT the prefix in the network command)
    - note: the OSPF and EIGRP network commands operate in the same way.
    - if there are no EIGRP neighbors connected to an interface, the router will still continue to send EIGRP advertisements. This is unnecessary traffic, that is unless the interface is configured as a **passive-interface**.

- `passive-interface <interface id>` - configures interface as passive, for example 'passive-interface g2/0'. It tells the router to stop sending EIGRP advertisements out of the specified interface. However, the router will still continue to advertise the network prefix of the interface to its EIGRP neighbors on active EIGRP interfaces. EIGRP and OSPF both have the same passive interface functionality, using the same command.
- `default-information originate` - advertises default gateway information to other routers, for example when you want a router to advertise its access point to the internet.
- `maximum-paths <number>` - sets the maximum number of paths allowed between two destinations for load balancing, for example 'maximum-paths 4' sets a maximum number of 4 paths available between routers for load balancing. 
- `distance <number>` - overrides the default administrative distance for EIGRP configuration mode.
- `eigrp router-id <32 bit number>` - configure a EIGRP router id, for example, 'eigrp router-id 1.1.1.1' 
- `variance <multiplier>` - if variance is set to 1, EIGRP will use ECMP (Equal Cost Multi-Path) load-balancing. Otherwise, if 'variance 2' is used, feasible successor routes with an FD (feasible distance) up to 2x the successor route's FD can be used to load-balance. 
    - EIGRP will only perform unequal-cost load-balancing over feasible successor routes.
    - If a route doesn't meet the feasibility requirement, it will NEVER be selected for load-balancing, regardless of variance.

# Configuration
 **Running-Config**
 Configuration for changes made during runtime
 
 **Startup-Config**
 Configuration preloaded for startup
 
# Shortcuts
 - many commands have a shortcut, for example: `conf t` is the shortcut for `configure terminal`...
 - Use `tab` to complete commands... 
 - Use `?` to search for command options
