# Compliance Documentation
## Ley Marco de Ciberseguridad N°21.663 — Chile

---

## Compliance Status Summary

| Requirement | Status | Implementation |
|---|---|---|
| Cybersecurity officer designated | ✅ Compliant | IT Department Head |
| Critical systems identified | ✅ Compliant | See inventory in IRP |
| Incident detection system | ✅ Compliant | Wazuh SIEM + Snort IPS |
| Alert levels defined | ✅ Compliant | 5-tier matrix |
| Max containment times defined | ✅ Compliant | L1:4h / L3:60m / L5:15m |
| CSIRT reporting procedure | ✅ Compliant | Documented format |
| Staff training program | ✅ Compliant | 9-module annual program |
| Incident documentation process | ✅ Compliant | Ticket-based system |
| Security audits planned | ✅ Compliant | Annual external audit |
| ISO 27001 alignment | ✅ Partial | Controls mapped |

---

## Ley N°21.180 — Digital Transformation

Target compliance date: **2027**

| Requirement | Preparation Status |
|---|---|
| Digital document management | Infrastructure ready (network layer) |
| Online citizen services | Network capacity supports rollout |
| Electronic record systems | VLAN 10/50 allocated for future services |

---

## Technical Controls Mapped to ISO 27001

| ISO 27001 Control | Implementation |
|---|---|
| A.9 — Access Control | VLAN segmentation + Port Security + ACLs |
| A.10 — Cryptography | SSH on all devices, HTTPS on management |
| A.12 — Operations Security | Wazuh monitoring + Syslog centralization |
| A.13 — Communications Security | VLAN isolation + Inter-VLAN ACLs |
| A.16 — Incident Management | 5-level response plan + CSIRT reporting |
| A.17 — Business Continuity | HSRP redundancy + LACP link aggregation |
| A.18 — Compliance | This document |
