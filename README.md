# 🏆 Jeremy's IT Lab – CCNA Mega Lab (Packet Tracer)

> A fully configured, end-to-end Cisco network lab covering every topic on the **CCNA exam**.  
> Built in Cisco Packet Tracer. All devices are operational and verified.

---

## 📋 Table of Contents

- [Network Topology](#-network-topology)
- [Lab Parts Overview](#-lab-parts-overview)
- [Device List](#-device-list)
- [IP Addressing Summary](#-ip-addressing-summary)
- [VLAN Summary](#-vlan-summary)
- [Technologies Configured](#-technologies-configured)
- [Device Configurations](#-device-configurations)
- [How to Use This Repo](#-how-to-use-this-repo)
- [Skills Demonstrated](#-skills-demonstrated)

---

## 🗺️ Network Topology
./topology.png
---

## 📚 Lab Parts Overview

| Part | Topic | Status |
|------|-------|--------|
| Part 1 | Initial Setup (Hostnames, Passwords, Users, Console) | ✅ Complete |
| Part 2 | VLANs & Layer-2 EtherChannel | ✅ Complete |
| Part 3 | IP Addresses, Layer-3 EtherChannel, HSRP | ✅ Complete |
| Part 4 | Rapid Spanning Tree Protocol (RSTP) | ✅ Complete |
| Part 5 | Static & Dynamic Routing (OSPF) | ✅ Complete |
| Part 6 | Network Services (DHCP, DNS, NTP, SNMP, Syslog, FTP, SSH, NAT) | ✅ Complete |
| Part 7 | Security (ACLs, Port Security, DHCP Snooping, DAI) | ✅ Complete |
| Part 8 | IPv6 | ✅ Complete |
| Part 9 | Wireless (WLC, WLAN, LWAP) | ✅ Complete |

---

## 🖥️ Device List

### Routers
| Device | Model | Role |
|--------|-------|------|
| R1 | Cisco 2911 | Edge Router / DHCP Server / NAT |

### Core Switches
| Device | Role |
|--------|------|
| CSW1 | Core Switch 1 (Layer-3) |
| CSW2 | Core Switch 2 (Layer-3) |

### Distribution Switches – Office A
| Device | Role |
|--------|------|
| DSW-A1 | Distribution Switch (HSRP Active for PCs/Mgmt) |
| DSW-A2 | Distribution Switch (HSRP Active for Phones/Wi-Fi) |

### Distribution Switches – Office B
| Device | Role |
|--------|------|
| DSW-B1 | Distribution Switch (HSRP Active for PCs/Mgmt) |
| DSW-B2 | Distribution Switch (HSRP Active for Phones/Servers) |

### Access Switches – Office A
| Device | Connected To |
|--------|-------------|
| ASW-A1 | WLC1, LWAP1 |
| ASW-A2 | PC1, Phone1 |
| ASW-A3 | PC2, Phone2 |

### Access Switches – Office B
| Device | Connected To |
|--------|-------------|
| ASW-B1 | LWAP2, Phone3 |
| ASW-B2 | PC3 |
| ASW-B3 | SRV1 |

### Other Devices
| Device | IP | Role |
|--------|-----|------|
| SRV1 | 10.5.0.4 | DNS / NTP / Syslog / FTP Server |
| WLC1 | 10.0.0.7 | Wireless LAN Controller |
| LWAP1 | DHCP | Lightweight Access Point – Office A |
| LWAP2 | DHCP | Lightweight Access Point – Office B |

---

## 🌐 IP Addressing Summary

### Point-to-Point Links (WAN/Core)

| Link | Interface | IP Address |
|------|-----------|------------|
| R1 → ISP-A | G0/0/0 | DHCP |
| R1 → ISP-B | G0/1/0 | DHCP |
| R1 → CSW1 | G0/0 | 10.0.0.33/30 |
| R1 → CSW2 | G0/1 | 10.0.0.37/30 |
| R1 Loopback0 | Lo0 | 10.0.0.76/32 |
| CSW1 → R1 | G1/0/1 | 10.0.0.34/30 |
| CSW2 → R1 | G1/0/1 | 10.0.0.38/30 |
| CSW1 ↔ CSW2 | Po1 | 10.0.0.41/30 (CSW1), 10.0.0.42/30 (CSW2) |
| CSW1 → DSW-A1 | G1/1/1 | 10.0.0.45/30 |
| CSW1 → DSW-A2 | G1/1/2 | 10.0.0.49/30 |
| CSW1 → DSW-B1 | G1/1/3 | 10.0.0.53/30 |
| CSW1 → DSW-B2 | G1/1/4 | 10.0.0.57/30 |
| CSW2 → DSW-A1 | G1/1/1 | 10.0.0.61/30 |
| CSW2 → DSW-A2 | G1/1/2 | 10.0.0.65/30 |
| CSW2 → DSW-B1 | G1/1/3 | 10.0.0.69/30 |
| CSW2 → DSW-B2 | G1/1/4 | 10.0.0.73/30 |
| DSW-A1 → CSW1 | G1/1/1 | 10.0.0.46/30 |
| DSW-A1 → CSW2 | G1/1/2 | 10.0.0.62/30 |
| DSW-A1 Loopback0 | Lo0 | 10.0.0.79/32 |
| DSW-A2 → CSW1 | G1/1/1 | 10.0.0.50/30 |
| DSW-A2 → CSW2 | G1/1/2 | 10.0.0.66/30 |
| DSW-A2 Loopback0 | Lo0 | 10.0.0.80/32 |
| DSW-B1 → CSW1 | G1/1/1 | 10.0.0.54/30 |
| DSW-B1 → CSW2 | G1/1/2 | 10.0.0.70/30 |
| DSW-B1 Loopback0 | Lo0 | 10.0.0.81/32 |
| DSW-B2 → CSW1 | G1/1/1 | 10.0.0.58/30 |
| DSW-B2 → CSW2 | G1/1/2 | 10.0.0.74/30 |
| DSW-B2 Loopback0 | Lo0 | 10.0.0.82/32 |

### HSRP / SVI Addresses

| VLAN | Subnet | VIP | DSW-A1 | DSW-A2 | Active Router |
|------|--------|-----|--------|--------|--------------|
| 99 (Mgmt-A) | 10.0.0.0/28 | 10.0.0.1 | 10.0.0.2 | 10.0.0.3 | DSW-A1 |
| 10 (PCs-A) | 10.1.0.0/24 | 10.1.0.1 | 10.1.0.2 | 10.1.0.3 | DSW-A1 |
| 20 (Phones-A) | 10.2.0.0/24 | 10.2.0.1 | 10.2.0.2 | 10.2.0.3 | DSW-A2 |
| 40 (Wi-Fi) | 10.6.0.0/24 | 10.6.0.1 | 10.6.0.2 | 10.6.0.3 | DSW-A2 |

| VLAN | Subnet | VIP | DSW-B1 | DSW-B2 | Active Router |
|------|--------|-----|--------|--------|--------------|
| 99 (Mgmt-B) | 10.0.0.16/28 | 10.0.0.17 | 10.0.0.18 | 10.0.0.19 | DSW-B1 |
| 10 (PCs-B) | 10.3.0.0/24 | 10.3.0.1 | 10.3.0.2 | 10.3.0.3 | DSW-B1 |
| 20 (Phones-B) | 10.4.0.0/24 | 10.4.0.1 | 10.4.0.2 | 10.4.0.3 | DSW-B2 |
| 30 (Servers) | 10.5.0.0/24 | 10.5.0.1 | 10.5.0.2 | 10.5.0.3 | DSW-B2 |

### Management IPs (Access Switches – VLAN 99)

| Device | IP | Default Gateway |
|--------|----|----------------|
| ASW-A1 | 10.0.0.4/28 | 10.0.0.1 |
| ASW-A2 | 10.0.0.5/28 | 10.0.0.1 |
| ASW-A3 | 10.0.0.6/28 | 10.0.0.1 |
| ASW-B1 | 10.0.0.20/28 | 10.0.0.17 |
| ASW-B2 | 10.0.0.21/28 | 10.0.0.17 |
| ASW-B3 | 10.0.0.22/28 | 10.0.0.17 |

---

## 🏷️ VLAN Summary

### Office A

| VLAN ID | Name | Subnet | Notes |
|---------|------|--------|-------|
| 10 | PCs | 10.1.0.0/24 | End-user PCs |
| 20 | Phones | 10.2.0.0/24 | VoIP phones |
| 40 | Wi-Fi | 10.6.0.0/24 | Wireless clients via WLC |
| 99 | Management | 10.0.0.0/28 | Switch management |
| 1000 | (unused) | — | Native VLAN on trunks |

### Office B

| VLAN ID | Name | Subnet | Notes |
|---------|------|--------|-------|
| 10 | PCs | 10.3.0.0/24 | End-user PCs |
| 20 | Phones | 10.4.0.0/24 | VoIP phones |
| 30 | Servers | 10.5.0.0/24 | Server farm |
| 99 | Management | 10.0.0.16/28 | Switch management |
| 1000 | (unused) | — | Native VLAN on trunks |

---

## ⚙️ Technologies Configured

| Technology | Details |
|------------|---------|
| **Hostnames & Passwords** | Type 9 (scrypt) where available, Type 5 fallback |
| **EtherChannel L2** | PAgP (Office A DSW pair), LACP (Office B DSW pair) |
| **EtherChannel L3** | PAgP between CSW1 and CSW2 |
| **VTP** | Version 2, domain JeremysITLab |
| **Trunking** | 802.1Q, native VLAN 1000, DTP disabled |
| **Spanning Tree** | Rapid PVST+, PortFast + BPDU Guard on edge ports |
| **HSRP** | Version 2, preemption enabled on Active routers |
| **OSPF** | Process 1, Area 0, point-to-point links, passive SVIs/loopbacks |
| **Static Routes** | Default routes with floating backup (AD+1) |
| **DHCP** | R1 as server, ip helper-address on Distribution switches |
| **DNS** | Static entries on SRV1, all devices point to SRV1 |
| **NTP** | R1 as stratum 5 server, MD5 authentication (key 1 / ccna) |
| **SNMP** | Community SNMPSTRING, read-only |
| **Syslog** | All levels → SRV1; 8192-byte local buffer |
| **SSH** | RSA 2048-bit, SSHv2 only, ACL restricting to Office A PCs |
| **NAT** | Static NAT for SRV1; Pool-based PAT for internal subnets |
| **IPv6** | Enabled on R1/CSW1/CSW2, EUI-64 addressing, dual-stack default routes |
| **ACLs** | Extended ACL (inter-office), Standard ACL (SSH, NAT) |
| **Port Security** | Sticky MAC, restrict mode on F0/1 ports |
| **DHCP Snooping** | Enabled per VLAN, rate limiting, Option 82 disabled |
| **DAI** | Dynamic ARP Inspection with all validation checks |
| **LLDP** | CDP disabled globally, LLDP enabled (Tx disabled on edge ports) |
| **Wireless** | WLC1 GUI, Wi-Fi WLAN (WPA2/AES/PSK), two LWAPs associated |

---

## 📁 Device Configurations

Each device has its own folder under [`configs/`](./configs/) containing:
- `running-config.txt` — Full device configuration
- `notes.md` — Key decisions and explanations for that device

| Device | Config Folder |
|--------|--------------|
| R1 | [configs/R1/](./configs/R1/) |
| CSW1 | [configs/CSW1/](./configs/CSW1/) |
| CSW2 | [configs/CSW2/](./configs/CSW2/) |
| DSW-A1 | [configs/DSW-A1/](./configs/DSW-A1/) |
| DSW-A2 | [configs/DSW-A2/](./configs/DSW-A2/) |
| DSW-B1 | [configs/DSW-B1/](./configs/DSW-B1/) |
| DSW-B2 | [configs/DSW-B2/](./configs/DSW-B2/) |
| ASW-A1 | [configs/ASW-A1/](./configs/ASW-A1/) |
| ASW-A2 | [configs/ASW-A2/](./configs/ASW-A2/) |
| ASW-A3 | [configs/ASW-A3/](./configs/ASW-A3/) |
| ASW-B1 | [configs/ASW-B1/](./configs/ASW-B1/) |
| ASW-B2 | [configs/ASW-B2/](./configs/ASW-B2/) |
| ASW-B3 | [configs/ASW-B3/](./configs/ASW-B3/) |

---

## 🚀 How to Use This Repo

### Option A – View the Configs
Browse any device folder under `configs/` to see the full running configuration and explanatory notes.

### Option B – Load into Packet Tracer
1. Open Cisco Packet Tracer (version 8.x recommended).
2. Open the `.pkt` file if included, OR manually build the topology using the diagram above.
3. Copy/paste configs from `running-config.txt` into each device's CLI.

### Option C – Add Your Own Configs
1. Fork this repository on GitHub.
2. After completing each part, run `show running-config` on the device.
3. Paste the output into the appropriate `configs/<device>/running-config.txt`.
4. Commit and push with a meaningful message, e.g.:
   ```
   git add configs/R1/running-config.txt
   git commit -m "Add R1 config: OSPF, NAT, DHCP complete"
   git push
   ```

---

## 🎓 Skills Demonstrated

- ✅ Cisco IOS CLI proficiency
- ✅ Network design (hierarchical three-tier model)
- ✅ Layer 2 switching (VLANs, trunking, STP, EtherChannel)
- ✅ Layer 3 routing (OSPF, static routes, floating routes)
- ✅ First Hop Redundancy (HSRP)
- ✅ Network services (DHCP, DNS, NTP, SNMP, Syslog)
- ✅ Network security (ACLs, SSH, Port Security, DHCP Snooping, DAI)
- ✅ NAT/PAT for Internet access
- ✅ IPv6 dual-stack
- ✅ Wireless networking (WLC, LWAP, WPA2)

---

## 📖 Credits

Lab design by [Jeremy's IT Lab](https://www.jeremysitlab.com/) — CCNA Mega Lab.  
Configurations completed and documented by MOSES CHAZAMA.

---

*Last updated: 2026*
