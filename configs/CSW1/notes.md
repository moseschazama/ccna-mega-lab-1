# CSW1 – Core Switch 1

## Role
CSW1 is one of two core Layer-3 switches. It connects to R1, all four Distribution switches, and forms a Layer-3 EtherChannel with CSW2 for redundancy and load sharing.

---

## Interface Summary

| Interface | IP Address | Connected To |
|-----------|-----------|-------------|
| G1/0/1 | 10.0.0.34/30 | R1 G0/0 |
| G1/1/1 | 10.0.0.45/30 | DSW-A1 G1/1/1 |
| G1/1/2 | 10.0.0.49/30 | DSW-A2 G1/1/1 |
| G1/1/3 | 10.0.0.53/30 | DSW-B1 G1/1/1 |
| G1/1/4 | 10.0.0.57/30 | DSW-B2 G1/1/1 |
| Po1 | 10.0.0.41/30 | CSW2 (L3 EtherChannel) |
| Loopback0 | 10.0.0.77/32 | OSPF RID |

### IPv6 Interfaces
| Interface | IPv6 Address |
|-----------|-------------|
| G1/0/1 | 2001:db8:a1::/64 EUI-64 |
| Po1 | IPv6 enabled (no ipv6 address – link-local only) |

---

## EtherChannel – Layer 3 (Po1)

- **Protocol**: PAgP (Cisco proprietary)
- **Mode**: Desirable on both CSW1 and CSW2 (actively tries to form)
- **Type**: Layer-3 (no switchport)
- CSW1 Po1: 10.0.0.41/30
- CSW2 Po1: 10.0.0.42/30

```
! Member interfaces (physical ports in the channel)
interface GigabitEthernet1/0/2
 no switchport
 channel-group 1 mode desirable

interface GigabitEthernet1/0/3
 no switchport
 channel-group 1 mode desirable

interface Port-channel1
 no switchport
 ip address 10.0.0.41 255.255.255.252
 ipv6 enable
```

---

## OSPF Configuration Notes
- Process ID: 1, Area 0
- OSPF RID: 10.0.0.77 (matches Loopback0)
- All Layer-3 interfaces participate in OSPF
- Loopback0 is passive
- Physical point-to-point links use `ip ospf network point-to-point`
- Po1 (to CSW2) uses default OSPF network type (broadcast)
- Use `network <exact-ip> 0.0.0.0 area 0` for each interface

## IPv4 Routing
```
ip routing
```

## Unused Interfaces
All interfaces not listed in the summary above are administratively shut down (`shutdown`).

## Security & Services
- Enable secret: type 9 (scrypt)
- Local user: cisco / ccna (type 9)
- SNMP community: SNMPSTRING (read-only)
- NTP server: 10.0.0.76 (R1 Loopback), key 1 / ccna
- Syslog: → 10.5.0.4, buffer 8192 bytes
- SSH: RSA 2048-bit, SSHv2 only, ACL 1 on VTY lines
- CDP disabled, LLDP enabled
- Domain: jeremysitlab.com, DNS server: 10.5.0.4
