# 🏛️ Network Infrastructure Renewal — Municipalidad de Melipilla

> **Enterprise-grade network redesign for a Chilean municipal government**, achieving full redundancy, security compliance, and modern segmentation across a 3-floor building with 11 departments.

---

## 📋 Project Overview

| Field | Detail |
|---|---|
| **Client** | Ilustre Municipalidad de Melipilla, Chile |
| **Scope** | Full LAN infrastructure renewal (Layer 2/3 + Security) |
| **Duration** | 105 days (March – August 2025) |
| **Team** | Juan Tapia Galleguillos · Lucas Huerta Cordero |
| **Institution** | Duoc UC — Ingeniería en Conectividad y Redes |
| **Compliance** | Ley Marco de Ciberseguridad N°21.663 · Ley Transformación Digital N°21.180 |

---

## 🚨 Problem Statement

The municipality was operating a **decade-old network** with critical vulnerabilities:

- ❌ Non-compliance with Chilean Cybersecurity Framework Law (N°21.663)
- ❌ Recurring connectivity failures with no redundancy
- ❌ No network segmentation — all departments on flat network
- ❌ Static IP addressing across 200+ endpoints
- ❌ No intrusion detection or centralized logging
- ❌ Unorganized, unlabeled cabling infrastructure

---

## ✅ Solution Architecture

### Network Topology
A **three-tier hierarchical model** was designed and implemented:

```
Internet (GTD ISP x2)
        │
  ┌─────┴─────┐
  FW1          FW2        ← Fortinet FortiGate 200F (HA pair)
  │             │
  SWC1 ════ SWC2          ← Core: Cisco Catalyst C9500-24Y4C
  │    LACP    │            HSRP Groups 1 & 2 (Active/Passive)
  SW3  ════ SW4           ← Distribution: Cisco C9300-48P-A
  │             │            RPVST+ per VLAN
  └─────────────┘
        │
  [11 Access Racks — 3 Floors]
```

### VLAN Segmentation

| VLAN ID | Department | Subnet |
|---|---|---|
| 10 | Administración / Finanzas | 10.255.10.0/24 |
| 20 | Alcaldía | 10.255.20.0/24 |
| 30 | RRHH | 10.255.30.0/24 |
| 40 | Informática | 10.255.40.0/24 |
| 50 | Servicios / DIDECO | 10.255.50.0/24 |
| 60 | Obras Municipales | 10.255.60.0/24 |
| 70 | Municipio / Biblioteca | 10.255.70.0/24 |
| 99 | Native VLAN | — |
| 100 | Network Management | 10.255.100.0/24 |

---

## 🔧 Technologies Implemented

### High Availability & Redundancy
- **HSRP** (Hot Standby Router Protocol) — dual active/passive groups per VLAN cluster
- **LACP** (IEEE 802.3ad) — link aggregation between core switches
- **RPVST+** (IEEE 802.1w) — rapid convergence spanning tree per VLAN

### Security Stack
- **Fortinet FortiGate 200F** — dual perimeter firewalls in HA
- **Snort IPS** — inline intrusion prevention on dedicated server
- **Wazuh SIEM** — centralized security event correlation
- **Rsyslog** — log aggregation from all network devices
- **DHCP Server** — dynamic addressing with asset tracking integration
- **Port Security** — MAC address binding on all access ports
- **BPDUGuard + PortFast** — edge port hardening
- **VACL + ACL** — inter-VLAN traffic filtering
- **SSH** — encrypted management on all devices (no Telnet)

### Physical Infrastructure
- **Fiber optic backbone** — multimode 50/125 OM3, 1500m across building
- **Structured cabling** — Cat6A UTP, labeled and documented
- **12 racks** — 1 core rack + 11 distribution-access racks (one per department)

---

## 🛡️ Cybersecurity Compliance (Ley N°21.663)

This project implements Chile's **Ley Marco de Ciberseguridad** requirements:

| Requirement | Implementation |
|---|---|
| Incident detection system | Wazuh SIEM + Snort IPS |
| Security alert levels | 5-tier alert matrix (Level 1–5) |
| Maximum containment times | L1: 4h · L3: 60min · L5: 15min |
| CSIRT national reporting | Standardized incident report format |
| Staff security awareness | 9-module training program |
| Dedicated security officer role | Defined in governance documentation |
| ISO 27001 alignment | Security controls mapped to standard |

---

## 📹 Live Demonstrations

All configurations were tested and recorded. Click to view each demo:

| Demo | Description | Duration |
|---|---|---|
| [🎬 DHCP Server](evidence/videos/demo-dhcp-server.mp4) | Dynamic IP assignment across all VLANs | ~3 min |
| [🎬 HSRP Redundancy](evidence/videos/demo-hsrp-redundancy.mp4) | Failover simulation — core switch down, traffic continues | ~4 min |
| [🎬 Syslog Server](evidence/videos/demo-syslog-server.mp4) | Centralized log collection from Cisco devices | ~3 min |
| [🎬 Wazuh SIEM](evidence/videos/demo-wazuh-siem.mp4) | Security event dashboard and alert correlation | ~5 min |
| [🎬 Network Tracking](evidence/videos/demo-network-tracking.mp4) | End-to-end packet flow verification | ~3 min |

---

## 💰 Budget Summary

| Equipment | Qty | Unit Price (CLP) | Total (CLP) |
|---|---|---|---|
| Cisco Catalyst C9500-24Y4C (Core SW) | 4 | $5,478,820 | $21,915,280 |
| Cisco Catalyst C9300-48P-A (Access SW) | 18 | $4,100,000 | $73,800,000 |
| Dell PowerEdge R440 (Servers) | 2 | $1,544,897 | $3,089,794 |
| Fiber Optic OM3 50/125 (1500m) | 1 lot | — | $1,101,000 |
| **Total** | | | **~$99,906,074** |

---

## 📁 Repository Structure

```
municipalidad-melipilla-network/
│
├── README.md                        ← You are here
│
├── diagrams/                        ← Network diagrams
│   ├── topology-logical.png         ← Full logical topology
│   ├── rack-core.png                ← Core rack layout
│   ├── floor-plan-physical.vsdx     ← Physical floor plans (Visio)
│   └── racks-distribution.vsdx     ← Distribution rack diagrams
│
├── docs/                            ← Full technical documentation
│   └── project-report-full.docx    ← Complete project portfolio report
│
├── evidence/                        ← Proof of implementation
│   └── videos/
│       ├── demo-dhcp-server.mp4
│       ├── demo-hsrp-redundancy.mp4
│       ├── demo-syslog-server.mp4
│       ├── demo-wazuh-siem.mp4
│       └── demo-network-tracking.mp4
│
├── planning/                        ← Project management
│   └── project-schedule.mpp        ← MS Project Gantt (105 days)
│
├── presentation/                    ← Final presentation
│   ├── presentation.pptx
│   └── presentation.pdf
│
└── security/                        ← Security documentation
    ├── VLAN-design.md
    ├── incident-response-plan.md
    └── compliance-ley21663.md
```

---

## 🗂️ Project Timeline

```
Mar 2025          Apr 2025          May 2025          Jun 2025          Jul 2025          Aug 2025
│─────────────────│─────────────────│─────────────────│─────────────────│─────────────────│
│◄──── INICIO (43 days) ──────────►│◄──── EJECUCIÓN (55 days) ────────────────────────►│
                                    │ Physical Install │  Net Config  │  Servers  │ Ley  │
│                                                                                   Cierre│
```

---

## 👤 About the Author

**Lucas Huerta Cordero**
Network Engineer · Cybersecurity Learner
📍 Región Metropolitana, Chile
🎓 Ingeniería en Conectividad y Redes — Duoc UC (2025)

Currently pursuing: `PortSwigger Web Academy` → `eJPT` → `OSCP`

[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-blue?style=flat&logo=linkedin)](https://linkedin.com/in/tu-perfil)
[![GitHub](https://img.shields.io/badge/GitHub-Portfolio-black?style=flat&logo=github)](https://github.com/tu-usuario)

---

## 📄 License

This project is shared for educational and portfolio purposes.
Network configurations and documentation are original work by the authors.

---

*Built with Cisco IOS · Fortinet FortiOS · Wazuh · Snort · Linux*
