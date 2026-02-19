# Servicios de Almacenamiento de AWS - Examen CLF-C02

Basado en las tres fuentes proporcionadas (Sequeira, Piper/Clinton y Kankaria), he analizado los Servicios de Almacenamiento de AWS.

En el contexto del examen **AWS Certified Cloud Practitioner (CLF-C02)**, este tema es el núcleo de la **Declaración de Tarea 3.6: Identificar los servicios de almacenamiento de AWS** dentro del **Dominio 3: Tecnología y Servicios en la Nube** (34% del examen). Además, es vital para entender la **Optimización de Costos (Dominio 4)** debido a las diversas clases de almacenamiento y sus precios.

A continuación, presento un análisis detallado estructurado por tipo de almacenamiento, tal como se evalúa en el examen:

---

## 1. Almacenamiento de Objetos: Amazon S3

El examen evalúa profusamente **Amazon Simple Storage Service (S3)**. Debe entender que es un almacenamiento de **objetos** (archivos planos + metadatos) diseñado para una durabilidad del **99.999999999% ("once nueves")**.

### Clases de Almacenamiento (Storage Classes)

Tabla esencial para las preguntas de escenarios de costos:

| Clase | Acceso | AZs | Costo | Caso de uso |
|---|---|---|---|---|
| **S3 Standard** | Frecuente | Multi-AZ | $$$ | Datos activos, sitios web, apps |
| **S3 Standard-IA** | Infrecuente | Multi-AZ | $$ + costo por recuperación | Backups, datos poco accedidos |
| **S3 One Zone-IA** | Infrecuente | 1 AZ | $ (20% menos que IA) | Datos recreables, réplicas |
| **S3 Intelligent-Tiering** | Automático | Multi-AZ | $$ + tarifa monitoreo | Patrones de acceso desconocidos |
| **S3 Glacier Instant Retrieval** | Archivado | Multi-AZ | $ | Archivos accesibles en ms |
| **S3 Glacier Flexible Retrieval** | Archivado | Multi-AZ | ¢ | Archivado, recuperación min-hrs |
| **S3 Glacier Deep Archive** | Archivado largo plazo | Multi-AZ | ¢¢ (más barato) | Retención regulatoria, 7-10 años |

> **Tip de examen:** "Acceso frecuente" = **Standard**. "No sé el patrón de acceso" = **Intelligent-Tiering**. "Archivado a largo plazo, costo más bajo" = **Glacier Deep Archive**. "Infrecuente pero necesito rapidez" = **Standard-IA**.

### Características Clave de S3

- **Versionado:** Protege contra borrados accidentales manteniendo múltiples variantes de un objeto.
- **Ciclos de Vida (Lifecycles):** Reglas automatizadas para mover objetos a clases más baratas o eliminarlos (ej. mover logs a Glacier después de 30 días).
- **Acceso Público:** Por defecto, los buckets S3 son **privados**. Se puede hacer público con Bucket Policies o ACLs, o prevenirlo con **Block Public Access**.
- **Namespace global:** Los nombres de bucket son únicos globalmente, pero los **datos se almacenan en una región específica**.

> **Tip de examen:** "Hosting de sitio web estático" = **S3**. "Durabilidad once nueves" = **S3**. "Mover datos automáticamente a clases más baratas" = **S3 Lifecycle Policies**.

### 📊 Diagrama: Clases de Almacenamiento S3 - Costo vs Acceso

```mermaid
flowchart TD
    subgraph S3CLASSES["📦 Clases de Almacenamiento S3"]
        direction TB

        subgraph FREQ["🔥 Acceso Frecuente"]
            STD["📦 S3 Standard<br/>Datos activos, apps, web<br/>Multi-AZ | $$$$"]
        end

        subgraph INFR["📋 Acceso Infrecuente"]
            IA["📋 Standard-IA<br/>Backups, datos poco accedidos<br/>Multi-AZ | $$ + recuperación"]
            OZ["📋 One Zone-IA<br/>Datos recreables<br/>1 AZ | $ (20% menos)"]
        end

        subgraph AUTO["🤖 Automático"]
            INT["🧠 Intelligent-Tiering<br/>Mueve datos automáticamente<br/>Patrón de acceso desconocido"]
        end

        subgraph ARCH["🧊 Archivado"]
            GIR["🧊 Glacier Instant<br/>Retrieval en ms"]
            GFR["🧊 Glacier Flexible<br/>Retrieval en min-hrs"]
            GDA["🧊 Glacier Deep Archive<br/>Costo más bajo<br/>Retrieval en hrs"]
        end

        STD --> IA --> OZ
        IA --> GIR --> GFR --> GDA
    end

    LC["🔄 Lifecycle Policies<br/>Mueven datos automáticamente<br/>entre clases"] -.-> S3CLASSES

    style S3CLASSES fill:#232F3E,stroke:#FF9900,color:#FFFFFF
    style FREQ fill:#FF4444,stroke:#FFFFFF,color:#FFFFFF
    style INFR fill:#e8710a,stroke:#FFFFFF,color:#FFFFFF
    style AUTO fill:#1a73e8,stroke:#FFFFFF,color:#FFFFFF
    style ARCH fill:#0d904f,stroke:#FFFFFF,color:#FFFFFF
    style LC fill:#FF9900,stroke:#232F3E,color:#232F3E
```

---

## 2. Almacenamiento en Bloque: Amazon EBS

El examen distingue claramente entre almacenamiento de **objetos (S3)** y almacenamiento en **bloque (EBS)**. **Elastic Block Store (EBS)** actúa como un **disco duro virtual** para las instancias EC2.

- **Persistencia:** A diferencia del **Instance Store** (que es efímero y se borra al detener la instancia), los volúmenes EBS son **persistentes** y se pueden desconectar de una instancia y conectar a otra.
- **Alcance:** Un volumen EBS reside en **una sola AZ** (igual que la instancia a la que se conecta).
- **Snapshots (Instantáneas):** Copias de seguridad puntuales que se almacenan en S3. Son **incrementales** (solo guardan los cambios desde la última copia).

### Tipos de Volúmenes EBS

| Tipo | Identificador | Uso | Caso de uso |
|---|---|---|---|
| **General Purpose SSD** | gp2/gp3 | Equilibrio precio-rendimiento | Boot volumes, apps generales |
| **Provisioned IOPS SSD** | io1/io2 | Alto rendimiento, baja latencia | Bases de datos críticas |
| **Throughput Optimized HDD** | st1 | Alto throughput secuencial | Big data, data warehouses |
| **Cold HDD** | sc1 | Menor costo HDD | Datos accedidos raramente |

> **Tip de examen:** "Disco duro para EC2" = **EBS**. "Almacenamiento efímero/temporal" = **Instance Store**. "Backup de volumen EBS" = **Snapshot**. "Base de datos de alta IOPS" = **io1/io2**.

### 📊 Diagrama: EBS vs Instance Store

```mermaid
flowchart LR
    subgraph EBS_SIDE["💾 Amazon EBS (Persistente)"]
        EBS["💾 Volumen EBS<br/>Persiste al detener EC2<br/>Se puede desconectar<br/>Snapshots incrementales"]
        SNAP["📸 Snapshots<br/>Backup en S3<br/>Incrementales"]
        EBS --> SNAP
    end

    subgraph IS_SIDE["⚡ Instance Store (Efímero)"]
        IS["⚡ Instance Store<br/>Se BORRA al detener EC2<br/>Mejor rendimiento I/O<br/>Solo para datos temporales"]
    end

    EC2["🖥️ EC2"]
    EC2 --> EBS
    EC2 --> IS

    subgraph TIPOS["📊 Tipos de Volúmenes EBS"]
        direction TB
        GP["🟢 gp2/gp3<br/>SSD General"]
        IO["🔵 io1/io2<br/>SSD Alto IOPS"]
        ST["🟠 st1<br/>HDD Throughput"]
        SC["🔴 sc1<br/>HDD Cold"]
    end

    style EBS_SIDE fill:#0d904f,stroke:#FF9900,color:#FFFFFF
    style IS_SIDE fill:#FF4444,stroke:#FF9900,color:#FFFFFF
    style TIPOS fill:#232F3E,stroke:#FF9900,color:#FFFFFF
    style EC2 fill:#FF9900,stroke:#232F3E,color:#232F3E
    style SNAP fill:#1a73e8,stroke:#232F3E,color:#FFFFFF
```

---

## 3. Almacenamiento de Archivos: Amazon EFS y FSx

Debe saber cuándo elegir un **sistema de archivos compartido** en lugar de un volumen de bloque.

- **Amazon Elastic File System (EFS):** Sistema de archivos totalmente gestionado, escalable y elástico para cargas de trabajo **Linux**. Permite que **cientos de instancias EC2 accedan al mismo sistema de archivos simultáneamente**.
- **Amazon FSx:** La respuesta correcta si el examen menciona **"Windows"** o **"High Performance Computing (HPC)"**.
  - **FSx for Windows File Server:** Sistema de archivos nativo de Windows (**SMB**).
  - **FSx for Lustre:** Para computación de **alto rendimiento (HPC)**.

> **Tip de examen:** "Sistema de archivos compartido Linux" = **EFS**. "Sistema de archivos compartido Windows (SMB)" = **FSx for Windows**. "HPC / alto rendimiento" = **FSx for Lustre**.

### 📊 Diagrama: S3 vs EBS vs EFS - Los 3 Tipos de Almacenamiento

```mermaid
flowchart TD
    Q["🤔 ¿Qué tipo de<br/>almacenamiento necesito?"]

    Q --> T{"¿Cómo accedo<br/>a los datos?"}

    T -->|"Archivos planos /<br/>objetos vía HTTP"| S3["📦 Amazon S3<br/>Almacenamiento de OBJETOS<br/>Ilimitado, once nueves<br/>Web estática, backups, data lakes"]

    T -->|"Disco duro para<br/>una instancia EC2"| EBS["💾 Amazon EBS<br/>Almacenamiento en BLOQUE<br/>1 AZ, persistente<br/>Boot volumes, bases de datos"]

    T -->|"Carpeta compartida<br/>para varias instancias"| FS{"¿Qué SO?"}

    FS -->|"Linux<br/>(NFS)"| EFS["📂 Amazon EFS<br/>Sistema de ARCHIVOS<br/>Multi-AZ, elástico<br/>Cientos de EC2 simultáneas"]

    FS -->|"Windows<br/>(SMB)"| FSXW["📂 FSx for Windows<br/>Nativo Windows<br/>Active Directory"]

    FS -->|"HPC / Alto<br/>rendimiento"| FSXL["📂 FSx for Lustre<br/>Computación de alto<br/>rendimiento"]

    style Q fill:#FF9900,stroke:#232F3E,color:#232F3E
    style T fill:#232F3E,stroke:#FF9900,color:#FFFFFF
    style FS fill:#232F3E,stroke:#FF9900,color:#FFFFFF
    style S3 fill:#0d904f,stroke:#232F3E,color:#FFFFFF
    style EBS fill:#1a73e8,stroke:#232F3E,color:#FFFFFF
    style EFS fill:#e8710a,stroke:#232F3E,color:#FFFFFF
    style FSXW fill:#e8710a,stroke:#232F3E,color:#FFFFFF
    style FSXL fill:#e8710a,stroke:#232F3E,color:#FFFFFF
```

---

## 4. Almacenamiento Híbrido y Migración de Datos

El examen presenta escenarios sobre cómo **mover datos desde un centro de datos local (on-premises) hacia AWS**.

### AWS Storage Gateway

Conecta aplicaciones locales con el almacenamiento en la nube **sin cambiar el código**. Tres tipos:

| Tipo | Protocolo | Respaldo | Caso de uso |
|---|---|---|---|
| **File Gateway** | NFS/SMB | S3 | Carpeta compartida local respaldada en S3 |
| **Volume Gateway** | iSCSI | S3 | Almacenamiento en bloque respaldado en S3 |
| **Tape Gateway** | VTL | S3 Glacier | Reemplazo de bibliotecas de cintas físicas |

### Familia AWS Snow

Para **transferencia de datos física** cuando la red es lenta o insuficiente:

| Dispositivo | Capacidad | Cómputo local | Caso de uso |
|---|---|---|---|
| **Snowcone** | 8-14 TB | Limitado | Entornos remotos, portátil |
| **Snowball Edge** | 80 TB - PB | Sí (EC2, Lambda) | Migraciones masivas, edge computing |
| **Snowmobile** | Hasta 100 PB | No | Exabytes de datos (camión contenedor) |

> **Tip de examen:** "Conectar on-premises con S3 sin cambiar código" = **Storage Gateway**. "Reemplazo de cintas" = **Tape Gateway**. "Mover TB/PB físicamente" = **Snowball**. "Exabytes" = **Snowmobile**.

### 📊 Diagrama: Migración de Datos - ¿Cómo muevo mis datos a AWS?

```mermaid
flowchart TD
    Q["🤔 ¿Cómo muevo datos<br/>a AWS?"]

    Q --> VOL{"¿Cuántos datos?"}

    VOL -->|"Pocos datos /<br/>conexión rápida"| NET["🌐 Por Internet<br/>(S3 Upload, CLI, SDK)"]
    VOL -->|"Necesito conexión<br/>híbrida continua"| SGW["🔗 Storage Gateway<br/>File / Volume / Tape"]
    VOL -->|"TB de datos /<br/>internet lento"| SNOW1["📦 Snowcone (8-14 TB)<br/>o Snowball Edge (80 TB)"]
    VOL -->|"PB de datos"| SNOW2["📦 Snowball Edge<br/>(múltiples dispositivos)"]
    VOL -->|"Exabytes"| SNOW3["🚛 Snowmobile<br/>(camión contenedor<br/>hasta 100 PB)"]

    subgraph SGW_TYPES["🔗 Tipos de Storage Gateway"]
        direction LR
        FG["📂 File Gateway<br/>NFS/SMB → S3"]
        VG["💾 Volume Gateway<br/>iSCSI → S3"]
        TG["📼 Tape Gateway<br/>VTL → Glacier"]
    end

    style Q fill:#FF9900,stroke:#232F3E,color:#232F3E
    style VOL fill:#232F3E,stroke:#FF9900,color:#FFFFFF
    style NET fill:#0d904f,stroke:#232F3E,color:#FFFFFF
    style SGW fill:#1a73e8,stroke:#232F3E,color:#FFFFFF
    style SNOW1 fill:#e8710a,stroke:#232F3E,color:#FFFFFF
    style SNOW2 fill:#e8710a,stroke:#232F3E,color:#FFFFFF
    style SNOW3 fill:#FF4444,stroke:#232F3E,color:#FFFFFF
    style SGW_TYPES fill:#232F3E,stroke:#FF9900,color:#FFFFFF
    style FG fill:#1a73e8,stroke:#FFFFFF,color:#FFFFFF
    style VG fill:#1a73e8,stroke:#FFFFFF,color:#FFFFFF
    style TG fill:#1a73e8,stroke:#FFFFFF,color:#FFFFFF
```

---

## 5. Servicios de Backup

- **AWS Backup:** Servicio **centralizado** para automatizar y gestionar copias de seguridad en múltiples servicios de AWS (EBS, RDS, DynamoDB, EFS, etc.). Permite crear políticas de backup consistentes desde un solo lugar.

> **Tip de examen:** "Backup centralizado de múltiples servicios AWS" = **AWS Backup**.

---

## Resumen para el Candidato

Para aprobar las preguntas de almacenamiento en el CLF-C02:

| Escenario en el examen | Respuesta |
|---|---|
| Almacenamiento de objetos / Archivos planos / Web estática | **S3** |
| Archivado a largo plazo / Costo más bajo | **S3 Glacier / Deep Archive** |
| Patrón de acceso desconocido | **S3 Intelligent-Tiering** |
| Disco duro para EC2 / Base de datos en EC2 | **EBS** |
| Almacenamiento temporal de alto rendimiento | **Instance Store** |
| Sistema de archivos compartido para Linux | **EFS** |
| Sistema de archivos compartido para Windows (SMB) | **FSx for Windows** |
| HPC / Alto rendimiento | **FSx for Lustre** |
| Conectar on-premises con S3 | **Storage Gateway** |
| Reemplazo de cintas físicas | **Tape Gateway** |
| Mover TB/PB de datos físicamente | **Snowball Edge** |
| Exabytes de datos | **Snowmobile** |
| Backup centralizado multi-servicio | **AWS Backup** |

### Palabras clave que debes asociar

- **"Objetos / archivos planos / once nueves"** → S3
- **"Acceso frecuente"** → S3 Standard
- **"No sé el patrón de acceso"** → S3 Intelligent-Tiering
- **"Archivado / retención regulatoria"** → Glacier Deep Archive
- **"Disco para EC2 / persistente"** → EBS
- **"Temporal / efímero / se borra al detener"** → Instance Store
- **"Compartido Linux / NFS"** → EFS
- **"Compartido Windows / SMB"** → FSx for Windows
- **"HPC / Lustre"** → FSx for Lustre
- **"Hybrid / on-premises ↔ S3"** → Storage Gateway
- **"Mover datos físicamente"** → Snow Family
- **"Backup centralizado"** → AWS Backup
- **"Lifecycle / mover a clase más barata"** → S3 Lifecycle Policies

---

### 📊 Diagrama: Árbol de Decisión para Preguntas del Examen

```mermaid
flowchart TD
    Q["❓ Pregunta sobre<br/>Almacenamiento"]

    Q --> A{"¿Qué tipo de<br/>almacenamiento?"}

    A -->|"Objetos / archivos /<br/>web estática"| S3{"¿Patrón de<br/>acceso?"}
    A -->|"Disco para EC2"| EBS{"¿Persistente?"}
    A -->|"Sistema de archivos<br/>compartido"| FS{"¿Qué SO?"}
    A -->|"Migración de datos<br/>a AWS"| MIG{"¿Cuántos datos?"}
    A -->|"Backup<br/>centralizado"| BK["✅ AWS Backup"]

    S3 -->|"Frecuente"| STD["✅ S3 Standard"]
    S3 -->|"Infrecuente"| IA["✅ S3 Standard-IA"]
    S3 -->|"Desconocido"| INTEL["✅ S3 Intelligent-Tiering"]
    S3 -->|"Archivado<br/>largo plazo"| GLAC["✅ S3 Glacier /<br/>Deep Archive"]

    EBS -->|"Sí, persistente"| EBSV["✅ EBS"]
    EBS -->|"No, temporal /<br/>efímero"| IS["✅ Instance Store"]

    FS -->|"Linux (NFS)"| EFS["✅ EFS"]
    FS -->|"Windows (SMB)"| FSXW["✅ FSx for Windows"]
    FS -->|"HPC"| FSXL["✅ FSx for Lustre"]

    MIG -->|"Conexión híbrida<br/>continua"| SGW["✅ Storage Gateway"]
    MIG -->|"TB/PB<br/>físicamente"| SNOW["✅ Snowball Edge"]
    MIG -->|"Exabytes"| SM["✅ Snowmobile"]

    style Q fill:#FF9900,stroke:#232F3E,color:#232F3E
    style A fill:#232F3E,stroke:#FF9900,color:#FFFFFF
    style S3 fill:#232F3E,stroke:#FF9900,color:#FFFFFF
    style EBS fill:#232F3E,stroke:#FF9900,color:#FFFFFF
    style FS fill:#232F3E,stroke:#FF9900,color:#FFFFFF
    style MIG fill:#232F3E,stroke:#FF9900,color:#FFFFFF
    style STD fill:#0d904f,stroke:#232F3E,color:#FFFFFF
    style IA fill:#0d904f,stroke:#232F3E,color:#FFFFFF
    style INTEL fill:#0d904f,stroke:#232F3E,color:#FFFFFF
    style GLAC fill:#0d904f,stroke:#232F3E,color:#FFFFFF
    style EBSV fill:#0d904f,stroke:#232F3E,color:#FFFFFF
    style IS fill:#0d904f,stroke:#232F3E,color:#FFFFFF
    style EFS fill:#0d904f,stroke:#232F3E,color:#FFFFFF
    style FSXW fill:#0d904f,stroke:#232F3E,color:#FFFFFF
    style FSXL fill:#0d904f,stroke:#232F3E,color:#FFFFFF
    style SGW fill:#0d904f,stroke:#232F3E,color:#FFFFFF
    style SNOW fill:#0d904f,stroke:#232F3E,color:#FFFFFF
    style SM fill:#0d904f,stroke:#232F3E,color:#FFFFFF
    style BK fill:#0d904f,stroke:#232F3E,color:#FFFFFF
```
