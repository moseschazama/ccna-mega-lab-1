# DSW-A1 – Distribution Switch, Office A (Primary)

## Role
DSW-A1 is the primary (HSRP Active) Distribution switch for Office A PCs and Management VLANs. It connects upward to both core switches and downward to all three Office A access switches via trunk links.

---

## Interface Summary (Layer-3)

| Interface | IP Address | Connected To |
|-----------|-----------|-------------|
| G1/1/1 | 10.0.0.46/30 | CSW1 G1/1/1 |
| G1/1/2 | 10.0.0.62/30 | CSW2 G1/1/1 |
| Loopback0 | 10.0.0.79/32 | OSPF RID |

## SVI (HSRP) Addresses

| VLAN | SVI IP | HSRP VIP | HSRP Group | Role | Priority |
|------|--------|----------|-----------|------|---------|
| 99 | 10.0.0.2/28 | 10.0.0.1 | 1 | Active | 110 (default 100 + 10) |
| 10 | 10.1.0.2/24 | 10.1.0.1 | 2 | Active | 110 |
| 20 | 10.2.0.2/24 | 10.2.0.1 | 3 | Standby | 100 (default) |
| 40 | 10.6.0.2/24 | 10.6.0.1 | 4 | Standby | 100 (default) |

> **Note**: Priority increase is +10 above default (100+10=110). The task says "5 above default" — but the standard increment in CCNA is often shown as +10. Follow your lab's exact requirement.  
> ⚠️ Re-check: the lab says "5 above the default", so the priority should be **105**, not 110.

**Corrected HSRP priorities for DSW-A1:**
- Active VLANs (99, 10): priority **105**, preempt enabled
- Standby VLANs (20, 40): priority **100** (default), no preempt

---

## HSRP Config Example (VLAN 99)
```
interface Vlan99
 ip address 10.0.0.2 255.255.255.240
 standby version 2
 standby 1 ip 10.0.0.1
 standby 1 priority 105
 standby 1 preempt
```

## HSRP Config Example (VLAN 20 – Standby only)
```
interface Vlan20
 ip address 10.2.0.2 255.255.255.0
 standby version 2
 standby 3 ip 10.2.0.1
```

---

## EtherChannel – Layer 2 (Po1 with DSW-A2)
- **Protocol**: PAgP (Cisco proprietary)
- **Mode**: Desirable (actively tries to form)
- Configured as trunk: native VLAN 1000, allowed VLANs 10, 20, 40, 99
- DTP disabled (`switchport nonegotiate`)

---

## Trunk Links (to Access Switches)
All uplinks to ASW-A1, ASW-A2, ASW-A3 are 802.1Q trunks:
- Native VLAN: 1000
- Allowed VLANs: 10, 20, 40, 99
- DTP: disabled

---

## Spanning Tree (Rapid PVST+)
| VLAN | Priority | Role |
|------|---------|------|
| 10 | 0 (lowest) | Root Bridge |
| 99 | 0 (lowest) | Root Bridge |
| 20 | 4096 | Non-root |
| 40 | 4096 | Non-root |

> STP priority aligns with HSRP: where DSW-A1 is HSRP Active, it is also STP Root.

---

## OSPF
- Process 1, Area 0
- RID: 10.0.0.79
- `network` command matching exact IPs of G1/1/1, G1/1/2, and Loopback0
- Loopback0: passive
- SVIs for VLANs 10, 20, 40: passive (Management VLAN SVI is NOT passive)
- Physical links: `ip ospf network point-to-point`

## IP Routing
```
ip routing
```

## DHCP Relay (ip helper-address)
All SVIs (except Management VLAN 99) relay DHCP to R1's Loopback0 (10.0.0.76):
```
interface Vlan10
 ip helper-address 10.0.0.76

interface Vlan20
 ip helper-address 10.0.0.76

interface Vlan40
 ip helper-address 10.0.0.76
```

## DAI
```
ip arp inspection vlan 10,20,40,99
! Trust uplinks to core switches
```

## Security & Services
- Enable secret: type 9 (scrypt)
- Local user: cisco / ccna
- NTP: server 10.0.0.76, key 1, password ccna
- SNMP: SNMPSTRING (ro)
- Syslog: → 10.5.0.4, buffer 8192
- SSH: RSA 2048, SSHv2, ACL 1 on VTY
- CDP disabled, LLDP enabled
