# DSW-A2 – Distribution Switch, Office A (Secondary)

## Role
DSW-A2 is the secondary Distribution switch for Office A. It is HSRP Active for the Phones (VLAN 20) and Wi-Fi (VLAN 40) subnets, and HSRP Standby for PCs (VLAN 10) and Management (VLAN 99).

---

## Interface Summary (Layer-3)

| Interface | IP Address | Connected To |
|-----------|-----------|-------------|
| G1/1/1 | 10.0.0.50/30 | CSW1 G1/1/2 |
| G1/1/2 | 10.0.0.66/30 | CSW2 G1/1/2 |
| Loopback0 | 10.0.0.80/32 | OSPF RID |

## SVI (HSRP) Addresses

| VLAN | SVI IP | HSRP VIP | HSRP Group | Role | Priority |
|------|--------|----------|-----------|------|---------|
| 99 | 10.0.0.3/28 | 10.0.0.1 | 1 | Standby | 100 |
| 10 | 10.1.0.3/24 | 10.1.0.1 | 2 | Standby | 100 |
| 20 | 10.2.0.3/24 | 10.2.0.1 | 3 | **Active** | **105** + preempt |
| 40 | 10.6.0.3/24 | 10.6.0.1 | 4 | **Active** | **105** + preempt |

---

## EtherChannel – Layer 2 (Po1 with DSW-A1)
- Protocol: PAgP, Mode: Desirable
- Trunk: native 1000, allowed 10, 20, 40, 99
- DTP disabled

## Spanning Tree (Rapid PVST+)
| VLAN | Priority | Role |
|------|---------|------|
| 20 | 0 (lowest) | Root Bridge |
| 40 | 0 (lowest) | Root Bridge |
| 10 | 4096 | Non-root |
| 99 | 4096 | Non-root |

## OSPF
- RID: 10.0.0.80
- All same OSPF settings as DSW-A1
- SVIs for VLANs 10, 20, 40 are passive; VLAN 99 SVI is not passive

## DHCP Relay
```
interface Vlan10
 ip helper-address 10.0.0.76

interface Vlan20
 ip helper-address 10.0.0.76

interface Vlan40
 ip helper-address 10.0.0.76
```

## Security & Services
Same as DSW-A1. See DSW-A1/notes.md for full details.
