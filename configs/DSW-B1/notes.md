# DSW-B1 – Distribution Switch, Office B (Primary)

## Role
DSW-B1 is the primary Distribution switch for Office B. It is HSRP Active for the PCs (VLAN 10) and Management (VLAN 99) subnets in Office B.

---

## Interface Summary (Layer-3)

| Interface | IP Address | Connected To |
|-----------|-----------|-------------|
| G1/1/1 | 10.0.0.54/30 | CSW1 G1/1/3 |
| G1/1/2 | 10.0.0.70/30 | CSW2 G1/1/3 |
| Loopback0 | 10.0.0.81/32 | OSPF RID |

## SVI (HSRP) Addresses

| VLAN | SVI IP | HSRP VIP | HSRP Group | Role | Priority |
|------|--------|----------|-----------|------|---------|
| 99 | 10.0.0.18/28 | 10.0.0.17 | 1 | **Active** | **105** + preempt |
| 10 | 10.3.0.2/24 | 10.3.0.1 | 2 | **Active** | **105** + preempt |
| 20 | 10.4.0.2/24 | 10.4.0.1 | 3 | Standby | 100 |
| 30 | 10.5.0.2/24 | 10.5.0.1 | 4 | Standby | 100 |

---

## EtherChannel – Layer 2 (Po1 with DSW-B2)
- **Protocol**: LACP (open standard, IEEE 802.3ad)
- **Mode**: Active on both DSW-B1 and DSW-B2
- Trunk: native 1000, allowed 10, 20, 30, 99
- DTP disabled

## Spanning Tree (Rapid PVST+)
| VLAN | Priority | Role |
|------|---------|------|
| 99 | 0 (lowest) | Root Bridge |
| 10 | 0 (lowest) | Root Bridge |
| 20 | 4096 | Non-root |
| 30 | 4096 | Non-root |

## OSPF
- RID: 10.0.0.81, Process 1, Area 0
- SVIs for VLANs 10, 20, 30 passive; VLAN 99 SVI not passive

## DHCP Relay
```
interface Vlan10
 ip helper-address 10.0.0.76

interface Vlan20
 ip helper-address 10.0.0.76

! VLAN 30 (Servers) has a static IP – no relay needed
```

## ACL: OfficeA_to_OfficeB
This ACL is applied on DSW-B1 (or the appropriate interface closest to Office B PCs):
```
! Applied inbound on the interface facing Office B PCs, or outbound on
! the interface toward Office A, depending on exact topology.
! Best practice for extended ACL: apply closest to the SOURCE.
! Apply outbound on DSW-A1's VLAN 10 SVI or inbound toward Office B.

ip access-list extended OfficeA_to_OfficeB
 permit icmp 10.1.0.0 0.0.0.255 10.3.0.0 0.0.0.255
 deny ip 10.1.0.0 0.0.0.255 10.3.0.0 0.0.0.255
 permit ip any any
```

## Security & Services
- Enable secret: type 9
- Local user: cisco / ccna
- NTP: server 10.0.0.76, key 1 / ccna
- SNMP: SNMPSTRING (ro)
- Syslog: → 10.5.0.4, buffer 8192
- SSH: RSA 2048, SSHv2, ACL 1 on VTY
- CDP disabled, LLDP enabled
