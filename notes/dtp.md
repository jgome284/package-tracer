# Dynamic Trunking Protocol (DTP)
DTP is a Cisco proprietary protocol used to negotiate the trunking (tagging) of Ethernet frames between two switches or between a switch and a router. Here are some key points about DTP:

1. **Trunk Negotiation:** DTP allows switches to dynamically negotiate whether a link between them should function as a trunk link or as an access link. A trunk link carries traffic for multiple VLANs and uses VLAN tagging to differentiate between the VLANs.

2. **Modes:** DTP supports several modes:
   - **Auto:** In this mode, a switch will listen for DTP messages from the other end of the link but won't actively initiate trunking.
   - **Desirable:** In this mode, a switch actively tries to form a trunk with the neighboring switch.
   - **On:** This mode forces the switch to be a trunk link regardless of the configuration on the other side.
   - **Nonegotiate:** This mode disables DTP negotiation, ensuring the link always operates as a trunk.

3. **Security Implications:** DTP can be a security concern if not configured correctly. For security reasons, many network administrators disable DTP on access ports to prevent unauthorized trunking.

DTP will not form a trunk with a router, PC, etc. The switchport will be in access mode. On older switches, `switchport mode dynamic desirable` is the default administrative mode. On newer switches `switchport mode dynamic auto` is the default administrative mode.

Switches that support both 802.1Q and ISL trunk encapsulations can use DTP to negotiate the encapsulation they will use. This negotiation is enabled by default, as the default trunk encapsulation mode is: `switchport trunk ecapsulation negotiate`. ISL is favored over 802.1Q