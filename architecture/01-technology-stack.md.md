# Technology Stack

> Inventario oficial de tecnologías aprobadas para la plataforma SOC.

>

> Este documento actúa como referencia central para todas las herramientas, plataformas, frameworks y dependencias tecnológicas utilizadas por el proyecto.

>

> El objetivo es mantener una visión consolidada de la arquitectura tecnológica actual, facilitar futuras revisiones arquitectónicas y documentar la justificación de las decisiones adoptadas.

---

# 1. Objetivo

## Propósito

Definir el stack tecnológico oficial de la plataforma SOC.

Este documento responde a las siguientes preguntas:

- ¿Qué herramientas forman parte de la plataforma?

- ¿Qué función cumple cada una?

- ¿Cuál es su estado dentro del proyecto?

- ¿Cuáles son las dependencias críticas?

- ¿Qué alternativas fueron consideradas?

---

## Alcance

Incluye:

- Infraestructura

- SIEM

- Network Detection

- Detection Engineering

- Incident Response

- SOAR

- Artificial Intelligence

- Gestión documental

- Automatización

- Seguridad de plataforma

No incluye:

- Infraestructura específica de AWS

- Infraestructura específica de GCP

- Herramientas experimentales

- Tecnologías descartadas

---

# 2. Principios de Selección Tecnológica

Toda tecnología seleccionada deberá cumplir la mayor cantidad posible de los siguientes criterios:

## Open Source Preferente

La prioridad será utilizar herramientas Open Source maduras.

---

## Cloud Agnostic

La tecnología debe poder ejecutarse:

- On-Premise

- AWS

- GCP

---

## Container Friendly

Debe ser compatible con:

```text

Docker

Docker Compose

```

---

## Comunidad Activa

Debe existir:

- Comunidad técnica activa

- Desarrollo continuado

- Documentación adecuada

---

## Escalabilidad

Debe permitir evolución futura hacia:

```text

SOC

↓

MSSP

↓

Plataforma SaaS

```

---

## Automatización

Debe exponer:

- API

- CLI

- Integraciones

---

# 3. Sistemas Operativos

## Plataforma Base

### Ubuntu Server LTS

Estado:

```text

Aprobado

```

### Funciones

- Host principal

- Sensores NDR

- Servicios auxiliares

### Motivos de Selección

- Amplia adopción empresarial

- Excelente soporte Docker

- Comunidad madura

- Ciclos LTS predecibles

### Riesgos

- Requiere conocimientos Linux

- Dependencia de mantenimiento propio

---

# 4. Virtualización y Contenedores

## Docker

Estado:

```text

Aprobado

```

### Funciones

- Empaquetado de servicios

- Despliegue reproducible

- Aislamiento

### Uso Previsto

Todos los componentes del SOC.

---

## Docker Compose

Estado:

```text

Aprobado

```

### Funciones

- Orquestación básica

- Gestión de redes

- Gestión de volúmenes

### Observación

Kubernetes queda fuera del alcance inicial.

---

## K3s

Estado:

```text

Evaluación Futura

```

---

## Kubernetes

Estado:

```text

Evaluación Futura

```

---

# 5. SIEM

## Wazuh

Estado:

```text

Aprobado

```

### Funciones

- Log Collection

- File Integrity Monitoring (FIM)

- Vulnerability Detection

- Compliance Monitoring

- Alerting

### Casos de Uso

- Windows Monitoring

- Linux Monitoring

- Security Events

- Compliance

### Ventajas

- Open Source

- Amplia adopción

- Integración OpenSearch

### Riesgos

- Curva de aprendizaje

- Requiere tuning continuo

---

## OpenSearch

Estado:

```text

Aprobado

```

### Funciones

- Almacenamiento de logs

- Búsquedas

- Indexación

- Dashboards

- Hunting

### Ventajas

- Escalable

- Open Source

- API potente

### Riesgos

- Consumo elevado de RAM

- Crecimiento continuo de almacenamiento

---

## OpenSearch Dashboards

Estado:

```text

Aprobado

```

### Funciones

- Visualización

- Exploración de eventos

- Dashboards

---

# 6. Network Detection

## Suricata

Estado:

```text

Aprobado

```

### Funciones

- IDS

- IPS (futuro)

- IOC Matching

- Network Detection

### Integraciones

- OpenSearch

- Wazuh

### Ventajas

- Alto rendimiento

- Comunidad madura

---

## Zeek

Estado:

```text

Aprobado

```

### Funciones

- Metadata Collection

- DNS Analysis

- TLS Analysis

- Threat Hunting

### Ventajas

- Excelente visibilidad

- Contexto de investigación

### Relación con Suricata

```text

Suricata = Detección

Zeek = Contexto

```

---

# 7. Incident Response

## TheHive

Estado:

```text

Aprobado

```

### Funciones

- Gestión de casos

- Gestión de incidentes

- Evidencias

### Ventajas

- Estándar Open Source de referencia para IR

---

## Cortex

Estado:

```text

Aprobado

```

### Funciones

- Enrichment

- IOC Analysis

- Integración de inteligencia

### Integraciones Previstas

- VirusTotal

- AlienVault OTX

- AbuseIPDB

- URLHaus

- Shodan

---

# 8. SOAR

## Shuffle

Estado:

```text

Aprobado

```

### Funciones

- Automatización

- Orquestación

- Integraciones

### Casos de Uso

- IOC Enrichment

- Alert to Case

- Reporting

- Notifications

### Ventajas

- Open Source

- Enfoque SOC

- Integración con TheHive

---

# 9. Artificial Intelligence

## Ollama

Estado:

```text

Aprobado

```

### Funciones

- Runtime local de modelos

- Inferencia privada

### Motivos

- Sin costes cloud

- Los datos permanecen localmente

---

## Qwen 2.5

Estado:

```text

Aprobado

```

### Uso Previsto

- Triage

- Hunting

- Clasificación

### Motivos

- Excelente rendimiento analítico

---

## Llama 3

Estado:

```text

Aprobado

```

### Uso Previsto

- Reporting

- Explicaciones

- Documentación

---

## LangGraph

Estado:

```text

Aprobado

```

### Funciones

- Orquestación multiagente

- Workflows de IA

### Casos de Uso

- Triage Agent

- Detection Agent

- Reporting Agent

- Threat Hunter Agent

---

# 10. Threat Intelligence

## AlienVault OTX

Estado:

```text

Aprobado

```

---

## VirusTotal

Estado:

```text

Aprobado

```

---

## AbuseIPDB

Estado:

```text

Aprobado

```

---

## URLHaus

Estado:

```text

Aprobado

```

---

## Shodan

Estado:

```text

Aprobado

```

---

# 11. PKI y Certificados

## Step-CA

Estado:

```text

Aprobado

```

### Funciones

- PKI interna

- Emisión de certificados

- Rotación

### Ventajas

- Ligero

- Open Source

---

# 12. Reverse Proxy

## Traefik

Estado:

```text

Aprobado

```

### Funciones

- Reverse Proxy

- TLS

- Routing

### Motivos

- Integración nativa con Docker

- Menor complejidad operativa

---

# 13. Gestión de Secretos

## Docker Secrets

Estado:

```text

Aprobado

```

### Uso

Fase inicial

---

## HashiCorp Vault

Estado:

```text

Evaluación Futura

```

### Uso

MSSP y producción avanzada

---

# 14. Gestión Documental

## Obsidian

Estado:

```text

Aprobado

```

### Funciones

- Blueprint

- Arquitectura

- Runbooks

- Investigación

### Ventajas

- Markdown nativo

- Mermaid

- Git Friendly

---

# 15. Control de Versiones

## Git

Estado:

```text

Aprobado

```

### Alcance

- Arquitectura

- Detecciones

- Automatizaciones

- Configuraciones

- Scripts

---

# 16. Detection Engineering

## Sigma

Estado:

```text

Aprobado

```

### Funciones

- Detections as Code

- Correlación portable

---

## MITRE ATT&CK

Estado:

```text

Aprobado

```

### Funciones

- Threat Modeling

- Cobertura

- Reporting

---

## YARA

Estado:

```text

Evaluación Futura

```

### Uso Previsto

- Malware Analysis

- IOC Detection

---

# 17. Herramientas Futuras

## MISP

Threat Intelligence Platform

Estado:

```text

Evaluación

```

---

## Velociraptor

DFIR y Enterprise Hunting

Estado:

```text

Evaluación

```

---

## FleetDM

Fleet Management

Estado:

```text

Evaluación

```

---

## osquery

Endpoint Telemetry

Estado:

```text

Evaluación

```

---

## Grafana

Observabilidad

Estado:

```text

Evaluación

```

---

## Prometheus

Métricas

Estado:

```text

Evaluación

```

---

# 18. Dependencias y Versiones

> Versiones objetivo para la implementación inicial del SOC Local First.

## Principios de Versionado

### Estabilidad Primero

Se priorizarán:

- Versiones LTS

- Releases estables

- Versiones ampliamente adoptadas

---

### Flujo de Actualización

```text

Laboratorio

↓

Staging

↓

Producción

```

---

## Matriz de Componentes

| Categoría | Componente | Versión Objetivo | Estado |

|------------|------------|------------------|---------|

| Sistema Operativo | Ubuntu Server | 24.04 LTS | ✅ |

| Docker | Docker Engine | Última estable soportada | ✅ |

| Docker Compose | Compose Plugin | Última estable soportada | ✅ |

| Reverse Proxy | Traefik | 3.x | ✅ |

| PKI | Step-CA | Última estable | ✅ |

| SIEM | Wazuh | 4.x | ✅ |

| Data Platform | OpenSearch | 2.x | ✅ |

| Dashboards | OpenSearch Dashboards | 2.x | ✅ |

| NDR | Suricata | 7.x | ✅ |

| NDR | Zeek | Última estable | ✅ |

| IR | TheHive | 5.x | ✅ |

| IR | Cortex | 3.x | ✅ |

| SOAR | Shuffle | Última estable | ✅ |

| AI Runtime | Ollama | Última estable | ✅ |

| AI Workflow | LangGraph | Última estable | ✅ |

| Secret Management | Vault | Última estable | Evaluación |

| Detecciones | Sigma | Última estable | ✅ |

| Threat Modeling | ATT&CK | Última Enterprise Matrix | ✅ |

| Malware Rules | YARA | Última estable | Evaluación |

| IaC | Terraform | 1.x | Evaluación |

---

# 19. Matriz de Dependencias

| Componente | Dependencias |

|------------|-------------|

| Wazuh Dashboard | Wazuh, OpenSearch |

| OpenSearch Dashboards | OpenSearch |

| TheHive | OpenSearch, Cortex |

| Cortex | APIs externas |

| Shuffle | TheHive, OpenSearch, Cortex |

| Ollama | Docker |

| LangGraph | Ollama |

| Triage Agent | LangGraph, Ollama, OpenSearch |

| Threat Hunter Agent | LangGraph, Ollama, OpenSearch |

| Detection Agent | LangGraph, Ollama, Sigma |

| Reporting Agent | LangGraph, Ollama, TheHive |

| Knowledge Agent | LangGraph, Obsidian |

| Traefik | Docker, Step-CA |

| Step-CA | Docker |

---

## Dependencias Críticas

### OpenSearch

Impacto:

```text

SIEM

Hunting

IR

SOAR

AI

```

---

### Docker

Impacto:

```text

Toda la plataforma

```

---

### TheHive

Impacto:

```text

Case Management

Incident Response

```

---

# 20. Compatibilidad Cloud

| Componente | Local | AWS | GCP |

|------------|--------|--------|--------|

| Docker | ✅ | ✅ | ✅ |

| Wazuh | ✅ | ✅ | ✅ |

| OpenSearch | ✅ | ✅ | ✅ |

| Suricata | ✅ | ✅ | ✅ |

| Zeek | ✅ | ✅ | ✅ |

| TheHive | ✅ | ✅ | ✅ |

| Cortex | ✅ | ✅ | ✅ |

| Shuffle | ✅ | ✅ | ✅ |

| Ollama | ✅ | ✅ | ✅ |

| LangGraph | ✅ | ✅ | ✅ |

| Step-CA | ✅ | ✅ | ✅ |

| Traefik | ✅ | ✅ | ✅ |

---

## Servicios Específicos Cloud

### AWS

```text

CloudTrail

GuardDuty

Security Hub

Bedrock

```

---

### GCP

```text

Cloud Audit Logs

Security Command Center

Vertex AI

```

---

# 21. Política de Gestión de Versiones

## Actualizaciones

### Mensual

- Docker

- Wazuh

- OpenSearch

---

### Trimestral

- Revisión del stack completo

- Dependencias IA

---

### Semestral

- Reevaluación tecnológica

- Incorporación de nuevas herramientas

---

## Compatibilidades Obligatorias

```text

Wazuh ↔ OpenSearch

OpenSearch ↔ Dashboards

TheHive ↔ Cortex

Shuffle ↔ Integraciones

Ollama ↔ Modelos

```

---

# 22. Resumen del Stack MVP

```text

SOC Platform

├── Ubuntu Server

├── Docker

├── Docker Compose

├── Traefik

├── Step-CA

├── Wazuh

├── OpenSearch

├── Suricata

├── Zeek

├── TheHive

├── Cortex

├── Shuffle

├── Ollama

├── Qwen

├── Llama

├── LangGraph

├── Obsidian

└── Git

```

---

# 23. Estado de Aprobación

| Categoría | Estado |

|------------|---------|

| Sistema Operativo | ✅ Aprobado |

| Plataforma Docker | ✅ Aprobado |

| SIEM | ✅ Aprobado |

| NDR | ✅ Aprobado |

| Incident Response | ✅ Aprobado |

| SOAR | ✅ Aprobado |

| AI Layer | ✅ Aprobado |

| PKI | ✅ Aprobado |

| Threat Intelligence | ✅ Aprobado |

| Documentación | ✅ Aprobado |

| MSSP Ready | ✅ Compatible |

| AWS Ready | ✅ Compatible |

| GCP Ready | ✅ Compatible |

---

# Última Revisión

```text

Estado Arquitectónico

Aprobado para implementación del SOC Local First.

```