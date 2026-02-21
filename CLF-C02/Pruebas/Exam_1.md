# Examen de Práctica CLF-C02 — Revisión Completa

---

## Puntuación General

| Resultado | Preguntas | Total |
|---|---|---|
| ✅ Correcta | 1, 7, 9, 10, 12, 15, 17, 18, 21, 24, 25 | **11** |
| ⚠️ Parcial | 16 (1/2), 22 (2/3) | **2** |
| ❌ Incorrecta | 2, 3, 4, 5, 6, 8, 13, 14, 19, 20, 23, 26, 27, 28, 29 | **15** |
| ❓ Incompleta | 11 (texto de pregunta faltante) | **1** |
| **Total** | | **29** |

> **Puntuación estimada:** ~11–12 / 28 evaluadas (~39–43%). El umbral de aprobación del CLF-C02 es **700/1000 (~70%)**. Hay margen de mejora significativo.

---

## Temáticas Prioritarias para Repasar

- **Servicios de seguridad** (WAF, Shield, Inspector, Macie, GuardDuty) — Q3, Q5, Q16
- **CloudWatch vs CloudTrail vs Config** — Q4, Q22
- **Beneficios del Cloud** (Agilidad vs Elasticidad, 6 ventajas) — Q6, Q11
- **Planes de soporte AWS** — Q13
- **Conectividad híbrida** (Direct Connect vs VPN) — Q14
- **Almacenamiento EC2** (EBS vs EFS vs Instance Store) — Q20, Q26
- **Precios EC2** (Reserved vs Spot vs Dedicated Host vs Dedicated Instance) — Q12, Q23
- **NACLs vs Security Groups** — Q8
- **Route 53 políticas de enrutamiento** — Q28
- **AWS CAF y socios APN** — Q2, Q27
- **Herramientas de diagnóstico** (X-Ray, CloudFormation) — Q29

---

## Pregunta 1 ✅ — Responsabilidad Compartida de AWS

**Según el modelo de responsabilidad compartida de AWS, ¿cuáles de las siguientes son responsabilidades de AWS? (Selecciona dos)**

- ✅ Mantener los datos de Amazon S3 en diferentes AZ para que sean duraderos ← **Tu selección**
- ✅ Sustituir el hardware defectuoso de las instancias Amazon EC2 ← **Tu selección**
- ❌ Crear políticas de bucket de S3 para el acceso adecuado de los usuarios
- ❌ Habilitar la autenticación multifactor (MFA) en las cuentas de tu organización
- ❌ Crear un rol IAM para acceder a instancias Amazon EC2

### Explicación

AWS gestiona la **"Seguridad DEL Cloud"** (infraestructura física). El cliente gestiona la **"Seguridad EN el Cloud"** (configuraciones, accesos, datos).

| Responsabilidad | Quién |
|---|---|
| Sustituir hardware defectuoso de EC2 | **AWS** |
| Mantener durabilidad de datos S3 entre AZ | **AWS** |
| Crear políticas de bucket S3 | **Cliente** |
| Habilitar MFA en cuentas propias | **Cliente** |
| Crear roles IAM | **Cliente** |

> **Regla de oro:** Todo lo físico y de infraestructura subyacente → **AWS**. Todo lo de configuración, acceso y datos → **Cliente**.

**Temática:** Seguridad y normativa

---

## Pregunta 2 ❌ — Socios APN para Migración

**Una empresa internacional desea asesoramiento profesional sobre migración al Cloud de AWS y gestión de sus aplicaciones. ¿Cuál entidad recomendarías?**

- ❌ Socio tecnológico de APN ← **Tu respuesta**
- ✅ Socio consultor de APN
- ❌ Equipo de soporte concierge
- ❌ AWS Trusted Advisor

### Explicación

| Entidad | Rol | ¿Ayuda a migrar? |
|---|---|---|
| **Socio consultor de APN** | Diseñan, construyen, migran y administran en AWS | ✅ Sí |
| **Socio tecnológico de APN** | Hardware, conectividad o software integrado con AWS | ❌ No |
| **Equipo concierge** | Solo facturación y cuentas (plan Enterprise) | ❌ No |
| **Trusted Advisor** | Herramienta automática de recomendaciones | ❌ No |

> **Regla de oro:** "Migrar / administrar aplicaciones en AWS con ayuda profesional" → **Socio consultor de APN**.

**Temática:** Conceptos del Cloud

---

## Pregunta 3 ❌ — AWS WAF y la Capa OSI

**¿En qué capa ofrece AWS WAF protección contra los exploits web más comunes?**

- ❌ Capa 4 y 7
- ❌ Capa 3
- ❌ Capa 4 ← **Tu respuesta**
- ✅ Capa 7

### Explicación

| Capa OSI | Nombre | Servicio AWS |
|---|---|---|
| Capa 3 | Red (Network) | **AWS Shield** |
| Capa 4 | Transporte (TCP/UDP) | **AWS Shield** |
| Capa 7 | Aplicación (HTTP/HTTPS) | **AWS WAF** |

AWS WAF monitoriza solicitudes HTTP/HTTPS enviadas a CloudFront, API Gateway o Application Load Balancer — todos de **capa 7** (Aplicación).

> **Regla de oro:** WAF = **capa 7** (HTTP/HTTPS). Shield = **capas 3 y 4** (red y transporte).

**Temática:** Seguridad y normativa

---

## Pregunta 4 ❌ — Auditoría y Gobernanza de Cuenta

**Una empresa financiera quiere asegurarse de que la actividad de su cuenta AWS cumple normativas de gobernanza y auditoría. ¿Qué servicio recomendarías?**

- ❌ Amazon CloudWatch
- ❌ AWS Config
- ❌ AWS Trusted Advisor ← **Tu respuesta**
- ✅ AWS CloudTrail

### Explicación

| Servicio | Pregunta que responde |
|---|---|
| **CloudTrail** | ¿**Quién** hizo **qué acción** y **cuándo** en la cuenta AWS? → auditoría de actividad |
| **Config** | ¿**Cómo cambió** la configuración de mis recursos? → cumplimiento |
| **CloudWatch** | ¿Cómo está **funcionando** mi sistema? → métricas y alertas |
| **Trusted Advisor** | ¿Estoy siguiendo las mejores prácticas? → recomendaciones automáticas |

> **Regla de oro:**
> - "Actividad / auditoría / quién hizo qué" → **CloudTrail**
> - "Cambios de configuración / cumplimiento de recursos" → **Config**
> - "Rendimiento / métricas / alarmas" → **CloudWatch**

**Temática:** Conceptos del Cloud

---

## Pregunta 5 ❌ — Cifrado Habilitado por Defecto

**¿Cuál de los siguientes servicios AWS tiene el cifrado activado por defecto?**

- ❌ Amazon EBS
- ❌ Amazon RDS
- ❌ Amazon EFS ← **Tu respuesta**
- ✅ Registros de AWS CloudTrail

### Explicación

| Servicio | Cifrado por defecto |
|---|---|
| **CloudTrail logs → S3** | ✅ Sí — SSE-S3 activado automáticamente |
| **Amazon EBS** | ❌ No — debe habilitarse manualmente |
| **Amazon EFS** | ❌ No — opcional (en tránsito y en reposo) |
| **Amazon RDS** | ❌ No — característica adicional a activar |

> **Regla de oro:** Solo los logs de **CloudTrail** tienen cifrado habilitado por defecto (SSE-S3). EBS, EFS y RDS requieren activación manual.

**Temática:** Seguridad y normativa

---

## Pregunta 6 ❌ — Beneficios del Cloud: Agilidad

**Una empresa quiere pasarse al Cloud de AWS y lanzar nuevas características con iteraciones rápidas. ¿Qué característica del Cloud quiere aprovechar?**

- ❌ Escalabilidad
- ❌ Fiabilidad
- ❌ Elasticidad ← **Tu respuesta**
- ✅ Agilidad

### Explicación

| Característica | Definición clave |
|---|---|
| **Agilidad** | Desarrollar, probar y lanzar rápidamente — experimentar en minutos sin esperar semanas |
| **Elasticidad** | Adquirir recursos cuando se necesitan y liberarlos cuando no (ajuste automático a la demanda) |
| **Escalabilidad** | Capacidad de crecer para adaptarse a mayor demanda |
| **Fiabilidad** | Recuperarse de interrupciones de infraestructura automáticamente |

La clave del enunciado es **"lanzar nuevas características con iteraciones rápidas"** → innovar y experimentar → **Agilidad**.

> **Regla de oro:** "Lanzar rápido / iterar / experimentar / innovar sin inversión inicial" → **Agilidad**. "Ajustar recursos a la demanda real" → **Elasticidad**.

**Temática:** Conceptos del Cloud

---

## Pregunta 7 ✅ — AWS Budgets y Alertas de Utilización

**¿Qué servicio de AWS te ayudará a recibir alertas cuando la utilización de la reserva caiga por debajo del umbral definido?**

- ✅ AWS Budgets ← **Tu respuesta**
- ❌ AWS Pricing Calculator
- ❌ AWS CloudTrail
- ❌ AWS Trusted Advisor

### Explicación

**AWS Budgets** permite:
- Alertas cuando el gasto supera (o se prevé que supere) el presupuesto.
- Alertas cuando la utilización de reservas cae por debajo del umbral definido.

Compatible con reservas de: EC2, RDS, Redshift, ElastiCache y Elasticsearch.

> **Regla de oro:** "Alertas de presupuesto / utilización de reservas" → **AWS Budgets**.

**Temática:** Facturación y precios

---

## Pregunta 8 ❌ — VPC: Security Groups vs NACLs

**¿Cuáles de las siguientes afirmaciones son CORRECTAS en relación con AWS VPC? (Selecciona dos)**

- ❌ Una NACL solo puede tener reglas de permiso ← **Tu selección**
- ✅ Un grupo de seguridad solo puede tener reglas de permitir
- ✅ Un Gateway NAT está gestionado por AWS
- ❌ Una instancia NAT está gestionada por AWS
- ❌ Un grupo de seguridad puede tener tanto reglas de permitir como de denegar ← **Tu selección**

### Explicación

| Componente | Reglas | Gestión | Nivel | Estado |
|---|---|---|---|---|
| **Security Group** | Solo **ALLOW** | Cliente | Instancia | Stateful |
| **NACL** | Allow **Y DENY** | Cliente | Subred | Stateless |
| **Gateway NAT** | N/A | **AWS** | — | Gestionado |
| **Instancia NAT** | N/A | **Cliente** | — | No gestionado |

> **Regla de oro:**
> - Security Group → solo **allow**, nivel de instancia, **stateful**
> - NACL → allow **y deny**, nivel de subred, **stateless**
> - Gateway NAT → gestionado por **AWS** (no tú)
> - Instancia NAT → la gestionas **tú**

**Temática:** Tecnología

---

## Pregunta 9 ✅ — Controles Compartidos: Gestión de Configuración

**Según el modelo de responsabilidad compartida de AWS, ¿cuál de las siguientes es una responsabilidad compartida tanto de AWS como del cliente?**

- ❌ Garantizar la separación de datos entre clientes de AWS
- ❌ Mantenimiento de la infraestructura de servidores S3
- ❌ Mantenimiento de la infraestructura de zonas de disponibilidad
- ✅ Gestión de la configuración ← **Tu respuesta**

### Explicación

La **gestión de la configuración** es un **control compartido**:
- **AWS** mantiene la configuración de sus dispositivos de infraestructura (hardware).
- **Cliente** configura sus propios SOs, bases de datos y aplicaciones.

Las demás opciones son responsabilidades exclusivas de **AWS** (infraestructura física, separación de datos entre clientes, AZ).

> **Regla de oro:** Controles compartidos = misma categoría de actividad, capas distintas. La gestión de configuración aplica tanto a AWS (hardware) como al cliente (software invitado).

**Temática:** Seguridad y normativa

---

## Pregunta 10 ✅ — Ahorro de Costes al Migrar a AWS

**Una empresa migra de on-premises a AWS. ¿Cuáles áreas de gasto supondrían un ahorro? (Selecciona dos)**

- ❌ Gasto en licencias de aplicaciones SaaS
- ❌ Salario del jefe de proyecto
- ✅ Gasto en infraestructura de hardware del centro de datos ← **Tu selección**
- ✅ Gasto en seguridad física del centro de datos ← **Tu selección**
- ❌ Salario del desarrollador

### Explicación

Al migrar a AWS, la empresa elimina:
- ✅ Compra y mantenimiento de **hardware** físico
- ✅ Costes de **seguridad física** del datacenter

Permanecen igual:
- ❌ Licencias SaaS (independientes de la infraestructura)
- ❌ Salarios del equipo de desarrollo y gestión

> **Regla de oro:** AWS elimina los costes de **hardware e instalaciones físicas**. Los gastos de personal y software de terceros permanecen iguales.

**Temática:** Conceptos del Cloud

---

## Pregunta 11 ❓ — Ventajas del Cloud Computing *(Texto de pregunta faltante)*

> ⚠️ El texto de la pregunta y las opciones no están disponibles en este archivo. Solo se conservó la sección de explicación.

**Opciones correctas identificadas:**
- ✅ Benefíciate de las economías de escala masivas
- ✅ Cambia gasto de capital (CapEx) por gasto variable (OpEx)
- ✅ Globalízate en minutos desplegando en varias regiones

**Opciones incorrectas:**
- ❌ Gastar dinero en construir y mantener centros de datos (AWS elimina este gasto)
- ❌ Planificar meses de capacidad de infraestructura (AWS elimina esta necesidad)
- ❌ Cambiar gastos variables por gastos de capital (es al revés: CapEx → OpEx)

### Las 6 Ventajas del Cloud Computing (AWS)

| # | Ventaja |
|---|---|
| 1 | Cambia CapEx por gasto variable (OpEx) |
| 2 | Benefíciate de economías de escala masivas |
| 3 | Deja de adivinar la capacidad |
| 4 | Aumenta la velocidad y la agilidad |
| 5 | Deja de gastar dinero en mantenimiento de datacenters |
| 6 | Globalízate en minutos |

> **Regla de oro:** Memoriza las 6 ventajas del Cloud — son pregunta segura en el examen.

**Temática:** Conceptos del Cloud

---

## Pregunta 12 ✅ — Precios EC2: Menor Coste Sin Interrupción

**Una startup quiere aprovisionar EC2 al menor coste posible a largo plazo, sin riesgo de interrupción. ¿Cuál opción recomiendas?**

- ❌ Instancia spot EC2
- ❌ Host dedicado EC2
- ✅ Instancia reservada EC2 (RI) ← **Tu respuesta**
- ❌ Instancia bajo demanda EC2

### Explicación

| Tipo | Descuento vs On-Demand | ¿Se interrumpe? | Compromiso |
|---|---|---|---|
| **Reservada (RI)** | Hasta 75% | ❌ Nunca | 1 o 3 años |
| **Spot** | Hasta 90% | ✅ Sí (aviso corto) | Ninguno |
| **On-Demand** | 0% (precio base) | ❌ Nunca | Ninguno |
| **Host dedicado** | Variable | ❌ Nunca | Más costoso |

> **Regla de oro:** "Menor coste + largo plazo + sin interrupción" → **Instancia Reservada (RI)**. "Máximo ahorro pero puede interrumpirse" → **Spot**.

**Temática:** Facturación y precios

---

## Pregunta 13 ❌ — Planes de Soporte: Orientación Contextual

**¿Qué plan AWS Support proporciona orientación arquitectónica contextual a tus casos de uso específicos?**

- ❌ AWS Developer Support
- ❌ AWS Enterprise On-Ramp Support
- ❌ AWS Enterprise Support ← **Tu respuesta**
- ✅ AWS Business Support

### Explicación

| Plan | Orientación arquitectónica | Disponibilidad |
|---|---|---|
| **Developer** | General (no contextual) | Horario laboral |
| **Business** | **Contextual a casos de uso específicos** ✅ | 24x7 |
| **Enterprise On-Ramp** | Contextual (una revisión al año) | 24x7 |
| **Enterprise** | Contextual + TAM dedicado | 24x7 |

El enunciado pide la opción que "proporciona orientación arquitectónica contextual" — el plan **Business** es el primer nivel que lo ofrece.

> **Regla de oro:** "Orientación **contextual** a casos de uso" → mínimo **Business**. Enterprise también lo tiene pero Business es la respuesta más ajustada al enunciado del examen.

**Temática:** Facturación y precios

---

## Pregunta 14 ❌ — Conectividad Híbrida sin Internet Público

**¿Cuál de los siguientes servicios puede conectar el entorno local de una empresa a una VPC sin usar el Internet público?**

- ❌ Amazon VPC Endpoint
- ❌ Internet Gateway
- ❌ VPN Site-to-Site ← **Tu respuesta**
- ✅ AWS Direct Connect

### Explicación

| Servicio | Conexión | ¿Usa Internet público? | Setup |
|---|---|---|---|
| **AWS Direct Connect** | Física dedicada on-premises → VPC | ❌ No (privada) | Semanas/meses |
| **VPN Site-to-Site** | Túnel seguro on-premises → VPC | ✅ Sí (sobre Internet) | Minutos/horas |
| **VPC Endpoint** | VPC → servicios AWS internamente | ❌ No (pero solo VPC→AWS) | — |
| **Internet Gateway** | VPC ↔ Internet público | ✅ Sí | — |

> **Regla de oro:** "Privada / sin Internet público / datacenter local → AWS" → **Direct Connect**. "Segura pero sobre Internet" → **VPN Site-to-Site**.

**Temática:** Tecnología

---

## Pregunta 15 ✅ — Evaluación de Vulnerabilidades en EC2

**Una empresa quiere automatizar evaluaciones de seguridad y vulnerabilidades del SO en instancias EC2. ¿Qué servicio recomendarías?**

- ❌ Amazon GuardDuty
- ❌ Amazon Macie
- ❌ AWS Shield
- ✅ Amazon Inspector ← **Tu respuesta**

### Explicación

| Servicio | Función |
|---|---|
| **Amazon Inspector** | Evaluación automatizada de seguridad — vulnerabilidades y desviaciones en EC2 |
| **GuardDuty** | Detección de amenazas a nivel de cuenta (CloudTrail, VPC Flow Logs, DNS) |
| **Macie** | Descubrimiento de datos sensibles (PII) en S3 |
| **Shield** | Protección DDoS (capas 3, 4 y 7 con Advanced) |

> **Regla de oro:** "Vulnerabilidades del SO / evaluación de seguridad en instancias EC2" → **Amazon Inspector**.

**Temática:** Seguridad y normativa

---

## Pregunta 16 ⚠️ — AWS Shield Advanced: Recursos Cubiertos

**AWS Shield Advanced proporciona protección DDoS ampliada para aplicaciones web en ¿cuáles recursos? (Selecciona dos)**

- ✅ Amazon Route 53 ← **Tu selección** ✅ Correcta
- ❌ Amazon API Gateway ← **Tu selección** ❌ Incorrecta
- ❌ AWS Elastic Beanstalk
- ❌ AWS CloudFormation
- ✅ AWS Global Accelerator ← No seleccionada (correcta)

### Explicación

**AWS Shield Advanced** cubre exactamente estos 5 recursos:

| Recurso cubierto por Shield Advanced |
|---|
| Amazon EC2 |
| Elastic Load Balancer (ELB) |
| Amazon CloudFront |
| Amazon **Route 53** ✅ |
| AWS **Global Accelerator** ✅ |

- **API Gateway**: usa AWS WAF, no Shield Advanced directamente.
- **Elastic Beanstalk**: cubierto solo por Shield Standard.
- **CloudFormation**: servicio de aprovisionamiento, no protección DDoS.

> **Regla de oro:** Shield Advanced = **EC2, ELB, CloudFront, Route 53, Global Accelerator**. Memoriza esta lista de 5.

**Temática:** Seguridad y normativa

---

## Pregunta 17 ✅ — Escalado Vertical vs Horizontal (Afirmación INCORRECTA)

**¿Cuál de las siguientes es una afirmación INCORRECTA sobre el Escalado en el pilar de Fiabilidad?**

- ❌ La tolerancia a fallos se consigue mediante una operación de escalado horizontal
- ❌ Una operación de ampliación (scale out) implica añadir más instancias al conjunto existente
- ✅ La tolerancia a fallos se consigue mediante una operación de escalado vertical ← **Tu respuesta**
- ❌ Una operación de escalado (scale up) implica añadir más potencia (CPU, RAM) al nodo existente

### Explicación

| Tipo | Mecanismo | Tolerancia a fallos |
|---|---|---|
| **Scale up (vertical)** | Añadir CPU/RAM a un único servidor | ❌ No — único punto de fallo |
| **Scale out (horizontal)** | Añadir más instancias | ✅ Sí — distribuido, sin punto único de fallo |

La afirmación **INCORRECTA** es: "La tolerancia a fallos se consigue mediante escalado vertical" → con un solo servidor, si falla, todo falla.

> **Regla de oro:** Tolerancia a fallos → siempre **escalado horizontal (scale out)**. El escalado vertical = único punto de fallo.

**Temática:** Conceptos del Cloud

---

## Pregunta 18 ✅ — Distribuir Tráfico Entrante

**¿Cuál de los siguientes servicios de AWS debe utilizarse para distribuir automáticamente el tráfico entrante entre varios objetivos?**

- ✅ AWS Elastic Load Balancer (ELB) ← **Tu respuesta**
- ❌ Amazon OpenSearch
- ❌ AWS Elastic Beanstalk
- ❌ AWS Auto Scaling

### Explicación

- **ELB**: Distribuye tráfico entrante entre instancias EC2, contenedores u otros destinos.
- **Auto Scaling**: Ajusta el número de instancias según la demanda (escala, no distribuye tráfico).
- **Elastic Beanstalk**: PaaS que internamente usa ELB y Auto Scaling — no es el servicio directo de distribución.
- **OpenSearch**: Motor de búsqueda y análisis de logs — no distribuye tráfico.

> **Regla de oro:** "Distribuir tráfico / balanceo de carga" → **Elastic Load Balancer (ELB)**. Trabaja junto con Auto Scaling pero son servicios distintos.

**Temática:** Tecnología

---

## Pregunta 19 ❌ — Descubrir Datos Sensibles en S3

**Una startup sanitaria almacena datos en S3 y quiere descubrir e identificar datos sensibles para evitar fugas. ¿Qué servicio recomiendas?**

- ❌ AWS Glue
- ❌ AWS Secrets Manager
- ✅ Amazon Macie
- ❌ Amazon Polly ← **Tu respuesta**

### Explicación

| Servicio | Función |
|---|---|
| **Amazon Macie** | Descubre y protege datos sensibles (PII) en S3 usando ML — inventario, buckets públicos, cifrado |
| **AWS Glue** | ETL (extracción, transformación y carga) — no para seguridad de datos |
| **AWS Secrets Manager** | Gestiona credenciales y secretos — no descubre datos en S3 |
| **Amazon Polly** | Convierte texto en voz (TTS) — sin relación con seguridad de datos |

> **Regla de oro:** "Datos sensibles / PII / descubrir información confidencial en S3" → **Amazon Macie**.

**Temática:** Tecnología

---

## Pregunta 20 ❌ — EBS vs EFS: Zonas de Disponibilidad

**¿Cuál afirmación es CORRECTA sobre las características de AZ de EBS y EFS?**

- ❌ EBS puede adjuntarse a varias instancias en varias AZ / EFS puede montarse en instancias de la misma AZ
- ✅ EBS puede adjuntarse a **una única instancia en la misma AZ** / EFS puede montarse en instancias de **múltiples AZ**
- ❌ EBS puede adjuntarse a una o más instancias en varias AZ / EFS puede montarse en instancias de varias AZ ← **Tu respuesta**
- ❌ EBS puede adjuntarse a una única instancia en la misma AZ / EFS solo puede montarse en instancias de la misma AZ

### Explicación

| Servicio | Scope | Multi-instancia | Multi-AZ |
|---|---|---|---|
| **Amazon EBS** | Una AZ | Limitado (Multi-Attach solo tipos io1/io2) | ❌ No (mismo AZ que la instancia) |
| **Amazon EFS** | Regional | ✅ Sí (NFS compartido) | ✅ Sí (accesible desde todas las AZ de la región) |

> **Regla de oro:**
> - EBS → **una instancia, una AZ** (volumen local a la AZ)
> - EFS → **múltiples instancias, múltiples AZ** (sistema de archivos compartido regional)

**Temática:** Tecnología

---

## Pregunta 21 ✅ — Base de Datos NoSQL Activo-Activo Multi-Región

**Una empresa necesita una BD NoSQL gestionada con configuración activo-activo en múltiples regiones AWS. ¿Cuál es la más adecuada?**

- ✅ Amazon DynamoDB con tablas globales ← **Tu respuesta**
- ❌ Amazon DynamoDB con DynamoDB Accelerator (DAX)
- ❌ Amazon Aurora con clústeres Multi-Master
- ❌ Amazon RDS para MySQL

### Explicación

- **DynamoDB Global Tables**: Replica datos automáticamente entre regiones — lectura/escritura en milisegundos desde cualquier región — **activo-activo multi-región** ✅
- **DAX**: Caché en memoria para DynamoDB (microsegundos) — no es multi-región activo-activo.
- **Aurora Multi-Master**: Solo activo-activo dentro de una región — además es SQL, no NoSQL.
- **RDS**: Relacional (SQL) — no NoSQL.

> **Regla de oro:** "NoSQL + activo-activo + multi-región" → **DynamoDB Global Tables**.

**Temática:** Tecnología

---

## Pregunta 22 ⚠️ — Gestión del Cambio: Pilar de Fiabilidad

**¿Qué servicios AWS facilitan la gestión del cambio organizativo (pilar de Fiabilidad del Well-Architected Framework)? (Selecciona tres)**

- ✅ AWS CloudTrail ← **Tu selección** ✅ Correcta
- ❌ AWS Trusted Advisor
- ✅ AWS Config ← No seleccionada (correcta)
- ❌ Amazon GuardDuty ← **Tu selección** ❌ Incorrecta
- ❌ Amazon Inspector
- ✅ Amazon CloudWatch ← **Tu selección** ✅ Correcta

### Explicación

El pilar **Fiabilidad** tiene tres áreas: fundamentos, **gestión del cambio** y gestión de fallos.

| Servicio | Contribución a gestión del cambio |
|---|---|
| **AWS Config** | Registra cambios en configuraciones de recursos — historial de compliance ✅ |
| **AWS CloudTrail** | Registra actividad de cuenta y cambios vía API ✅ |
| **Amazon CloudWatch** | Monitoriza métricas — detecta impacto de cambios ✅ |
| **Trusted Advisor** | Recomendaciones de buenas prácticas (no gestión de cambio) |
| **Inspector** | Evaluación de seguridad en EC2 (no gestión de cambio) |
| **GuardDuty** | Detección de amenazas — no gestión de cambio |

> **Regla de oro:** Gestión del cambio = **CloudTrail + Config + CloudWatch** (visibilidad completa de qué cambió, quién lo cambió y qué impacto tuvo).

**Temática:** Tecnología

---

## Pregunta 23 ❌ — EC2: Licencias de Software Vinculadas al Servidor

**Una empresa tiene licencias de software vinculadas a servidores (Microsoft, Oracle) y quiere usarlas en AWS. ¿Qué tipo de instancia EC2 recomiendas?**

- ❌ Instancia bajo demanda
- ✅ Host dedicado
- ❌ Instancia dedicada ← **Tu respuesta**
- ❌ Instancia reservada (RI)

### Explicación

| Tipo | Servidor físico exclusivo | Visibilidad del servidor | Licencias por servidor |
|---|---|---|---|
| **Host dedicado** | ✅ Servidor físico entero para ti | ✅ Sí (socket/núcleo físico visible) | ✅ Compatible |
| **Instancia dedicada** | Hardware dedicado pero compartido entre instancias propias | ❌ No visible | ❌ No compatible |
| **On-Demand** | ❌ Hardware compartido | ❌ No | ❌ No |
| **Reservada** | ❌ Hardware compartido | ❌ No | ❌ No |

La diferencia clave: el **Host dedicado** te da el servidor físico completo con visibilidad de sockets/núcleos, necesaria para las licencias BYOL.

> **Regla de oro:** "Licencias de software vinculadas a servidor (Microsoft/Oracle BYOL)" → **Host dedicado** (NO Instancia Dedicada). Son conceptos distintos.

**Temática:** Tecnología

---

## Pregunta 24 ✅ — Seguridad Habilitada por Defecto sin Coste

**¿Qué servicio de seguridad AWS está habilitado para todos los clientes por defecto sin coste adicional?**

- ✅ AWS Shield Standard ← **Tu respuesta**
- ❌ AWS Secrets Manager
- ❌ AWS WAF
- ❌ AWS Shield Advanced

### Explicación

| Servicio | ¿Gratuito por defecto? | Cobertura |
|---|---|---|
| **AWS Shield Standard** | ✅ Sí — todos los clientes | DDoS capas 3 y 4 |
| **AWS Shield Advanced** | ❌ No — servicio de pago | DDoS capas 3, 4 y 7 |
| **AWS WAF** | ❌ No — cobra por ACL y reglas | HTTP/HTTPS (capa 7) |
| **AWS Secrets Manager** | ❌ No — cobra por secretos y API calls | Gestión de credenciales |

> **Regla de oro:** Shield **Standard** → gratuito para todos. Shield **Advanced** → de pago, protección adicional contra DDoS.

**Temática:** Seguridad y normativa

---

## Pregunta 25 ✅ — Amazon EC2: Modelo de Servicio (IaaS)

**¿Qué tipo de Cloud Computing representa Amazon EC2?**

- ❌ Plataforma como servicio (PaaS)
- ✅ Infraestructura como servicio (IaaS) ← **Tu respuesta**
- ❌ Red como servicio (NaaS)
- ❌ Software como servicio (SaaS)

### Explicación

| Modelo | Ejemplo AWS | Control del cliente |
|---|---|---|
| **IaaS** | EC2, VPC, EBS | Máximo — elige SO, configura red |
| **PaaS** | Elastic Beanstalk, Lambda, RDS | Medio — solo código y datos |
| **SaaS** | WorkMail, Connect | Mínimo — solo consume |

EC2 da control total sobre el SO, red, almacenamiento y aplicaciones → **IaaS**.

> **Nota:** "NaaS" (Red como servicio) es un distractor — no es una categoría estándar del examen CLF-C02.

**Temática:** Conceptos del Cloud

---

## Pregunta 26 ❌ — Almacenamiento EC2 de Alto Rendimiento con Tolerancia a Fallos

**Un grupo ejecuta una app de cálculo científico con arquitectura tolerante a fallos. Necesita discos de alto rendimiento I/O. ¿Cuál es la solución MÁS rentable?**

- ❌ Amazon S3
- ❌ Amazon EBS
- ✅ Almacén de instancias (Instance Store)
- ❌ Amazon EFS ← **Tu respuesta**

### Explicación

| Almacenamiento | Latencia | Persistencia | Disponible como disco físico | Coste extra |
|---|---|---|---|---|
| **Instance Store** | Muy baja (físico) | ❌ Temporal (se pierde al parar) | ✅ Sí | ❌ Ninguno (incluido) |
| **EBS** | Baja (red) | ✅ Persistente | ❌ No | ✅ Adicional |
| **EFS** | Media (NFS) | ✅ Persistente | ❌ No | ✅ Adicional |
| **S3** | Alta (HTTP) | ✅ Persistente | ❌ No | ✅ Adicional |

En este caso: la aplicación **ya es tolerante a fallos** → gestiona la pérdida de datos automáticamente → Instance Store perfecto: máximo rendimiento, sin coste adicional.

> **Regla de oro:** "Alto rendimiento I/O + arquitectura tolerante a fallos + menor coste" → **Instance Store** (temporal pero incluido en el precio de la instancia).

**Temática:** Tecnología

---

## Pregunta 27 ❌ — AWS CAF: Ser Más Receptivo al Cliente

**Según AWS CAF, ¿qué tareas debe realizar una empresa para ser más receptiva a las consultas y comentarios de los clientes? (Selecciona dos)**

- ✅ Aprovecha los métodos ágiles para iterar y evolucionar rápidamente ← No seleccionada (correcta)
- ✅ Organiza tus equipos en torno a productos y flujos de valor ← No seleccionada (correcta)
- ❌ Aprovechar la infraestructura heredada para aumentar la eficiencia de costes ← **Tu selección**
- ❌ Crea nuevas perspectivas analíticas con los productos y servicios existentes ← **Tu selección**
- ❌ Organiza tus equipos en torno a principios de diseño burocrático

### Explicación

El **AWS CAF** agrupa capacidades en 6 perspectivas: Negocio, Personas, Gobernanza, Plataforma, Seguridad y Operaciones.

Para ser más **receptivo al cliente**:
- ✅ **Métodos ágiles**: iteración rápida → respuesta ágil al feedback de clientes.
- ✅ **Equipos orientados a productos/flujos de valor**: estructuras centradas en el cliente, no en funciones internas.

Las opciones incorrectas (infraestructura heredada, burocracia, analytics con productos existentes) contradicen la transformación digital ágil y centrada en el cliente.

> **Regla de oro:** AWS CAF + receptividad al cliente = **métodos ágiles** + **equipos orientados a productos**.

**Temática:** Conceptos del Cloud

---

## Pregunta 28 ❌ — Route 53: Política de Enrutamiento Ponderado

**¿Qué política de enrutamiento de AWS Route 53 utilizarías para enrutar tráfico a múltiples recursos y elegir cuánto tráfico va a cada uno?**

- ✅ Enrutamiento ponderado (Weighted routing)
- ❌ Enrutamiento basado en la latencia ← **Tu respuesta**
- ❌ Enrutamiento de conmutación por error (Failover)
- ❌ Enrutamiento simple

### Explicación

| Política Route 53 | Cuándo usarla |
|---|---|
| **Simple** | Un único recurso destino |
| **Ponderado (Weighted)** | Múltiples recursos con % de tráfico definido → A/B testing, distribución |
| **Basado en latencia** | Múltiples regiones — dirige al que tenga menor latencia para el usuario |
| **Failover** | Activo-pasivo — redirige al secundario si el primario falla |
| **Geolocalización** | Dirige según la ubicación geográfica del usuario |
| **Multi-value** | Devuelve múltiples valores con health checks |

La clave del enunciado es **"elegir cuánto tráfico se enruta a cada recurso"** → pesos → **Weighted**.

> **Regla de oro:** "Controlar **cuánto** tráfico va a cada recurso (porcentaje/peso)" → **Weighted**. "Región más **rápida**" → Latency-based.

**Temática:** Tecnología

---

## Pregunta 29 ❌ — Depurar Aplicaciones Serverless de Microservicios

**Un equipo DevOps necesita depurar problemas de rendimiento en una aplicación serverless con arquitectura de microservicios. ¿Qué servicio recomendarías?**

- ❌ AWS CloudFormation ← **Tu respuesta**
- ✅ AWS X-Ray
- ❌ AWS Trusted Advisor
- ❌ Amazon Pinpoint

### Explicación

| Servicio | Función |
|---|---|
| **AWS X-Ray** | Tracing distribuido — analiza y depura aplicaciones serverless y microservicios |
| **CloudFormation** | Infraestructura como código — aprovisiona recursos, no depura rendimiento |
| **Trusted Advisor** | Recomendaciones de mejores prácticas — no depura rendimiento de aplicaciones |
| **Pinpoint** | Engagement y análisis de usuarios (marketing/comunicaciones) |

> **Regla de oro:** "Depurar / tracing / rendimiento en serverless / microservicios" → **AWS X-Ray**.

**Temática:** Tecnología

---

## Guía Rápida de Repaso — Servicios Frecuentemente Confundidos

### CloudWatch vs CloudTrail vs Config

| Servicio | Pregunta que responde | Caso de uso |
|---|---|---|
| **CloudWatch** | ¿Cómo está **funcionando** mi sistema? | Métricas, alarmas, dashboards |
| **CloudTrail** | ¿**Quién hizo qué** y cuándo en mi cuenta? | Auditoría de llamadas a API |
| **Config** | ¿**Cómo cambió** la configuración de mis recursos? | Compliance, historial de cambios |

### Opciones de Precios EC2

| Tipo | Ahorro | ¿Se interrumpe? | Mejor para |
|---|---|---|---|
| **On-Demand** | 0% | No | Cargas variables, testing |
| **Savings Plans** | ~66% | No | Flexible (EC2 + Lambda + Fargate) |
| **Reservada 1 año** | ~40% | No | Cargas predecibles largo plazo |
| **Reservada 3 años** | ~75% | No | Máximo ahorro garantizado |
| **Spot** | ~90% | ✅ Sí | Tolerante a fallos, batch |
| **Host dedicado** | Variable | No | Licencias BYOL (Microsoft/Oracle) |

### Seguridad: Qué Servicio Para Qué

| Necesidad | Servicio |
|---|---|
| Vulnerabilidades SO en instancias EC2 | **Amazon Inspector** |
| Datos sensibles (PII) en S3 | **Amazon Macie** |
| Amenazas y actividad maliciosa en la cuenta | **Amazon GuardDuty** |
| Protección DDoS capas 3/4 (gratis) | **AWS Shield Standard** |
| Protección DDoS avanzada capas 3/4/7 | **AWS Shield Advanced** |
| Firewall de aplicaciones web HTTP/HTTPS | **AWS WAF** (capa 7) |
| Credenciales, secretos y claves API | **AWS Secrets Manager** |

### Almacenamiento para Instancias EC2

| Servicio | Tipo | Persistencia | Multi-AZ | Mejor para |
|---|---|---|---|---|
| **Instance Store** | Bloque físico | Temporal | No | Alto I/O, tolerante a fallos, sin coste extra |
| **EBS** | Bloque red | Persistente | No (una AZ) | BD, volumen de arranque |
| **EFS** | Archivos NFS | Persistente | Sí (multi-AZ) | Contenido compartido entre instancias |
| **S3** | Objetos | Persistente | Sí (11 nueves) | Backups, activos estáticos, data lakes |

### Conectividad Híbrida (On-Premises ↔ AWS)

| Servicio | Privado | Velocidad de Setup | Ancho de banda |
|---|---|---|---|
| **AWS Direct Connect** | ✅ Sí (sin Internet) | Semanas/meses | Alto (dedicado) |
| **VPN Site-to-Site** | ❌ No (sobre Internet cifrado) | Minutos/horas | Limitado por Internet |

### Políticas de Enrutamiento Route 53

| Política | Cuándo usarla |
|---|---|
| Simple | Un único destino |
| **Weighted** | Controlar % de tráfico por recurso (A/B testing) |
| Latency-based | Dirigir a la región más rápida para el usuario |
| Failover | Activo-pasivo (DR) |
| Geolocation | Dirigir según país/continente del usuario |
| Multi-value | Múltiples IPs con health checks |

### Security Groups vs NACLs

| Aspecto | Security Group | NACL |
|---|---|---|
| Reglas | Solo **ALLOW** | ALLOW y **DENY** |
| Nivel | Instancia | Subred |
| Estado | **Stateful** (recuerda conexiones) | **Stateless** (evalúa cada paquete) |
| Aplicación | Adjunto a la instancia EC2 | Adjunto a la subred |
