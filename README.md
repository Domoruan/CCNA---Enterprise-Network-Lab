# CCNA-Enterprise-Network-Lab
Documentation of completed network designed in packet tracer

Downlaod the .pkt file (Enterprise Segmentation) to access the lab in packet tracer

# Enterprise Network Lab — VLANs, ROAS, DHCP, ACLs

## Overview
This project simulates a small enterprise network using Cisco Packet Tracer. The lab includes VLAN segmentation, router-on-a-stick inter-VLAN routing, DHCP pools, trunking, access ports, and extended ACLs to control traffic between VLANs.

The goal of this lab was to practice real-world network administration tasks including network segmentation, IP addressing, routing, DHCP configuration, access control, and troubleshooting using Cisco IOS commands.

## Network Design

| VLAN | Purpose | Subnet | Default Gateway |
|---|---|---|---|
| VLAN 10 | Users | 192.168.10.0/24 | 192.168.10.1 |
| VLAN 20 | Admin/IT | 192.168.20.0/24 | 192.168.20.1 |

## Configurations

### Router-on-a-Stick
Configured router subinterfaces for each VLAN using 802.1Q encapsulation.

```bash
interface GigabitEthernet0/0/0.10
encapsulation dot1Q 10
ip address 192.168.10.1 255.255.255.0

interface GigabitEthernet0/0/0.20
encapsulation dot1Q 20
ip address 192.168.20.1 255.255.255.0
```

### DHCP Pools
```bash
VLAN 10
ip dhcp pool VLAN10
network 192.168.10.0 255.255.255.0
default-router 192.168.10.1
dns-server 8.8.8.8

VLAN 20 
ip dhcp pool VLAN20
network 192.168.20.0 255.255.255.0
default-router 192.168.20.1
dns-server 8.8.8.8
```

### Extended ACL
```bash
access-list 100 deny ip 192.168.10.0 0.0.0.255 192.168.20.0 0.0.0.255
access-list 100 deny ip 192.168.20.0 0.0.0.255 192.168.10.0 0.0.0.255
access-list 100 permit ip any any
```

### Applied ACL Inbound on both interface 
```bash
interface GigabitEthernet0/0/0.10
ip access-group 100 in

interface GigabitEthernet0/0/0.20
ip access-group 100 in
```

## Testing and Validation
- Verified that devices in VLAN 10 and VLAN 20 received valid DHCP addresses from their assigned DHCP pools.
- Confirmed that router subinterfaces were configured with the correct 802.1Q VLAN tags and default gateway IP addresses.
- Verified that the switch port connected to the router was operating as a trunk and allowing VLAN 10 and VLAN 20.
- Tested inter-VLAN routing between VLAN 10 and VLAN 20 before applying ACL restrictions.
- Applied extended ACLs to router subinterfaces and confirmed that traffic between VLAN 10 and VLAN 20 was blocked.
- Used 'show' commands to verify configurations 

## Skills Demonstrated
- VLAN segmentation
- Router-on-a-stick configuration
- DHCP scope configuration
- Extended ACL implementation
- Trunk port configuration
- Cisco IOS troubleshooting commands
