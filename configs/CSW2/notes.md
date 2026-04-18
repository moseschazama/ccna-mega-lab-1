# CSW2 – Core Switch 2

## Role
CSW2 is the second core Layer-3 switch, mirroring CSW1 for redundancy. It connects to R1, all four Distribution switches, and forms a Layer-3 EtherChannel with CSW1.

---

## Interface Summary

| Interface | IP Address | Connected To |
|-----------|-----------|-------------|
| G1/0/1 | 10.0.0.38/30 | R1 G0/1 |
| G1/1/1 | 10.0.0.61/30 | DSW-A1 G1/1/2 |
| G1/1/2 | 10.0.0.65/30 | DSW-A2 G1/1/2 |
| G1/1/3 | 10.0.0.69/30 | DSW-B1 G1/1/2 |
| G1/1/4 | 10.0.0.73/30 | DSW-B2 G1/1/2 |
| Po1 | 10.0.0.42/30 | CSW1 (L3 EtherChannel) |
| Loopback0 | 10.0.0.78/32 | OSPF RID |

### IPv6 Interfaces
| Interface | IPv6 Address |
|-----------|-------------|
| G1/0/1 | 2001:db8:a2::/64 EUI-64 |
| Po1 | IPv6 enabled (no ipv6 address – link-local only) |

---

## EtherChannel – Layer 3 (Po1)

- **Protocol**: PAgP (Cisco proprietary)  
- **Mode**: Desirable on both CSW1 and CSW2
- CSW2 Po1: 10.0.0.42/30

```
interface Port-channel1
 no switchport
 ip address 10.0.0.42 255.255.255.252
 ipv6 enable
```

---

## OSPF Configuration Notes
- Process ID: 1, Area 0
- OSPF RID: 10.0.0.78
- All Layer-3 interfaces in OSPF
- Loopback0 passive
- Point-to-point on physical links (except Po1 — default network type)

## IPv4 Routing
```
ip routing
```

## Unused Interfaces
All interfaces not listed above are administratively shut down.

## Security & Services
- Same as CSW1 — see CSW1/notes.md for full details.
