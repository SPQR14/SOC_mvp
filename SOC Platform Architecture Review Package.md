
# Executive Summary

Este documento describe la arquitectura propuesta para construir una plataforma SOC (Security Operations Center) moderna, portable y escalable.

La estrategia seleccionada consiste en:

```text

Local First

↓

SOC Operativo

↓

Clientes Piloto

↓

MSSP

↓

AWS o GCP

```

El objetivo es minimizar costes iniciales, validar procesos operativos y construir propiedad intelectual antes de adoptar dependencias cloud específicas.

La plataforma está diseñada para proporcionar:

- Security Monitoring

- SIEM

- Network Detection & Response

- Detection Engineering

- Threat Hunting

- Incident Response

- SOAR

- AI-Assisted Security Operations

- MSSP Multi-Tenant

---

# Objetivos Estratégicos

## Corto Plazo

Construir un SOC funcional completamente operable en infraestructura local.

Capacidades objetivo:

- Recolección de eventos

- SIEM funcional

- IDS/NDR

- Gestión de incidentes

- Automatización básica

---

## Medio Plazo

Incrementar madurez operativa.

Capacidades objetivo:

- Threat Hunting

- SOAR

- Casos de uso ATT&CK

- IA aplicada

- Reporting automatizado

---

## Largo Plazo

Transformar la plataforma en un MSSP.

Capacidades objetivo:

- Multi-tenancy

- Automatización avanzada

- Operación multi-cliente

- Escalabilidad cloud

- Agentes IA especializados

---

# Principios Arquitectónicos

## Cloud Agnostic

La arquitectura no dependerá de AWS ni GCP.

Debe poder desplegarse en:

```text

On-Premise

AWS

GCP

```

sin rediseños significativos.

---

## Container First

Todos los componentes deben poder desplegarse mediante contenedores.

Tecnología inicial:

```text

Docker

Docker Compose

```

---

## Automation First

Todo procedimiento repetitivo debe ser susceptible de automatización futura.

---

## AI Native

La IA es una capacidad nativa de la plataforma.

No se considera una integración secundaria.

---

## Detection as Code

Las detecciones son propiedad intelectual.

Todo el contenido deberá:

- Versionarse

- Mantenerse en Git

- Revisarse

- Probarse

- Documentarse

---

# Arquitectura General

```text

Telemetry Sources

↓

Collection Layer

↓

SIEM Layer

↓

Detection Layer

↓

Incident Response Layer

↓

SOAR Layer

↓

AI Layer

↓

Reporting Layer

```

---

# Arquitectura por Capas

## Collection Layer

Responsable de recopilar telemetría.

Fuentes previstas:

- Windows

- Linux

- Aplicaciones

- Dispositivos de Red

- Entornos Cloud

Herramienta propuesta:

```text

Wazuh Agents

```

---

## SIEM Layer

Responsable de:

- Ingesta

- Correlación

- Almacenamiento

- Búsquedas

- Dashboards

Herramientas propuestas:

```text

Wazuh

OpenSearch

```

---

## Network Detection Layer

Responsable de:

- IDS

- Metadata

- Network Hunting

- Detección de amenazas

Herramientas propuestas:

```text

Suricata

Zeek

```

---

## Detection Engineering Layer

Responsable de:

- Sigma

- Correlación

- ATT&CK Mapping

- Casos de uso

Herramientas propuestas:

```text

Sigma

MITRE ATT&CK

Git

```

---

## Incident Response Layer

Responsable de:

- Casos

- Evidencias

- Investigación

Herramientas propuestas:

```text

TheHive

Cortex

```

---

## SOAR Layer

Responsable de:

- Automatización

- Orquestación

- Integraciones

Herramienta propuesta:

```text

Shuffle

```

---

## AI Layer

Responsable de:

- Triage

- Hunting

- Reporting

- Detección asistida

Herramientas propuestas:

```text

Ollama

Qwen

Llama

```

---

# Herramientas Open Source Seleccionadas

## SIEM

### Wazuh

Funciones:

- Log Collection

- FIM

- Vulnerability Detection

- Compliance Monitoring

Ventajas:

- Open Source

- Amplia comunidad

- Integración nativa con OpenSearch

---

### OpenSearch

Funciones:

- Almacenamiento de logs

- Índices

- Dashboards

- Hunting

Ventajas:

- Escalable

- Open Source

- Compatible con observabilidad

---

# Network Detection & Response

## Suricata

Funciones:

- IDS

- IPS futuro

- IOC Matching

Ventajas:

- Alto rendimiento

- ET Open

- Amplia adopción

---

## Zeek

Funciones:

- Metadata Collection

- Análisis de protocolos

- Hunting

Ventajas:

- Contexto muy rico

- Excelente para investigación

---

# Incident Response

## TheHive

Funciones:

- Gestión de casos

- Coordinación de incidentes

- Evidencias

---

## Cortex

Funciones:

- Enriquecimiento automático

- Integración Threat Intelligence

Fuentes previstas:

- VirusTotal

- OTX

- AbuseIPDB

- URLHaus

- Shodan

---

# SOAR

## Shuffle

Funciones:

- Automatización

- Integraciones

- Workflows

Ventajas:

- Open Source

- Orientado a SOC

- Integración con TheHive

---

# Inteligencia Artificial

## Ollama

Funciones:

- Hosting local de modelos

- Privacidad de datos

Ventajas:

- Sin costes cloud

- Fácil despliegue

---

## Qwen 2.5

Uso previsto:

- Triage

- Hunting

- Clasificación

---

## Llama 3

Uso previsto:

- Reporting

- Explicaciones

- Documentación

---

# Reverse Proxy

## Traefik

Funciones:

- TLS

- Reverse Proxy

- Service Discovery

Ventajas:

- Nativo para Docker

---

# PKI

## Step-CA

Funciones:

- Certificados internos

- PKI corporativa

Ventajas:

- Ligero

- Open Source

---

# Secret Management

## Inicial

```text

Docker Secrets

```

---

## Futuro

```text

HashiCorp Vault

```

---

# Documentación

## Obsidian

Funciones:

- Blueprint

- Runbooks

- Playbooks

- Investigación

---

# Control de Versiones

## Git

Funciones:

- Infraestructura

- Detecciones

- Automatizaciones

- Documentación

---

# Stack MVP Propuesto

```text

SIEM

├── Wazuh

└── OpenSearch

NDR

├── Suricata

└── Zeek

IR

├── TheHive

└── Cortex

SOAR

└── Shuffle

AI

├── Ollama

├── Qwen

└── Llama

Platform

├── Docker

├── Traefik

├── Step-CA

└── Ubuntu Server

```

---

# Comparativa Local vs AWS vs GCP

| Capacidad | Local | AWS | GCP |

|------------|---------|---------|---------|

| Compute | Docker | EC2 | Compute Engine |

| Logging | Syslog/Wazuh | CloudTrail | Cloud Audit Logs |

| Detección Cloud | Suricata/Zeek | GuardDuty | Security Command Center |

| Compliance | OpenSCAP | AWS Config | Security Health Analytics |

| Storage | MinIO/NFS | S3 | Cloud Storage |

| SOAR | Shuffle | Shuffle | Shuffle |

| IA | Ollama | Bedrock | Vertex AI |

---

# Ventajas de la Estrategia Local First

## Técnicas

- Sin dependencia cloud

- Mayor control

- Aprendizaje operativo acelerado

---

## Económicas

- Costes mínimos

- Sin consumo cloud prematuro

- Menor riesgo financiero

---

## Estratégicas

- Diseño neutral

- Validación antes de escalar

- Construcción de propiedad intelectual

---

# Riesgos Identificados

## Complejidad Prematura

Mitigación:

```text

Implementación progresiva

```

---

## Dependencia Tecnológica

Mitigación:

```text

Cloud Agnostic Design

```

---

## Costes Operativos

Mitigación:

```text

Validación local antes de migrar

```

---

## IA Inmadura

Mitigación:

```text

Human-in-the-loop

```

---

# Arquitectura Multi-Tenant

Modelo objetivo:

```text

Tenant A

Tenant B

Tenant C

```

Separación mediante:

- tenant_id

- RBAC

- Dashboards

- Reporting

- Casos

- Automatizaciones

---

# Agentes IA Planificados

## Triage Agent

Responsable de:

- Clasificar alertas

- Priorizar incidentes

---

## Threat Hunter Agent

Responsable de:

- Generar hipótesis

- Proponer consultas

---

## Detection Engineer Agent

Responsable de:

- Crear Sigma

- Crear reglas Wazuh

- Crear reglas Suricata

---

## Reporting Agent

Responsable de:

- Informes técnicos

- Informes ejecutivos

---

## Knowledge Agent

Responsable de:

- Consultar documentación

- Buscar procedimientos

- Recuperar conocimiento histórico

---

# Estado Actual del Proyecto

## Completado

- Arquitectura Global

- Platform Foundation

- SIEM

- NDR

- Detection Engineering

- Incident Response

- SOAR

- AI Layer

- MSSP Layer

- Governance Layer

---

## Próximo Paso

```text

Definir Topología Docker

↓

Docker Compose

↓

Infraestructura Base

↓

Primer SOC Operativo

```

---

# Preguntas Recomendadas para Revisar con Gemini

## Arquitectura

1. ¿La arquitectura propuesta es adecuada para un futuro MSSP?

2. ¿Qué componentes deberían replantearse antes de construir la primera versión?

---

## Plataforma

3. ¿Docker Compose es suficiente para los primeros clientes?

4. ¿Tiene sentido introducir K3s o Kubernetes desde fases tempranas?

---

## SIEM

5. ¿Wazuh + OpenSearch sigue siendo la mejor combinación Open Source?

6. ¿Existen alternativas viables que reduzcan complejidad operativa?

---

## NDR

7. ¿Suricata + Zeek sigue siendo la mejor arquitectura NDR Open Source?

8. ¿Existen herramientas emergentes que aporten ventajas significativas?

---

## Incident Response

9. ¿TheHive + Cortex continúa siendo la mejor opción Open Source?

10. ¿Existen limitaciones conocidas para entornos MSSP?

---

## SOAR

11. ¿Shuffle es actualmente la mejor alternativa Open Source?

12. ¿Existen opciones más maduras para crecimiento empresarial?

---

## IA

13. ¿Ollama es suficiente para la fase inicial?

14. ¿Qué modelo local recomendaría Gemini actualmente?

15. ¿Cuándo tendría sentido migrar a Vertex AI o Bedrock?

---

## Multi-Tenant

16. ¿La segregación lógica mediante tenant_id es suficiente inicialmente?

17. ¿Cuándo debería considerarse segregación física?

---

## Negocio

18. ¿Cuál debería ser el primer servicio comercializable?

19. ¿Monitoring, Threat Hunting o Incident Response?

20. ¿Cuál sería la secuencia óptima para adquirir los primeros clientes?

---

# Conclusión

La propuesta arquitectónica busca equilibrar:

- Bajo coste inicial

- Independencia tecnológica

- Escalabilidad futura

- Construcción de propiedad intelectual

- Preparación para MSSP

La recomendación actual es:

```text

SOC Local

↓

Validación Operativa

↓

Clientes Piloto

↓

MSSP

↓

AWS o GCP basándose en datos reales

```

en lugar de adoptar un proveedor cloud antes de validar la operación, el modelo de negocio y el valor real aportado por la plataforma.