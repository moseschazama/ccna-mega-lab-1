# ASW-A1 – Access Switch, Office A

## Role
ASW-A1 connects WLC1 and LWAP1. It is a VTP client and does NOT create VLANs — it receives them from the Distribution switch VTP server.

---

## Interface Summary

| Interface | Mode | VLANs | Connected To |
|-----------|------|-------|-------------|
| F0/1 | Trunk | 40 (tagged), 99 (untagged/native) | WLC1 |
| F0/2 | Access | VLAN 99 (management) | LWAP1 |
| G0/1 | Trunk | 10, 20, 40, 99 | DSW-A1 |
| G0/2 | Trunk | 10, 20, 40, 99 | DSW-A2 |
| All others | Shutdown | — | Unused |

> **WLC1 port (F0/1)**: Must support both Wi-Fi (VLAN 40) and Management (VLAN 99) VLANs.  
> Management VLAN (99) should be **untagged** (native), Wi-Fi VLAN (40) should be **tagged**.

---

## Management IP (VLAN 99)
```
interface Vlan99
 ip address 10.0.0.4 255.255.255.240
 no shutdown

ip default-gateway 10.0.0.1
```

---

## VTP
```
vtp mode client
vtp domain JeremysITLab
vtp version 2
```

---

## Trunk Configuration (to Distribution switches)
```
interface GigabitEthernet0/1
 switchport mode trunk
 switchport nonegotiate
 switchport trunk native vlan 1000
 switchport trunk allowed vlan 10,20,40,99
```

---

## WLC1 Port (F0/1) – Special Trunk
```
interface FastEthernet0/1
 switchport mode trunk
 switchport nonegotiate
 switchport trunk native vlan 99
 switchport trunk allowed vlan 40,99
```

> Native VLAN 99 means Management frames are untagged on this port (WLC1 expects untagged Management traffic).

---

## Spanning Tree (Rapid PVST+)
```
spanning-tree mode rapid-pvst

! PortFast + BPDU Guard on WLC1 port and LWAP port
interface FastEthernet0/1
 spanning-tree portfast
 spanning-tree bpduguard enable

interface FastEthernet0/2
 spanning-tree portfast
 spanning-tree bpduguard enable
```

---

## Port Security (F0/1 – WLC1)
```
interface FastEthernet0/1
 switchport port-security
 switchport port-security maximum 1
 switchport port-security violation restrict
 switchport port-security mac-address sticky
```

---

## DHCP Snooping
```
ip dhcp snooping
ip dhcp snooping vlan 10,20,40,99
no ip dhcp snooping information option

! Trust uplinks to Distribution switches
interface GigabitEthernet0/1
 ip dhcp snooping trust
interface GigabitEthernet0/2
 ip dhcp snooping trust

! Rate limit on access ports
interface FastEthernet0/1
 ip dhcp snooping limit rate 100

interface FastEthernet0/2
 ip dhcp snooping limit rate 15
```

---

## DAI (Dynamic ARP Inspection)
```
ip arp inspection vlan 10,20,40,99

! Trust uplinks
interface GigabitEthernet0/1
 ip arp inspection trust
interface GigabitEthernet0/2
 ip arp inspection trust

ip arp inspection validate src-mac dst-mac ip
```

---

## LLDP
```
no cdp run
lldp run

! Disable LLDP Tx on F0/1 (access/edge port)
interface FastEthernet0/1
 no lldp transmit
```

## Security & Services
- Enable secret: type 9
- Local user: cisco / ccna
- NTP: server 10.0.0.76, key 1 / ccna
- SNMP: SNMPSTRING (ro)
- Syslog: → 10.5.0.4, buffer 8192
- SSH: RSA 2048, SSHv2, ACL 1 on VTY
