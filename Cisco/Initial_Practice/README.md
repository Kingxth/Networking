# NETWORK IP ( )

## RED A IPs
- **Router 1 Gateway (Gig0/0):** `192.168.1.1` (Subnet: `255.255.255.0`)
- **PC 1:** `192.168.1.15`
- **Serial WAN Link (R1 to R2):** `10.0.0.1` (Subnet: `255.255.255.252`)

## NET B IPs
- **Router 2 Gateway (Gig0/0):** `192.168.2.1` (Subnet: `255.255.255.0`)
- **PC 2:** `192.168.2.15`
- **Serial WAN Link (R2 to R1):** `10.0.0.2` (Subnet: `255.255.255.252`)
- **Inter-VLAN Segments:** VLAN 10 (`192.168.10.0/24`), VLAN 20 (`192.168.20.0/24`)

---

## Commands for sw (including my mistake with VLAN 10)

```text
enable
configure terminal
hostname SW1

vlan 10
 name Administration
vlan 20
 name Sales

interface FastEthernet0/1
 switchport mode access
 switchport access vlan 10
 no shutdown

interface FastEthernet0/2
 switchport mode access
 switchport access vlan 20
 no shutdown

interface GigabitEthernet0/1
 switchport mode trunk
 no shutdown

end
write memory


## Router config

```enable
configure terminal
hostname R1

interface GigabitEthernet0/0
 ip address 192.168.1.1 255.255.255.0
 no shutdown

interface Serial0/0/0
 ip address 10.0.0.1 255.255.255.252
 no shutdown

router ospf 1
 network 192.168.1.0 0.0.0.255 area 0
 network 10.0.0.0 0.0.0.3 area 0
end
write memory```


## Problem & Solution

During testing, I kept getting request  timed out errors when trying to ping between PCs, even though the OSPF routes and IP addresses were completely correct.
The issue was that I had assigned the switch ports to VLAN 10 by mistake, causing traffic isolation and dropped packets because the devices were not matching up correctly

Solution Commands
To fix the mistake, I cleared the incorrect access vlan configurations from the switch ports:

```enable
configure terminal
interface FastEthernet0/1
 no switchport access vlan 10
 no shutdown
interface FastEthernet0/2
 no switchport access vlan 20
 no shutdown
end
write memory
```
