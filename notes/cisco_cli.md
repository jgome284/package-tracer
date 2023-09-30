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

## Global Config Mode
 Enter with `configure terminal`
 Enter `exit` to leave mode

 If you are in another mode, you can elevate commands global config modeby typing `do` infront of the command of interest. 

 Global Commands include:
 - `enable password <YOUR PASSWORD HERE>` - enables `<YOUR PASSWORD HERE>` as the password
 - `service password-encryption` - used to encrypt password so that it does not display on `show running-config` command.
 - `enable secret <YOUR SECRET HERE>` enables `<YOUR SECRET HERE>` as a secret which offers more robust encryption
 - `hostname` - change hostname
 

To cancel commands, use `no` before the command of interest. For example, to avoid future passwords from automatically being encrypted, run `no service password-encryption`. 



# Configuration
 **Running-Config**
 Configuration for changes made during runtime
 
 **Startup-Config**
 Configuration preloaded for startup
 
# Shortcuts
 - many commands have a shortcut, for example: `conf t` is the shortcut for `configure terminal`...
 - Use `tab` to complete commands... 
 - Use `?` to search for command options