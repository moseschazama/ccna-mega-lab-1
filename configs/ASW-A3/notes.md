# ASW-A2 – Access Switch, Office A

## Role
ASW-A2 connects PC1 (VLAN 10) and Phone1 (VLAN 20). VTP client.

---

## Interface Summary

| Interface | Mode | VLAN(s) | Connected To |
|-----------|------|---------|-------------|
| F0/1 | Access | VLAN 10 (data) + VLAN 20 (voice) | PC1 + Phone1 |
| G0/1 | Trunk | 10, 20, 40, 99 | DSW-A1 |
| G0/2 | Trunk | 10, 20, 40, 99 | DSW-A2 |
| All others | Shutdown | — | Unused |

---

## Management IP (VLAN 99)
```
interface Vlan99
 ip address 10.0.0.5 255.255.255.240
 no shutdown

ip default-gateway 10.0.0.1
```

## Access Port (F0/1) – PC + Phone
```
interface FastEthernet0/1
 switchport mode access
 switchport nonegotiate
 switchport access vlan 10
 switchport voice vlan 20
 spanning-tree portfast
 spanning-tree bpduguard enable
```

## Port Security (F0/1)
```
 switchport port-security
 switchport port-security maximum 2
 switchport port-security violation restrict
 switchport port-security mac-address sticky
```
> Maximum 2: one for the PC (VLAN 10), one for the Phone (VLAN 20).

## DHCP Snooping
```
ip dhcp snooping
ip dhcp snooping vlan 10,20,99
no ip dhcp snooping information option

interface GigabitEthernet0/1
 ip dhcp snooping trust
interface GigabitEthernet0/2
 ip dhcp snooping trust

interface FastEthernet0/1
 ip dhcp snooping limit rate 15
```

## DAI
```
ip arp inspection vlan 10,20,99
interface GigabitEthernet0/1
 ip arp inspection trust
interface GigabitEthernet0/2
 ip arp inspection trust
ip arp inspection validate src-mac dst-mac ip
```

## LLDP
```
no cdp run
lldp run
interface FastEthernet0/1
 no lldp transmit
```

## VTP / STP / Security
- VTP client, domain JeremysITLab
- Rapid PVST+
- Same security/services as ASW-A1

---
---

# ASW-A3 – Access Switch, Office A

## Role
ASW-A3 connects PC2 (VLAN 10) and Phone2 (VLAN 20). Identical to ASW-A2 except for the management IP.

---

## Management IP (VLAN 99)
```
interface Vlan99
 ip address 10.0.0.6 255.255.255.240
 no shutdown

ip default-gateway 10.0.0.1
```

## Everything Else
Identical to ASW-A2 (same VLANs, same access port config, same port security max 2, same DHCP snooping, DAI, LLDP settings).
