# Diseño de VLANs

## Objetivo

Describir la segmentación por VLAN propuesta para la renovación de la red de la Municipalidad de Melipilla. El direccionamiento corresponde al diseño propuesto, no a la red en operación.

## Principios de diseño

- **Mínimo privilegio:** cada departamento queda aislado por defecto.
- **Protección de segmentos críticos:** Finanzas y la gestión de red en segmentos separados.
- **Escalabilidad:** subredes /24, hasta 254 equipos por departamento.
- **Cumplimiento:** la segmentación apoya las medidas de seguridad exigidas por la Ley N° 21.663.

## Tabla de VLANs

| VLAN | Nombre | Subred | Gateway (VIP HSRP) | Grupo HSRP | Uso |
| --- | --- | --- | --- | --- | --- |
| 10 | ADMINISTRACION | 10.255.10.0/24 | 10.255.10.1 | 1 | Administración y Finanzas |
| 20 | ALCALDIA | 10.255.20.0/24 | 10.255.20.1 | 1 | Alcaldía |
| 30 | RRHH | 10.255.30.0/24 | 10.255.30.1 | 1 | Recursos Humanos |
| 40 | INFORMATICA | 10.255.40.0/24 | 10.255.40.1 | 1 | Departamento de Informática |
| 50 | SERVICIOS | 10.255.50.0/24 | 10.255.50.1 | 2 | DIDECO y servicios |
| 60 | OBRAS | 10.255.60.0/24 | 10.255.60.1 | 2 | Obras |
| 70 | MUNICIPIO | 10.255.70.0/24 | 10.255.70.1 | 2 | Municipal general |
| 99 | NATIVA | — | — | — | VLAN nativa sin uso (endurecida) |
| 100 | GESTION | 10.255.100.0/24 | 10.255.100.1 | 1 y 2 | Gestión de switches y equipos |

## Grupos HSRP

### Grupo 1 (SWC1 activo)

- VLANs: 10, 20, 30, 40
- Activo: SWC1 (prioridad 110)
- Standby: SWC2 (prioridad 90)
- IP virtual por VLAN: x.x.x.1

### Grupo 2 (SWC2 activo)

- VLANs: 50, 60, 70
- Activo: SWC2 (prioridad 110)
- Standby: SWC1 (prioridad 90)

Con dos grupos, cada switch de núcleo es activo para la mitad de las VLANs y la carga se reparte entre ambos.

## Control de acceso entre VLANs

ACL aplicadas en las SVI:

```
- VLAN 100 (Gestión): accesible solo desde la VLAN 40 (Informática)
- VLAN 10 (Finanzas): sin acceso entrante desde la VLAN 70 (General)
- Todas las VLANs: denegado por defecto hacia la VLAN 100, salvo lo permitido explícitamente
- Tráfico de agentes Wazuh permitido desde todas las VLANs hacia la VLAN 40
```

## Port Security en puertos de acceso

```cisco
switchport port-security maximum 2
switchport port-security violation restrict
switchport port-security mac-address sticky
spanning-tree portfast
spanning-tree bpduguard enable
```

## Raíz de RPVST+

La raíz de cada grupo de VLANs coincide con su gateway HSRP activo, para evitar que el tráfico cruce el enlace entre switches de núcleo.

| VLANs | Raíz primaria | Raíz secundaria |
| --- | --- | --- |
| 10, 20, 30, 40 | SWC1 | SWC2 |
| 50, 60, 70 | SWC2 | SWC1 |
| 100 | SWC1 | SWC2 |
