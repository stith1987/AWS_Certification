# Estrategias de Migración a AWS - Examen CLF-C02

Basado en las tres fuentes proporcionadas (Sequeira, Piper/Clinton y Kankaria), he analizado las Estrategias de Migración a AWS.

En el contexto del examen **AWS Certified Cloud Practitioner (CLF-C02)**, este tema es fundamental y se evalúa principalmente en:

- **Dominio 1: Conceptos de la Nube** — Declaración de Tarea 1.3: Comprender los beneficios y estrategias de migración a AWS Cloud.
- **Dominio 3: Tecnología y Servicios en la Nube** — Identificación de servicios de migración y transferencia de datos.

A continuación, presento un análisis detallado estructurado según los objetivos del examen.

---

## 1. Marco Estratégico: AWS Cloud Adoption Framework (CAF)

Para el examen, es crucial entender que la migración no es solo tecnológica, sino también organizacional. Las fuentes destacan el **AWS Cloud Adoption Framework (CAF)** como la guía teórica principal.

- **Objetivo del CAF:** Proporciona orientación detallada para facilitar la transición a la nube, reduciendo el riesgo empresarial y mejorando la eficiencia operativa.

### Las 6 Perspectivas del CAF

El examen a menudo pregunta qué perspectiva aborda un problema específico. Se dividen en dos grupos:

#### Capacidades de Negocio

| Perspectiva | Enfoque | Stakeholders |
|---|---|---|
| **Negocio (Business)** | Que la migración cumpla objetivos empresariales y financieros | CEO, CFO |
| **Personas (People)** | Cultura, estructura organizacional y habilidades del personal | RRHH, CTO |
| **Gobernanza (Governance)** | Minimizar riesgos y asegurar cumplimiento de normativas | CIO, CTO |

#### Capacidades Técnicas

| Perspectiva | Enfoque | Stakeholders |
|---|---|---|
| **Plataforma (Platform)** | Arquitectura y tecnología (autoscaling, almacenamiento) | CTO, Arquitectos |
| **Seguridad (Security)** | Confidencialidad, integridad y disponibilidad | CISO |
| **Operaciones (Operations)** | Que los servicios satisfagan las necesidades del negocio día a día | SysOps, DevOps |

> **Tip de examen:** Si la pregunta menciona "RRHH", "capacitación" o "cultura organizacional", la respuesta es la perspectiva de **Personas**. Si menciona "cumplimiento" o "riesgo", es **Gobernanza**.

### Las 6 perspectivas del CAF

```mermaid
flowchart TD
    CAF["🏗️ AWS Cloud Adoption\nFramework (CAF)"] --> BIZ["📊 Capacidades\nde Negocio"]
    CAF --> TECH["⚙️ Capacidades\nTécnicas"]

    BIZ --> B1["💼 Negocio\nCEO, CFO\nObjetivos empresariales"]
    BIZ --> B2["👥 Personas\nRRHH, CTO\nCultura y habilidades"]
    BIZ --> B3["📋 Gobernanza\nCIO, CTO\nRiesgos y cumplimiento"]

    TECH --> T1["🖥️ Plataforma\nArquitectos\nAutoscaling, almacenamiento"]
    TECH --> T2["🔒 Seguridad\nCISO\nCIA: Confidencialidad,\nIntegridad, Disponibilidad"]
    TECH --> T3["🔧 Operaciones\nSysOps, DevOps\nServicios día a día"]

    style CAF fill:#FF9900,color:#fff,stroke:#FF9900
    style BIZ fill:#1a73e8,color:#fff
    style TECH fill:#0d904f,color:#fff
    style B1 fill:#232F3E,color:#fff
    style B2 fill:#232F3E,color:#fff
    style B3 fill:#232F3E,color:#fff
    style T1 fill:#232F3E,color:#fff
    style T2 fill:#232F3E,color:#fff
    style T3 fill:#232F3E,color:#fff
```

### El Viaje de Transformación

Sequeira describe cuatro fases del viaje de transformación en la nube:

1. **Visión** — Identificar oportunidades y crear un caso de negocio.
2. **Alineación** — Identificar brechas en las 6 perspectivas del CAF.
3. **Lanzamiento** — Implementar iniciativas piloto en producción.
4. **Escala** — Expandir las iniciativas a toda la organización.

```mermaid
flowchart LR
    V["1️⃣ Visión\nCaso de negocio\nOportunidades"] --> AL["2️⃣ Alineación\nIdentificar brechas\nen las 6 perspectivas"]
    AL --> L["3️⃣ Lanzamiento\nPilotos en\nproducción"]
    L --> E["4️⃣ Escala\nExpandir a toda\nla organización"]

    style V fill:#FF9900,color:#fff
    style AL fill:#e8710a,color:#fff
    style L fill:#1a73e8,color:#fff
    style E fill:#0d904f,color:#fff
```

---

## 2. Las 7 Estrategias de Migración (7 R's)

Las estrategias de migración, conocidas como las **7 R's**, definen cómo se mueve cada aplicación a la nube. El examen espera que identifiques la estrategia adecuada según el escenario:

| Estrategia | Descripción | Ejemplo |
|---|---|---|
| **Rehost (Lift & Shift)** | Mover la aplicación tal cual a la nube sin cambios | Migrar una VM on-premises a EC2 |
| **Replatform (Lift, Tinker & Shift)** | Hacer optimizaciones mínimas sin cambiar la arquitectura central | Migrar una BD a RDS sin cambiar el código |
| **Refactor / Re-architect** | Rediseñar la aplicación para aprovechar capacidades nativas de la nube | Convertir un monolito a microservicios con Lambda y API Gateway |
| **Repurchase** | Reemplazar con un producto diferente, típicamente SaaS | Migrar CRM on-premises a Salesforce |
| **Retire** | Descomisionar aplicaciones que ya no son necesarias | Eliminar aplicaciones redundantes |
| **Retain** | Mantener la aplicación on-premises (no migrar aún) | Aplicaciones con dependencias complejas que requieren más análisis |
| **Relocate** | Mover infraestructura a la nube sin comprar nuevo hardware | Migrar VMware on-premises a VMware Cloud on AWS |

> **Tip de examen:** "Lift and Shift" = **Rehost** (la más rápida, sin cambios). "Rediseñar para la nube" = **Refactor** (la más compleja, mayor beneficio a largo plazo).

### Espectro de las 7 R's: Esfuerzo vs Beneficio

```mermaid
flowchart LR
    subgraph NO["🚫 No migrar"]
        direction TB
        RT["Retire\n🗑️ Eliminar"] ~~~ RN["Retain\n🏠 Mantener\non-premises"]
    end

    subgraph MIGRAR["✅ Migrar a AWS"]
        direction LR
        RL["Relocate\n📦 VMware\na VMware\non AWS"] --> RH["Rehost\n🏗️ Lift &\nShift\n(Sin cambios)"]
        RH --> RP["Replatform\n🔧 Lift,\nTinker &\nShift\n(Optimizar)"]
        RP --> RF["Refactor\n🏛️ Re-architect\n(Rediseñar\nnativo nube)"]
    end

    subgraph REEMPLAZAR["🔄 Reemplazar"]
        RPU["Repurchase\n🛒 Comprar\nSaaS"]
    end

    NO --> MIGRAR --> REEMPLAZAR

    RL -.->|"⬅️ Menor esfuerzo"| RF
    RL -.->|"Mayor beneficio ➡️"| RF

    style NO fill:#FF4444,color:#fff
    style MIGRAR fill:#1a73e8,color:#fff
    style REEMPLAZAR fill:#0d904f,color:#fff
```

---

## 3. Herramientas de Migración de Bases de Datos

El examen pone un fuerte énfasis en cómo mover bases de datos, ya sea manteniendo el mismo motor o cambiándolo.

### AWS Database Migration Service (DMS)

Servicio totalmente gestionado que permite migrar bases de datos de forma segura y con un tiempo de inactividad mínimo.

- Soporta migraciones **homogéneas** (ej. Oracle a Oracle) y **heterogéneas** (ej. Oracle a Aurora).
- Mantiene la base de datos de origen **operativa durante la migración**.
- Soporta migración continua (replicación de datos en curso).

### AWS Schema Conversion Tool (SCT)

Herramienta complementaria a DMS para migraciones **heterogéneas**.

- Analiza la base de datos de origen y convierte el esquema para que sea compatible con el motor de destino.
- Ejemplo: convertir PL/SQL de Oracle a Amazon Aurora (PostgreSQL/MySQL).
- Identifica qué partes del esquema no pueden convertirse automáticamente y requieren intervención manual.

> **Tip de examen:** DMS = migrar los **datos**. SCT = convertir el **esquema/estructura**. Para migraciones heterogéneas, se usan ambos juntos.

### Flujo de migración de bases de datos

```mermaid
flowchart TD
    subgraph HOMO["Migración Homogénea (mismo motor)"]
        direction LR
        H1["🗄️ Oracle\n(On-Premises)"] -->|"DMS"| H2["🗄️ Oracle\n(RDS)"]
    end

    subgraph HETERO["Migración Heterogénea (distinto motor)"]
        direction LR
        E1["🗄️ Oracle\n(On-Premises)"] -->|"1️⃣ SCT\nConvertir esquema"| E2["📋 Esquema\nconvertido"] -->|"2️⃣ DMS\nMigrar datos"| E3["🗄️ Aurora\nPostgreSQL (RDS)"]
    end

    style HOMO fill:#0d904f,color:#fff
    style HETERO fill:#e8710a,color:#fff
```

---

## 4. Migración y Transferencia de Datos (Almacenamiento)

El examen evalúa la capacidad de elegir la herramienta adecuada según el volumen de datos y la conectividad disponible.

### Familia AWS Snow (Transferencia Física)

Ideal para mover grandes volúmenes de datos (terabytes a petabytes) cuando la transferencia por red es demasiado lenta o costosa.

| Dispositivo | Capacidad | Caso de uso |
|---|---|---|
| **Snowcone** | Hasta 8 TB (HDD) / 14 TB (SSD) | Dispositivo portátil y robusto para entornos con espacio limitado |
| **Snowball Edge** | Hasta 80 TB | Migración de datos masivos + cómputo local (EC2, Lambda) |
| **Snowmobile** | Hasta 100 PB (exabytes) | Contenedor de 45 pies para migraciones a escala masiva |

> **Tip de examen:** Si la pregunta menciona "sin conexión a internet", "ancho de banda limitado" o "petabytes de datos", piensa en la **familia Snow**.

### Decisión: Transferencia en línea vs física

```mermaid
flowchart TD
    Q["❓ ¿Cómo transferir\ndatos a AWS?"] --> SIZE{"¿Cuántos datos?"}

    SIZE -->|"GBs - TBs\nBuen ancho de banda"| ONLINE["🌐 Transferencia en Línea"]
    SIZE -->|"TBs - PBs\nAncho de banda limitado"| OFFLINE["📦 Transferencia Física\n(Snow Family)"]

    ONLINE --> DS["AWS DataSync\n🚀 Rápido, automatizado"]
    ONLINE --> SG["Storage Gateway\n🔗 Híbrido, NFS/SMB"]
    ONLINE --> TF["Transfer Family\n📁 SFTP/FTPS"]

    OFFLINE --> SC["Snowcone\n📱 8-14 TB\nPortátil"]
    OFFLINE --> SB["Snowball Edge\n📦 80 TB\n+ Cómputo local"]
    OFFLINE --> SM["Snowmobile\n🚛 100 PB\nContenedor"]

    style Q fill:#FF9900,color:#fff
    style ONLINE fill:#1a73e8,color:#fff
    style OFFLINE fill:#232F3E,color:#fff
    style SC fill:#0d904f,color:#fff
    style SB fill:#0d904f,color:#fff
    style SM fill:#0d904f,color:#fff
```

### Transferencia en Línea

| Servicio | Descripción |
|---|---|
| **AWS Storage Gateway** | Puente híbrido que conecta aplicaciones locales con almacenamiento en la nube (S3, Glacier, EBS) usando protocolos estándar (NFS, SMB, iSCSI) |
| **AWS Transfer Family** | Transferencia de archivos hacia/desde S3 usando SFTP, FTPS y FTP |
| **AWS DataSync** | Acelera la transferencia de datos en línea entre almacenamiento local y AWS (hasta 10x más rápido que herramientas de código abierto) |

> **Tip de examen:** "Almacenamiento híbrido" o "acceso local a datos en la nube" = **Storage Gateway**. "Transferir archivos con SFTP" = **Transfer Family**.

---

## 5. Planificación y Seguimiento de la Migración

Para gestionar migraciones complejas, las fuentes identifican herramientas de gestión específicas:

| Servicio | Función |
|---|---|
| **AWS Application Discovery Service** | Descubre y recopila información sobre aplicaciones e infraestructura locales (servidores, dependencias, rendimiento) |
| **AWS Migration Hub** | Panel centralizado para rastrear el progreso de migraciones a través de múltiples herramientas de AWS y socios |
| **AWS Application Migration Service (MGN)** | Automatiza la migración lift-and-shift de servidores a AWS (sucesor de CloudEndure) |
| **Migration Evaluator** | Crea un caso de negocio para la migración estimando el TCO en AWS |

> **Tip de examen:** "Descubrir servidores on-premises" = **Application Discovery Service**. "Rastrear progreso de migración" = **Migration Hub**.

### Flujo completo de migración

```mermaid
flowchart LR
    subgraph PLAN["1️⃣ Planificar"]
        direction TB
        P1["Migration Evaluator\n💰 Caso de negocio\n(TCO)"]
        P2["Application Discovery\n🔍 Descubrir servidores\ny dependencias"]
    end

    subgraph MIGR["2️⃣ Migrar"]
        direction TB
        M1["MGN\n🏗️ Lift & Shift\n(servidores)"]
        M2["DMS + SCT\n🗄️ Bases de datos"]
        M3["Snow Family\n📦 Datos masivos"]
    end

    subgraph TRACK["3️⃣ Rastrear"]
        direction TB
        T1["Migration Hub\n📊 Panel centralizado\nde progreso"]
    end

    PLAN --> MIGR --> TRACK

    style PLAN fill:#FF9900,color:#fff
    style MIGR fill:#1a73e8,color:#fff
    style TRACK fill:#0d904f,color:#fff
```

---

## Resumen para el Candidato

Para aprobar las secciones relacionadas con la migración en el examen CLF-C02, debe ser capaz de:

| Escenario | Servicio / Concepto |
|---|---|
| Objetivos no técnicos (RRHH, procesos comerciales) | Perspectivas del **CAF** (Personas, Negocio) |
| Ancho de banda limitado y datos masivos | **AWS Snow Family** (Snowball, Snowcone, Snowmobile) |
| Migración de bases de datos con mínima interrupción | **AWS DMS** (+ SCT si es heterogénea) |
| Almacenamiento híbrido y acceso local a datos en la nube | **AWS Storage Gateway** |
| Mover aplicación sin cambios a la nube | **Rehost** (Lift & Shift) |
| Rediseñar para aprovechar servicios nativos | **Refactor / Re-architect** |
| Descubrir infraestructura on-premises | **Application Discovery Service** |
| Seguimiento centralizado de migración | **Migration Hub** |

### Palabras clave que debes asociar

- **"Lift and Shift"** → Rehost, sin cambios, la más rápida
- **"Sin internet / petabytes"** → Snow Family
- **"Base de datos con distinto motor"** → DMS + SCT
- **"Híbrido / NFS / SMB"** → Storage Gateway
- **"Cultura / capacitación / RRHH"** → CAF - Perspectiva de Personas
- **"Riesgo / cumplimiento"** → CAF - Perspectiva de Gobernanza
- **"Descubrir servidores"** → Application Discovery Service

### Árbol de decisión para preguntas del examen

```mermaid
flowchart TD
    Q["❓ Pregunta sobre\nMigración a AWS"] --> K1{"¿Habla de cultura,\nRRHH o capacitación?"}
    Q --> K2{"¿Habla de mover app\nsin cambios?"}
    Q --> K3{"¿Habla de rediseñar\npara la nube?"}
    Q --> K4{"¿Habla de migrar\nbases de datos?"}
    Q --> K5{"¿Habla de datos masivos\no sin internet?"}
    Q --> K6{"¿Habla de descubrir\nservidores o rastrear?"}

    K1 -->|Sí| A1["👥 CAF - Personas\nGobernanza, Negocio"]
    K2 -->|Sí| A2["🏗️ Rehost\nLift & Shift\nMGN"]
    K3 -->|Sí| A3["🏛️ Refactor\nMicroservicios\nLambda, containers"]
    K4 -->|Sí| A4["🗄️ DMS + SCT\nHomogénea o\nheterogénea"]
    K5 -->|Sí| A5["📦 Snow Family\nSnowcone, Snowball\nSnowmobile"]
    K6 -->|Sí| A6["🔍 Discovery Service\n📊 Migration Hub"]

    style Q fill:#FF9900,color:#fff
    style A1 fill:#232F3E,color:#fff
    style A2 fill:#232F3E,color:#fff
    style A3 fill:#232F3E,color:#fff
    style A4 fill:#232F3E,color:#fff
    style A5 fill:#232F3E,color:#fff
    style A6 fill:#232F3E,color:#fff
```
