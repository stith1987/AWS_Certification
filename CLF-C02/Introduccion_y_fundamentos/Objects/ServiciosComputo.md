# Servicios de Cómputo de AWS - Examen CLF-C02

Basado en las tres fuentes proporcionadas (Sequeira, Piper/Clinton y Kankaria), he analizado los Servicios de Cómputo de AWS.

En el contexto del examen **AWS Certified Cloud Practitioner (CLF-C02)**, este tema es el núcleo del **Dominio 3: Tecnología y Servicios en la Nube** (34% del examen), abordando específicamente la **Declaración de Tarea 3.3: Identificar servicios de cómputo de AWS**. Además, tiene un fuerte solapamiento con el **Dominio 4 (Facturación)** debido a los modelos de precios de cómputo.

A continuación, presento un análisis detallado estructurado para el examen:

---

## 1. Amazon Elastic Compute Cloud (EC2) - IaaS

El servicio fundamental de **Infraestructura como Servicio (IaaS)**. El examen evalúa su capacidad para seleccionar la configuración correcta según el caso de uso.

- **Definición:** Proporciona capacidad de cómputo redimensionable (máquinas virtuales) en la nube. Ofrece **control total** a nivel de sistema operativo (root/admin).
- **Imágenes de Máquina de Amazon (AMIs):** Son las "plantillas" preconfiguradas que contienen el sistema operativo y el software necesario para lanzar una instancia. Las fuentes destacan cuatro categorías de AMIs:
  - **Quick Start** (inicio rápido)
  - **Mis AMIs** (personalizadas)
  - **Marketplace** (de proveedores externos)
  - **Comunitarias**
- **Tipos de Instancias (Familias):** Debe memorizar las categorías generales para elegir la instancia adecuada según el escenario:

| Familia | Letras | Caso de uso |
|---|---|---|
| **Propósito General** | T, M | Equilibrio de recursos (servidores web, repositorios de código) |
| **Optimizadas para Cómputo** | C | Alto rendimiento de procesador (procesamiento por lotes, transcodificación) |
| **Optimizadas para Memoria** | R, X | Grandes conjuntos de datos en memoria |
| **Cómputo Acelerado** | P, G | GPU para aprendizaje automático o gráficos |
| **Optimizadas para Almacenamiento** | I, D | Acceso secuencial de lectura/escritura muy alto |

> **Tip de examen:** La letra de la familia es la clave: **C** = Compute (cómputo), **R** = RAM (memoria), **T** = Turbo/general, **P/G** = GPU, **I/D** = I/O disco.

### 📊 Diagrama: Familias de Instancias EC2 - ¿Cuál elegir?

```mermaid
flowchart TD
    Q["🤔 ¿Qué tipo de instancia<br/>EC2 necesito?"]

    Q --> W{"¿Qué recurso es<br/>más importante?"}

    W -->|"Equilibrio<br/>general"| GP["🟢 Propósito General<br/>(T, M)<br/>Servidores web,<br/>repos de código"]
    W -->|"CPU<br/>intensivo"| CO["🔵 Optimizada Cómputo<br/>(C)<br/>Batch, transcodificación,<br/>modelado científico"]
    W -->|"RAM<br/>intensiva"| MEM["🟣 Optimizada Memoria<br/>(R, X)<br/>Bases de datos in-memory,<br/>caché"]
    W -->|"GPU<br/>requerida"| ACC["🟠 Cómputo Acelerado<br/>(P, G)<br/>Machine Learning,<br/>gráficos"]
    W -->|"Disco I/O<br/>intensivo"| STO["🔴 Optimizada Almacenamiento<br/>(I, D)<br/>Data warehousing,<br/>sistemas de archivos"]

    style Q fill:#FF9900,stroke:#232F3E,color:#232F3E
    style W fill:#232F3E,stroke:#FF9900,color:#FFFFFF
    style GP fill:#0d904f,stroke:#232F3E,color:#FFFFFF
    style CO fill:#1a73e8,stroke:#232F3E,color:#FFFFFF
    style MEM fill:#7B1FA2,stroke:#232F3E,color:#FFFFFF
    style ACC fill:#e8710a,stroke:#232F3E,color:#FFFFFF
    style STO fill:#FF4444,stroke:#232F3E,color:#FFFFFF
```

---

## 2. Modelos de Precios de Cómputo

Aunque es parte del **Dominio 4**, es inseparable de EC2. Las fuentes coinciden en cuatro modelos clave que **siempre aparecen en el examen**:

| Modelo | Descuento | Compromiso | Caso de uso |
|---|---|---|---|
| **Bajo Demanda (On-Demand)** | 0% (precio base) | Ninguno | Cargas impredecibles, corto plazo, no interrumpibles |
| **Instancias Reservadas / Savings Plans** | Hasta 72% | 1 o 3 años | Cargas estables y predecibles |
| **Instancias de Spot** | Hasta 90% | Ninguno (pueden ser reclamadas con 2 min de aviso) | Cargas flexibles, sin estado, tolerantes a fallos |
| **Hosts Dedicados** | Variable | Variable | Licencias BYOL, cumplimiento normativo estricto |

> **Tip de examen:** "Más barato posible + puede interrumpirse" = **Spot**. "Uso predecible a largo plazo" = **Reservadas/Savings Plans**. "Sin compromiso + no puede interrumpirse" = **On-Demand**. "Licencias existentes" = **Dedicated Hosts**.

### 📊 Diagrama: Modelos de Precios EC2 - Costo vs Flexibilidad

```mermaid
flowchart LR
    subgraph COST["💰 De MÁS CARO a MÁS BARATO"]
        direction TB
        DH["🏢 Dedicated Hosts<br/>Servidor físico dedicado<br/>Para licencias BYOL<br/>y cumplimiento"]
        OD["💳 On-Demand<br/>Paga por hora/segundo<br/>Sin compromiso<br/>Máxima flexibilidad"]
        RI["📋 Reservadas / Savings Plans<br/>Hasta 72% descuento<br/>Compromiso 1-3 años<br/>Uso predecible"]
        SP["⚡ Spot<br/>Hasta 90% descuento<br/>Puede ser reclamada (2 min)<br/>Solo cargas tolerantes a fallos"]

        DH --> OD --> RI --> SP
    end

    subgraph DECIDE["🎯 ¿Cuál elegir?"]
        Q1{"¿Puede<br/>interrumpirse?"}
        Q1 -->|"Sí"| SPOT["✅ Spot"]
        Q1 -->|"No"| Q2{"¿Uso predecible<br/>a largo plazo?"}
        Q2 -->|"Sí"| RES["✅ Reservadas"]
        Q2 -->|"No"| Q3{"¿Necesita servidor<br/>físico dedicado?"}
        Q3 -->|"Sí (BYOL)"| DED["✅ Dedicated Hosts"]
        Q3 -->|"No"| OND["✅ On-Demand"]
    end

    style COST fill:#232F3E,stroke:#FF9900,color:#FFFFFF
    style DECIDE fill:#232F3E,stroke:#FF9900,color:#FFFFFF
    style DH fill:#FF4444,stroke:#232F3E,color:#FFFFFF
    style OD fill:#e8710a,stroke:#232F3E,color:#FFFFFF
    style RI fill:#1a73e8,stroke:#232F3E,color:#FFFFFF
    style SP fill:#0d904f,stroke:#232F3E,color:#FFFFFF
    style SPOT fill:#0d904f,stroke:#232F3E,color:#FFFFFF
    style RES fill:#1a73e8,stroke:#232F3E,color:#FFFFFF
    style DED fill:#FF4444,stroke:#232F3E,color:#FFFFFF
    style OND fill:#e8710a,stroke:#232F3E,color:#FFFFFF
```

---

## 3. Servicios de Contenedores

El examen distingue entre la **orquestación** (gestión) y el **cómputo** (dónde se ejecutan) de los contenedores.

- **Amazon Elastic Container Service (ECS):** Servicio de orquestación de contenedores altamente escalable que soporta **Docker**. Es la forma **"nativa" de AWS** para ejecutar contenedores.
- **Amazon Elastic Kubernetes Service (EKS):** Servicio gestionado para ejecutar **Kubernetes** en AWS. Elija esto si la pregunta menciona migrar cargas de trabajo de Kubernetes existentes o usar herramientas de código abierto.
- **AWS Fargate:** Es un motor de cómputo **serverless para contenedores**. Funciona tanto con ECS como con EKS. Con Fargate, **no tiene que aprovisionar ni gestionar servidores** (no ve las instancias EC2 subyacentes), solo paga por los recursos que consume el contenedor.

> **Tip de examen:** "Contenedores Docker nativos de AWS" = **ECS**. "Kubernetes" = **EKS**. "Contenedores sin gestionar servidores" = **Fargate**.

### 📊 Diagrama: Ecosistema de Contenedores en AWS

```mermaid
flowchart TD
    subgraph ORQUESTACION["🎼 Capa de Orquestación (¿Quién gestiona los contenedores?)"]
        ECS["🐳 Amazon ECS<br/>Orquestación nativa AWS<br/>Contenedores Docker"]
        EKS["☸️ Amazon EKS<br/>Kubernetes gestionado<br/>Código abierto / migración K8s"]
    end

    subgraph COMPUTO["⚙️ Capa de Cómputo (¿Dónde se ejecutan?)"]
        EC2C["🖥️ EC2<br/>Tú gestionas las instancias<br/>Más control, más responsabilidad"]
        FARG["☁️ Fargate<br/>Serverless: sin servidores que gestionar<br/>Solo defines CPU y RAM"]
    end

    ECS --> EC2C
    ECS --> FARG
    EKS --> EC2C
    EKS --> FARG

    subgraph EXTRAS["📦 Servicios Complementarios"]
        ECR["🗃️ Amazon ECR<br/>Registro de imágenes<br/>de contenedores"]
    end

    ECR -->|"Almacena<br/>imágenes"| ECS
    ECR -->|"Almacena<br/>imágenes"| EKS

    style ORQUESTACION fill:#232F3E,stroke:#FF9900,color:#FFFFFF
    style COMPUTO fill:#232F3E,stroke:#FF9900,color:#FFFFFF
    style EXTRAS fill:#232F3E,stroke:#FF9900,color:#FFFFFF
    style ECS fill:#FF9900,stroke:#232F3E,color:#232F3E
    style EKS fill:#FF9900,stroke:#232F3E,color:#232F3E
    style EC2C fill:#e8710a,stroke:#232F3E,color:#FFFFFF
    style FARG fill:#0d904f,stroke:#232F3E,color:#FFFFFF
    style ECR fill:#1a73e8,stroke:#232F3E,color:#FFFFFF
```

---

## 4. Cómputo Serverless (Sin Servidor)

Un tema **crítico** para la arquitectura moderna en la nube.

- **AWS Lambda:** Ejecuta código **sin aprovisionar ni gestionar servidores**. Se factura por milisegundos de tiempo de cómputo y número de solicitudes. Es **basado en eventos** (se activa por cambios en S3, DynamoDB, etc.).
  - **Límite clave para el examen:** Las funciones tienen un tiempo de espera máximo de **15 minutos**.

> **Tip de examen:** "Ejecutar código sin servidores" o "basado en eventos" = **Lambda**. Si la tarea dura más de 15 minutos, Lambda **no** es la respuesta.

### 📊 Diagrama: AWS Lambda - Arquitectura Basada en Eventos

```mermaid
flowchart LR
    subgraph TRIGGERS["🎯 Eventos que activan Lambda"]
        S3["📦 S3<br/>(archivo subido)"]
        DDB["🗄️ DynamoDB<br/>(cambio en tabla)"]
        API["🌐 API Gateway<br/>(solicitud HTTP)"]
        CW["⏰ CloudWatch Events<br/>(programado/cron)"]
        SQS["📨 SQS<br/>(mensaje en cola)"]
    end

    subgraph LAMBDA["⚡ AWS Lambda"]
        FN["🔧 Tu código<br/>Python, Node.js, Java...<br/>⏱️ Máx 15 minutos<br/>💰 Pago por ms + solicitudes"]
    end

    subgraph DEST["📤 Destinos"]
        S3O["📦 S3"]
        DDBO["🗄️ DynamoDB"]
        SNS["📢 SNS"]
        SQS2["📨 SQS"]
        OTHER["🔗 Otros servicios"]
    end

    S3 --> FN
    DDB --> FN
    API --> FN
    CW --> FN
    SQS --> FN
    FN --> S3O
    FN --> DDBO
    FN --> SNS
    FN --> SQS2
    FN --> OTHER

    style TRIGGERS fill:#232F3E,stroke:#FF9900,color:#FFFFFF
    style LAMBDA fill:#FF9900,stroke:#232F3E,color:#232F3E
    style DEST fill:#232F3E,stroke:#FF9900,color:#FFFFFF
    style FN fill:#FF9900,stroke:#232F3E,color:#232F3E
```

---

## 5. Servicios de Cómputo Gestionados y Especializados

El examen a menudo presenta escenarios buscando la solución con **"menor carga administrativa"**.

- **AWS Elastic Beanstalk (PaaS):** Servicio fácil de usar para desplegar y escalar aplicaciones web. Usted sube el código y Elastic Beanstalk maneja automáticamente el despliegue (aprovisionamiento de capacidad, equilibrio de carga, auto-escalado). Usted **mantiene el control** de la configuración si lo desea.
- **Amazon Lightsail:** Servidores privados virtuales (VPS) simplificados. Incluye todo lo necesario (cómputo, almacenamiento, redes) por un **precio mensual bajo y predecible**. Ideal para sitios web simples o desarrolladores principiantes que no necesitan las funciones avanzadas de EC2.
- **AWS Batch:** Permite ejecutar cargas de trabajo de **computación por lotes** a cualquier escala. Aprovisiona dinámicamente la cantidad y el tipo óptimos de recursos de cómputo (como instancias optimizadas para memoria o CPU).

> **Tip de examen:** "Subir código y que AWS se encargue del resto (PaaS)" = **Elastic Beanstalk**. "VPS simple y barato" = **Lightsail**. "Procesamiento por lotes masivo" = **Batch**.

### 📊 Diagrama: Nivel de Control vs Gestión de AWS

```mermaid
flowchart TD
    subgraph SPECTRUM["📊 Espectro de Servicios: Control del usuario ↔ Gestión de AWS"]
        direction LR

        subgraph MASC["🔧 Más Control (Tú gestionas)"]
            EC2["🖥️ EC2 (IaaS)<br/>SO, red, almacenamiento,<br/>escalado, todo manual"]
        end

        subgraph MEDIO["⚖️ Equilibrio"]
            EB["🌱 Elastic Beanstalk (PaaS)<br/>Subes el código,<br/>AWS gestiona infra<br/>(pero puedes configurar)"]
            BATCH["📊 AWS Batch<br/>Cargas por lotes,<br/>AWS aprovisiona recursos"]
        end

        subgraph MASG["☁️ Más Gestión de AWS (Serverless)"]
            LAMBDA["⚡ Lambda<br/>Solo código,<br/>sin servidores"]
            FARGATE["🐳 Fargate<br/>Solo contenedor,<br/>sin servidores"]
            LS["💡 Lightsail<br/>VPS simplificado,<br/>precio fijo mensual"]
        end
    end

    style SPECTRUM fill:#232F3E,stroke:#FF9900,color:#FFFFFF
    style MASC fill:#FF4444,stroke:#FFFFFF,color:#FFFFFF
    style MEDIO fill:#e8710a,stroke:#FFFFFF,color:#FFFFFF
    style MASG fill:#0d904f,stroke:#FFFFFF,color:#FFFFFF
    style EC2 fill:#FF4444,stroke:#232F3E,color:#FFFFFF
    style EB fill:#e8710a,stroke:#232F3E,color:#FFFFFF
    style BATCH fill:#e8710a,stroke:#232F3E,color:#FFFFFF
    style LAMBDA fill:#0d904f,stroke:#232F3E,color:#FFFFFF
    style FARGATE fill:#0d904f,stroke:#232F3E,color:#FFFFFF
    style LS fill:#0d904f,stroke:#232F3E,color:#FFFFFF
```

---

## 6. Cómputo Híbrido y en el Borde

Las fuentes más recientes (Piper/Clinton y Kankaria) destacan extensiones de la infraestructura que aparecen en la versión CLF-C02 del examen:

- **AWS Outposts:** Lleva la infraestructura y los servicios de AWS a su **centro de datos local (on-premises)** para una experiencia híbrida consistente.
- **AWS Wavelength:** Despliega servicios de cómputo y almacenamiento en el **borde de las redes 5G** de telecomunicaciones para aplicaciones de latencia ultrabaja.
- **AWS Local Zones:** Extiende la infraestructura de AWS a **áreas metropolitanas específicas** para acercar el cómputo a los usuarios finales y reducir la latencia.

> **Tip de examen:** "AWS on-premises" = **Outposts**. "Latencia ultrabaja en 5G" = **Wavelength**. "Cómputo cerca de una ciudad" = **Local Zones**.

### 📊 Diagrama: ¿Dónde ejecutar cómputo en AWS?

```mermaid
flowchart TD
    Q["🤔 ¿Dónde necesito<br/>ejecutar mi cómputo?"]

    Q --> LOC{"¿Ubicación?"}

    LOC -->|"En la nube<br/>(regiones AWS)"| CLOUD
    LOC -->|"En mi propio<br/>datacenter"| OP["🏭 Outposts<br/>AWS on-premises"]
    LOC -->|"Cerca de una<br/>ciudad específica"| LZ["🏙️ Local Zones<br/>Baja latencia metropolitana"]
    LOC -->|"En redes<br/>5G"| WL["📱 Wavelength<br/>Latencia ultrabaja móvil"]

    subgraph CLOUD["☁️ En la Nube AWS"]
        direction TB
        CL1["🖥️ EC2 en Regiones/AZs"]
        CL2["⚡ Lambda (serverless)"]
        CL3["🐳 ECS/EKS + Fargate"]
    end

    style Q fill:#FF9900,stroke:#232F3E,color:#232F3E
    style LOC fill:#232F3E,stroke:#FF9900,color:#FFFFFF
    style CLOUD fill:#1a73e8,stroke:#FF9900,color:#FFFFFF
    style OP fill:#e8710a,stroke:#232F3E,color:#FFFFFF
    style LZ fill:#0d904f,stroke:#232F3E,color:#FFFFFF
    style WL fill:#7B1FA2,stroke:#232F3E,color:#FFFFFF
```

---

## Resumen para el Candidato

Para aprobar las preguntas de Servicios de Cómputo:

| Escenario en el examen | Respuesta |
|---|---|
| IaaS y Control Total | **EC2** |
| Código sin servidores (Eventos / < 15 min) | **Lambda** |
| Fácil despliegue web (PaaS) | **Elastic Beanstalk** |
| VPS simple / Precio fijo | **Lightsail** |
| Contenedores Docker (nativo AWS) | **ECS** |
| Contenedores Kubernetes | **EKS** |
| Contenedores sin gestionar servidores | **Fargate** |
| Procesamiento por lotes masivo | **AWS Batch** |
| AWS en tu datacenter | **Outposts** |
| Latencia ultrabaja en 5G | **Wavelength** |
| Cómputo cerca de una ciudad | **Local Zones** |

### Palabras clave que debes asociar

- **"Control total / SO / IaaS"** → EC2
- **"Sin servidores / basado en eventos"** → Lambda
- **"Subir código / PaaS / despliegue fácil"** → Elastic Beanstalk
- **"VPS simple / precio fijo mensual"** → Lightsail
- **"Docker nativo AWS"** → ECS
- **"Kubernetes"** → EKS
- **"Contenedores sin gestionar infra"** → Fargate
- **"Procesamiento por lotes"** → AWS Batch
- **"Más barato + puede interrumpirse"** → Spot Instances
- **"Uso predecible a largo plazo"** → Reserved Instances / Savings Plans
- **"Licencias existentes (BYOL)"** → Dedicated Hosts

---

### 📊 Diagrama: Árbol de Decisión para Preguntas del Examen

```mermaid
flowchart TD
    Q["❓ Pregunta sobre<br/>Servicios de Cómputo"]

    Q --> A{"¿Qué tipo de<br/>carga de trabajo?"}

    A -->|"Máquinas virtuales<br/>/ control total SO"| EC2["✅ EC2 (IaaS)"]
    A -->|"Contenedores"| B{"¿Qué orquestador?"}
    A -->|"Sin servidores<br/>/ basado en eventos"| C{"¿Duración?"}
    A -->|"Despliegue web<br/>fácil (PaaS)"| EB["✅ Elastic Beanstalk"]
    A -->|"VPS simple<br/>precio fijo"| LS["✅ Lightsail"]
    A -->|"Procesamiento<br/>por lotes"| BATCH["✅ AWS Batch"]

    B -->|"Docker<br/>nativo AWS"| ECS["✅ ECS"]
    B -->|"Kubernetes"| EKS["✅ EKS"]
    B --> D{"¿Gestionar<br/>servidores?"}
    D -->|"No quiero"| FARG["✅ Fargate"]
    D -->|"Sí, necesito<br/>control"| EC2B["✅ EC2"]

    C -->|"< 15 minutos"| LAM["✅ Lambda"]
    C -->|"> 15 minutos"| EC2C["✅ EC2 o Fargate"]

    EC2 --> P{"¿Modelo de<br/>precios?"}
    P -->|"Sin compromiso"| OD["💳 On-Demand"]
    P -->|"Uso predecible<br/>1-3 años"| RI["📋 Reserved /<br/>Savings Plans"]
    P -->|"Puede<br/>interrumpirse"| SPOT["⚡ Spot (90% desc)"]
    P -->|"Licencias BYOL /<br/>cumplimiento"| DH["🏢 Dedicated Hosts"]

    style Q fill:#FF9900,stroke:#232F3E,color:#232F3E
    style A fill:#232F3E,stroke:#FF9900,color:#FFFFFF
    style B fill:#232F3E,stroke:#FF9900,color:#FFFFFF
    style C fill:#232F3E,stroke:#FF9900,color:#FFFFFF
    style D fill:#232F3E,stroke:#FF9900,color:#FFFFFF
    style P fill:#232F3E,stroke:#FF9900,color:#FFFFFF
    style EC2 fill:#0d904f,stroke:#232F3E,color:#FFFFFF
    style ECS fill:#0d904f,stroke:#232F3E,color:#FFFFFF
    style EKS fill:#0d904f,stroke:#232F3E,color:#FFFFFF
    style FARG fill:#0d904f,stroke:#232F3E,color:#FFFFFF
    style EC2B fill:#0d904f,stroke:#232F3E,color:#FFFFFF
    style LAM fill:#0d904f,stroke:#232F3E,color:#FFFFFF
    style EC2C fill:#0d904f,stroke:#232F3E,color:#FFFFFF
    style EB fill:#0d904f,stroke:#232F3E,color:#FFFFFF
    style LS fill:#0d904f,stroke:#232F3E,color:#FFFFFF
    style BATCH fill:#0d904f,stroke:#232F3E,color:#FFFFFF
    style OD fill:#e8710a,stroke:#232F3E,color:#FFFFFF
    style RI fill:#1a73e8,stroke:#232F3E,color:#FFFFFF
    style SPOT fill:#0d904f,stroke:#232F3E,color:#FFFFFF
    style DH fill:#FF4444,stroke:#232F3E,color:#FFFFFF
```
