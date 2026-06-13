# VLAN Design Documentation

## Overview

This document details the VLAN segmentation strategy implemented for the Municipalidad de Melipilla network renewal project.

## Design Principles

- **Least privilege**: each department isolated by default
- **Critical VLAN protection**: finance and management on separate segments
- **Scalability**: /24 subnets allow up to 253 hosts per department
- **Compliance**: segmentation required by Ley Marco 21.663 for critical systems

## VLAN Table

| VLAN ID | Name | Subnet | Gateway | HSRP Group | Purpose |
|---|---|---|---|---|---|
| 10 | ADMINISTRACION | 10.255.10.0/24 | 10.255.10.1 | HSRP-1 | Finance & Administration |
| 20 | ALCALDIA | 10.255.20.0/24 | 10.255.20.1 | HSRP-1 | Mayor's Office |
| 30 | RRHH | 10.255.30.0/24 | 10.255.30.1 | HSRP-1 | Human Resources |
| 40 | INFORMATICA | 10.255.40.0/24 | 10.255.40.1 | HSRP-1 | IT Department |
| 50 | SERVICIOS | 10.255.50.0/24 | 10.255.50.1 | HSRP-2 | DIDECO & Services |
| 60 | OBRAS | 10.255.60.0/24 | 10.255.60.1 | HSRP-2 | Public Works |
| 70 | MUNICIPIO | 10.255.70.0/24 | 10.255.70.1 | HSRP-2 | General Municipal |
| 99 | NATIVE | — | — | — | Native/Untagged (hardened) |
| 100 | MANAGEMENT | 10.255.100.0/24 | 10.255.100.1 | Both | Switch & device management |

## HSRP Groups

### Group 1 (SWC1 Active)
- VLANs: 10, 20, 30, 40
- Active: SWC1 (priority 110)
- Standby: SWC2 (priority 90)
- Virtual IP per VLAN: x.x.x.1

### Group 2 (SWC2 Active)
- VLANs: 50, 60, 70, 99
- Active: SWC2 (priority 110)
- Standby: SWC1 (priority 90)
- Load balanced across both core switches

## Inter-VLAN Access Control

ACLs applied at SVI (Switched Virtual Interface) level:

```
- VLAN 100 (Management) → accessible only from VLAN 40 (IT)
- VLAN 10 (Finance) → no inbound access from VLAN 70 (General)
- All VLANs → deny by default to VLAN 100 unless explicitly permitted
- Wazuh agent traffic permitted outbound from all VLANs to VLAN 40
```

## Port Security Configuration

All access ports configured with:

```cisco
switchport port-security maximum 2
switchport port-security violation restrict
switchport port-security mac-address sticky
spanning-tree portfast
spanning-tree bpduguard enable
```

## RPVST+ Root Bridge Assignment

| VLAN Group | Primary Root | Secondary Root |
|---|---|---|
| 10, 20, 30, 40 | SWC1 | SWC2 |
| 50, 60, 70, 99 | SWC2 | SWC1 |
| 100 | SWC1 | SWC2 |
