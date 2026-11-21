# Multi-Branch Office Network with VLANs and Static Routing

## Project Overview

This project demonstrates the design and implementation of a multi-branch office network using Cisco Packet Tracer.

The network connects a Head Office (HQ) and a Branch Office using routers, switches, VLAN segmentation, DHCP, and static routing.

The objective is to provide organized network segmentation, automatic IP address allocation, and communication between two office locations.

## Network Topology

The network consists of:

- Head Office (HQ)
  - 1 Cisco 2911 Router
  - 1 Cisco 2960 Switch
  - 5 PCs across three VLANs
- Branch Office
  - 1 Cisco 2911 Router
  - 1 Cisco 2960 Switch
  - 1 PC

## Technologies Used

- Cisco Packet Tracer
- VLAN Configuration
- Inter-VLAN Routing (Router-on-a-Stick)
- DHCP Configuration
- Static Routing
- IPv4 Addressing
- Network Connectivity Testing

## IP Addressing Scheme

| Network | VLAN | Subnet | Default Gateway |
|---|---|---|---|
| Sales | 10 | 192.168.10.0/24 | 192.168.10.1 |
| HR | 20 | 192.168.20.0/24 | 192.168.20.1 |
| IT | 30 | 192.168.30.0/24 | 192.168.30.1 |
| Branch LAN | — | 192.168.40.0/24 | 192.168.40.1 |
| WAN | — | 10.0.0.0/30 | HQ: 10.0.0.1 / Branch: 10.0.0.2 |

## Key Configurations

### VLAN Segmentation

Created three VLANs at HQ:
- VLAN 10 – Sales
- VLAN 20 – HR
- VLAN 30 – IT

### Inter-VLAN Routing

Implemented router-on-a-stick using 802.1Q encapsulation and router subinterfaces.

### DHCP

Configured DHCP pools on both routers to automatically assign IP addresses to client PCs.

### Static Routing

- HQ Router: Static route to the Branch LAN through 10.0.0.2.
- Branch Router: Default route through 10.0.0.1.

## Testing and Verification

The following tests were performed successfully:

- Verified VLAN membership.
- Verified DHCP address allocation for all six PCs.
- Tested connectivity between HQ VLANs.
- Tested WAN connectivity between routers.
- Tested communication between HQ and Branch PCs.
- Verified routing tables on both routers.

## Project Files

- `Multi-Branch-Office-Network.pkt` – Cisco Packet Tracer project file.
- `screenshots/` – Network topology, VLAN, DHCP, routing, and connectivity screenshots.

## Learning Outcomes

- Understanding VLAN-based network segmentation.
- Configuring inter-VLAN routing.
- Implementing DHCP services.
- Configuring static routes between different networks.
- Troubleshooting and verifying network connectivity.

## Tools

Cisco Packet Tracer

---

**Project Status:** Completed and tested successfully.
