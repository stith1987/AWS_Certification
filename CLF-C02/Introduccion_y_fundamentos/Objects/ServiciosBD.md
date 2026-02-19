# Servicios de Base de Datos de AWS - Examen CLF-C02

Basado en las tres fuentes proporcionadas (Sequeira, Piper/Clinton y Kankaria), he analizado los Servicios de Base de Datos de AWS.

En el contexto del examen **AWS Certified Cloud Practitioner (CLF-C02)**, este tema se encuadra principalmente en el **Dominio 3: Tecnología y Servicios en la Nube**, abordando específicamente la **Declaración de Tarea 3.4: Identificar servicios de base de datos de AWS**. Además, es crucial para entender la migración (Dominio 1.3) y los costos (Dominio 4).

A continuación, presento un análisis detallado estructurado según los objetivos del examen, diferenciando entre bases de datos relacionales, no relacionales y otros servicios especializados.

---

## 1. Servicios de Bases de Datos Relacionales (SQL)

El examen evalúa si comprende cuándo usar una base de datos relacional (estructurada, con filas y columnas) y qué opciones ofrece AWS.

### Amazon Relational Database Service (RDS)

- **Propósito:** Es un servicio **gestionado** que facilita la configuración, operación y escalado de bases de datos relacionales en la nube. Automatiza tareas administrativas como el aprovisionamiento de hardware, la configuración de bases de datos, los parches y las copias de seguridad.
- **Motores Soportados:** Debe memorizar que RDS soporta **seis motores**: MySQL, MariaDB, PostgreSQL, Oracle, Microsoft SQL Server y Amazon Aurora.
- **Características Clave:**
  - **Multi-AZ:** Implementación para **Alta Disponibilidad** y recuperación ante desastres (failover automático en caso de fallo).
  - **Read Replicas (Réplicas de Lectura):** Se utilizan para mejorar el **rendimiento de lectura** (escalado horizontal), **no** para alta disponibilidad ante desastres.

> **Tip de examen:** **Multi-AZ** = Alta Disponibilidad (failover). **Read Replicas** = Rendimiento de lectura (escalado). No confundir estos conceptos.

### Amazon Aurora

- **Definición:** Es una base de datos relacional compatible con **MySQL y PostgreSQL** creada por AWS para la nube.
- **Ventaja Competitiva:** Ofrece el rendimiento y la disponibilidad de bases de datos comerciales de alta gama a una **décima parte del costo**. Es hasta **5 veces más rápida que MySQL** estándar y **3 veces más rápida que PostgreSQL** estándar.
- **Arquitectura:** Utiliza almacenamiento distribuido y tolerante a fallos, replicando datos **6 veces a través de 3 Zonas de Disponibilidad (AZ)**.

> **Tip de examen:** Si la pregunta menciona "relacional + alto rendimiento + compatibilidad MySQL/PostgreSQL + costo-eficiente" = **Aurora**.

### 📊 Diagrama: RDS vs Aurora - ¿Cuándo usar cada uno?

```mermaid
flowchart TD
    Q["🤔 ¿Necesito una base de datos<br/>RELACIONAL (SQL)?"] -->|"Sí"| R{"¿Qué motor necesito?"}

    R -->|"MySQL o<br/>PostgreSQL"| AUR{"¿Necesito máximo<br/>rendimiento y<br/>disponibilidad?"}
    R -->|"Oracle o<br/>SQL Server"| RDS1["✅ Amazon RDS<br/>(único motor compatible)"]
    R -->|"MariaDB"| RDS2["✅ Amazon RDS"]

    AUR -->|"Sí, rendimiento<br/>5x MySQL / 3x PostgreSQL"| AURORA["✅ Amazon Aurora<br/>6 copias en 3 AZs<br/>Auto-scaling"]
    AUR -->|"No, estándar<br/>es suficiente"| RDS3["✅ Amazon RDS<br/>(MySQL/PostgreSQL)"]

    subgraph HA["🔄 Alta Disponibilidad"]
        MULTI["🛡️ Multi-AZ<br/>Failover automático<br/>= DISPONIBILIDAD"]
        READ["📖 Read Replicas<br/>Escalado horizontal<br/>= RENDIMIENTO lectura"]
    end

    style Q fill:#FF9900,stroke:#232F3E,color:#232F3E
    style R fill:#232F3E,stroke:#FF9900,color:#FFFFFF
    style AUR fill:#232F3E,stroke:#FF9900,color:#FFFFFF
    style AURORA fill:#0d904f,stroke:#232F3E,color:#FFFFFF
    style RDS1 fill:#1a73e8,stroke:#232F3E,color:#FFFFFF
    style RDS2 fill:#1a73e8,stroke:#232F3E,color:#FFFFFF
    style RDS3 fill:#1a73e8,stroke:#232F3E,color:#FFFFFF
    style HA fill:#232F3E,stroke:#FF9900,color:#FFFFFF
    style MULTI fill:#FF4444,stroke:#232F3E,color:#FFFFFF
    style READ fill:#e8710a,stroke:#232F3E,color:#FFFFFF
```

---

## 2. Servicios de Bases de Datos No Relacionales (NoSQL)

El examen distingue claramente entre casos de uso SQL (estructurados) y NoSQL (flexibles, alto rendimiento).

### Amazon DynamoDB

- **Definición:** Base de datos de **clave-valor y documentos**, totalmente gestionada y **sin servidor (serverless)**.
- **Casos de Uso:** Aplicaciones que requieren latencia de **milisegundos de un solo dígito** a cualquier escala, como juegos, tecnología publicitaria, IoT y aplicaciones móviles.
- **Características:**
  - No requiere administración de servidores (serverless)
  - Escala automáticamente
  - Soporta **tablas globales** para replicación en múltiples regiones

> **Tip de examen:** "NoSQL + serverless + latencia de milisegundos + escala ilimitada" = **DynamoDB**. Es el servicio NoSQL estrella de AWS.

### 📊 Diagrama: SQL vs NoSQL - ¿Cuándo usar cada tipo?

```mermaid
flowchart TD
    Q["🤔 ¿Qué tipo de base<br/>de datos necesito?"]

    Q --> TYPE{"¿Cómo son<br/>mis datos?"}

    TYPE -->|"Estructurados<br/>Relaciones complejas<br/>Transacciones ACID"| SQL
    TYPE -->|"Flexibles<br/>Clave-valor / Documentos<br/>Escala masiva"| NOSQL

    subgraph SQL["🗃️ Relacional (SQL)"]
        direction TB
        RDS["📊 Amazon RDS<br/>6 motores<br/>MySQL, PostgreSQL,<br/>Oracle, SQL Server,<br/>MariaDB, Aurora"]
        AUR["⚡ Amazon Aurora<br/>MySQL/PostgreSQL<br/>5x más rápida<br/>6 copias en 3 AZs"]
    end

    subgraph NOSQL["📋 No Relacional (NoSQL)"]
        direction TB
        DDB["⚡ DynamoDB<br/>Clave-valor / Documentos<br/>Serverless, ms de latencia<br/>Escala ilimitada"]
    end

    style Q fill:#FF9900,stroke:#232F3E,color:#232F3E
    style TYPE fill:#232F3E,stroke:#FF9900,color:#FFFFFF
    style SQL fill:#1a73e8,stroke:#FF9900,color:#FFFFFF
    style NOSQL fill:#0d904f,stroke:#FF9900,color:#FFFFFF
    style RDS fill:#1a73e8,stroke:#FFFFFF,color:#FFFFFF
    style AUR fill:#1a73e8,stroke:#FFFFFF,color:#FFFFFF
    style DDB fill:#0d904f,stroke:#FFFFFF,color:#FFFFFF
```

---

## 3. Servicios de Almacenamiento en Memoria (Caching)

Para el examen, debe identificar qué servicio mejora el rendimiento de lectura de bases de datos mediante caché.

### Amazon ElastiCache

- **Propósito:** Servicio de almacén de datos **en memoria** totalmente gestionado. Mejora el rendimiento de las aplicaciones web al permitir recuperar información de cachés en memoria rápidos y gestionados, en lugar de depender totalmente de bases de datos basadas en disco más lentas.
- **Motores:** Soporta **Redis** y **Memcached**.

### Amazon MemoryDB

- Servicio compatible con **Redis** que ofrece **durabilidad** y puede actuar como **base de datos primaria** (a diferencia de ElastiCache que es puramente caché), aunque ElastiCache es el foco principal del examen CLF-C02.

> **Tip de examen:** "Mejorar rendimiento de lectura con caché en memoria" = **ElastiCache**. "Base de datos durable compatible con Redis" = **MemoryDB**.

---

## 4. Servicios de Almacenamiento de Datos (Data Warehousing)

Debe diferenciar entre una base de datos **operativa (transaccional = OLTP)** y un **almacén de datos (analítico = OLAP)**.

### Amazon Redshift

- **Propósito:** Servicio de **almacén de datos (data warehouse)** a escala de petabytes, rápido y totalmente gestionado. Permite analizar todos los datos utilizando herramientas de inteligencia empresarial estándar y SQL.
- **Tecnología:** Utiliza **almacenamiento en columnas** y **procesamiento paralelo masivo** para consultas analíticas complejas (OLAP), a diferencia de RDS que es para transacciones (OLTP).

> **Tip de examen:** "Analítica / Data Warehouse / BI / SQL complejo sobre petabytes" = **Redshift**. "Transacciones operativas" = **RDS/Aurora**.

### 📊 Diagrama: OLTP vs OLAP - ¿Transaccional o Analítico?

```mermaid
flowchart LR
    subgraph OLTP["💼 OLTP (Transaccional)"]
        direction TB
        DESC1["📝 Operaciones del día a día<br/>INSERT, UPDATE, DELETE<br/>Muchas transacciones pequeñas"]
        RDS["📊 Amazon RDS"]
        AUR["⚡ Amazon Aurora"]
        DDB["⚡ DynamoDB"]
    end

    subgraph OLAP["📈 OLAP (Analítico)"]
        direction TB
        DESC2["🔍 Análisis de grandes volúmenes<br/>SELECT complejos, agregaciones<br/>Pocas consultas sobre muchos datos"]
        RED["🏢 Amazon Redshift<br/>Data Warehouse<br/>Petabytes, columnar,<br/>procesamiento paralelo"]
    end

    subgraph CACHE["⚡ Caché en Memoria"]
        direction TB
        DESC3["🚀 Acelerar lecturas frecuentes<br/>Microsegundos de latencia"]
        EC["🧊 ElastiCache<br/>Redis / Memcached<br/>Caché puro"]
        MDB["💾 MemoryDB<br/>Redis durable<br/>BD primaria"]
    end

    style OLTP fill:#1a73e8,stroke:#FF9900,color:#FFFFFF
    style OLAP fill:#e8710a,stroke:#FF9900,color:#FFFFFF
    style CACHE fill:#0d904f,stroke:#FF9900,color:#FFFFFF
    style RDS fill:#1a73e8,stroke:#FFFFFF,color:#FFFFFF
    style AUR fill:#1a73e8,stroke:#FFFFFF,color:#FFFFFF
    style DDB fill:#1a73e8,stroke:#FFFFFF,color:#FFFFFF
    style RED fill:#e8710a,stroke:#FFFFFF,color:#FFFFFF
    style EC fill:#0d904f,stroke:#FFFFFF,color:#FFFFFF
    style MDB fill:#0d904f,stroke:#FFFFFF,color:#FFFFFF
```

---

## 5. Servicios de Bases de Datos Especializadas

Las guías de estudio más recientes (Kankaria) destacan servicios adicionales que pueden aparecer como distractores o respuestas específicas:

| Servicio | Tipo | Caso de uso |
|---|---|---|
| **Amazon DocumentDB** | Documentos (compatible MongoDB) | Apps que usan MongoDB |
| **Amazon Neptune** | Grafos | Redes sociales, motores de recomendación |
| **Amazon Timestream** | Series temporales (serverless) | IoT, datos operativos |
| **Amazon QLDB** | Libro mayor inmutable | Auditoría, cadena de suministro verificable |
| **Amazon Keyspaces** | Compatible Apache Cassandra | Migración de cargas Cassandra |

> **Tip de examen:** "Grafos / redes sociales" = **Neptune**. "Compatible MongoDB" = **DocumentDB**. "Inmutable / libro mayor" = **QLDB**. "Series temporales / IoT" = **Timestream**.

### 📊 Diagrama: Mapa de Bases de Datos Especializadas

```mermaid
flowchart TD
    Q["🤔 ¿Qué tipo de datos<br/>especializado tengo?"]

    Q --> D1{"¿Documentos<br/>JSON?"}
    Q --> D2{"¿Relaciones<br/>entre entidades<br/>(grafos)?"}
    Q --> D3{"¿Datos con<br/>marca temporal<br/>(time series)?"}
    Q --> D4{"¿Registro<br/>inmutable y<br/>verificable?"}
    Q --> D5{"¿Migración<br/>desde<br/>Cassandra?"}

    D1 -->|"Compatible<br/>MongoDB"| DOC["✅ DocumentDB"]
    D2 -->|"Redes sociales,<br/>recomendaciones"| NEP["✅ Neptune"]
    D3 -->|"IoT, métricas<br/>operativas"| TS["✅ Timestream"]
    D4 -->|"Auditoría,<br/>cadena de suministro"| QLDB["✅ QLDB"]
    D5 -->|"Apache<br/>Cassandra"| KS["✅ Keyspaces"]

    style Q fill:#FF9900,stroke:#232F3E,color:#232F3E
    style D1 fill:#232F3E,stroke:#FF9900,color:#FFFFFF
    style D2 fill:#232F3E,stroke:#FF9900,color:#FFFFFF
    style D3 fill:#232F3E,stroke:#FF9900,color:#FFFFFF
    style D4 fill:#232F3E,stroke:#FF9900,color:#FFFFFF
    style D5 fill:#232F3E,stroke:#FF9900,color:#FFFFFF
    style DOC fill:#0d904f,stroke:#232F3E,color:#FFFFFF
    style NEP fill:#0d904f,stroke:#232F3E,color:#FFFFFF
    style TS fill:#0d904f,stroke:#232F3E,color:#FFFFFF
    style QLDB fill:#0d904f,stroke:#232F3E,color:#FFFFFF
    style KS fill:#0d904f,stroke:#232F3E,color:#FFFFFF
```

---

## 6. Herramientas de Migración de Bases de Datos

Vinculado al **Dominio 1.3 (Migración)**:

- **AWS Database Migration Service (DMS):** Ayuda a migrar bases de datos a AWS de forma rápida y segura. La base de datos de origen **permanece totalmente operativa** durante la migración, minimizando el tiempo de inactividad.
- **AWS Schema Conversion Tool (SCT):** Se utiliza en **migraciones heterogéneas** (ej. Oracle a Aurora) para convertir el esquema de la base de datos de origen al formato de destino.

> **Tip de examen:** "Migrar base de datos a AWS con mínimo downtime" = **DMS**. "Convertir esquema de un motor a otro (heterogénea)" = **SCT + DMS**.

### 📊 Diagrama: Migración de Bases de Datos con DMS

```mermaid
flowchart LR
    subgraph SOURCE["🏢 Origen (On-Premises o Cloud)"]
        DB1["🗄️ Oracle"]
        DB2["🗄️ SQL Server"]
        DB3["🗄️ MySQL"]
    end

    subgraph MIGRATION["🔄 Herramientas de Migración"]
        SCT["🔧 Schema Conversion Tool<br/>(SCT)<br/>Solo si motor origen ≠ destino<br/>(migración heterogénea)"]
        DMS["📦 Database Migration Service<br/>(DMS)<br/>Migra datos continuamente<br/>Origen operativo durante migración"]
    end

    subgraph TARGET["☁️ Destino en AWS"]
        TRDS["📊 Amazon RDS"]
        TAUR["⚡ Amazon Aurora"]
        TDDB["⚡ DynamoDB"]
    end

    DB1 -->|"Heterogénea<br/>(Oracle → Aurora)"| SCT
    DB2 -->|"Heterogénea"| SCT
    SCT --> DMS
    DB3 -->|"Homogénea<br/>(MySQL → MySQL)"| DMS
    DMS --> TRDS
    DMS --> TAUR
    DMS --> TDDB

    style SOURCE fill:#e8710a,stroke:#232F3E,color:#FFFFFF
    style MIGRATION fill:#232F3E,stroke:#FF9900,color:#FFFFFF
    style TARGET fill:#0d904f,stroke:#232F3E,color:#FFFFFF
    style SCT fill:#1a73e8,stroke:#FFFFFF,color:#FFFFFF
    style DMS fill:#FF9900,stroke:#232F3E,color:#232F3E
```

---

## Resumen para el Candidato

Para aprobar las preguntas sobre bases de datos:

| Escenario en el examen | Respuesta |
|---|---|
| Transaccional / SQL / Relacional | **RDS** o **Aurora** |
| NoSQL / Clave-Valor / Serverless / ms de latencia | **DynamoDB** |
| Analítica / Data Warehouse / SQL complejo | **Redshift** |
| Caché / Rendimiento de lectura en memoria | **ElastiCache** |
| Migración de BBDD con mínimo downtime | **DMS** |
| Conversión de esquema (motor diferente) | **SCT** |
| Grafos / Redes sociales | **Neptune** |
| Compatible con MongoDB | **DocumentDB** |
| Series temporales / IoT | **Timestream** |
| Registro inmutable / Libro mayor | **QLDB** |

### Palabras clave que debes asociar

- **"SQL / Relacional / Transaccional"** → RDS o Aurora
- **"5x MySQL / 3x PostgreSQL"** → Aurora
- **"NoSQL / Clave-valor / Serverless"** → DynamoDB
- **"Caché en memoria / Redis / Memcached"** → ElastiCache
- **"Data Warehouse / Analítica / OLAP / Petabytes"** → Redshift
- **"Migrar base de datos"** → DMS
- **"Convertir esquema"** → SCT
- **"Grafos"** → Neptune
- **"MongoDB"** → DocumentDB
- **"Series temporales"** → Timestream
- **"Inmutable / Libro mayor"** → QLDB
- **"Multi-AZ"** → Alta Disponibilidad (failover)
- **"Read Replicas"** → Rendimiento de lectura (escalado)

---

### 📊 Diagrama: Árbol de Decisión para Preguntas del Examen

```mermaid
flowchart TD
    Q["❓ Pregunta sobre<br/>Bases de Datos"]

    Q --> A{"¿Qué necesita<br/>la aplicación?"}

    A -->|"Base de datos<br/>relacional (SQL)"| B{"¿Qué motor?"}
    A -->|"NoSQL / Clave-valor<br/>/ Serverless"| DDB["✅ DynamoDB"]
    A -->|"Analítica / BI<br/>/ Data Warehouse"| RED["✅ Redshift"]
    A -->|"Caché en memoria<br/>/ Acelerar lecturas"| EC["✅ ElastiCache"]
    A -->|"Migrar base<br/>de datos"| MIG{"¿Mismo motor<br/>o diferente?"}
    A -->|"Datos<br/>especializados"| ESP{"¿Qué tipo?"}

    B -->|"MySQL / PostgreSQL<br/>+ alto rendimiento"| AUR["✅ Aurora"]
    B -->|"Oracle / SQL Server /<br/>MariaDB / estándar"| RDS["✅ RDS"]

    MIG -->|"Mismo motor<br/>(homogénea)"| DMS1["✅ DMS"]
    MIG -->|"Motor diferente<br/>(heterogénea)"| SCTDMS["✅ SCT + DMS"]

    ESP -->|"Grafos /<br/>redes sociales"| NEP["✅ Neptune"]
    ESP -->|"Compatible<br/>MongoDB"| DOC["✅ DocumentDB"]
    ESP -->|"Series temporales<br/>/ IoT"| TS["✅ Timestream"]
    ESP -->|"Registro inmutable<br/>/ auditoría"| QLDB["✅ QLDB"]

    style Q fill:#FF9900,stroke:#232F3E,color:#232F3E
    style A fill:#232F3E,stroke:#FF9900,color:#FFFFFF
    style B fill:#232F3E,stroke:#FF9900,color:#FFFFFF
    style MIG fill:#232F3E,stroke:#FF9900,color:#FFFFFF
    style ESP fill:#232F3E,stroke:#FF9900,color:#FFFFFF
    style DDB fill:#0d904f,stroke:#232F3E,color:#FFFFFF
    style RED fill:#0d904f,stroke:#232F3E,color:#FFFFFF
    style EC fill:#0d904f,stroke:#232F3E,color:#FFFFFF
    style AUR fill:#0d904f,stroke:#232F3E,color:#FFFFFF
    style RDS fill:#0d904f,stroke:#232F3E,color:#FFFFFF
    style DMS1 fill:#0d904f,stroke:#232F3E,color:#FFFFFF
    style SCTDMS fill:#0d904f,stroke:#232F3E,color:#FFFFFF
    style NEP fill:#0d904f,stroke:#232F3E,color:#FFFFFF
    style DOC fill:#0d904f,stroke:#232F3E,color:#FFFFFF
    style TS fill:#0d904f,stroke:#232F3E,color:#FFFFFF
    style QLDB fill:#0d904f,stroke:#232F3E,color:#FFFFFF
```
