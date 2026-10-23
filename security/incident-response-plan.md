# Plan de respuesta a incidentes

**Ámbito:** red propuesta para la Municipalidad de Melipilla
**Marco:** Ley N° 21.663 (Ley Marco de Ciberseguridad)
**Versión:** 1.1 · 2025

---

## Matriz de niveles de alerta

| Nivel | Condición de activación | Tiempo máximo de contención |
| --- | --- | --- |
| **1** | 5 o más eventos correlacionados en menos de 15 minutos | 4 horas |
| **2** | Autenticaciones fallidas repetidas en la VLAN de gestión | 2 horas |
| **3** | Caída de servicio en una VLAN crítica (10, 20, 40) | 60 minutos |
| **4** | Equipo no autorizado detectado por Port Security | 30 minutos |
| **5** | Exfiltración de datos superior a 1 GB detectada por el IPS | 15 minutos |

---

## Flujo de respuesta

```
DETECCIÓN (Wazuh / Snort)
        │
        ▼
CLASIFICACIÓN (nivel 1–5)
        │
        ├─ Nivel 1–2 → Equipo de Informática notificado → investiga y documenta
        │
        ├─ Nivel 3–4 → Se activa al encargado de ciberseguridad → aislamiento de la VLAN afectada
        │
        └─ Nivel 5 → Alerta temprana al CSIRT Nacional (ANCI) dentro de 3 horas
                   → Preservación de evidencia
                   → Escalamiento a la autoridad municipal
```

Cualquier incidente de nivel 4 o 5 con efectos significativos se reporta al CSIRT Nacional según los plazos legales: alerta temprana en 3 horas, segundo reporte en 72 horas e informe final en 15 días.

## Contención rápida

1. Identificar la VLAN afectada.
2. Bloquear los puertos comprometidos.
3. Respaldar la configuración de los equipos involucrados.
4. Notificar al CSIRT Nacional con el formato definido.

---

## Inventario de sistemas críticos

| Sistema | VLAN | Criticidad | Prioridad de recuperación |
| --- | --- | --- | --- |
| Switches de núcleo SWC1 / SWC2 | 100 | Crítica | P1 |
| Par de firewalls FortiGate | — | Crítica | P1 |
| Enlaces a Internet | — | Crítica | P1 |
| Servidor DHCP + Syslog | 40 | Alta | P2 |
| Wazuh SIEM + Snort IPS | 40 | Alta | P2 |
| Sistemas de Finanzas | 10 | Alta | P2 |
| Sistemas de Alcaldía | 20 | Media | P3 |

---

## Contenido del reporte al CSIRT Nacional

```
- Número de ticket interno
- Fecha y hora de detección
- Descripción técnica del incidente
- VLANs o servidores afectados
- Evidencia: capturas de tráfico, alertas de Wazuh, logs de Snort
- Medidas de contención aplicadas
- Estado actual de la red
- Impacto estimado (usuarios afectados, datos en riesgo)
```

**Canal:** plataforma de reporte de incidentes de la Agencia Nacional de Ciberseguridad (ANCI).

## Registro documental

- **Bitácora del incidente:** fecha, hora, VLANs o equipos afectados y acciones realizadas.
- **Informe forense:** hash SHA-256 de las evidencias y diagrama del ataque basado en MITRE ATT&CK.
- **Reporte al CSIRT Nacional:** según el formato anterior.

---

## Programa de capacitación

Programa anual de 9 módulos para funcionarios municipales:

1. Qué es la ciberseguridad y por qué importa
2. Amenazas comunes: phishing, ransomware e ingeniería social
3. Buenas prácticas de contraseñas
4. Seguridad en el correo y la navegación web
5. Seguridad en dispositivos móviles y trabajo remoto
6. Políticas internas de seguridad
7. Cómo reportar un incidente (a quién y cuándo)
8. Actualizaciones de software y parches
9. Configuración y uso de la autenticación multifactor (MFA)

**Frecuencia:** anual obligatoria, con recordatorios trimestrales
**Responsable:** Departamento de Informática
