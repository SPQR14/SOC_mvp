# Análisis Comparativo Cloud: AWS vs. GCP y Cumplimiento LFPDPPP

**Proyecto:** SPQR-Intell (*Autonomous & AI-Native Security Operations Platform*)  
**Alcance:** Evaluación de infraestructura en la nube para el despliegue del stack SOC + Agente Windows (*Speculator*), análisis de costos operativos e implicaciones jurídicas bajo la **Ley Federal de Protección de Datos Personales en Posesión de los Particulares (LFPDPPP)** en México.  
**Enfoque:** Imparcial, agnóstico a proveedores (*Cloud-Agnostic*) y alineado a modelo MSSP Multi-Tenant.

---

## 1. Resumen Ejecutivo

Dado que **SPQR-Intell** está diseñado bajo una filosofía *Container-First* (Docker/Kubernetes) y 100% *Open Source* (OpenSearch, ClickHouse, Wazuh, TheHive, Shuffle, Ollama), la plataforma **no depende de servicios propietarios** de ningún proveedor de nube. La elección entre **Amazon Web Services (AWS)** y **Google Cloud Platform (GCP)** se reduce a tres factores críticos:

1. **Economía Unitaria (Unit Economics) para el modelo MSSP:** Costo de cómputo intensivo en RAM (OpenSearch/ClickHouse), GPUs para inferencia local de LLMs (Ollama) y tráfico de red (NDR/Ingesta).
2. **Soberanía y Residencia de Datos en México:** Disponibilidad de regiones locales en Querétaro para evitar transferencias internacionales innecesarias.
3. **Operabilidad y Escala:** Facilidad para automatizar el aprovisionamiento de nuevos *tenants* mediante Terraform.

---

## 2. Comparativa Técnica y de Costos Operativos (Sin Sesgos)

### 2.1. Cómputo y Memoria (OpenSearch, ClickHouse y Contenedores)
El núcleo de almacenamiento y búsqueda de logs (**Praetorium**) demanda altas cantidades de memoria RAM y almacenamiento NVMe de alto rendimiento.

| Criterio | Amazon Web Services (AWS) | Google Cloud Platform (GCP) | Ventaja Operativa |
| :--- | :--- | :--- | :--- |
| **Dimensionamiento de VMs** | Instancias con tamaños fijos (ej. `r6i.2xlarge`, `r7g.2xlarge`). Si necesitas 48 GB de RAM, debes pagar la instancia de 64 GB. | **Custom Machine Types** (N2/E2/T2D): permite asignar exactamente los vCPU y GB de RAM requeridos (ej. 10 vCPU y 48 GB RAM). | **GCP** (Evita desperdicio de presupuesto en etapas iniciales). |
| **Descuentos por Compromiso** | *Compute Savings Plans* (hasta 40–50% de ahorro a 1 o 3 años), flexibles entre familias de instancias. | *Committed Use Discounts (CUDs)* flexibles por recurso (vCPU/RAM) sin atarse a un tipo de máquina específico. | **Empate** (Ambos ofrecen esquemas sólidos de ahorro). |
| **Procesadores ARM (Costo/Rendimiento)** | **AWS Graviton3/4** ofrece hasta 20–30% mejor relación precio/rendimiento para contenedores Linux, OpenSearch y ClickHouse. | **GCP Tau T2A / Axion** ofrece buen rendimiento ARM, pero con menor disponibilidad regional que Graviton. | **AWS** (Madurez del ecosistema ARM). |

### 2.2. Almacenamiento y Retención de Logs (Online 90d / Warm 180d / Archivo 1 Año)
* **Almacenamiento Caliente (Hot/Online - 90 días):**
  * **AWS EBS gp3:** Permite desacoplar IOPS y *throughput* del tamaño del disco (~$0.08 USD/GB-mes base), ideal para picos de indexación en OpenSearch.
  * **GCP Hyperdisk / Persistent Disk SSD:** Rendimiento muy estable con redimensionamiento en caliente sin degradación, aunque ligeramente más costoso por GB (~$0.10–$0.17 USD/GB-mes según tier).
* **Almacenamiento Frío (Archivo Forense - 1 año):**
  * **AWS S3 Glacier Deep Archive:** El costo por almacenamiento más bajo del mercado (~$0.00099 USD/GB-mes). *Desventaja:* Tiempos de recuperación de 12 a 48 horas y costos altos por solicitudes de API si un auditor exige logs de emergencia.
  * **GCP Cloud Storage Archive:** Costo ligeramente superior (~$0.0012 USD/GB-mes), pero ofrece **acceso en milisegundos** sin tiempos de descongelamiento, agilizando investigaciones forenses retrospectivas.

### 2.3. Red, Ingesta de Agentes (*Speculator*) y NDR (*Vigiles*)
* **Costos de Egress (Salida de Datos):** Ambos proveedores penalizan la salida de datos hacia Internet (~$0.085–$0.09 USD/GB después del primer tier). Sin embargo, **AWS cobra tráfico entre Zonas de Disponibilidad (AZ)** ($0.01 USD/GB en cada sentido), lo cual encarece la replicación de clústeres de OpenSearch y Redpanda multi-AZ. GCP también cobra tráfico inter-zona, pero su red troncal privada global suele ofrecer menor latencia para ingesta de agentes distribuidos.
* **Captura de Tráfico de Red en la Nube (Cloud NDR):**
  * **AWS VPC Traffic Mirroring:** Cobra una tarifa fija por hora por cada interfaz de red (ENI) monitoreada (~$0.015 USD/hora por ENI). Si un cliente tiene 200 máquinas virtuales, el costo fijo escala rápidamente incluso si tienen poco tráfico.
  * **GCP Packet Mirroring:** Cobra principalmente por volumen de datos procesados (~$0.01–$0.02 USD/GB) más el balanceador interno, resultando más económico en entornos con muchos servidores de bajo tráfico.

### 2.4. Inferencia de IA (*Consilium* - Ollama + Qwen 2.5 / Llama 3)
* **AWS:** Mayor disponibilidad de instancias con GPU bajo demanda (ej. `g5.xlarge` con NVIDIA A10G o `g6.xlarge` con L4) en múltiples regiones. Ideal si se encienden/apagan instancias por lotes.
* **GCP:** Las instancias `g2-standard` (con **NVIDIA L4**) tienen una excelente relación costo/rendimiento para modelos de 7B a 32B parámetros. *Riesgo operativo:* Las cuotas de GPU en GCP suelen estar en cero por defecto y requieren aprobación manual de soporte, lo que puede retrasar despliegues urgentes.

---

## 3. Cumplimiento de la LFPDPPP (Marco Legal Mexicano)

Al operar **SPQR-Intell** como plataforma SOC/MSSP para empresas en México, la relación jurídica bajo la **Ley Federal de Protección de Datos Personales en Posesión de los Particulares (LFPDPPP)** se estructura de la siguiente manera:

* **Cliente Empresarial = Responsable** (Art. 3 Fracc. XIV): Decide sobre el tratamiento de los datos de sus empleados, clientes y sistemas.
* **SPQR-Intell (Su empresa) = Encargado** (Art. 3 Fracc. IX): Trata datos personales por cuenta del Responsable (direcciones IP, nombres de usuario, correos corporativos, rutas de archivos con nombres propios, logs de acceso).
* **Proveedor Cloud (AWS / GCP) = Sub-encargado / Tercero de Infraestructura** (Art. 14).

### Matriz de Cumplimiento Normativo: AWS vs. GCP

| Requisito Legal (LFPDPPP) | Implicación para SPQR-Intell | Implementación en AWS | Implementación en GCP |
| :--- | :--- | :--- | :--- |
| **Soberanía y Transferencia de Datos**<br>*(Arts. 36 y 37)* | Si los logs salen de México hacia EE. UU., se configura una **transferencia internacional**, obligando al cliente a declararlo en su Aviso de Privacidad y firmar cláusulas contractuales específicas. | **Región México (Central) en Querétaro (`mx-central-1`)**. Permite mantener el 100% de los logs dentro de territorio mexicano. | **Región Querétaro (`northamerica-south1`)**. También permite residencia de datos local en México. |
| **Uso de Servicios Gestionados de IA**<br>*(Arts. 12 y 36)* | Si usan APIs externas (Bedrock/Vertex AI) en lugar de **Ollama** local, muchos modelos solo corren en regiones de EE. UU. (`us-east-1` / `us-central1`), sacando telemetría del país. | **Ollama en EC2 dentro de `mx-central-1`** garantiza que ningún dato personal salga de México hacia terceros. | **Ollama en Compute Engine dentro de `northamerica-south1`** cumple el mismo aislamiento soberano. |
| **Medidas de Seguridad Técnicas**<br>*(Art. 19)* | Exige medidas administrativas, técnicas y físicas para evitar pérdida, alteración o acceso no autorizado, acordes al riesgo y desarrollo tecnológico. | Cumplimiento **ISO 27001, SOC 2 Type II**. Uso de **AWS KMS (CMEK)** para que el cliente o SPQR-Intell controlen las llaves de cifrado. | Cumplimiento **ISO 27001, SOC 2 Type II**. Uso de **Cloud KMS (CMEK)** con cifrado en reposo por defecto en todos los discos y buckets. |
| **Deber de Confidencialidad y Disociación**<br>*(Arts. 3 Fracc. VIII y 21)* | Obligación de guardar secreto profesional incluso tras terminar el contrato y anonimizar datos cuando sea posible. | Aislamiento por VPC/Subredes, IAM granular y tokenización/enmascaramiento en el pipeline **Vector** antes de indexar. | Aislamiento por Proyectos/VPC Service Controls, IAM granular y enmascaramiento en **Vector**. |
| **Derechos ARCO, Bloqueo y Cancelación**<br>*(Arts. 3 Fracc. III, 11 y 25)* | Al terminar la relación con un cliente o ante solicitud de cancelación de un titular, los datos deben pasar a **bloqueo** (conservación sin uso activo por plazo legal) y luego suprimirse. | Políticas **ISM en OpenSearch** + **S3 Object Lock / Lifecycle Policies** para bloqueo inmutable y purga automatizada certificable. | Políticas **ISM en OpenSearch** + **Cloud Storage Bucket Lock / Retention Policies** para bloqueo y borrado automático. |
| **Notificación de Vulneraciones**<br>*(Art. 20)* | Si la infraestructura del SOC sufre una brecha que afecte datos personales, debe notificarse de inmediato al Responsable. | **AWS CloudTrail + GuardDuty** (monitoreo interno del plano de control) para auditoría forense inmediata. | **Cloud Audit Logs + Security Command Center** para trazabilidad completa de accesos administrativos. |

---

## 4. Estimación Comparativa de Costo Mensual (Escenario Piloto MSSP)

*Escenario base: 5 clientes piloto (~500 endpoints Windows con agente Speculator), ingesta promedio de 50 GB/día, retención de 90 días en caliente (4.5 TB) + 1 año en archivo (18 TB), y nodo de IA con GPU.*

| Componente de Infraestructura | Especificación Técnica | Costo Estimado AWS (`mx-central-1` / `us-east-1`*) | Costo Estimado GCP (`northamerica-south1`) |
| :--- | :--- | :--- | :--- |
| **Clúster SIEM & Analítica** *(Praetorium)* | 2 nodos (16 vCPU, 64 GB RAM c/u) + 5 TB NVMe SSD (OpenSearch + ClickHouse) | ~$1,180 USD / mes | ~$1,090 USD / mes *(Custom VMs)* |
| **Core SOC & SOAR** *(Wazuh, TheHive, Shuffle, Vector, Redpanda)* | 1 nodo (8 vCPU, 32 GB RAM) + 500 GB SSD | ~$310 USD / mes | ~$285 USD / mes |
| **Nodo de IA** *(Consilium - Ollama)* | 1 nodo con GPU (NVIDIA L4 / A10G, 24 GB VRAM) - Uso bajo demanda (12h/día) | ~$390 USD / mes | ~$340 USD / mes |
| **Almacenamiento Frío (1 Año)** | 18 TB en Object Storage (S3 Glacier Deep Archive vs. GCS Archive) | ~$25 USD / mes *(+ costos de recuperación)* | ~$30 USD / mes *(acceso inmediato)* |
| **Red y Transferencia (Egress)** | 1.5 TB/mes de tráfico saliente (dashboards, reportes, APIs) | ~$135 USD / mes | ~$130 USD / mes |
| **TOTAL MENSUAL ESTIMADO** | *(Con compromisos a 1 año / CUDs o Savings Plans aplicados)* | **~$2,040 USD / mes** | **~$1,875 USD / mes** |

*\*Nota: Algunos servicios avanzados o instancias GPU específicas en la región recién inaugurada de Querétaro (`mx-central-1`) pueden tener disponibilidad limitada frente a `us-east-1` (Virginia), lo cual debe validarse antes del aprovisionamiento.*

---

## 5. Veredicto y Recomendación Estratégica

1. **Recomendación Financiera y Operativa (Corto/Mediano Plazo): GCP**
   * **Por qué:** Los *Custom Machine Types* permiten ajustar la RAM de OpenSearch y ClickHouse al milímetro sin pagar por capacidad ociosa, sus costos de red inter-zona y almacenamiento de archivo con lectura instantánea son más predecibles, y ofrece región nativa en Querétaro (`northamerica-south1`).
2. **Recomendación por Ecosistema y Disponibilidad de Hardware (Escala Enterprise): AWS**
   * **Por qué:** Si prevén escalar agresivamente el uso de instancias con GPU bajo demanda o aprovechar procesadores ARM **Graviton** para reducir el costo por GB indexado en ClickHouse, AWS ofrece mayor profundidad de catálogo y mayor facilidad para contratar ingenieros *DevSecOps* certificados en México.
3. **Estrategia Óptima para SPQR-Intell (Zero Lock-In):**
   * Mantener toda la definición de infraestructura en **Terraform** y los despliegues en **Docker Compose / K3s**. Esto les permite iniciar en la nube que les otorgue **créditos para startups** (tanto *AWS Activate* como *Google for Startups Cloud Program* otorgan entre $2,000 y $100,000 USD en créditos iniciales) y migrar entre nubes en menos de 48 horas si las condiciones comerciales o regulatorias de un cliente lo exigen.