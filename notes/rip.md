# RIP
## About
- Uses routing-by-rumor logic. (Distance vector IGP)
- Uses hop count as its metric. The maximum hop count is 15. (bandwidth is irrelevant)

## Versions
Has three versions 
- RIPv1: used for IPv4, only support classful addresses, i.e. class A, B, C...
- RIPv2: used for IPv4, supports VLSM, CIDR, and provides subnet mask info in its advertisements, messages are multicast to 224.0.0.9. Multicast messages are delivered to devices that have joined that specific multicast group.
- RIPng: used for IPV6

## Messages
- Has two message types, **Request**, to ask RIP-enabled neighbor routers to send their routing table, and **Response**, to send the local router's routing table to neighboring routers. 
- By default RIP-enabled routers will share their routing table every 30 seconds.