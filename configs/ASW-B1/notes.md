# ASW-B1 – Access Switch, Office B

## Role
ASW-B1 connects LWAP2 and Phone3 (VLAN 20). VTP client.

---

## Management IP (VLAN 99)
```
interface Vlan99
 ip address 10.0.0.20 255.255.255.240
 no shutdown

ip default-gateway 10.0.0.17
```

## Interface Summary

| Interface | Mode | VLAN(s) | Connected To |
|-----------|------|---------|-------------|
| F0/1 | Access | VLAN 99 (management) | LWAP2 |
| F0/2 | Access | VLAN 20 (voice) | Phone3 |
| G0/1 | Trunk | 10, 20, 30, 99 | DSW-B1 |
| G0/2 | Trunk | 10, 20, 30, 99 | DSW-B2 |
| All others | Shutdown | — | Unused |

## Port Security (F0/1 – LWAP2)
```
switchport port-security maximum 1
switchport port-security violation restrict
switchport port-security mac-address sticky
```

## DHCP Snooping
```
ip dhcp snooping
ip dhcp snooping vlan 10,20,30,99
no ip dhcp snooping information option

interface GigabitEthernet0/1
 ip dhcp snooping trust
interface GigabitEthernet0/2
 ip dhcp snooping trust

interface FastEthernet0/1
 ip dhcp snooping limit rate 15
interface FastEthernet0/2
 ip dhcp snooping limit rate 15
```

## DAI
```
ip arp inspection vlan 10,20,30,99
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

---
---

# ASW-B2 – Access Switch, Office B

## Role
ASW-B2 connects PC3 (VLAN 10). VTP client.

---

## Management IP (VLAN 99)
```
interface Vlan99
 ip address 10.0.0.21 255.255.255.240
 no shutdown

ip default-gateway 10.0.0.17
```

## Interface Summary

| Interface | Mode | VLAN | Connected To |
|-----------|------|------|-------------|
| F0/1 | Access | VLAN 10 | PC3 |
| G0/1 | Trunk | 10, 20, 30, 99 | DSW-B1 |
| G0/2 | Trunk | 10, 20, 30, 99 | DSW-B2 |
| All others | Shutdown | — | Unused |

## Port Security (F0/1 – PC3)
```
switchport port-security maximum 1
switchport port-security violation restrict
switchport port-security mac-address sticky
```

DHCP Snooping, DAI, LLDP: same pattern as ASW-B1, active VLANs: 10, 20, 30, 99.

---
---

# ASW-B3 – Access Switch, Office B

## Role
ASW-B3 connects SRV1 (VLAN 30). VTP client.

---

## Management IP (VLAN 99)
```
interface Vlan99
 ip address 10.0.0.22 255.255.255.240
 no shutdown

ip default-gateway 10.0.0.17
```

## Interface Summary

| Interface | Mode | VLAN | Connected To |
|-----------|------|------|-------------|
| F0/1 | Access | VLAN 30 | SRV1 |
| G0/1 | Trunk | 10, 20, 30, 99 | DSW-B1 |
| G0/2 | Trunk | 10, 20, 30, 99 | DSW-B2 |
| All others | Shutdown | — | Unused |

## Port Security (F0/1 – SRV1)
```
switchport port-security maximum 1
switchport port-security violation restrict
switchport port-security mac-address sticky
```

> SRV1 does not use virtualization → only 1 MAC address needed.

DHCP Snooping, DAI, LLDP: same pattern as ASW-B1, active VLANs: 10, 20, 30, 99.
