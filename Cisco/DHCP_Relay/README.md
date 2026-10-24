# Multi-Router Inter-VLAN Routing and DHCP Lab

## Overview
A simple lab built in Cisco Packet Tracer to connect two routers, set up Inter-VLAN routing, configure static routes, and run a local DHCP server.

---

## Topology & IPs

* **Inter-Ro	uter Link:** `10.0.0.0/30`
  * Router 1: `10.0.0.2`
  * Router 2: `10.0.0.1`
* **VLAN 10:** `192.168.10.0/24` (Gateway: `192.168.10.1`)
* **VLAN 20:** `192.168.20.0/24` (Gateway: `192.168.20.1`)

---

## Step-by-Step Configuration

### 1. Point-to-Point Link
Set the IPs on the link connecting the two routers.

**Router 2:**
```
conf t
int gi0/1
 ip address 10.0.0.1 255.255.255.252
 no shutdown

```
**Router 1:**

```
conf t
int gi0/0
 ip address 10.0.0.2 255.255.255.252
 no shutdown
```

### 2. Sub-Interfaces ( Router 2)
```
conf t

int gi0/0.10
 encapsulation dot1Q 10
 ip address 192.168.10.1 255.255.255.0

int gi0/0.20
 encapsulation dot1Q 20
 ip address 192.168.20.1 255.255.255.0
```

### 3. Static Routes ( Router 1)
```
conf t
ip route 192.168.10.0 255.255.255.0 10.0.0.1
ip route 192.168.20.0 255.255.255.0 10.0.0.1
```

### 4. DCHP Configuration ( Router 2)

```
conf t

ip dhcp excluded-address 192.168.10.1 192.168.10.15
ip dhcp excluded-address 192.168.20.1 192.168.20.15

ip dhcp pool VLAN10
 network 192.168.10.0 255.255.255.0
 default-router 192.168.10.1
 dns-server 8.8.8.8

ip dhcp pool VLAN20
 network 192.168.20.0 255.255.255.0
 default-router 192.168.20.1
 dns-server 8.8.8.8

```
