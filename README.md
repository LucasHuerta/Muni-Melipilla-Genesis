# Network Infrastructure Renewal — Municipalidad de Melipilla

Senior capstone project developed for the Ilustre Municipalidad de Melipilla, addressing a full network infrastructure overhaul of a facility that had not been significantly updated in over a decade.

---

## Project Overview

| | |
|---|---|
| **Client** | Ilustre Municipalidad de Melipilla, Chile |
| **Scope** | Full LAN infrastructure renewal (Layer 2/3 + Security) |
| **Duration** | 105 days (March – August 2025) |
| **Team** | Juan Tapia Galleguillos · Lucas Huerta Cordero |
| **Institution** | Duoc UC — Network and Connectivity Engineering |
| **Compliance** | Chilean Cybersecurity Framework Law N°21.663 · Digital Transformation Law N°21.180 |

---

## Problem Statement

The existing network had recurring connectivity failures and did not comply with Chile's Cybersecurity Framework Law (N°21.663), putting daily municipal operations and citizen data at risk.

Specific issues identified during the initial assessment:

- Flat network with no segmentation — all departments sharing the same broadcast domain
- Static IP addressing across 200+ endpoints
- No redundancy — a single core failure would take down the entire building
- No intrusion detection or centralized event logging
- Unlabeled and undocumented cabling
- Non-compliance with Law N°21.663

---

## Solution Architecture

### Network Topology

A three-tier hierarchical model was designed and implemented:

```
Internet (GTD ISP x2)
        |
  ------+------
  FW1         FW2          Fortinet FortiGate 200F (HA pair)
  |            |
  SWC1 ==== SWC2           Core: Cisco Catalyst C9500-24Y4C
       LACP                HSRP Groups 1 & 2 (Active/Passive)
  SW3  ==== SW4            Distribution: Cisco C9300-48P-A
  |            |           RPVST+ per VLAN
  +-----------+
        |
  [11 access racks across 3 floors]
```

### VLAN Segmentation

| VLAN | Department | Subnet |
|---|---|---|
| 10 | Administration / Finance | 10.255.10.0/24 |
| 20 | Mayor's Office | 10.255.20.0/24 |
| 30 | Human Resources | 10.255.30.0/24 |
| 40 | IT Department | 10.255.40.0/24 |
| 50 | Services / DIDECO | 10.255.50.0/24 |
| 60 | Public Works | 10.255.60.0/24 |
| 70 | General Municipal | 10.255.70.0/24 |
| 99 | Native VLAN | — |
| 100 | Network Management | 10.255.100.0/24 |

---

## Technology Stack

**High Availability**
- HSRP — two active/passive groups, one per VLAN cluster
- LACP IEEE 802.3ad — link aggregation between core switches
- RPVST+ IEEE 802.1w — rapid per-VLAN spanning tree convergence

**Security**
- Fortinet FortiGate 200F in HA pair as network perimeter
- Snort IPS — inline intrusion detection and prevention
- Wazuh SIEM — centralized security event correlation
- Rsyslog — log collection from all network devices
- Port Security — MAC binding on all access ports
- BPDUGuard + PortFast — edge port hardening
- VACL + ACL — inter-VLAN traffic filtering
- SSH on all devices — Telnet disabled

**Physical Infrastructure**
- Multimode fiber optic OM3 50/125 — 1500m backbone
- Cat6A UTP structured cabling, fully labeled and documented
- 12 racks: 1 core + 11 distribution-access

---

## Cybersecurity Compliance — Law N°21.663

| Requirement | Implementation |
|---|---|
| Incident detection system | Wazuh SIEM + Snort IPS |
| Defined alert levels | 5-tier matrix |
| Maximum containment times | L1: 4h / L3: 60min / L5: 15min |
| CSIRT national reporting | Standardized report format documented |
| Staff training program | 9-module program for municipal employees |
| Designated security officer | Defined in governance documentation |

---

## Live Demonstrations

All configurations were tested and recorded:

| Demo | Description |
|---|---|
| [DHCP Server](evidence/videos/demo-dhcp-server.mp4) | Dynamic IP assignment across all VLANs |
| [HSRP Redundancy](evidence/videos/demo-hsrp-redundancy.mp4) | Failover simulation with core switch down |
| [Syslog Server](evidence/videos/demo-syslog-server.mp4) | Centralized log collection from Cisco devices |
| [Wazuh SIEM](evidence/videos/demo-wazuh-siem.mp4) | Security event dashboard and alert correlation |
| [Network Tracking](evidence/videos/demo-network-tracking.mp4) | End-to-end packet flow verification |

---

## Budget

| Equipment | Qty | Unit Price (CLP) | Total (CLP) |
|---|---|---|---|
| Cisco Catalyst C9500-24Y4C | 4 | $5,478,820 | $21,915,280 |
| Cisco Catalyst C9300-48P-A | 18 | $4,100,000 | $73,800,000 |
| Dell PowerEdge R440 | 2 | $1,544,897 | $3,089,794 |
| Fiber Optic OM3 50/125 (1500m) | 1 | — | $1,101,000 |
| **Total** | | | **~$99,906,074** |

---

## Repository Structure

```
Muni-Melipilla-Genesis/
├── README.md
├── diagrams/
│   ├── topology-logical.png
│   ├── rack-core.png
│   ├── floor-plan-physical.vsdx
│   └── racks-distribution.vsdx
├── docs/
│   └── project-report-full.docx
├── evidence/
│   └── videos/
│       ├── demo-dhcp-server.mp4
│       ├── demo-hsrp-redundancy.mp4
│       ├── demo-syslog-server.mp4
│       ├── demo-wazuh-siem.mp4
│       └── demo-network-tracking.mp4
├── planning/
│   └── project-schedule.mpp
├── presentation/
│   ├── presentation.pptx
│   └── presentation.pdf
└── security/
    ├── VLAN-design.md
    ├── incident-response-plan.md
    └── compliance-ley21663.md
```

---

## Lessons Learned

The biggest challenge was getting HSRP and RPVST+ to work correctly together — aligning root bridges with the active HSRP gateways for each VLAN cluster required several iterations and failover tests before the behavior was consistent.

Integrating Wazuh with Rsyslog to receive logs from Cisco devices also took longer than expected, mainly due to syslog format differences across IOS versions.

---

## Author

Lucas Huerta Cordero
Network and Connectivity Engineering — Duoc UC (2025)
Santiago, Chile

[LinkedIn](https://linkedin.com/in/tu-perfil) · [GitHub](https://github.com/LucasHuerta)

---

*Cisco IOS · Fortinet FortiOS · Wazuh · Snort · Linux*
