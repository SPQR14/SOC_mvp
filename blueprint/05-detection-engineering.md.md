# Detection Engineering

> Este documento define los principios, procesos, estándares y arquitectura de detección de la plataforma SOC.

>

> La disciplina de Detection Engineering constituye el núcleo de la capacidad defensiva del SOC y representa uno de los principales activos intelectuales de la organización.

---

# 1. Objetivo

## Propósito

Diseñar, implementar y mantener mecanismos de detección capaces de identificar actividad maliciosa o anómala dentro de los entornos monitorizados.

Las detecciones deberán:

- Ser reproducibles.

- Ser auditables.

- Estar documentadas.

- Ser medibles.

- Evolucionar continuamente.

---

## Resultado Esperado

Transformar telemetría en:

```text

Alertas Accionables

Incidentes

Investigaciones

Casos de Uso

```

---

# 2. Principios de Diseño

---

## Detection as Code

Toda detección debe gestionarse como código.

No se permitirán reglas creadas únicamente desde interfaces gráficas.

---

### Requisitos

- Versionado Git.

- Revisión por pares.

- Historial de cambios.

- Aprobación formal.

---

## ATT&CK First

Toda detección deberá estar asociada a una o varias técnicas MITRE ATT&CK.

---

### Ejemplo

```text

PowerShell EncodedCommand

-> T1059.001

```

---

## Context Driven

Las alertas deben proporcionar contexto suficiente para su investigación.

---

### Incorrecto

```text

PowerShell detected

```

---

### Correcto

```text

PowerShell detected

Usuario

Host

Comando

IP origen

Hora

MITRE

```

---

## Reduce Alert Fatigue

La calidad es más importante que la cantidad.

---

# 3. Framework de Referencia

## MITRE ATT&CK

Framework principal de modelado adversario.

---

### Objetivos

- Cobertura de amenazas.

- Medición.

- Reporting.

- Priorización.

---

## ATT&CK Domains

Inicialmente:

```text

Enterprise ATT&CK

```

---

## Técnicas Prioritarias

### Execution

```text

T1059

```

---

### Persistence

```text

T1547

```

---

### Credential Access

```text

T1003

```

---

### Discovery

```text

T1087

```

---

### Lateral Movement

```text

T1021

```

---

### Command and Control

```text

T1071

```

---

# 4. Arquitectura de Detección

```mermaid

flowchart TB

Telemetry

--> Detection Pipeline

Detection Pipeline

--> Rules

Rules

--> Alert

Alert

--> Validation

Validation

--> Incident

Validation

--> False Positive

Incident

--> TheHive

```

---

# 5. Fuentes de Detección

## Endpoint

### Windows

- Sysmon

- Security Logs

- PowerShell

---

### Linux

- auditd

- Syslog

- Auth Logs

---

## Network

### Suricata

- IDS

- IOC Matching

---

### Zeek

- DNS

- TLS

- HTTP

- Metadata

---

## Futuras

### AWS

- CloudTrail

- GuardDuty

---

### GCP

- Audit Logs

- SCC

---

# 6. Ciclo de Vida de una Detección

## Fase 1

Identificación de amenaza.

---

## Fase 2

Mapeo ATT&CK.

---

## Fase 3

Diseño de lógica.

---

## Fase 4

Implementación.

---

## Fase 5

Validación.

---

## Fase 6

Despliegue.

---

## Fase 7

Monitorización.

---

## Fase 8

Mejora continua.

---

# 7. Estructura del Repositorio

```text

detections/

├── sigma/

│

├── wazuh/

│

├── suricata/

│

├── zeek/

│

├── mappings/

│

└── testing/

```

---

# 8. Sigma como Lenguaje Fuente

## Decisión Arquitectónica

Sigma será el formato canónico de detección.

---

## Motivos

- Portable.

- Independiente del SIEM.

- Fácil de mantener.

- Compatible con ATT&CK.

---

## Flujo

```text

Sigma

↓

Conversión

↓

Wazuh

↓

Validación

↓

Producción

```

---

# 9. Clasificación de Detecciones

## Nivel 1

IOC Based.

---

### Ejemplos

- Hash

- Dominio

- IP

---

## Nivel 2

Behavior Based.

---

### Ejemplo

```text

PowerShell

+

Network Connection

```

---

## Nivel 3

Correlation Based.

---

### Ejemplo

```text

Create User

+

Privilege Escalation

```

---

## Nivel 4

Threat Hunting Derived.

---

### Fuente

Hallazgos propios.

---

# 10. Cobertura ATT&CK

## Objetivo Inicial

```text

25 técnicas

```

---

## Objetivo Intermedio

```text

50 técnicas

```

---

## Objetivo MSSP

```text

100+ técnicas

```

---

# 11. Testing

Toda detección deberá probarse.

---

## Tipos

### Positivo

Verifica que detecta.

---

### Negativo

Verifica que no genera ruido.

---

### Rendimiento

Verifica impacto.

---

# 12. Falsos Positivos

## Registro Obligatorio

Toda regla documentará:

```text

Casos esperados

Casos no esperados

Limitaciones

```

---

## Métrica

```text

False Positive Rate

```

---

# 13. Integración con SIEM

Las reglas deberán generar:

```text

Severidad

MITRE

Descripción

Recomendación

Referencia

```

---

# 14. Integración con Incident Response

Toda alerta relevante deberá proporcionar:

- Contexto.

- Artefactos.

- Evidencia.

- Técnica ATT&CK.

---

# 15. Integración con IA

## Detection Agent

Responsable de:

- Proponer reglas.

- Generar Sigma.

- Generar Suricata.

- Generar Wazuh.

---

## QA Agent

Responsable de:

- Revisar reglas.

- Detectar errores.

- Buscar redundancias.

---

# 16. Métricas de Calidad

## Cobertura

Técnicas ATT&CK cubiertas.

---

## Precisión

False Positive Rate.

---

## Eficiencia

MTTD.

---

## Efectividad

Incidentes detectados.

---

# 17. Roadmap de Madurez

## Nivel 1

IOC Matching.

---

## Nivel 2

Behavioral Detection.

---

## Nivel 3

Correlation.

---

## Nivel 4

Threat Hunting Driven Detection.

---

## Nivel 5

AI Assisted Detection Engineering.

---

# 18. Riesgos

## Riesgo

Exceso de reglas.

### Mitigación

Catálogo centralizado.

---

## Riesgo

Cobertura insuficiente.

### Mitigación

Matriz ATT&CK.

---

## Riesgo

Reglas obsoletas.

### Mitigación

Revisión periódica.

---

# 19. KPIs

- Total de detecciones.

- Cobertura ATT&CK.

- False Positive Rate.

- Alertas por técnica.

- MTTD.

- Reglas revisadas por mes.

---

# 20. Criterios de Aceptación

La capa Detection Engineering se considerará implementada cuando:

- [ ] Existe repositorio de detecciones.

- [ ] Sigma es el formato canónico.

- [ ] Existe mapeo ATT&CK.

- [ ] Se documenta el ciclo de vida.

- [ ] Existe proceso de testing.

- [ ] Existe control de falsos positivos.

- [ ] Existe catálogo centralizado.

- [ ] Existen métricas de cobertura.

- [ ] Existe integración con IR.

- [ ] Existe integración con IA.