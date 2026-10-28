# Cisco Router - RIPv2 & DHCP

This project demonstrates a multi-router network topology using Cisco routers. It implements **Inter-VLAN Routing**, **DHCP Services**, point-to-point serial links, and dynamic routing using **RIP version 2 (RIPv2)**.

---

## 🌐 IP Addressing Table

### Routers & Subinterfaces (LANs & Serial Links)

| Device | Interface | IP Address / Mask | Description |
| :--- | :--- | :--- | :--- |
| **Router 1** | Gi0/0.10 | 192.168.10.1/24 | VLAN 10 Gateway |
| | Gi0/0.20 | 192.168.20.1/24 | VLAN 20 Gateway |
| | Gi0/0.30 | 192.168.30.1/24 | VLAN 30 Gateway |
| | Gi0/1.10 | 192.168.40.1/24 | VLAN 40 Gateway |
| | Gi0/1.20 | 192.168.50.1/24 | VLAN 50 Gateway |
| | Gi0/1.30 | 192.168.60.1/24 | VLAN 60 Gateway |
| | Serial0/0/0 | 10.0.12.1/30 | Link to Router 2 |
| | Serial0/0/1 | 10.0.13.1/30 | Link to Router 3 |
| **Router 2** | Gi0/0.10 | 192.168.70.1/24 | VLAN 10 Gateway |
| | Gi0/1.10 | 192.168.75.1/24 | VLAN 10 Gateway |
| | Serial0/0/0 | 10.0.12.2/30 | Link to Router 1 |
| | Serial0/0/1 | 10.0.24.1/30 | Link to Router 4 |
| **Router 3** | Gi0/0.8 | 192.168.150.1/24 | VLAN 8 Gateway |
| | Gi0/0.9 | 192.168.140.1/24 | VLAN 9 Gateway |
| | Gi0/1.40 | 192.168.130.1/24 | VLAN 40 Gateway |
| | Serial0/0/0 | 10.0.13.2/30 | Link to Router 1 |
| | Serial0/0/1 | 10.0.34.1/30 | Link to Router 4 |
| **Router 4** | Gi0/0.120 | 192.168.110.1/24 | VLAN 120 Gateway |
| | Gi0/1.40 | 192.168.90.1/24 | VLAN 40 Gateway |
| | Gi0/1.902 | 192.168.100.1/24 | VLAN 902 Gateway |
| | Gi0/1.1005 | 192.168.160.1/24 | VLAN 1005 Gateway |
| | Serial0/0/0 | 10.0.34.2/30 | Link to Router 3 |
| | Serial0/0/1 | 10.0.24.2/30 | Link to Router 2 |

---

## Network Protocols Used

1. **DHCP (Dynamic Host Configuration Protocol):**
   * Configured on all routers to automatically assign IP addresses to end devices across different VLANs.
   * Includes IP exclusions (`ip dhcp excluded-address`) to reserve gateway addresses.

2. **RIPv2 (Routing Information Protocol version 2):**
   * Used as the dynamic routing protocol to allow routers to share network reachability information across the square topology.
   * Configured with `no auto-summary` to support VLSM and ensure accurate route propagation.

---

## My Mistake: IP Overlapping Error

During the initial phase of the project, a major issue occurred: **IP Overlapping**. 
* **What happened:** Duplicate IP addresses and subnets were accidentally assigned across different routers (for example, the `192.168.50.0/24` and `192.168.60.0/24` ranges were repeated on both Router 1 and Router 2/3). 
* **The consequence:** This created routing loops, address conflicts, and prevented RIPv2 from converging correctly because the routers could not distinguish unique destination paths.

---

## The Fix: Reassigning Unique Subnets

To resolve the overlapping issue, we completely redesigned the conflicting subnets, assigning unique ranges to Router 2, Router 3, and Router 4, and properly defined `/30` point-to-point networks for the serial links.
Here is an example of the configuration used to clean up the conflicting pools and interfaces (applied to **Router 2**):

```cisco
configure terminal
! Remove old overlapping DHCP pools and excluded addresses
no ip dhcp pool vlan10-red1
no ip dhcp pool vlan10-red2
no ip dhcp excluded-address 192.168.120.1 192.168.120.10
no ip dhcp excluded-address 192.168.50.1 192.168.50.10

! Set up new clean DHCP pools
ip dhcp excluded-address 192.168.70.1 192.168.70.10
ip dhcp pool vlan70-red1
 network 192.168.70.0 255.255.255.0
 default-router 192.168.70.1
 dns-server 8.8.8.8

ip dhcp excluded-address 192.168.75.1 192.168.75.10
ip dhcp pool vlan75-red2
 network 192.168.75.0 255.255.255.0
 default-router 192.168.75.1
 dns-server 8.8.8.8

! Update interface IP addresses
interface GigabitEthernet0/0.10
 ip address 192.168.70.1 255.255.255.0

interface GigabitEthernet0/1.10
 ip address 192.168.75.1 255.255.255.0

end
write memory
