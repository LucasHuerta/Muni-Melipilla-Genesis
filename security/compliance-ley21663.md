# Cumplimiento normativo

## Ley N° 21.663 — Ley Marco de Ciberseguridad

Medidas incluidas en el diseño propuesto. Al tratarse de una propuesta validada en laboratorio, el estado indica si la obligación quedó **cubierta en el diseño**, no si está implementada en el municipio.

| Obligación | Estado en el diseño | Medida propuesta |
| --- | --- | --- |
| Responsable de ciberseguridad | Cubierta | Perfil de encargado de ciberseguridad (requisitos y certificaciones) |
| Identificación de sistemas críticos | Cubierta | Inventario en el [plan de respuesta](incident-response-plan.md) |
| Detección de incidentes | Cubierta | Wazuh SIEM + Snort IPS + Syslog centralizado |
| Niveles de alerta | Cubierta | Matriz de 5 niveles |
| Tiempos máximos de contención | Cubierta | N1: 4 h · N3: 60 min · N5: 15 min |
| Reporte de incidentes al CSIRT Nacional (ANCI) | Cubierta | Procedimiento y formato de reporte con los plazos legales |
| Capacitación del personal | Cubierta | Programa anual para funcionarios |
| Registro documental de incidentes | Cubierta | Bitácora, informe forense y reporte al CSIRT |
| Auditorías de seguridad | Propuesta | Auditoría externa anual |

### Plazos de reporte de incidentes

| Reporte | Plazo |
| --- | --- |
| Alerta temprana | 3 horas desde que se toma conocimiento del incidente |
| Segundo reporte | 72 horas |
| Informe final | 15 días desde la alerta temprana |

Los reportes se envían al CSIRT Nacional a través de la plataforma de la Agencia Nacional de Ciberseguridad (ANCI).

---

## Ley N° 21.180 — Transformación Digital del Estado

| Exigencia | Preparación desde la red |
| --- | --- |
| Gestión documental digital | Infraestructura de red con capacidad y segmentación para soportarla |
| Servicios en línea a la ciudadanía | Doble enlace a Internet y núcleo redundante |
| Expedientes electrónicos | Segmentos dedicados (VLAN 10 y 50) |

---

## Controles técnicos y su relación con ISO/IEC 27001:2022 (Anexo A)

| Control | Medida en el diseño |
| --- | --- |
| 5.24 – 5.26 Gestión de incidentes | Plan de respuesta de 5 niveles y reporte al CSIRT Nacional |
| 5.30 Preparación de TIC para la continuidad | HSRP, LACP y doble enlace a Internet |
| 6.3 Concientización y capacitación | Programa de capacitación para funcionarios |
| 7.11 Servicios de soporte | UPS en sala principal y puntos críticos |
| 8.5 Autenticación segura | Administración de equipos por SSH |
| 8.15 Registro de eventos | Syslog centralizado y Wazuh |
| 8.16 Actividades de monitoreo | Wazuh SIEM y Snort IPS |
| 8.17 Sincronización de relojes | NTP en todos los equipos |
| 8.20 Seguridad de redes | Port Security, BPDU Guard, ACL y VACL |
| 8.22 Segregación de redes | Segmentación por VLAN y gestión aislada (VLAN 100) |
