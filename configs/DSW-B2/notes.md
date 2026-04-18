# DSW-B2 – Distribution Switch, Office B (Secondary)

## Role
DSW-B2 is the secondary Distribution switch for Office B. It is HSRP Active for Phones (VLAN 20) and Servers (VLAN 30) subnets.

---

## Interface Summary (Layer-3)

| Interface | IP Address | Connected To |
|-----------|-----------|-------------|
| G1/1/1 | 10.0.0.58/30 | CSW1 G1/1/4 |
| G1/1/2 | 10.0.0.74/30 | CSW2 G1/1/4 |
| Loopback0 | 10.0.0.82/32 | OSPF RID |

## SVI (HSRP) Addresses

| VLAN | SVI IP | HSRP VIP | HSRP Group | Role | Priority |
|------|--------|----------|-----------|------|---------|
| 99 | 10.0.0.19/28 | 10.0.0.17 | 1 | Standby | 100 |
| 10 | 10.3.0.3/24 | 10.3.0.1 | 2 | Standby | 100 |
| 20 | 10.4.0.3/24 | 10.4.0.1 | 3 | **Active** | **105** + preempt |
| 30 | 10.5.0.3/24 | 10.5.0.1 | 4 | **Active** | **105** + preempt |

---

## EtherChannel – Layer 2 (Po1 with DSW-B1)
- Protocol: LACP, Mode: Active
- Trunk: native 1000, allowed 10, 20, 30, 99
- DTP disabled

## Spanning Tree (Rapid PVST+)
| VLAN | Priority | Role |
|------|---------|------|
| 20 | 0 (lowest) | Root Bridge |
| 30 | 0 (lowest) | Root Bridge |
| 99 | 4096 | Non-root |
| 10 | 4096 | Non-root |

## OSPF
- RID: 10.0.0.82, Process 1, Area 0
- VLAN 99 SVI not passive; all other SVIs passive

## Security & Services
Same as DSW-B1. See DSW-B1/notes.md for details.
