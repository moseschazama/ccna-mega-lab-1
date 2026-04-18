# R1 – Edge Router

## Role
R1 is the network's edge router. It connects to two ISPs for redundancy, routes traffic between the core switches and the Internet, and hosts multiple network services.

## Key Responsibilities
- **DHCP Server** for all Office A and B subnets
- **NAT/PAT** for Internet access
- **OSPF ASBR** – redistributes default route into OSPF domain
- **NTP Server** – stratum 5, syncs from 216.239.35.0
- **SSH** access restricted via ACL 1

---

## Interface Summary

| Interface | IP Address | Description |
|-----------|-----------|-------------|
| G0/0/0 | DHCP (ISP-A) | Primary Internet uplink |
| G0/1/0 | DHCP (ISP-B) | Backup Internet uplink |
| G0/0 | 10.0.0.33/30 | Link to CSW1 |
| G0/1 | 10.0.0.37/30 | Link to CSW2 |
| Loopback0 | 10.0.0.76/32 | OSPF RID / NTP source / DHCP helper target |

### IPv6 Interfaces
| Interface | IPv6 Address |
|-----------|-------------|
| G0/0/0 | 2001:db8:a::2/64 |
| G0/1/0 | 2001:db8:b::2/64 |
| G0/0 | 2001:db8:a1::/64 EUI-64 |
| G0/1 | 2001:db8:a2::/64 EUI-64 |

---

## OSPF Configuration Notes
- Process ID: 1, Area: 0
- OSPF RID: 10.0.0.76 (matches Loopback0)
- OSPF enabled on G0/0, G0/1, Loopback0 (passive) via interface config mode
- `default-information originate` redistributes default route to all OSPF routers
- G0/0 and G0/1 use `ip ospf network point-to-point` (no DR/BDR election)

## Static Routes
```
! Primary default route (AD 1 – default)
ip route 0.0.0.0 0.0.0.0 <ISP-A-gateway>

! Floating backup route via G0/1/0 (AD 2)
ip route 0.0.0.0 0.0.0.0 <ISP-B-gateway> 2
```

## DHCP Pools Summary
All pools exclude the first 10 usable addresses.

| Pool Name | Subnet | Gateway | DNS | WLC Option |
|-----------|--------|---------|-----|-----------|
| A-Mgmt | 10.0.0.0/28 | 10.0.0.1 | 10.5.0.4 | 10.0.0.7 |
| A-PC | 10.1.0.0/24 | 10.1.0.1 | 10.5.0.4 | — |
| A-Phone | 10.2.0.0/24 | 10.2.0.1 | 10.5.0.4 | — |
| B-Mgmt | 10.0.0.16/28 | 10.0.0.17 | 10.5.0.4 | 10.0.0.7 |
| B-PC | 10.3.0.0/24 | 10.3.0.1 | 10.5.0.4 | — |
| B-Phone | 10.4.0.0/24 | 10.4.0.1 | 10.5.0.4 | — |
| Wi-Fi | 10.6.0.0/24 | 10.6.0.1 | 10.5.0.4 | — |

## NAT Configuration
- **Static NAT**: SRV1 (10.5.0.4) ↔ 203.0.113.113
- **Dynamic PAT (POOL1)**: 203.0.113.200–203.0.113.207/29
  - Covers: Office A PCs, Office A Phones, Office B PCs, Office B Phones, Wi-Fi
  - ACL 2 defines inside local addresses (in the order listed above)
- Inside interfaces: G0/0, G0/1
- Outside interfaces: G0/0/0, G0/1/0

## SSH Access Control
- ACL 1 permits only 10.1.0.0/24 (Office A PCs)
- Applied to all VTY lines (0–15)
- SSHv2 only, RSA 2048-bit keys
- Domain name: jeremysitlab.com

## NTP
```
ntp master 5
ntp server 216.239.35.0
```

## Security
- Enable secret: type 9 (scrypt)
- Local user: cisco / ccna (type 9)
- SNMP community: SNMPSTRING (read-only)
- Syslog: → 10.5.0.4 (all levels), buffer 8192 bytes
- CDP disabled, LLDP enabled
