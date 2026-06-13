# Incident Response Plan
## Municipalidad de Melipilla — Network Security

**Compliance:** Ley Marco de Ciberseguridad N°21.663 (Chile)
**Version:** 1.0 | **Date:** 2025

---

## Alert Level Matrix

| Level | Trigger Condition | Max Containment Time |
|---|---|---|
| **1** | 5+ correlated events in < 15 minutes | 4 hours |
| **2** | Repeated failed authentications on management VLAN | 2 hours |
| **3** | Service outage on critical VLAN (10, 20, 40) | 60 minutes |
| **4** | Unauthorized device detected via port security | 30 minutes |
| **5** | Data exfiltration > 1 GB detected by IPS | 15 minutes |

---

## Response Workflow

```
DETECTION (Wazuh / Snort)
        │
        ▼
CLASSIFICATION (Level 1–5)
        │
        ├─ Level 1–2 → IT Team notified → investigate & document
        │
        ├─ Level 3–4 → Security Officer activated → isolate affected VLAN
        │
        └─ Level 5 → CSIRT Nacional notified within 3 hours (legal requirement)
                   → Evidence preservation
                   → Management escalation
```

---

## Critical Systems Inventory

| System | VLAN | Criticality | Recovery Priority |
|---|---|---|---|
| Core switches SWC1/SWC2 | 100 | Critical | P1 |
| Fortinet FortiGate HA pair | — | Critical | P1 |
| DHCP + Syslog Server | 40 | High | P2 |
| Wazuh SIEM + Snort IPS | 40 | High | P2 |
| Finance systems (VLAN 10) | 10 | High | P2 |
| Alcaldía systems | 20 | Medium | P3 |

---

## CSIRT Nacional Report Format

When a Level 4–5 incident occurs, the following must be reported:

```
- Ticket number (internal)
- Detection date and time
- Technical description of the incident
- Affected VLANs or servers
- Evidence: packet captures, Wazuh alerts, Snort logs
- Containment measures applied
- Current network status
- Estimated impact (users affected, data at risk)
```

**Reporting channel:** https://csirt.gob.cl

---

## Staff Security Training Program

9-module program delivered to all municipal employees:

1. What is cybersecurity and why it matters
2. Common threats: phishing, ransomware, social engineering
3. Password best practices and MFA
4. Email and web browsing security
5. Mobile device and remote work security
6. Internal security policies
7. Incident reporting procedures (who to call, when)
8. Software updates and patch management
9. Multi-factor authentication (MFA) setup and use

**Training frequency:** Annual mandatory + quarterly reminders
**Responsible:** IT Department (VLAN 40 team)
