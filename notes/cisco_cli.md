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
 - `show interfaces decription` - Provides table of interfaces with decription included.
 - `show interfaces status` - shows interface information like port, name, status, Vlan, duplex, speed, and type.
 - `show ip route` - shows the routers ip routing table. If prompt state *Gateway of last resort is not set* then a default route has not been configured yet.
 - `show vlan brief` - shows VLANs setup on a switch with associated ports. VLANs 1, 1002-1005 exist by default and cannot be deleted.
- `show interfaces trunk` - shows vlans allowed on trunk port

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

 To set a default route to the internet you can use the `ip route` command with the least specific destination ip address, i.e. `ip route 0.0.0.0 0.0.0.0 <SELECT EXIT-INTERFACE or NEXT-HOP IP or both>`

To cancel commands, use `no` before the command of interest. For example, to avoid future passwords from automatically being encrypted, run `no service password-encryption`. 

## Interface Config Mode
To enter interface configuration mode, you must be in global config mode, then enter `interface <INTERFACE NAME>` | `in <INTERFACE NAME>`. For example, `interface gigabitethernet 0/0` | `in g0/0`.

To configure several interfaces all at once, type `interface range <INTERFACE START> - <INTERFACE END>`

- `ip address <IP ADDRESS> <NETMASK>` | `ip add <IP ADDRESS> <NETMASK>` - used to set IP address. For example, `ip address 10.255.255.254 255.0.0.0` for an ip address that is equal to the Class A address, 10.255.255.254/8.
- `shutdown` - used to disable interface on network device.
- `no shutdown` | `no shut` - used to enable the interface on the network device. Note: Cisco router interface have the `shutdown` command applied to them by default.
- `description <YOUR DECRIPTION> | desc <YOUR DECRIPTION>` - used to add interface description.
- `speed <SPEED>` - sets interface speed: 10, 100, auto, etc.
- `duplex <DUPLEX>` - sets interface duplex: auto, full, half.
- `switchport mode access` - sets interface as an access port, a switchport which belongs to a single VLAN, and usually connects to end hosts like PCs.
- `switchport mode trunk` - sets interface as an trunk port, a switchport which belongs to multiple VLANs. Before this works, the trunk encapsulation must be set to 802.1Q or ISL. However, on switches that only support 802.1Q, this is not necessary. By default all VLANs are allowed on the trunk.
- `switchport trunk encapsulation dot1q` - sets trunk encapsulation mode to 802.1Q on **switch** interface. 
- `switchport access vlan <VLAN #>` - sets switchport to vlan number specified. 
- `switchport trunk allowed vlan ?` - sets VLANs allowed on trunk port. input`?` to see all options
- `switchport trunk native vlan <VLAN #>` - changes the native VLAN on the switch. Make sure the native VLAN matches between switches in your network.

## Subinterface Config Mode
To create a *Router on a Stick* (ROAS) you can create subinterfaces by entering subinterface configuration mode. For example, `interface g0/0.10` creates a subinterface 10 on port g0/0... it is highly recommended for the subinterface number to match the VLAN number!
- `encapsulation dot1q 10` - sets encapsulation mode to 802.1Q on VLAN 10
- `ip address <ADDRESS> <NETMASK>` - sets ip address and net mask on sub interface.

## VLAN Config Mode
To enter VLAN configuration mode, you must be in global config mode, then enter `vlan <VLAN #>`. For example, `vlan 1`.
- `name` - provides name to vlan.

# Configuration
 **Running-Config**
 Configuration for changes made during runtime
 
 **Startup-Config**
 Configuration preloaded for startup
 
# Shortcuts
 - many commands have a shortcut, for example: `conf t` is the shortcut for `configure terminal`...
 - Use `tab` to complete commands... 
 - Use `?` to search for command options