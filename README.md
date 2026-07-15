Renovación de Infraestructura de Red — Municipalidad de Melipilla
Proyecto de título desarrollado para la Ilustre Municipalidad de Melipilla, que abordó una renovación completa de la infraestructura de red de un recinto que no había sido actualizado significativamente en más de una década.
---
Resumen del Proyecto
	
Cliente	Ilustre Municipalidad de Melipilla, Chile
Alcance	Renovación completa de infraestructura LAN (Capa 2/3 + Seguridad)
Duración	105 días (marzo – agosto 2025)
Equipo	Juan Tapia Galleguillos · Lucas Huerta Cordero
Institución	Duoc UC — Ingeniería en Conectividad y Redes
Cumplimiento	Ley N°21.663 (Marco de Ciberseguridad) · Ley N°21.180 (Transformación Digital)
---
Problemática
La red existente presentaba fallas recurrentes de conectividad y no cumplía con la Ley Marco de Ciberseguridad (N°21.663), poniendo en riesgo la operación diaria del municipio y los datos de la ciudadanía.
Problemas específicos detectados en el diagnóstico inicial:
Red plana sin segmentación — todos los departamentos compartían el mismo dominio de broadcast
Direccionamiento IP estático en más de 200 equipos
Sin redundancia — una falla en el core dejaba caído todo el edificio
Sin detección de intrusiones ni registro centralizado de eventos
Cableado sin etiquetar ni documentar
Incumplimiento de la Ley N°21.663
---
Arquitectura de la Solución
Topología de Red
Se diseñó e implementó un modelo jerárquico de tres capas:
```
Internet (ISP GTD x2)
        |
  ------+------
  FW1         FW2          Fortinet FortiGate 200F (par HA)
  |            |
  SWC1 ==== SWC2           Core: Cisco Catalyst C9500-24Y4C
       LACP                Grupos HSRP 1 y 2 (Activo/Pasivo)
  SW3  ==== SW4            Distribución: Cisco C9300-48P-A
  |            |           RPVST+ por VLAN
  +-----------+
        |
  [11 racks de acceso distribuidos en 3 pisos]
```
Segmentación de VLANs
VLAN	Departamento	Subred
10	Administración / Finanzas	10.255.10.0/24
20	Alcaldía	10.255.20.0/24
30	Recursos Humanos	10.255.30.0/24
40	Departamento de TI	10.255.40.0/24
50	Servicios / DIDECO	10.255.50.0/24
60	Obras Públicas	10.255.60.0/24
70	Municipal General	10.255.70.0/24
99	VLAN Nativa	—
100	Gestión de Red	10.255.100.0/24
---
Stack Tecnológico
Alta Disponibilidad
HSRP — dos grupos activo/pasivo, uno por clúster de VLANs
LACP IEEE 802.3ad — agregación de enlaces entre switches core
RPVST+ IEEE 802.1w — convergencia rápida de spanning tree por VLAN
Seguridad
Fortinet FortiGate 200F en par HA como perímetro de red
Suricata IDS/IPS — detección y prevención de intrusiones en línea, modo inline con reglas Emerging Threats
Wazuh SIEM — correlación centralizada de eventos de seguridad, integrado nativamente con las alertas de Suricata
Rsyslog — recolección de logs de todos los dispositivos de red
Port Security — vinculación de MAC en todos los puertos de acceso
BPDUGuard + PortFast — endurecimiento de puertos de borde
VACL + ACL — filtrado de tráfico entre VLANs
SSH en todos los dispositivos — Telnet deshabilitado
Infraestructura Física
Fibra óptica multimodo OM3 50/125 — backbone de 1500m
Cableado estructurado UTP Cat6A, completamente etiquetado y documentado
12 racks: 1 core + 11 de distribución-acceso
---
Cumplimiento de Ciberseguridad — Ley N°21.663
Requisito	Implementación
Sistema de detección de incidentes	Wazuh SIEM + Suricata IDS/IPS
Niveles de alerta definidos	Matriz de 5 niveles
Tiempos máximos de contención	N1: 4h / N3: 60min / N5: 15min
Reporte al CSIRT nacional	Formato de reporte estandarizado documentado
Programa de capacitación	Programa de 9 módulos para funcionarios municipales
Encargado de seguridad designado	Definido en la documentación de gobernanza
---
Demostraciones en Video
Todas las configuraciones fueron probadas y grabadas:
Demo	Descripción
Servidor DHCP	Asignación dinámica de IPs en todas las VLANs
Redundancia HSRP	Simulación de failover con caída del switch core
Servidor Syslog	Recolección centralizada de logs desde dispositivos Cisco
Wazuh SIEM	Dashboard de eventos de seguridad y correlación de alertas
Trazabilidad de Red	Verificación de flujo de paquetes de extremo a extremo
---
Presupuesto
Equipo	Cant.	Precio Unit. (CLP)	Total (CLP)
Cisco Catalyst C9500-24Y4C	4	$5.478.820	$21.915.280
Cisco Catalyst C9300-48P-A	18	$4.100.000	$73.800.000
Dell PowerEdge R440	2	$1.544.897	$3.089.794
Fibra Óptica OM3 50/125 (1500m)	1	—	$1.101.000
Total			~$99.906.074
---
Estructura del Repositorio
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
Lecciones Aprendidas
El mayor desafío fue lograr que HSRP y RPVST+ funcionaran correctamente en conjunto — alinear los root bridges con los gateways HSRP activos de cada clúster de VLANs requirió varias iteraciones y pruebas de failover antes de lograr un comportamiento consistente.
Integrar Wazuh con Rsyslog para recibir logs desde dispositivos Cisco también tomó más tiempo del esperado, principalmente por diferencias en el formato de syslog entre versiones de IOS.
---
Autor
Lucas Huerta Cordero
Ingeniería en Conectividad y Redes — Duoc UC (2025)
Santiago, Chile
LinkedIn · GitHub
---
Cisco IOS · Fortinet FortiOS · Wazuh · Suricata · Linux
