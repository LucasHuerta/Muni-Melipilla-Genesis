# Renovación de Infraestructura de Red — Municipalidad de Melipilla

Proyecto de título desarrollado para la Ilustre Municipalidad de Melipilla, abordando la renovación completa de su infraestructura de red, la cual llevaba más de una década sin actualizaciones significativas.

---

## Contexto del proyecto

| | |
|---|---|
| **Cliente** | Ilustre Municipalidad de Melipilla, Chile |
| **Alcance** | Renovación completa LAN (Capa 2/3 + Seguridad) |
| **Duración** | 105 días (Marzo – Agosto 2025) |
| **Equipo** | Juan Tapia Galleguillos · Lucas Huerta Cordero |
| **Institución** | Duoc UC — Ingeniería en Conectividad y Redes |
| **Normativa** | Ley N°21.663 · Ley N°21.180 |

---

## El problema

La red existente presentaba fallas recurrentes de conectividad y no cumplía con la Ley Marco de Ciberseguridad (N°21.663), lo que ponía en riesgo la operación diaria de la municipalidad y los datos de los ciudadanos.

Problemas específicos detectados en el levantamiento inicial:

- Red plana sin segmentación — todos los departamentos en el mismo dominio de broadcast
- Direccionamiento IP estático en más de 200 equipos
- Sin redundancia: una falla en el core dejaba sin red a todo el edificio
- Sin sistema de detección de intrusos ni registro centralizado de eventos
- Cableado sin etiquetar ni documentar
- Incumplimiento de la Ley N°21.663

---

## Solución implementada

### Topología de red

Se diseñó e implementó un modelo jerárquico de tres capas:

```
Internet (GTD ISP x2)
        |
  ------+------
  FW1         FW2          Fortinet FortiGate 200F (par HA)
  |            |
  SWC1 ==== SWC2           Core: Cisco Catalyst C9500-24Y4C
       LACP                HSRP Grupos 1 y 2 (Activo/Pasivo)
  SW3  ==== SW4            Distribucion: Cisco C9300-48P-A
  |            |           RPVST+ por VLAN
  +-----------+
        |
  [11 racks de acceso — 3 pisos]
```

### Segmentación por VLANs

| VLAN | Departamento | Red |
|---|---|---|
| 10 | Administración / Finanzas | 10.255.10.0/24 |
| 20 | Alcaldía | 10.255.20.0/24 |
| 30 | RRHH | 10.255.30.0/24 |
| 40 | Informática | 10.255.40.0/24 |
| 50 | Servicios / DIDECO | 10.255.50.0/24 |
| 60 | Obras Municipales | 10.255.60.0/24 |
| 70 | Municipio / Biblioteca | 10.255.70.0/24 |
| 99 | VLAN Nativa | — |
| 100 | Gestión de red | 10.255.100.0/24 |

---

## Stack tecnológico

**Alta disponibilidad**
- HSRP — dos grupos activo/pasivo, uno por cluster de VLANs
- LACP IEEE 802.3ad — agregación de enlaces entre switches core
- RPVST+ IEEE 802.1w — convergencia rápida por VLAN

**Seguridad**
- Fortinet FortiGate 200F en par HA como perímetro
- Snort IPS — detección y prevención de intrusos en línea
- Wazuh SIEM — correlación centralizada de eventos de seguridad
- Rsyslog — recolección de logs desde todos los dispositivos
- Port Security — binding de MAC en puertos de acceso
- BPDUGuard + PortFast — hardening de puertos edge
- VACL + ACL — filtrado de tráfico inter-VLAN
- SSH en todos los dispositivos (sin Telnet)

**Infraestructura física**
- Fibra óptica multimodo OM3 50/125 — 1500m de backbone
- Cableado estructurado Cat6A etiquetado y documentado
- 12 racks: 1 core + 11 de distribución-acceso

---

## Cumplimiento Ley N°21.663

| Requisito | Implementación |
|---|---|
| Sistema de detección de incidentes | Wazuh SIEM + Snort IPS |
| Niveles de alerta definidos | Matriz de 5 niveles |
| Tiempos máximos de contención | N1: 4h / N3: 60min / N5: 15min |
| Reporte al CSIRT Nacional | Formato estandarizado documentado |
| Programa de capacitación | 9 módulos para funcionarios |
| Encargado de ciberseguridad | Definido en documentación de gobernanza |

---

## Demostraciones

Las configuraciones fueron probadas y grabadas en video:

| Demo | Descripción |
|---|---|
| [DHCP Server](evidence/videos/demo-dhcp-server.mp4) | Asignación dinámica de IPs en todas las VLANs |
| [HSRP Redundancy](evidence/videos/demo-hsrp-redundancy.mp4) | Simulación de failover con caída del switch core |
| [Syslog Server](evidence/videos/demo-syslog-server.mp4) | Recolección centralizada de logs desde equipos Cisco |
| [Wazuh SIEM](evidence/videos/demo-wazuh-siem.mp4) | Dashboard de eventos y correlación de alertas |
| [Network Tracking](evidence/videos/demo-network-tracking.mp4) | Verificación de flujo de paquetes extremo a extremo |

---

## Presupuesto

| Equipo | Cant. | Precio unit. (CLP) | Total (CLP) |
|---|---|---|---|
| Cisco Catalyst C9500-24Y4C | 4 | $5.478.820 | $21.915.280 |
| Cisco Catalyst C9300-48P-A | 18 | $4.100.000 | $73.800.000 |
| Dell PowerEdge R440 | 2 | $1.544.897 | $3.089.794 |
| Fibra óptica OM3 50/125 (1500m) | 1 | — | $1.101.000 |
| **Total** | | | **~$99.906.074** |

---

## Estructura del repositorio

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

## Lo que aprendimos en el proceso

El mayor desafío fue la implementación de HSRP junto con RPVST+ de forma simultánea — hacer que los root bridges coincidieran con los gateways activos de cada grupo requirió varias iteraciones y pruebas de failover hasta que el comportamiento fue el esperado.

La integración de Wazuh con Rsyslog para recibir logs de los equipos Cisco también tomó más tiempo del planificado, principalmente por diferencias en el formato de los syslog entre IOS versions.

---

## Autor

Lucas Huerta Cordero
Ingeniería en Conectividad y Redes — Duoc UC (2025)
Región Metropolitana, Chile

[LinkedIn](https://linkedin.com/in/tu-perfil) · [GitHub](https://github.com/LucasHuerta)

---

*Cisco IOS · Fortinet FortiOS · Wazuh · Snort · Linux*
