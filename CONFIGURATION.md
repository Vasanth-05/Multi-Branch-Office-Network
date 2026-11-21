# Network Configuration Commands

This document contains the Cisco IOS configuration commands used in the **Multi-Branch Office Network with VLANs and Static Routing** project, implemented using Cisco Packet Tracer.

---

## 1. HQ Router Configuration

**Device:** Cisco 2911 Router

**Purpose:** Router-on-a-Stick, inter-VLAN routing, DHCP, WAN connectivity and static routing.

```cisco
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
```

---

## 2. Branch Router Configuration

**Device:** Cisco 2911 Router

**Purpose:** Branch LAN configuration, DHCP, WAN connectivity and default static routing.

```cisco
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
```

---

## 3. HQ Switch Configuration

**Device:** Cisco 2960 Switch

**Purpose:** VLAN creation, access port assignment and trunk configuration.

```cisco
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
```

---

## 4. Branch Switch Configuration

**Device:** Cisco 2960 Switch

**Purpose:** Connect the Branch Router and Branch PC.

```cisco
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
```

---

## 5. Verification Commands

Use the following commands to verify the device configurations.

### Router Verification

```cisco
show ip interface brief
show ip route
show ip dhcp binding
show running-config
```

### Switch Verification

```cisco
show vlan brief
show interfaces trunk
show running-config
```

---

## 6. Connectivity Testing

Use these commands to verify communication between the HQ and Branch networks.

### From HQ Router

```cisco
ping 10.0.0.2
ping 192.168.40.11
```

### From Branch Router

```cisco
ping 10.0.0.1
ping 192.168.10.11
```

### From HQ PC

```text
ping 192.168.40.11
```

---

## 7. Network Addressing Summary

| Network | IP Address | Default Gateway |
|---|---|---|
| Sales VLAN 10 | 192.168.10.0/24 | 192.168.10.1 |
| HR VLAN 20 | 192.168.20.0/24 | 192.168.20.1 |
| IT VLAN 30 | 192.168.30.0/24 | 192.168.30.1 |
| Branch LAN | 192.168.40.0/24 | 192.168.40.1 |
| WAN Link | 10.0.0.0/30 | HQ: 10.0.0.1 / Branch: 10.0.0.2 |

---

## 8. Project Notes

- Configured using Cisco Packet Tracer.
- Implemented VLAN segmentation for Sales, HR and IT departments.
- Configured inter-VLAN routing using Router-on-a-Stick.
- Implemented DHCP services for automatic IP address allocation.
- Configured static routing between HQ and Branch networks.
- Verified end-to-end connectivity using ICMP ping tests.

**Project Repository:**  
[Multi-Branch Office Network with VLANs and Static Routing](https://github.com/Vasanth-05/Multi-Branch-Office-Network)
