# Examen de Práctica 2 — AWS CLF-C02

## Resumen de Resultados

| Métrica | Valor |
|---|---|
| **Preguntas totales** | 35 (Q30–Q65, falta Q55) |
| **Correctas** ✅ | 14 |
| **Parciales** ⚠️ | 6 |
| **Incorrectas** ❌ | 15 |
| **Sin respuesta** ❓ | 1 (Q55) |
| **Puntuación estimada** | ~40–46% |

---

## Temas Prioritarios para Repasar

1. **Facturación de EC2** — Tiempo mínimo por instancia (Linux vs Windows)
2. **S3 Storage Classes** — Cuándo usar Standard-IA vs Glacier vs Deep Archive
3. **Reserved Instances** — Qué servicios admiten reservas; qué plan es más económico
4. **VPC Endpoints** — Gateway (S3 + DynamoDB) vs Interface (resto)
5. **Acceso programático** — Access Keys vs MFA vs consola
6. **KMS y CMK** — CMK para control total de claves; Secrets Manager para secretos
7. **EFS vs EBS vs S3** — Cuándo montar en múltiples instancias simultáneamente
8. **RDS Multi-AZ** — Failover automático; Aurora parches gestionados por AWS
9. **AWS Organizations** — Compartir RI entre cuentas; facturación consolidada
10. **Herramientas de costos** — Pricing Calculator vs Cost Explorer vs Budgets vs Compute Optimizer
11. **AWS Abuse Team** — Equipo específico para uso prohibido
12. **AWS CAF** — Perspectivas y stakeholders por perspectiva
13. **Beneficios de la nube** — Los 6 beneficios (CapEx→OpEx, escala, velocidad, elasticidad, agilidad, global)

---

## Preguntas

---

### Pregunta 30 ❌ — Facturación Mínima de EC2 Linux

Un becario aprovisionó una instancia EC2 bajo demanda Linux con facturación por segundos, pero la canceló a los 30 segundos. ¿Cuál es la duración por la que se factura?

| Opción | Correcta | Tu respuesta |
|---|---|---|
| 300 segundos | ❌ | |
| **600 segundos** | ❌ | ← Tu respuesta |
| 30 segundos | ❌ | |
| **60 segundos** | ✅ | |

**Explicación:**

Las instancias EC2 basadas en Linux tienen **facturación por segundo** con una **duración mínima de 1 minuto (60 segundos)**. Aunque la instancia fue cancelada a los 30 segundos, se cobra el mínimo de 60 segundos.

| Sistema Operativo | Unidad mínima de facturación |
|---|---|
| **Linux** | 60 segundos (1 minuto) |
| **Windows** | 3.600 segundos (1 hora) |
| **RHEL / SLES** | 60 segundos (1 minuto) |

> **Regla de oro:** EC2 Linux = mínimo 60 segundos. Si la instancia se cancela antes de 1 minuto, igual se cobra 1 minuto completo. Instancias Windows = mínimo 1 hora.

`Dominio: Facturación y Precios`

---

### Pregunta 31 ✅ — Optimización de Recursos con ML

Una empresa quiere identificar la configuración óptima de recursos AWS para sus cargas de trabajo. ¿Cuál es el servicio correcto?

| Opción | Correcta | Tu respuesta |
|---|---|---|
| AWS Budgets | ❌ | |
| **AWS Compute Optimizer** | ✅ | ← Tu respuesta ✅ |
| AWS Cost Explorer | ❌ | |
| AWS Systems Manager | ❌ | |

**Explicación:**

**AWS Compute Optimizer** usa Machine Learning para analizar las métricas de uso de recursos y recomendar el tipo y tamaño óptimo de instancia EC2, funciones Lambda, volúmenes EBS, y grupos de Auto Scaling.

| Servicio | Para qué sirve |
|---|---|
| **Compute Optimizer** | Recomendar recursos óptimos con ML (reduce sobreaprovisionamiento) |
| **Cost Explorer** | Analizar y visualizar costes históricos y tendencias |
| **Budgets** | Crear alertas cuando el gasto supera un umbral |
| **Systems Manager** | Gestionar instancias EC2 (parches, inventario, run commands) |

> **Regla de oro:** ¿Quiero saber qué tipo de instancia debería usar para ahorrar? → **Compute Optimizer**. ¿Quiero ver cuánto gasté el mes pasado? → **Cost Explorer**. ¿Quiero que me avisen si me paso del presupuesto? → **Budgets**.

`Dominio: Tecnología`

---

### Pregunta 32 ✅ — Protección contra DDoS

¿Qué servicio de AWS protege automáticamente contra ataques DDoS?

| Opción | Correcta | Tu respuesta |
|---|---|---|
| **AWS Shield** | ✅ | ← Tu respuesta ✅ |

**Explicación:**

**AWS Shield** ofrece protección contra ataques DDoS:

| Nivel | Coste | Protección | A quién aplica |
|---|---|---|---|
| **Shield Standard** | Gratis | Ataques DDoS comunes (L3/L4) | Todos los clientes automáticamente |
| **Shield Advanced** | ~$3.000/mes | Ataques sofisticados + WAF + soporte 24/7 | Clientes con alto riesgo |

> **Regla de oro:** AWS Shield Standard = gratis y automático para todos. Shield Advanced = protección premium adicional de pago. Para preguntas sobre DDoS → **AWS Shield**.

`Dominio: Seguridad y Conformidad`

---

### Pregunta 33 ✅ — Trusted Advisor con Plan Basic/Developer

¿A qué comprobaciones de Trusted Advisor tienen acceso los planes Basic y Developer?

| Opción | Correcta | Tu respuesta |
|---|---|---|
| **Solo comprobaciones básicas de seguridad y límites de servicio** | ✅ | ← Tu respuesta ✅ |
| Todas las comprobaciones de Trusted Advisor | ❌ | |

**Explicación:**

| Plan de Soporte | Acceso a Trusted Advisor |
|---|---|
| **Basic** | 7 comprobaciones básicas (seguridad y límites de servicio) |
| **Developer** | 7 comprobaciones básicas (seguridad y límites de servicio) |
| **Business** | Todas las comprobaciones (5 categorías completas) |
| **Enterprise On-Ramp** | Todas las comprobaciones |
| **Enterprise** | Todas las comprobaciones |

> **Regla de oro:** Para acceder a **todas** las comprobaciones de Trusted Advisor (optimización de costes, rendimiento, tolerancia a errores, etc.) se necesita **mínimo el plan Business**.

`Dominio: Facturación y Soporte`

---

### Pregunta 34 ❌ — Compartir Reserved Instances entre Cuentas

¿Qué servicio permite que una empresa comparta Reserved Instances entre múltiples cuentas AWS?

| Opción | Correcta | Tu respuesta |
|---|---|---|
| **AWS Systems Manager** | ❌ | ← Tu respuesta |
| **AWS Organizations** | ✅ | |

**Explicación:**

**AWS Organizations** con facturación consolidada permite que los descuentos de Reserved Instances (RI) y Savings Plans se compartan automáticamente entre todas las cuentas de la organización. Si una cuenta tiene RI sin usar, otra cuenta puede beneficiarse del descuento.

| Servicio | Para qué sirve |
|---|---|
| **AWS Organizations** | Agrupar cuentas, facturación consolidada, compartir RI/Savings Plans |
| **AWS Systems Manager** | Gestionar operaciones de infraestructura (parches, inventario, automatización) |

> **Regla de oro:** Compartir RI entre cuentas → **AWS Organizations** (facturación consolidada). Systems Manager es para gestión operacional de recursos, no para facturación.

`Dominio: Facturación y Precios`

---

### Pregunta 35 ❌ — Eliminar Cuenta de AWS Organizations

¿Qué requisito debe cumplir una cuenta antes de poder eliminarse de AWS Organizations?

| Opción | Correcta | Tu respuesta |
|---|---|---|
| Enviar un ticket de soporte a AWS | ❌ | ← Tu respuesta |
| **La cuenta debe poder funcionar de forma independiente (tener método de pago propio e información de contacto)** | ✅ | |

**Explicación:**

Para eliminar una cuenta de AWS Organizations, la cuenta debe ser capaz de operar de manera independiente, lo que requiere:
- **Método de pago propio** configurado en la cuenta
- **Información de contacto** válida
- **Que no sea la cuenta de administración** (management account)

No se requiere un ticket de soporte para este proceso; se hace directamente desde la consola de AWS Organizations.

> **Regla de oro:** Eliminar cuenta de Organizations ≠ ticket de soporte. La cuenta debe poder sobrevivir sola (pago propio + info de contacto). Sin estos requisitos, AWS no permite la separación.

`Dominio: Facturación y Gestión de Cuentas`

---

### Pregunta 36 ✅ — Transcripción y Síntesis de Voz

¿Qué par de servicios convierte voz a texto y texto a voz respectivamente?

| Opción | Correcta | Tu respuesta |
|---|---|---|
| **Amazon Transcribe + Amazon Polly** | ✅ | ← Tu respuesta ✅ |

**Explicación:**

| Servicio | Función | Dirección |
|---|---|---|
| **Amazon Transcribe** | Reconocimiento de voz automático | Audio/Voz → Texto |
| **Amazon Polly** | Síntesis de voz (TTS) | Texto → Audio/Voz |
| **Amazon Comprehend** | Procesamiento de lenguaje natural (NLP) | Texto → Análisis |
| **Amazon Rekognition** | Análisis de imágenes y vídeo | Imagen/Vídeo → Análisis |
| **Amazon Lex** | Chatbots con voz y texto | Conversación |

> **Regla de oro:** Transcribe = voz→texto (como un secretario que transcribe). Polly = texto→voz (como un narrador que lee en voz alta).

`Dominio: Tecnología`

---

### Pregunta 37 ⚠️ — AWS CAF Perspectiva Platform

¿Qué stakeholders están asociados con la perspectiva **Platform** del AWS Cloud Adoption Framework (CAF)?

| Opción | Correcta | Tu respuesta |
|---|---|---|
| **CTO (Chief Technology Officer)** | ✅ | ← Tu respuesta ✅ |
| CIO (Chief Information Officer) | ❌ | ← Tu respuesta ❌ |
| **Arquitectos / Ingenieros de soluciones** | ✅ | (no seleccionado) |

**Explicación:**

El AWS CAF tiene **6 perspectivas** con sus stakeholders correspondientes:

| Perspectiva | Stakeholders principales |
|---|---|
| **Business** | CEO, CFO, COO, CIO, CMO |
| **People** | VP RRHH, Director RRHH, CIO |
| **Governance** | CIO, Program Managers, Enterprise Architects |
| **Platform** | **CTO, Arquitectos de soluciones, Ingenieros** |
| **Security** | CISO, Ingenieros de seguridad |
| **Operations** | VP Operaciones, Site Reliability Engineers |

> **Regla de oro:** Perspectiva Platform → **CTO + Arquitectos/Ingenieros** (los que diseñan y construyen la infraestructura técnica). El CIO está más en Business/Governance/People.

`Dominio: Conceptos de la Nube`

---

### Pregunta 38 ✅ — Controles Físicos en Responsabilidad Compartida

En el Modelo de Responsabilidad Compartida, ¿quién es responsable de los controles físicos y medioambientales?

| Opción | Correcta | Tu respuesta |
|---|---|---|
| **AWS** | ✅ | ← Tu respuesta ✅ |
| El cliente | ❌ | |

**Explicación:**

La seguridad **DE** la nube es responsabilidad de AWS, que incluye:
- Instalaciones físicas (centros de datos)
- Seguridad física (guardias, cámaras, acceso biométrico)
- Controles medioambientales (temperatura, energía, extinción de incendios)
- Hardware físico (servidores, switches, routers)
- Hipervisor / capa de virtualización

| Responsabilidad | AWS | Cliente |
|---|---|---|
| Seguridad física | ✅ | ❌ |
| Hardware | ✅ | ❌ |
| Sistema Operativo (EC2) | ❌ | ✅ |
| Datos del cliente | ❌ | ✅ |
| Configuración de red (VPC) | ❌ | ✅ |

> **Regla de oro:** Todo lo que es físico (edificios, servidores, cables) → AWS. Todo lo que es lógico (SO, datos, red virtual, acceso) → Cliente. La división es: seguridad **DE** la nube (AWS) vs. seguridad **EN** la nube (Cliente).

`Dominio: Seguridad y Conformidad`

---

### Pregunta 39 ⚠️ — Infraestructura Global de AWS

¿Cuáles de las siguientes afirmaciones sobre la infraestructura global de AWS son correctas? (Selecciona 2)

| Opción | Correcta | Tu respuesta |
|---|---|---|
| Mínimo 2 Zonas de Disponibilidad por región | ❌ | ← Tu respuesta ❌ |
| **Mínimo 3 Zonas de Disponibilidad por región** | ✅ | (no seleccionado) |
| **Cada AZ consta de uno o más centros de datos** | ✅ | ← Tu respuesta ✅ |

**Explicación:**

| Concepto | Descripción |
|---|---|
| **Región** | Área geográfica con múltiples AZ (mínimo 3, normalmente 3-6) |
| **Zona de Disponibilidad (AZ)** | Uno o más centros de datos físicamente separados dentro de una región |
| **Local Zone** | Extensión de región más cercana a usuarios finales (baja latencia) |
| **Edge Location / PoP** | Puntos de presencia para CloudFront y Route 53 (400+) |

> **Regla de oro:** Región = mínimo **3 AZ** (no 2). Cada AZ = **1 o más** centros de datos aislados con energía, refrigeración y red independientes.

`Dominio: Conceptos de la Nube / Tecnología`

---

### Pregunta 40 ⚠️ — Orden de Aplicación de Créditos AWS

¿Cómo aplica AWS los créditos a la factura mensual?

| Opción | Correcta | Tu respuesta |
|---|---|---|
| **Primero al servicio con créditos que expiran antes** | ✅ | ← Tu respuesta (parcial) |
| El crédito 2 se aplica a S3 | ❌ | ← Tu respuesta ❌ |
| **Primero los créditos que aplican a menos productos** | ✅ | (no seleccionado) |

**Explicación:**

AWS aplica los créditos en el siguiente orden de prioridad:

| Prioridad | Criterio | Descripción |
|---|---|---|
| 1° | **Soonest-expiring** | El crédito que caduca primero se aplica antes |
| 2° | **Fewest products** | Si hay empate, el crédito que aplica a menos productos va primero |
| 3° | **Oldest** | Si hay empate en ambos, el crédito más antiguo se aplica primero |

> **Regla de oro:** Los créditos AWS siguen la regla del "más urgente primero": el que caduca antes → el que aplica menos → el más viejo. AWS aplica automáticamente los créditos para minimizar la factura dentro de estas reglas.

`Dominio: Facturación y Precios`

---

### Pregunta 41 ✅ — AWS Lambda: Serverless

¿Cuál es la característica principal de AWS Lambda?

| Opción | Correcta | Tu respuesta |
|---|---|---|
| **Serverless (sin servidor)** | ✅ | ← Tu respuesta ✅ |

**Explicación:**

**AWS Lambda** es el servicio de computación serverless de AWS:

| Característica | Descripción |
|---|---|
| **Sin servidores** | No hay que aprovisionar ni gestionar instancias |
| **Orientado a eventos** | Se ejecuta en respuesta a eventos (S3, API Gateway, DynamoDB, SNS, etc.) |
| **Pago por uso** | Solo se paga por el tiempo de ejecución (milisegundos) |
| **Escalado automático** | Escala automáticamente según la demanda |
| **Máximo 15 minutos** | Tiempo máximo de ejecución por invocación |

> **Regla de oro:** "Sin servidor" + "en respuesta a eventos" + "código sin gestionar infraestructura" → **AWS Lambda** (PaaS / Serverless).

`Dominio: Tecnología`

---

### Pregunta 42 ❌ — Clase de Almacenamiento S3 para Acceso Infrecuente

Una empresa necesita almacenar datos que se acceden raramente, pero cuando se necesitan, la recuperación debe ser inmediata. ¿Qué clase de S3 es más adecuada?

| Opción | Correcta | Tu respuesta |
|---|---|---|
| S3 Standard | ❌ | |
| **S3 Glacier** | ❌ | ← Tu respuesta |
| **S3 Standard-IA** | ✅ | |
| S3 Intelligent-Tiering | ❌ | |

**Explicación:**

| Clase S3 | Acceso | Recuperación | Caso de uso |
|---|---|---|---|
| **S3 Standard** | Frecuente | Inmediata (ms) | Datos activos |
| **S3 Standard-IA** | Infrecuente | **Inmediata (ms)** | Backups, DR, datos de referencia |
| **S3 One Zone-IA** | Infrecuente | Inmediata | Datos secundarios (solo 1 AZ) |
| **S3 Intelligent-Tiering** | Variable | Inmediata | Patrones de acceso impredecibles |
| **S3 Glacier Instant** | Archivado | Inmediata | Archivos con acceso ocasional |
| **S3 Glacier Flexible** | Archivado | 1–12 horas | Archivado a largo plazo |
| **S3 Glacier Deep Archive** | Archivado | 12–48 horas | Archivado máximo a 7-10 años |

> **Regla de oro:** Acceso infrecuente + recuperación INMEDIATA → **S3 Standard-IA**. Si la recuperación puede tardar horas → S3 Glacier. Si la recuperación puede tardar días → Deep Archive.

`Dominio: Tecnología`

---

### Pregunta 43 ❌ — Costo de Transferencia EC2 → S3 Misma Región

¿Cuánto cuesta transferir datos de una instancia EC2 a un bucket S3 dentro de la misma región?

| Opción | Correcta | Tu respuesta |
|---|---|---|
| Se cobra la transferencia de entrada | ❌ | |
| Se cobra la transferencia de salida | ❌ | |
| **Se cobra transferencia de entrada y salida** | ❌ | ← Tu respuesta |
| **La transferencia es gratuita** | ✅ | |

**Explicación:**

| Escenario de transferencia | Costo |
|---|---|
| **EC2 → S3 (misma región)** | **GRATIS** |
| **S3 → EC2 (misma región)** | **GRATIS** |
| EC2 → S3 (diferente región) | Se cobra |
| S3 → Internet | Se cobra (transferencia de salida) |
| Internet → S3 | GRATIS (entrada a AWS) |
| EC2 (AZ-A) → EC2 (AZ-B) | Se cobra |
| EC2 → EC2 (misma AZ, IP privada) | GRATIS |

> **Regla de oro:** Transferencia de datos DENTRO de la misma región entre EC2 y S3 = **GRATIS**. La regla general: dentro de la misma región entre servicios AWS suele ser gratuita o muy barata; la salida a internet siempre se cobra.

`Dominio: Facturación y Precios`

---

### Pregunta 44 ❌ — Reserved Instance Más Rentable

¿Cuál de las siguientes opciones de Reserved Instance ofrece el mayor descuento?

| Opción | Correcta | Tu respuesta |
|---|---|---|
| 1 año sin pago inicial | ❌ | |
| 1 año con pago completo adelantado | ❌ | |
| **3 años sin pago inicial** | ❌ | ← Tu respuesta |
| **3 años con pago parcial adelantado** | ✅ | |

**Explicación:**

El orden de mayor a menor descuento de las Reserved Instances:

| Opción | Ahorro aprox. |
|---|---|
| **3 años + All Upfront (pago completo)** | Máximo (~66%) |
| **3 años + Partial Upfront (parcial)** | Alto (~60%) |
| 3 años + No Upfront (sin pago inicial) | Moderado-alto |
| 1 año + All Upfront | Moderado |
| 1 año + Partial Upfront | Menor |
| 1 año + No Upfront | Mínimo (~40%) |

> **Regla de oro:** Más años = más descuento. Más pago adelantado = más descuento. El máximo es **3 años + All Upfront**. Si entre opciones similares, el "pago completo adelantado" siempre supera al "parcial" que supera al "sin pago".

`Dominio: Facturación y Precios`

---

### Pregunta 45 ✅ — AWS SDK

¿Qué herramienta proporciona APIs específicas por lenguaje de programación para interactuar con servicios AWS?

| Opción | Correcta | Tu respuesta |
|---|---|---|
| **AWS SDK** | ✅ | ← Tu respuesta ✅ |
| AWS CLI | ❌ | |
| AWS Management Console | ❌ | |

**Explicación:**

| Herramienta | Acceso | Para quién |
|---|---|---|
| **AWS SDK** | APIs por lenguaje (Python/Boto3, Java, JS, .NET, etc.) | Desarrolladores |
| **AWS CLI** | Comandos de terminal | Administradores / DevOps |
| **AWS Console** | Interfaz web gráfica | Todos |
| **AWS CloudShell** | Terminal en el navegador con CLI preinstalada | Administradores |

> **Regla de oro:** "API específica de lenguaje" + "integrar AWS en código" → **SDK**. "Comandos de terminal" → **CLI**. "Interfaz gráfica web" → **Console**.

`Dominio: Tecnología`

---

### Pregunta 46 ✅ — Ventajas de Amazon RDS Gestionado

¿Cuál es una ventaja de usar Amazon RDS en lugar de gestionar una base de datos en EC2?

| Opción | Correcta | Tu respuesta |
|---|---|---|
| **AWS gestiona automáticamente el hardware, SO y parches** | ✅ | ← Tu respuesta ✅ |

**Explicación:**

| Tarea | EC2 (BD auto-gestionada) | Amazon RDS (gestionado) |
|---|---|---|
| Parches del SO | Cliente | **AWS** |
| Backups automáticos | Cliente | **AWS** |
| Failover Multi-AZ | Cliente (complejo) | **AWS** (automático) |
| Réplicas de lectura | Cliente | **AWS** (fácil) |
| Monitoreo | Cliente | **AWS** (CloudWatch integrado) |
| Escalado de almacenamiento | Manual | **AWS** (Auto Scaling) |

> **Regla de oro:** RDS = PaaS → AWS gestiona SO, parches, backups, failover. El cliente solo gestiona los datos y la configuración de la BD. EC2+BD propia = IaaS → el cliente gestiona todo desde el SO.

`Dominio: Tecnología`

---

### Pregunta 47 ✅ — Desacoplar Microservicios

¿Qué par de servicios AWS permite desacoplar microservicios para mejorar la resiliencia?

| Opción | Correcta | Tu respuesta |
|---|---|---|
| **Amazon SQS + Amazon SNS** | ✅ | ← Tu respuesta ✅ |

**Explicación:**

| Servicio | Tipo | Caso de uso |
|---|---|---|
| **Amazon SQS** | Cola de mensajes (pull) | Desacoplar productores y consumidores; buffer de mensajes |
| **Amazon SNS** | Notificaciones push (pub/sub) | Enviar mensajes a múltiples suscriptores simultáneamente |
| **Amazon EventBridge** | Bus de eventos | Integrar aplicaciones basadas en eventos |
| **Amazon Kinesis** | Streaming de datos | Procesar grandes volúmenes de datos en tiempo real |

> **Regla de oro:** Desacoplar microservicios → **SQS** (cola) + **SNS** (notificaciones). SQS garantiza que los mensajes se procesen aunque el receptor esté ocupado. SNS distribuye el mismo mensaje a múltiples destinos.

`Dominio: Tecnología`

---

### Pregunta 48 ❌ — Control Total de Claves de Cifrado

Una empresa quiere tener control total sobre las claves de cifrado utilizadas para proteger sus datos en AWS. ¿Qué servicio debe usar?

| Opción | Correcta | Tu respuesta |
|---|---|---|
| AWS Secrets Manager | ❌ | ← Tu respuesta |
| **AWS KMS con CMK (Customer Managed Key)** | ✅ | |
| AWS Certificate Manager | ❌ | |

**Explicación:**

| Servicio | Para qué sirve |
|---|---|
| **AWS KMS** | Gestión de claves de cifrado |
| **CMK (Customer Managed Key)** | Claves creadas/controladas totalmente por el cliente en KMS |
| **AWS Managed Key** | Claves gestionadas por AWS en KMS (menos control) |
| **AWS Secrets Manager** | Gestionar secretos (contraseñas, API keys, connection strings) |
| **ACM (Certificate Manager)** | Certificados TLS/SSL para dominios |

| Tipo de clave en KMS | Control del cliente | Rotación |
|---|---|---|
| **CMK (Customer Managed)** | Total (crear, rotar, eliminar, políticas) | Manual o anual automática |
| AWS Managed Key | Parcial (no se puede eliminar) | Automática cada 3 años |
| AWS Owned Key | Ninguno | AWS gestiona |

> **Regla de oro:** "Control total de mis claves de cifrado" → **KMS con CMK**. "Guardar contraseñas de bases de datos o API keys" → **Secrets Manager** (son secretos, no claves criptográficas).

`Dominio: Seguridad y Conformidad`

---

### Pregunta 49 ✅ — Cifrado del Lado del Cliente

Una empresa quiere cifrar los datos **antes** de enviarlos a Amazon S3 (cifrado del lado del cliente). ¿Qué herramienta debe usar?

| Opción | Correcta | Tu respuesta |
|---|---|---|
| **AWS Encryption SDK** | ✅ | ← Tu respuesta ✅ |
| AWS KMS (cifrado lado servidor) | ❌ | |

**Explicación:**

| Tipo de cifrado | Dónde ocurre | Herramienta |
|---|---|---|
| **Lado del cliente (client-side)** | En la aplicación ANTES de enviar a AWS | **AWS Encryption SDK** |
| **Lado del servidor SSE-S3** | En S3, AWS gestiona las claves | S3 nativo |
| **Lado del servidor SSE-KMS** | En S3, claves en KMS | S3 + KMS |
| **Lado del servidor SSE-C** | En S3, el cliente provee la clave | S3 + clave del cliente |

> **Regla de oro:** "Cifrar ANTES de subir a S3" = cifrado del lado del cliente → **AWS Encryption SDK**. "S3 cifra automáticamente cuando guarda" = cifrado del lado del servidor.

`Dominio: Seguridad y Conformidad`

---

### Pregunta 50 ❌ — Almacenamiento Compartido para Múltiples EC2

Una empresa necesita almacenamiento compartido al que puedan acceder simultáneamente cientos de instancias EC2. ¿Cuál es el servicio correcto?

| Opción | Correcta | Tu respuesta |
|---|---|---|
| **Amazon S3** | ❌ | ← Tu respuesta |
| **Amazon EFS** | ✅ | |
| Amazon EBS | ❌ | |

**Explicación:**

| Servicio | Tipo | Acceso múltiple | Caso de uso |
|---|---|---|---|
| **Amazon EFS** | Sistema de archivos NFS | ✅ Cientos de instancias simultáneamente | Contenido compartido, CMS, Big Data |
| **Amazon EBS** | Disco en bloque | ❌ Solo 1 instancia (excepto io1/io2 Multi-Attach limitado) | Volumen de arranque, BD en EC2 |
| **Amazon S3** | Almacenamiento de objetos | ✅ (acceso HTTP/HTTPS, no montado como disco) | Backups, activos web, data lakes |

> **Regla de oro:** Montar como disco en múltiples EC2 simultáneamente → **EFS**. S3 es de objetos (no se monta como sistema de archivos nativo). EBS es de bloque para una instancia.

`Dominio: Tecnología`

---

### Pregunta 51 ✅ — Parches del SO en Amazon Aurora

¿Quién es responsable de aplicar parches al sistema operativo subyacente de Amazon Aurora?

| Opción | Correcta | Tu respuesta |
|---|---|---|
| **AWS** | ✅ | ← Tu respuesta ✅ |
| El cliente | ❌ | |

**Explicación:**

Amazon Aurora (y Amazon RDS) son servicios **gestionados (PaaS)**. AWS es responsable de:
- Parches del sistema operativo subyacente
- Actualizaciones del motor de base de datos (con ventana de mantenimiento)
- Backups automáticos
- Failover automático (Multi-AZ)
- Replicación

El cliente es responsable de:
- Configuración de la base de datos
- Gestión de usuarios y permisos de la BD
- Diseño del esquema
- Optimización de consultas

> **Regla de oro:** RDS y Aurora = PaaS → AWS gestiona el SO y los parches. Si la pregunta dice "base de datos gestionada" → los parches del SO son de **AWS**.

`Dominio: Seguridad y Conformidad`

---

### Pregunta 52 ❌ — Mayor Latencia en Recuperación de Datos

¿Qué clase de almacenamiento S3 tiene la mayor latencia en la recuperación de datos?

| Opción | Correcta | Tu respuesta |
|---|---|---|
| S3 Standard-IA | ❌ | |
| **S3 Glacier Flexible** | ❌ | ← Tu respuesta |
| S3 Glacier Instant Retrieval | ❌ | |
| **S3 Glacier Deep Archive** | ✅ | |

**Explicación:**

Orden de menor a mayor latencia de recuperación:

| Clase S3 | Recuperación | Latencia |
|---|---|---|
| S3 Standard / Standard-IA / One Zone-IA | Milisegundos | Más rápida |
| S3 Intelligent-Tiering | Milisegundos a horas | Variable |
| S3 Glacier Instant Retrieval | Milisegundos | Rápida |
| S3 Glacier Flexible (Expedited) | 1–5 minutos | Media |
| S3 Glacier Flexible (Standard) | 3–5 horas | Lenta |
| S3 Glacier Flexible (Bulk) | 5–12 horas | Lenta |
| **S3 Glacier Deep Archive** | **12–48 horas** | **Más lenta (Mayor latencia)** |

> **Regla de oro:** Mayor latencia = **Glacier Deep Archive** (12-48 horas). Es la clase más barata de S3 pero con el tiempo de recuperación más largo. Ideal para datos que se archivan por 7-10 años y rara vez se recuperan.

`Dominio: Tecnología`

---

### Pregunta 53 ❌ — Descuento Máximo de Instancias Spot

¿Cuál es el descuento máximo aproximado de las instancias EC2 Spot respecto a las instancias On-Demand?

| Opción | Correcta | Tu respuesta |
|---|---|---|
| 25% | ❌ | |
| **50%** | ❌ | ← Tu respuesta |
| 75% | ❌ | |
| **90%** | ✅ | |

**Explicación:**

| Tipo de instancia EC2 | Descuento vs On-Demand | Interrupción |
|---|---|---|
| **On-Demand** | 0% (precio base) | Nunca |
| **Reserved (1 año)** | hasta ~40% | Nunca |
| **Reserved (3 años)** | hasta ~66% | Nunca |
| **Savings Plans** | hasta ~66% | Nunca |
| **Spot Instances** | **hasta ~90%** | Sí (con 2 min de aviso) |

> **Regla de oro:** Las instancias Spot ofrecen hasta un **90% de descuento** sobre el precio On-Demand. La contrapartida: AWS puede interrumpirlas con 2 minutos de aviso cuando necesita la capacidad. Ideales para cargas de trabajo tolerantes a interrupciones (procesamiento por lotes, CI/CD, análisis).

`Dominio: Facturación y Precios`

---

### Pregunta 54 ⚠️ — Servicios Compatibles con VPC Gateway Endpoint

¿Qué servicios AWS son compatibles con VPC Gateway Endpoints? (Selecciona 2)

| Opción | Correcta | Tu respuesta |
|---|---|---|
| **Amazon S3** | ✅ | ← Tu respuesta ✅ |
| Amazon EC2 | ❌ | ← Tu respuesta ❌ |
| **Amazon DynamoDB** | ✅ | (no seleccionado) |
| Amazon RDS | ❌ | |

**Explicación:**

Existen dos tipos de VPC Endpoints:

| Tipo | Servicios compatibles | Costo |
|---|---|---|
| **Gateway Endpoint** | **Solo S3 y DynamoDB** | Gratuito |
| **Interface Endpoint (PrivateLink)** | La mayoría de servicios AWS | ~$0.01/hora por AZ |

Los Gateway Endpoints permiten acceder a S3 y DynamoDB desde dentro de la VPC **sin pasar por internet**, usando la red privada de AWS.

> **Regla de oro:** VPC Gateway Endpoint = **solo S3 y DynamoDB**. Para cualquier otro servicio AWS (EC2, RDS, SNS, SQS, etc.) se usa Interface Endpoint (PrivateLink).

`Dominio: Tecnología / Redes`

---

### Pregunta 55 ❓ — Pregunta No Disponible

*Esta pregunta no aparece en el archivo del examen.*

---

### Pregunta 56 ✅ — Servicios de Almacenamiento AWS

¿Cuáles de los siguientes son servicios de almacenamiento de AWS? (Selecciona 2)

| Opción | Correcta | Tu respuesta |
|---|---|---|
| **Amazon EFS** | ✅ | ← Tu respuesta ✅ |
| **Amazon S3** | ✅ | ← Tu respuesta ✅ |

**Explicación:**

| Servicio | Tipo de almacenamiento |
|---|---|
| **Amazon S3** | Almacenamiento de objetos |
| **Amazon EBS** | Almacenamiento en bloque |
| **Amazon EFS** | Sistema de archivos compartido (NFS) |
| **Amazon FSx** | Sistemas de archivos gestionados (Windows, Lustre, NetApp) |
| **AWS Storage Gateway** | Híbrido (on-premises + nube) |
| Amazon EC2 | Cómputo (no almacenamiento) |
| Amazon RDS | Base de datos (no almacenamiento directo) |

> **Regla de oro:** Los tres pilares del almacenamiento AWS: **S3** (objetos), **EBS** (bloque), **EFS** (archivos). Cualquiera de estos es almacenamiento. EC2, RDS, Lambda = no son servicios de almacenamiento.

`Dominio: Tecnología`

---

### Pregunta 57 ✅ — Plan de Soporte para Interoperabilidad con Software Terceros

¿Qué planes de soporte AWS incluyen asistencia de interoperabilidad con software de terceros?

| Opción | Correcta | Tu respuesta |
|---|---|---|
| Basic | ❌ | |
| Developer | ❌ | |
| **Business** | ✅ | ← Tu respuesta ✅ |
| **Enterprise** | ✅ | ← Tu respuesta ✅ |

**Explicación:**

| Plan de Soporte | Interoperabilidad 3rd party | Tiempo de respuesta crítico |
|---|---|---|
| **Basic** | ❌ | Solo foros |
| **Developer** | ❌ | 12–24 horas |
| **Business** | ✅ | 1 hora (sistema de producción caído) |
| **Enterprise On-Ramp** | ✅ | 30 minutos |
| **Enterprise** | ✅ | 15 minutos |

> **Regla de oro:** Interoperabilidad con software de terceros (sistemas operativos, bases de datos, middleware, etc.) → mínimo **plan Business**. Basic y Developer son para aprendizaje, no para entornos de producción complejos.

`Dominio: Facturación y Soporte`

---

### Pregunta 58 ✅ — Documentos de Conformidad HIPAA

¿Qué servicio AWS permite acceder a documentos de conformidad como los informes HIPAA?

| Opción | Correcta | Tu respuesta |
|---|---|---|
| **AWS Artifact** | ✅ | ← Tu respuesta ✅ |
| AWS Config | ❌ | |
| AWS Compliance Center | ❌ | |

**Explicación:**

| Servicio | Para qué sirve |
|---|---|
| **AWS Artifact** | Repositorio de informes de conformidad (SOC, PCI-DSS, HIPAA, ISO, etc.) y acuerdos |
| **AWS Config** | Registrar y evaluar la configuración de recursos AWS |
| **AWS Security Hub** | Vista centralizada de alertas de seguridad y conformidad |
| **AWS Audit Manager** | Automatizar la evaluación de controles de conformidad |

> **Regla de oro:** "Descargar informe de conformidad" (HIPAA, SOC 1/2/3, PCI-DSS, ISO 27001) → **AWS Artifact**. Es el portal de documentos de auditoría y acuerdos legales de AWS.

`Dominio: Seguridad y Conformidad`

---

### Pregunta 59 ❌ — Servicio de Almacén de Datos (Data Warehouse)

¿Qué servicio AWS es un almacén de datos (data warehouse) gestionado para análisis a gran escala?

| Opción | Correcta | Tu respuesta |
|---|---|---|
| **AWS DMS** | ❌ | ← Tu respuesta |
| **Amazon Redshift** | ✅ | |
| Amazon RDS | ❌ | |
| Amazon Aurora | ❌ | |

**Explicación:**

| Servicio | Tipo | Caso de uso |
|---|---|---|
| **Amazon Redshift** | Data Warehouse (columnar) | Análisis de petabytes de datos, BI, reporting |
| **Amazon RDS** | Base de datos relacional (OLTP) | Aplicaciones transaccionales |
| **Amazon Aurora** | BD relacional (OLTP) | Alta disponibilidad y rendimiento |
| **AWS DMS** | Servicio de migración | Migrar bases de datos a AWS |
| **Amazon Athena** | Query engine serverless | Consultar datos en S3 con SQL |
| **Amazon EMR** | Big Data | Hadoop, Spark, análisis masivo |

> **Regla de oro:** "Data warehouse" + "análisis de grandes volúmenes" + "consultas complejas" → **Amazon Redshift**. DMS = migrar bases de datos (no analizarlas).

`Dominio: Tecnología`

---

### Pregunta 60 ❌ — Reportar Uso Prohibido de AWS

Un cliente detecta que otra empresa está usando servicios AWS de forma maliciosa (spam, malware). ¿A quién debe reportarlo?

| Opción | Correcta | Tu respuesta |
|---|---|---|
| **AWS Support** | ❌ | ← Tu respuesta |
| **AWS Abuse Team** | ✅ | |

**Explicación:**

| Equipo | Para qué contactar |
|---|---|
| **AWS Support** | Problemas técnicos con los propios recursos del cliente |
| **AWS Abuse Team** | Reportar uso prohibido de AWS por terceros (spam, malware, escaneo de puertos, ataques) |
| **AWS Security** | Vulnerabilidades de seguridad en servicios propios de AWS |

El AWS Abuse Team se puede contactar en: `abuse@amazonaws.com`

Usos prohibidos que deben reportarse: spam, ataques DDoS originados desde AWS, distribución de malware, contenido ilegal, phishing.

> **Regla de oro:** "Otro usuario AWS hace algo malicioso/ilegal" → **AWS Abuse Team** (abuse@amazonaws.com). "Tengo un problema técnico con mis propios recursos" → **AWS Support**.

`Dominio: Soporte y Seguridad`

---

### Pregunta 61 ❌ — Alta Disponibilidad con Failover Automático de Base de Datos

¿Qué servicio proporciona alta disponibilidad para una base de datos con failover automático?

| Opción | Correcta | Tu respuesta |
|---|---|---|
| **AWS CloudFormation** | ❌ | ← Tu respuesta |
| **Amazon RDS Multi-AZ** | ✅ | |
| Amazon DynamoDB | ❌ | |

**Explicación:**

**Amazon RDS Multi-AZ** replica la base de datos de forma síncrona en una segunda Zona de Disponibilidad. En caso de fallo:
- El **failover es automático** (generalmente en menos de 2 minutos)
- El **endpoint de conexión no cambia** (la aplicación se reconecta automáticamente)
- **No hay pérdida de datos** (réplica síncrona)

| Servicio | Relación con alta disponibilidad |
|---|---|
| **RDS Multi-AZ** | Failover automático de BD entre AZ |
| CloudFormation | Infraestructura como código (IaC); no gestiona failover |
| DynamoDB | Alta disponibilidad nativa, sin configuración |
| Route 53 | DNS con enrutamiento de failover para aplicaciones |

> **Regla de oro:** "Failover automático de base de datos relacional" → **RDS Multi-AZ**. CloudFormation es para aprovisionar infraestructura, no para gestionar failover de bases de datos.

`Dominio: Tecnología`

---

### Pregunta 62 ❌ — Acceso Programático a AWS

¿Qué credenciales se usan para el acceso programático a AWS (SDK, CLI, API)?

| Opción | Correcta | Tu respuesta |
|---|---|---|
| Usuario y contraseña de la consola | ❌ | |
| **MFA (Multi-Factor Authentication)** | ❌ | ← Tu respuesta |
| **Access Key ID + Secret Access Key** | ✅ | |

**Explicación:**

| Tipo de acceso | Credenciales |
|---|---|
| **Consola Web (Management Console)** | Usuario + Contraseña (+ MFA opcional) |
| **Programático (SDK, CLI, API)** | **Access Key ID + Secret Access Key** |
| **Roles IAM (recomendado para EC2/Lambda)** | Credenciales temporales (STS) |

MFA es un factor de autenticación adicional para la consola web. No es la credencial de acceso programático (aunque MFA puede requerirse para asumir ciertos roles).

> **Regla de oro:** Acceso programático (SDK/CLI/API) → **Access Key ID + Secret Access Key**. Las Access Keys se crean en IAM y deben tratarse como contraseñas (nunca subirlas a repositorios públicos).

`Dominio: Seguridad y Conformidad`

---

### Pregunta 63 ⚠️ — Servicios que Admiten Instancias Reservadas

¿Cuáles de los siguientes servicios AWS admiten el modelo de compra de Reserved Instances/capacidad reservada? (Selecciona 3)

| Opción | Correcta | Tu respuesta |
|---|---|---|
| **Amazon EC2** | ✅ | ← Tu respuesta ✅ |
| **Amazon S3** | ❌ | ← Tu respuesta ❌ |
| **Amazon RDS** | ✅ | ← Tu respuesta ✅ |
| **Amazon DynamoDB** | ✅ | (no seleccionado) |

**Explicación:**

| Servicio | ¿Admite reservas? | Tipo de reserva |
|---|---|---|
| **Amazon EC2** | ✅ | Reserved Instances (1 o 3 años) |
| **Amazon RDS** | ✅ | Reserved DB Instances |
| **Amazon DynamoDB** | ✅ | Reserved Capacity |
| **Amazon ElastiCache** | ✅ | Reserved Cache Nodes |
| **Amazon Redshift** | ✅ | Reserved Nodes |
| **Amazon OpenSearch** | ✅ | Reserved Instances |
| **Amazon S3** | ❌ | No tiene reservas (precio por uso) |
| **AWS Lambda** | ❌ | No tiene reservas |
| **Amazon DocumentDB** | ❌ | No tiene reservas |

> **Regla de oro:** **S3 y Lambda NO tienen reservas**. Los principales servicios con reservas: EC2, RDS, DynamoDB, ElastiCache, Redshift. S3 solo tiene precio por uso (GB almacenado + solicitudes).

`Dominio: Facturación y Precios`

---

### Pregunta 64 ⚠️ — Beneficios de la Nube AWS

¿Cuáles son beneficios de migrar a AWS Cloud? (Selecciona 2)

| Opción | Correcta | Tu respuesta |
|---|---|---|
| **Cambiar gastos de capital (CapEx) a gastos operativos (OpEx)** | ✅ | ← Tu respuesta ✅ |
| Aumentar la velocidad de despliegue manteniendo los servidores propios | ❌ | ← Tu respuesta ❌ |
| **Eliminar las conjeturas sobre la capacidad de infraestructura** | ✅ | (no seleccionado) |
| Aumentar el CapEx para mayor control | ❌ | |

**Explicación:**

Los **6 beneficios de la nube AWS** (según AWS):

| Beneficio | Descripción |
|---|---|
| **CapEx → OpEx** | De inversión en capital a gasto operativo (pago por uso) |
| **Economías de escala** | AWS compra a mayor escala → precios más bajos para todos |
| **Sin conjeturas de capacidad** | Escalar según demanda real, no predicciones |
| **Mayor velocidad y agilidad** | Provisionar recursos en minutos, no semanas |
| **Enfoque en el negocio** | Eliminar mantenimiento de datacenters |
| **Alcance global** | Desplegar en múltiples regiones en minutos |

> **Regla de oro:** Los 6 beneficios clave de AWS: (1) CapEx→OpEx, (2) economías de escala, (3) sin conjeturas de capacidad, (4) mayor velocidad/agilidad, (5) enfoque en el negocio, (6) alcance global. "Mantener servidores propios" contradice la propuesta de valor de la nube.

`Dominio: Conceptos de la Nube`

---

### Pregunta 65 ❌ — Estimar el Costo Mensual de AWS

Una empresa quiere estimar el costo mensual de usar un conjunto de servicios AWS antes de desplegarlos. ¿Qué herramienta debe usar?

| Opción | Correcta | Tu respuesta |
|---|---|---|
| **AWS Budgets** | ❌ | ← Tu respuesta |
| **AWS Pricing Calculator** | ✅ | |
| AWS Cost Explorer | ❌ | |
| AWS Compute Optimizer | ❌ | |

**Explicación:**

| Herramienta | Para qué sirve | Cuándo usar |
|---|---|---|
| **AWS Pricing Calculator** | Estimar costos FUTUROS de servicios planificados | Antes de desplegar (planificación) |
| **AWS Cost Explorer** | Analizar y visualizar costos históricos | Después de desplegar (análisis) |
| **AWS Budgets** | Crear alertas cuando el gasto supera un umbral | Monitoreo continuo |
| **AWS Compute Optimizer** | Recomendar recursos óptimos con ML | Optimización de recursos existentes |
| **AWS Trusted Advisor** | Revisar configuración con mejores prácticas | Auditoría general |

> **Regla de oro:** "Estimar costos antes de desplegar" → **AWS Pricing Calculator**. "Ver cuánto gasté" → **Cost Explorer**. "Alertarme si me paso del presupuesto" → **Budgets**.

`Dominio: Facturación y Precios`

---

## Guía Rápida de Repaso — Temas Clave del Examen 2

### Herramientas de Costos AWS

| Herramienta | Función principal | Cuándo usar |
|---|---|---|
| **Pricing Calculator** | Estimar costos futuros | Pre-despliegue |
| **Cost Explorer** | Analizar histórico de costos | Post-despliegue |
| **Budgets** | Alertas de presupuesto | Monitoreo continuo |
| **Compute Optimizer** | Recomendar recursos óptimos (ML) | Optimización |
| **Trusted Advisor** | Mejores prácticas (costos, seguridad, rendimiento, límites) | Auditoría |

---

### Clases de Almacenamiento S3 (de mayor a menor acceso)

| Clase | Acceso | Recuperación | Caso de uso |
|---|---|---|---|
| S3 Standard | Frecuente | ms | Datos activos |
| S3 Intelligent-Tiering | Variable | ms | Patrones impredecibles |
| S3 Standard-IA | Infrecuente | ms | Backups, DR |
| S3 One Zone-IA | Infrecuente | ms | Datos secundarios |
| S3 Glacier Instant | Archivado | ms | Archivos con acceso ocasional |
| S3 Glacier Flexible | Archivado | 1-12 h | Archivado largo plazo |
| **S3 Glacier Deep Archive** | **Raro** | **12-48 h** | **Máximo archivado (7-10 años)** |

---

### VPC Endpoints: Tipos

| Tipo | Servicios | Costo |
|---|---|---|
| **Gateway Endpoint** | **Solo S3 y DynamoDB** | Gratuito |
| **Interface Endpoint (PrivateLink)** | La mayoría de servicios | ~$0.01/hora/AZ |

---

### Servicios AWS con Reservas (vs sin reservas)

| ✅ Con reservas | ❌ Sin reservas |
|---|---|
| EC2, RDS, DynamoDB, ElastiCache, Redshift, OpenSearch | S3, Lambda, DocumentDB |

---

### AWS CAF — 6 Perspectivas y Stakeholders

| Perspectiva | Stakeholders |
|---|---|
| **Business** | CEO, CFO, COO, CIO, CMO |
| **People** | VP RRHH, Director RRHH |
| **Governance** | CIO, Enterprise Architects, Program Managers |
| **Platform** | **CTO, Arquitectos de soluciones, Ingenieros** |
| **Security** | CISO, Ingenieros de seguridad |
| **Operations** | VP Operaciones, SRE, IT Ops |

---

### Infraestructura Global AWS

| Concepto | Detalle |
|---|---|
| **Región** | Mínimo **3 AZ** por región |
| **AZ** | **1 o más** centros de datos físicamente separados |
| **Edge Location** | 400+ puntos de presencia (CloudFront, Route 53) |
| **Local Zone** | Extensión de región para baja latencia local |

---

### Tipos de Acceso a AWS

| Tipo de acceso | Credencial |
|---|---|
| **Consola web** | Usuario + contraseña (+ MFA) |
| **Programático (SDK/CLI/API)** | Access Key ID + Secret Access Key |
| **Roles IAM (recomendado)** | Credenciales temporales (STS AssumeRole) |

---

### Descuentos EC2 por Tipo de Instancia

| Tipo | Descuento vs On-Demand | Interrupción |
|---|---|---|
| On-Demand | 0% | Nunca |
| Reserved (1 año) | ~40% | Nunca |
| Reserved (3 años) | ~66% | Nunca |
| Savings Plans | ~66% | Nunca |
| **Spot** | **hasta 90%** | Sí (2 min aviso) |

---

### EFS vs EBS vs S3

| Servicio | Tipo | Acceso múltiple | Montable en EC2 |
|---|---|---|---|
| **EFS** | Sistema de archivos NFS | ✅ Múltiples instancias | ✅ Sí |
| **EBS** | Bloque | ❌ Solo 1 (excepto io1/io2) | ✅ Sí |
| **S3** | Objetos | ✅ (HTTP) | ❌ No directamente |

---

### Contactos AWS Clave

| Situación | Contacto |
|---|---|
| Problema técnico propio | **AWS Support** |
| Uso prohibido por terceros (spam, malware) | **AWS Abuse Team** (abuse@amazonaws.com) |
| Vulnerabilidad en servicio AWS | **AWS Security** |
| Facturación / contratos | **AWS Billing** |
