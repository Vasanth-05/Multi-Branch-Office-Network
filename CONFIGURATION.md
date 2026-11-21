# Network Configuration Commands

This document contains the Cisco IOS configuration commands used in the Multi-Branch Office Network project.

## 1. HQ Router Configuration
Cisco 2911 – Router-on-a-Stick, DHCP, WAN and Static Routing.

enable
configure terminal
hostname HQ-Router

! Configure VLAN 10 - Sales
interface gigabitEthernet 0/0.10
 encapsulation dot1Q 10
 ip address 192.168.10.1 255.255.255.0
 no shutdown
exit

! Configure VLAN 20 - HR
interface gigabitEthernet 0/0.20
 encapsulation dot1Q 20
 ip address 192.168.20.1 255.255.255.0
 no shutdown
exit

! Configure VLAN 30 - IT
interface gigabitEthernet 0/0.30
 encapsulation dot1Q 30
 ip address 192.168.30.1 255.255.255.0
 no shutdown
exit

! Enable physical LAN interface
interface gigabitEthernet 0/0
 no shutdown
exit

! Configure WAN interface
interface gigabitEthernet 0/1
 ip address 10.0.0.1 255.255.255.252
 no shutdown
exit

! Exclude gateway and reserved addresses
ip dhcp excluded-address 192.168.10.1 192.168.10.10
ip dhcp excluded-address 192.168.20.1 192.168.20.10
ip dhcp excluded-address 192.168.30.1 192.168.30.10

! DHCP for Sales
ip dhcp pool SALES
 network 192.168.10.0 255.255.255.0
 default-router 192.168.10.1
 dns-server 8.8.8.8
exit

! DHCP for HR
ip dhcp pool HR
 network 192.168.20.0 255.255.255.0
 default-router 192.168.20.1
 dns-server 8.8.8.8
exit

! DHCP for IT
ip dhcp pool IT
 network 192.168.30.0 255.255.255.0
 default-router 192.168.30.1
 dns-server 8.8.8.8
exit

! Static route to Branch LAN
ip route 192.168.40.0 255.255.255.0 10.0.0.2

end
write memory

## 2. Branch Router Configuration
Cisco 2911 – Branch LAN, DHCP, WAN and Default Static Route.

enable
configure terminal
hostname Branch-Router

! Configure Branch LAN
interface gigabitEthernet 0/0
 ip address 192.168.40.1 255.255.255.0
 no shutdown
exit

! Configure WAN
interface gigabitEthernet 0/1
 ip address 10.0.0.2 255.255.255.252
 no shutdown
exit

! Exclude gateway address
ip dhcp excluded-address 192.168.40.1

! Configure DHCP
ip dhcp pool BRANCH
 network 192.168.40.0 255.255.255.0
 default-router 192.168.40.1
 dns-server 8.8.8.8
exit

! Default route to HQ
ip route 0.0.0.0 0.0.0.0 10.0.0.1

end
write memory

## 3. HQ Switch Configuration
Cisco 2960 – VLAN creation, access ports and trunk configuration.

enable
configure terminal
hostname HQ-Switch

! Create VLANs
vlan 10
 name Sales
exit

vlan 20
 name HR
exit

vlan 30
 name IT
exit

! Configure trunk to HQ Router
interface fastEthernet 0/1
 switchport mode trunk
 switchport trunk allowed vlan 10,20,30
 no shutdown
exit

! Sales PCs
interface range fastEthernet 0/2 - 3
 switchport mode access
 switchport access vlan 10
 no shutdown
exit

! HR PCs
interface range fastEthernet 0/4 - 5
 switchport mode access
 switchport access vlan 20
 no shutdown
exit

! IT PC
interface fastEthernet 0/6
 switchport mode access
 switchport access vlan 30
 no shutdown
exit

end
write memory

## 4. Branch Switch Configuration
Cisco 2960 – Router and PC port configuration.

enable
configure terminal
hostname Branch-Switch

! Router connection
interface gigabitEthernet 0/1
 switchport mode access
 no shutdown
exit

! Branch PC connection
interface fastEthernet 0/1
 switchport mode access
 no shutdown
exit

end
write memory

## 5. Verification Commands

```cisco
show ip interface brief
show ip route
show ip dhcp binding
show vlan brief
show interfaces trunk
show running-config
```

## 6. Connectivity Testing

```cisco
ping 10.0.0.2
ping 192.168.40.11
ping 192.168.10.11
```

## Notes
- All configurations were implemented using Cisco Packet Tracer.
- The network uses VLANs, DHCP, Router-on-a-Stick and static routing.
- Refer to the Packet Tracer project file for the complete topology.
