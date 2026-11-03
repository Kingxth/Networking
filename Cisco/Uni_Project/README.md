# Networking (Uni Practice)

## Obj.
Implement a network topology using VLSM, VLANs, OSPF, DHCP, DNS, and WAN links between routers.
All this occurred in my class :D

---

## 1. VLAN B

Private blocks assigned by the instructor: `172.16.0.0/16` and onwards.

| VLAN | Network Address / Mask | IP Range | Gateway | Broadcast |
| :---: | :--- | :--- | :--- | :--- |
| 2003 | 172.16.0.0/21 | 172.16.0.1 - 172.16.7.254 | 172.16.0.1 | 172.16.7.255 |
| 628 | 172.17.0.0/22 | 172.17.0.1 - 172.17.3.254 | 172.17.0.1 | 172.17.3.255 |
| 515 | 172.18.0.0/22 | 172.18.0.1 - 172.18.3.254 | 172.18.0.1 | 172.18.3.255 |
| 472 | 172.19.0.0/23 | 172.19.0.1 - 172.19.1.254 | 172.19.0.1 | 172.19.1.255 |
| 396 | 172.20.0.0/23 | 172.20.0.1 - 172.20.1.254 | 172.20.0.1 | 172.20.1.255 |

---

## 2. VLAN C

Private block used: `192.168.0.0/16`.

| VLAN | Network Address / Mask | IP Range | Gateway | Broadcast |
| :---: | :--- | :--- | :--- | :--- |
| 170 | 192.168.11.0/24 | 192.168.11.1 - 192.168.11.254 | 192.168.11.1 | 192.168.11.255 |
| 170 | 192.168.12.0/24 | 192.168.12.1 - 192.168.12.254 | 192.168.12.1 | 192.168.12.255 |
| 45 | 192.168.13.0/26 | 192.168.13.1 - 192.168.13.62 | 192.168.13.1 | 192.168.13.63 |
| 110 | 192.168.14.0/25 | 192.168.14.1 - 192.168.14.126 | 192.168.14.1 | 192.168.14.127 |
| 21 | 192.168.15.0/27 | 192.168.15.1 - 192.168.15.30 | 192.168.15.1 | 192.168.15.31 |
| 170 | 192.168.16.0/24 | 192.168.16.1 - 192.168.16.254 | 192.168.16.1 | 192.168.16.255 |
| 30 | 192.168.10.0/27 | 192.168.10.1 - 192.168.10.30 | 192.168.10.1 | 192.168.10.31 |
| 10 | 192.168.10.32/28 | 192.168.10.33 - 192.168.10.46 | 192.168.10.33 | 192.168.10.47 |
| 5 | 192.168.4.16/29 | 192.168.4.17 - 192.168.4.22 | 192.168.4.17 | 192.168.4.23 |

---

## 3. Management Networks

Management networks are used for network administration and device management.

| Network | Network Address / Mask | IP Range | Gateway | Broadcast |
| :---: | :--- | :--- | :--- | :--- |
| R1/R2 Management | 192.168.99.0/24 | 192.168.99.1 - 192.168.99.254 | 192.168.99.1 | 192.168.99.255 |
| R3 Management | 192.168.100.0/24 | 192.168.100.1 - 192.168.100.254 | 192.168.100.1 | 192.168.100.255 |
| R5 Management | 192.168.101.0/24 | 192.168.101.1 - 192.168.101.254 | 192.168.101.1 | 192.168.101.255 |

---

## 4. WAN Links Between Routers

Point-to-point links between routers use a `/30` mask.

| Link | Network Address / Mask | Router A IP | Router B IP | Broadcast |
| :---: | :--- | :--- | :--- | :--- |
| WAN 1 (R1-R2) | 192.168.4.24/30 | 192.168.4.25 | 192.168.4.26 | 192.168.4.27 |
| WAN 2 (R2-R3) | 192.168.4.28/30 | 192.168.4.29 | 192.168.4.30 | 192.168.4.31 |
| WAN 3 (R3-R5) | 192.168.4.32/30 | 192.168.4.33 | 192.168.4.34 | 192.168.4.35 |
| WAN 4 (R4-R5) | 192.168.4.36/30 | 192.168.4.37 | 192.168.4.38 | 192.168.4.39 |
| WAN 5 (R5-R6) | 192.168.4.40/30 | 192.168.4.41 | 192.168.4.42 | 192.168.4.43 |

---

## 5. DHCP and DNS Server

A centralized DHCP and DNS server is connected to R6.

| Parameter | Configuration |
| :--- | :--- |
| Server IP Address | 192.168.200.2 |
| Subnet Mask | 255.255.255.0 |
| Default Gateway | 192.168.200.1 |
| DNS Server | 192.168.200.2 |
| Connected Router | R6 |
| Router Interface | GigabitEthernet0/0 |
| Network Address | 192.168.200.0/24 |

The routers use DHCP Relay with the following command to forward DHCP requests to the centralized server:

```bash
ip helper-address 192.168.200.2
```

---

## 6. Router R6 Addressing

| Interface | IP Address | Subnet Mask | Description |
| :--- | :--- | :--- | :--- |
| GigabitEthernet0/0 | 192.168.200.1 | 255.255.255.0 | DHCP/DNS Server LAN |
| Serial0/0/0 | 192.168.4.42 | 255.255.255.252 | WAN connection to R5 |

---

## 7. OSPF Configuration

OSPF is used as the dynamic routing protocol to exchange routing information between all six routers.

| Router | Router ID | OSPF Process | Area |
| :---: | :---: | :---: | :---: |
| R1 | 1.1.1.1 | 1 | 0 |
| R2 | 2.2.2.2 | 1 | 0 |
| R3 | 3.3.3.3 | 1 | 0 |
| R4 | 4.4.4.4 | 1 | 0 |
| R5 | 5.5.5.5 | 1 | 0 |
| R6 | 6.6.6.6 | 1 | 0 |

---

## 8. Subnet Masks Used

| CIDR Mask | Decimal Mask | Usable Hosts |
| :---: | :--- | :---: |
| /21 | 255.255.248.0 | 2046 |
| /22 | 255.255.252.0 | 1022 |
| /23 | 255.255.254.0 | 510 |
| /24 | 255.255.255.0 | 254 |
| /25 | 255.255.255.128 | 126 |
| /26 | 255.255.255.192 | 62 |
| /27 | 255.255.255.224 | 30 |
| /28 | 255.255.255.240 | 14 |
| /29 | 255.255.255.248 | 6 |
| /30 | 255.255.255.252 | 2 |

---

## 9. Technologies Implemented

- VLSM (Variable Length Subnet Masking)
- VLAN Segmentation
- Router-on-a-Stick
- OSPF Dynamic Routing
- OSPF Area 0
- Centralized DHCP Server
- DNS Services
- DHCP Relay (`ip helper-address`)
- Inter-VLAN Routing
- Serial WAN Connections
- Point-to-Point Networking
- Management Networks
- Default Gateway Configuration
- ICMP Connectivity Testing (Ping)
- Cisco Packet Tracer Simulation
---

## 10. Network Testing

Network connectivity was verified using ICMP ping tests between routers and end devices.
OSPF neighbor adjacencies were checked using:

```bash
show ip ospf neighbor
```

Routing tables were verified using:

```bash
show ip route
show ip route ospf
```

DHCP functionality was tested by configuring end devices to obtain IP addresses automatically.

The network successfully exchanged routing information and provided connectivity between the configured network segments.

---
