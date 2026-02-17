# Infraestructura Global de AWS - Examen CLF-C02

Basado en las tres fuentes proporcionadas (Sequeira, Piper/Clinton y Kankaria), he analizado la Infraestructura Global de AWS.

En el contexto del examen **AWS Certified Cloud Practitioner (CLF-C02)**, este tema es el núcleo del **Dominio 3: Tecnología y Servicios en la Nube**, que representa el **34% de la puntuación total del examen**. Específicamente, aborda la **Declaración de Tarea 3.2: Definir la infraestructura global de AWS**.

A continuación, presento un análisis detallado de los componentes que debe dominar para el examen.

---

## 1. Jerarquía de la Infraestructura Global

Antes de profundizar, es fundamental entender la jerarquía:

```
Regiones (33+)
  └── Zonas de Disponibilidad (105+) (mínimo 3 por región)
        └── Centros de datos (uno o más por AZ)

Edge Locations (600+) → CloudFront, Route 53, Shield, WAF
Local Zones → Extensión de regiones en ciudades metropolitanas
Wavelength Zones → AWS en redes 5G
Outposts → AWS en su centro de datos
```

| Componente | Cantidad aprox. | Función principal |
|---|---|---|
| **Regiones** | 33+ | Ubicaciones geográficas aisladas |
| **Zonas de Disponibilidad (AZ)** | 105+ | Centros de datos redundantes dentro de una región |
| **Edge Locations** | 600+ | Caché de contenido cercano a los usuarios |
| **Local Zones** | 30+ | Baja latencia en ciudades metropolitanas |
| **Wavelength Zones** | 30+ | Latencia ultrabaja en redes 5G |

> **Tip de examen:** Recuerda la jerarquía de mayor a menor: **Regiones > AZ > Centros de datos**. Las Edge Locations están separadas de esta jerarquía.

### 📊 Diagrama: Jerarquía de la Infraestructura Global de AWS

```mermaid
flowchart TD
    subgraph CORE["🌍 Infraestructura Principal"]
        direction TB
        R["🌐 Regiones (33+)<br/>Ubicaciones geográficas aisladas"]
        AZ["🏢 Zonas de Disponibilidad (105+)<br/>Mínimo 3 por región"]
        DC["🖥️ Centros de Datos<br/>Uno o más por AZ"]
        R --> AZ
        AZ --> DC
    end

    subgraph EDGE["📡 Infraestructura de Borde"]
        direction TB
        EL["⚡ Edge Locations (600+)<br/>CloudFront, Route 53, Shield, WAF"]
        REC["🗄️ Regional Edge Cache<br/>Caché intermedio más grande"]
        EL --> REC
    end

    subgraph EXT["🔌 Extensiones"]
        direction TB
        LZ["🏙️ Local Zones<br/>Ciudades metropolitanas"]
        WZ["📱 Wavelength Zones<br/>Redes 5G"]
        OP["🏭 Outposts<br/>Tu centro de datos"]
    end

    style CORE fill:#232F3E,stroke:#FF9900,color:#FFFFFF
    style EDGE fill:#1a73e8,stroke:#FF9900,color:#FFFFFF
    style EXT fill:#0d904f,stroke:#FF9900,color:#FFFFFF
    style R fill:#FF9900,stroke:#232F3E,color:#232F3E
    style AZ fill:#e8710a,stroke:#232F3E,color:#FFFFFF
    style DC fill:#d45b07,stroke:#232F3E,color:#FFFFFF
    style EL fill:#1a73e8,stroke:#FFFFFF,color:#FFFFFF
    style REC fill:#1565c0,stroke:#FFFFFF,color:#FFFFFF
    style LZ fill:#0d904f,stroke:#FFFFFF,color:#FFFFFF
    style WZ fill:#0d904f,stroke:#FFFFFF,color:#FFFFFF
    style OP fill:#0d904f,stroke:#FFFFFF,color:#FFFFFF
```

---

## 2. Regiones (Regions)

Una Región es una **ubicación física geográfica** que contiene múltiples Zonas de Disponibilidad.

### Características clave

- **Aislamiento completo:** Cada región es completamente independiente y está aislada de las demás para garantizar la máxima tolerancia a fallos y estabilidad.
- **Autonomía:** Los datos **no se replican automáticamente** entre regiones (a menos que el cliente lo configure explícitamente).
- **Mínimo 3 AZ:** Cada región tiene como mínimo 3 Zonas de Disponibilidad.

### Los 4 factores de selección de una región

El examen espera que sepa elegir una región basándose en estos criterios:

| Factor | Descripción | Ejemplo |
|---|---|---|
| **1. Cumplimiento (Compliance)** | Mantener datos dentro de fronteras nacionales por requisitos legales | GDPR exige datos en la UE → elegir eu-west-1 (Irlanda) |
| **2. Latencia (Proximity)** | Acercar los recursos a los usuarios finales | Usuarios en Brasil → elegir sa-east-1 (São Paulo) |
| **3. Disponibilidad de servicios** | No todos los servicios están disponibles en todas las regiones | Servicios nuevos se lanzan primero en us-east-1 (N. Virginia) |
| **4. Precios** | Los costos varían entre regiones por impuestos y costos locales | us-east-1 suele ser la más económica |

> **Tip de examen:** El **cumplimiento/soberanía de datos** es siempre el **primer factor** a considerar. Si una regulación exige que los datos estén en un país específico, eso anula cualquier otra consideración.

### 📊 Diagrama: Los 4 Factores para Elegir una Región

```mermaid
flowchart TD
    START["🤔 ¿Qué región elegir?"] --> F1

    F1{"1️⃣ ¿Hay requisitos de<br/>CUMPLIMIENTO o<br/>soberanía de datos?"}
    F1 -->|"Sí"| R1["⚖️ Elegir región que<br/>cumpla la regulación<br/>(ej. GDPR → eu-west-1)"]
    F1 -->|"No"| F2

    F2{"2️⃣ ¿Dónde están<br/>mis USUARIOS?"}
    F2 -->|"Identificados"| R2["📍 Elegir región más<br/>cercana a los usuarios<br/>(ej. Brasil → sa-east-1)"]
    F2 -->|"Globales"| F3

    F3{"3️⃣ ¿El servicio que necesito<br/>está DISPONIBLE<br/>en la región?"}
    F3 -->|"No en todas"| R3["🔍 Elegir región donde<br/>el servicio exista<br/>(nuevos → us-east-1 primero)"]
    F3 -->|"Sí, en varias"| F4

    F4{"4️⃣ ¿Cuál tiene<br/>mejor PRECIO?"}
    F4 --> R4["💰 Elegir la más económica<br/>(us-east-1 suele ser<br/>la más barata)"]

    style START fill:#FF9900,stroke:#232F3E,color:#232F3E
    style F1 fill:#FF4444,stroke:#232F3E,color:#FFFFFF
    style F2 fill:#e8710a,stroke:#232F3E,color:#FFFFFF
    style F3 fill:#1a73e8,stroke:#232F3E,color:#FFFFFF
    style F4 fill:#0d904f,stroke:#232F3E,color:#FFFFFF
    style R1 fill:#FF4444,stroke:#232F3E,color:#FFFFFF
    style R2 fill:#e8710a,stroke:#232F3E,color:#FFFFFF
    style R3 fill:#1a73e8,stroke:#232F3E,color:#FFFFFF
    style R4 fill:#0d904f,stroke:#232F3E,color:#FFFFFF
```

### Servicios Globales (no atados a una región)

El examen a menudo pregunta qué servicios son globales:

| Servicio | Por qué es global |
|---|---|
| **IAM** | Usuarios, roles y políticas aplican a toda la cuenta |
| **Amazon CloudFront** | CDN distribuida globalmente en Edge Locations |
| **Amazon Route 53** | DNS global distribuido |
| **AWS WAF** | Se asocia a CloudFront (global) o ALB (regional) |
| **AWS Shield** | Protección DDoS global |
| **S3** | Namespace global (nombres de bucket únicos), pero datos en una región específica |

> **Tip de examen:** Aunque la **consola de S3** parece global, los **datos se almacenan en una región específica** que usted elige al crear el bucket.

---

## 3. Zonas de Disponibilidad (Availability Zones - AZs)

Este es un concepto **crítico** para la Alta Disponibilidad (HA).

### Características clave

- **Definición:** Una AZ consta de **uno o más centros de datos** discretos con energía, redes y conectividad redundantes.
- **Separación física:** Las AZ dentro de una región están físicamente separadas (diferentes llanuras de inundación, diferentes sistemas eléctricos) para que un desastre local no afecte a más de una a la vez.
- **Interconexión de baja latencia:** Conectadas entre sí mediante enlaces de red de **alta velocidad y baja latencia**, permitiendo la replicación síncrona de datos.
- **Nomenclatura:** Se identifican con letras (ej. us-east-1**a**, us-east-1**b**, us-east-1**c**).
- **Mapeo aleatorio:** AWS mapea las letras de AZ de forma diferente para cada cuenta, para distribuir la carga.

### Alta Disponibilidad con Multi-AZ

| Patrón | Descripción | Servicios que lo implementan |
|---|---|---|
| **Multi-AZ activo-pasivo** | Réplica en standby en otra AZ, failover automático | RDS Multi-AZ |
| **Multi-AZ activo-activo** | Recursos activos en múltiples AZ simultáneamente | ELB + Auto Scaling, DynamoDB |
| **Multi-AZ por diseño** | El servicio distribuye automáticamente en múltiples AZ | S3, DynamoDB, Lambda, ELB |

> **Tip de examen:** **Alta Disponibilidad = Multi-AZ** (dentro de una región). **Recuperación ante desastres (DR) = Multi-Región**. No confundir estos dos conceptos.

### 📊 Diagrama: Patrones de Alta Disponibilidad Multi-AZ

```mermaid
flowchart LR
    subgraph REGION["🌐 Región AWS (ej. us-east-1)"]
        direction LR
        subgraph AZ1["AZ-a"]
            P1["🟢 Primario<br/>EC2 / RDS"]
            S3_1["📦 S3"]
            L1["⚡ Lambda"]
        end
        subgraph AZ2["AZ-b"]
            P2["🟡 Standby<br/>RDS Réplica"]
            S3_2["📦 S3"]
            L2["⚡ Lambda"]
        end
        subgraph AZ3["AZ-c"]
            P3["🔵 Activo<br/>EC2"]
            S3_3["📦 S3"]
            L3["⚡ Lambda"]
        end

        ELB["⚖️ ELB<br/>Distribuye tráfico"]
        ELB --> P1
        ELB --> P3
        P1 -.->|"Failover<br/>automático"| P2
    end

    subgraph PATTERNS["📋 Patrones"]
        PA["🔄 Activo-Pasivo<br/>RDS Multi-AZ"]
        PB["⚡ Activo-Activo<br/>ELB + Auto Scaling"]
        PC["🏗️ Por Diseño<br/>S3, DynamoDB, Lambda"]
    end

    style REGION fill:#232F3E,stroke:#FF9900,color:#FFFFFF
    style AZ1 fill:#0d904f,stroke:#FFFFFF,color:#FFFFFF
    style AZ2 fill:#e8710a,stroke:#FFFFFF,color:#FFFFFF
    style AZ3 fill:#1a73e8,stroke:#FFFFFF,color:#FFFFFF
    style PATTERNS fill:#FF9900,stroke:#232F3E,color:#232F3E
    style ELB fill:#FF9900,stroke:#232F3E,color:#232F3E
    style PA fill:#0d904f,stroke:#232F3E,color:#FFFFFF
    style PB fill:#e8710a,stroke:#232F3E,color:#FFFFFF
    style PC fill:#1a73e8,stroke:#232F3E,color:#FFFFFF
```

---

## 4. Ubicaciones de Borde (Edge Locations)

El examen distingue claramente entre **dónde se ejecutan los servidores** (Regiones/AZ) y **dónde se entrega el contenido** (Edge Locations).

### Características clave

- **Función principal:** Endpoints utilizados por **Amazon CloudFront (CDN)** para almacenar en caché contenido cerca de los usuarios finales y reducir la latencia.
- **Cantidad:** Más de **600** Edge Locations en todo el mundo (superan ampliamente la cantidad de regiones y AZ).
- **No son Zonas de Disponibilidad:** No se ejecutan cargas de trabajo de cómputo general en ellas.

### Servicios que usan Edge Locations

| Servicio | Función en Edge Locations |
|---|---|
| **Amazon CloudFront** | CDN: almacena en caché contenido estático y dinámico |
| **Amazon Route 53** | DNS: resuelve nombres de dominio con baja latencia |
| **AWS Shield** | Protección DDoS en el borde de la red |
| **AWS WAF** | Filtra tráfico malicioso antes de llegar al origen |
| **Lambda@Edge** | Ejecuta funciones Lambda en las Edge Locations |
| **CloudFront Functions** | Funciones ligeras para transformaciones de solicitudes/respuestas |

### Caché Regional de Borde (Regional Edge Cache)

- Nivel intermedio entre las Edge Locations y el servidor de origen.
- Cachés **más grandes** que retienen contenido menos popular por más tiempo.
- Reduce la cantidad de solicitudes que llegan al origen.

> **Tip de examen:** "Reducir latencia para usuarios globales al entregar contenido" = **CloudFront + Edge Locations**. "Ejecutar código cerca de los usuarios" = **Lambda@Edge**.

### 📊 Diagrama: Flujo de Entrega de Contenido con Edge Locations

```mermaid
flowchart LR
    USER["👤 Usuario<br/>en cualquier parte<br/>del mundo"] --> EL

    subgraph EL["📡 Edge Location más cercana"]
        CACHE{"¿Contenido<br/>en caché?"}
        CF["☁️ CloudFront"]
        R53["🔗 Route 53 (DNS)"]
        SHIELD["🛡️ Shield (DDoS)"]
        WAF["🔥 WAF (Filtrado)"]
        LE["⚡ Lambda@Edge"]
    end

    CACHE -->|"✅ HIT"| RESP["⚡ Respuesta<br/>inmediata<br/>(baja latencia)"]
    CACHE -->|"❌ MISS"| REC

    subgraph REC["🗄️ Regional Edge Cache"]
        CACHE2{"¿Contenido<br/>en caché?"}
    end

    CACHE2 -->|"✅ HIT"| RESP2["📦 Respuesta<br/>desde caché regional"]
    CACHE2 -->|"❌ MISS"| ORIGIN

    subgraph ORIGIN["🌐 Región AWS (Origen)"]
        S3["📦 S3"]
        ALB["⚖️ ALB + EC2"]
    end

    style USER fill:#FF9900,stroke:#232F3E,color:#232F3E
    style EL fill:#1a73e8,stroke:#FF9900,color:#FFFFFF
    style REC fill:#e8710a,stroke:#FF9900,color:#FFFFFF
    style ORIGIN fill:#232F3E,stroke:#FF9900,color:#FFFFFF
    style RESP fill:#0d904f,stroke:#232F3E,color:#FFFFFF
    style RESP2 fill:#0d904f,stroke:#232F3E,color:#FFFFFF
```

---

## 5. Extensiones de la Infraestructura

Las guías de estudio actualizadas para el CLF-C02 incluyen componentes de infraestructura híbrida y de borde:

| Servicio | Ubicación | Caso de uso | Latencia |
|---|---|---|---|
| **AWS Local Zones** | Ciudades metropolitanas | Juegos en tiempo real, streaming, renderizado | Un solo dígito de ms |
| **AWS Wavelength** | Redes 5G de telecos | Apps móviles de latencia ultrabaja, IoT | Un solo dígito de ms |
| **AWS Outposts** | Centro de datos del cliente | Experiencia híbrida consistente con AWS | Depende de la red local |
| **AWS Direct Connect** | Punto de interconexión | Conexión privada dedicada entre on-premises y AWS | Consistente y predecible |
| **AWS Global Accelerator** | Red global de AWS | Mejorar rendimiento del tráfico global | Optimizada por la red AWS |

### AWS Local Zones

- Extienden la infraestructura de AWS (cómputo, almacenamiento, bases de datos) a **grandes áreas metropolitanas**.
- Para aplicaciones que requieren latencia de **un solo dígito de milisegundo**.
- Ejemplo: videojuegos en tiempo real, producción de medios, machine learning en tiempo real.

### AWS Wavelength Zones

- Infraestructura de AWS desplegada en el **borde de las redes 5G** de proveedores de telecomunicaciones.
- Para aplicaciones móviles de **latencia ultrabaja** (gaming móvil, realidad aumentada, IoT).

### AWS Outposts

- Lleva los servicios e infraestructura nativos de AWS **al centro de datos del cliente** (on-premises).
- Experiencia híbrida **consistente**: mismas APIs, herramientas y hardware de AWS.
- Ideal cuando las regulaciones exigen que los datos permanezcan en instalaciones locales pero se quiere usar la experiencia de AWS.

### AWS Direct Connect

- Conexión de red **privada y dedicada** entre las instalaciones del cliente y AWS.
- Evita el internet público para mayor **seguridad**, menor latencia y ancho de banda consistente.
- Tiempos de aprovisionamiento largos (semanas/meses).
- Para conexiones rápidas cifradas por internet, usar **AWS VPN** en su lugar.

### AWS Global Accelerator

- Utiliza la **red global de AWS** y direcciones IP estáticas (anycast).
- Mejora la **disponibilidad y rendimiento** del tráfico de usuarios globales hacia las aplicaciones.
- Diferencia con CloudFront: CloudFront cachea **contenido**; Global Accelerator optimiza la **ruta de red** sin caché.

> **Tip de examen:** "AWS en mi datacenter" = **Outposts**. "Conexión privada dedicada" = **Direct Connect**. "Latencia ultrabaja en 5G" = **Wavelength**. "Latencia ultrabaja en una ciudad" = **Local Zones**. "Optimizar ruta de red global" = **Global Accelerator**.

### 📊 Diagrama: Extensiones de Infraestructura - ¿Dónde se ejecuta AWS?

```mermaid
flowchart TD
    AWS["☁️ AWS Cloud<br/>(Regiones + AZs)"]

    AWS -->|"Extiende a<br/>ciudades"| LZ["🏙️ Local Zones<br/>Latencia ~1ms<br/>Gaming, streaming,<br/>renderizado"]
    AWS -->|"Extiende a<br/>redes 5G"| WL["📱 Wavelength<br/>Latencia ultrabaja<br/>Apps móviles, AR/VR,<br/>IoT"]
    AWS -->|"Extiende a<br/>tu datacenter"| OP["🏭 Outposts<br/>Mismas APIs de AWS<br/>Datos on-premises<br/>por regulación"]
    AWS -->|"Conexión<br/>dedicada"| DC["🔌 Direct Connect<br/>Privada, hasta 100 Gbps<br/>Semanas de aprovisionamiento"]
    AWS -->|"Optimiza<br/>ruta de red"| GA["🌍 Global Accelerator<br/>IPs estáticas anycast<br/>Sin caché, optimiza red"]

    subgraph VS["🆚 CloudFront vs Global Accelerator"]
        direction LR
        CFR["☁️ CloudFront<br/>Cachea CONTENIDO<br/>en Edge Locations"]
        GAR["🌍 Global Accelerator<br/>Optimiza RUTA DE RED<br/>sin caché"]
    end

    style AWS fill:#FF9900,stroke:#232F3E,color:#232F3E
    style LZ fill:#0d904f,stroke:#232F3E,color:#FFFFFF
    style WL fill:#1a73e8,stroke:#232F3E,color:#FFFFFF
    style OP fill:#e8710a,stroke:#232F3E,color:#FFFFFF
    style DC fill:#232F3E,stroke:#FF9900,color:#FFFFFF
    style GA fill:#232F3E,stroke:#FF9900,color:#FFFFFF
    style VS fill:#232F3E,stroke:#FF9900,color:#FFFFFF
    style CFR fill:#1a73e8,stroke:#FFFFFF,color:#FFFFFF
    style GAR fill:#0d904f,stroke:#FFFFFF,color:#FFFFFF
```

---

## 6. Conectividad de Red

El examen puede preguntar cómo conectar diferentes entornos:

| Servicio | Tipo de conexión | Velocidad | Seguridad |
|---|---|---|---|
| **Internet Gateway** | VPC a Internet público | Variable | Pública (necesita SG/NACL) |
| **NAT Gateway** | Instancias privadas → Internet (solo salida) | Variable | Solo tráfico de salida |
| **VPC Peering** | VPC a VPC (misma o diferente cuenta/región) | Alta | Privada (red de AWS) |
| **AWS Transit Gateway** | Hub central para conectar múltiples VPCs y on-premises | Alta | Privada |
| **AWS VPN** | On-premises a AWS (cifrada por internet) | Variable | Cifrada (IPsec) |
| **AWS Direct Connect** | On-premises a AWS (dedicada privada) | Hasta 100 Gbps | Privada (no cifrada por defecto) |
| **AWS PrivateLink** | Acceso privado a servicios AWS sin salir de la red | Alta | Privada |

> **Tip de examen:** "Conectar dos VPCs" = **VPC Peering** (2 VPCs) o **Transit Gateway** (muchas VPCs). "Acceso privado a S3 sin internet" = **VPC Gateway Endpoint** o **PrivateLink**.

### 📊 Diagrama: Conectividad de Red - ¿Cómo conecto mis entornos?

```mermaid
flowchart TD
    subgraph ONPREM["🏢 On-Premises"]
        CORP["🖥️ Centro de datos<br/>corporativo"]
    end

    subgraph AWSCLOUD["☁️ AWS Cloud"]
        subgraph VPC1["VPC A"]
            PUB1["🌐 Subnet Pública"]
            PRIV1["🔒 Subnet Privada"]
        end
        subgraph VPC2["VPC B"]
            PUB2["🌐 Subnet Pública"]
            PRIV2["🔒 Subnet Privada"]
        end
        subgraph VPC3["VPC C"]
            PRIV3["🔒 Subnet Privada"]
        end

        IGW["🚪 Internet Gateway<br/>VPC ↔ Internet público"]
        NAT["📤 NAT Gateway<br/>Privada → Internet (solo salida)"]
        TGW["🔀 Transit Gateway<br/>Hub central para muchas VPCs"]
        PL["🔗 PrivateLink<br/>Acceso privado a servicios AWS"]

        IGW --> PUB1
        NAT --> PRIV1
        TGW --> VPC1
        TGW --> VPC2
        TGW --> VPC3
    end

    CORP -->|"🔐 VPN<br/>Cifrada por internet"| TGW
    CORP -->|"🔌 Direct Connect<br/>Privada dedicada"| TGW

    VPC1 <-->|"🤝 VPC Peering<br/>(2 VPCs)"| VPC2

    INTERNET["🌍 Internet"] --> IGW

    style ONPREM fill:#e8710a,stroke:#232F3E,color:#FFFFFF
    style AWSCLOUD fill:#232F3E,stroke:#FF9900,color:#FFFFFF
    style VPC1 fill:#1a73e8,stroke:#FFFFFF,color:#FFFFFF
    style VPC2 fill:#1a73e8,stroke:#FFFFFF,color:#FFFFFF
    style VPC3 fill:#1a73e8,stroke:#FFFFFF,color:#FFFFFF
    style TGW fill:#FF9900,stroke:#232F3E,color:#232F3E
    style IGW fill:#0d904f,stroke:#232F3E,color:#FFFFFF
    style NAT fill:#0d904f,stroke:#232F3E,color:#FFFFFF
    style PL fill:#0d904f,stroke:#232F3E,color:#FFFFFF
    style INTERNET fill:#FFFFFF,stroke:#232F3E,color:#232F3E
```

---

## Resumen para el Candidato

Para aprobar las preguntas sobre Infraestructura Global en el CLF-C02:

| Escenario en el examen | Respuesta |
|---|---|
| Alta disponibilidad dentro de una región | **Multi-AZ** (Zonas de Disponibilidad) |
| Recuperación ante desastres | **Multi-Región** |
| Baja latencia al entregar contenido | **Edge Locations + CloudFront** |
| Latencia ultrabaja para computación en una ciudad | **AWS Local Zones** |
| Latencia ultrabaja en red 5G | **AWS Wavelength** |
| AWS en mi centro de datos (on-premises) | **AWS Outposts** |
| Conexión privada dedicada a AWS | **AWS Direct Connect** |
| Conexión cifrada por internet a AWS | **AWS VPN** |
| Optimizar tráfico global sin caché | **AWS Global Accelerator** |
| Soberanía de datos | Elegir la **Región** correcta |
| Servicio global (no atado a región) | **IAM, CloudFront, Route 53** |
| Conectar muchas VPCs entre sí | **AWS Transit Gateway** |

### Los 4 factores para elegir una región (en orden de prioridad)

1. **Cumplimiento** → ¿La regulación exige una ubicación específica?
2. **Latencia** → ¿Dónde están mis usuarios?
3. **Disponibilidad de servicios** → ¿El servicio que necesito existe en esta región?
4. **Precios** → ¿Cuál es la región más económica que cumple los demás criterios?

### Palabras clave que debes asociar

- **"Alta disponibilidad"** → Multi-AZ
- **"Recuperación ante desastres"** → Multi-Región
- **"Caché de contenido / CDN"** → CloudFront + Edge Locations
- **"Soberanía de datos / GDPR"** → Elegir la Región correcta
- **"Un solo dígito de ms en una ciudad"** → Local Zones
- **"5G / latencia ultrabaja móvil"** → Wavelength
- **"AWS on-premises"** → Outposts
- **"Conexión privada dedicada"** → Direct Connect
- **"Conexión cifrada por internet"** → VPN
- **"Optimizar ruta de red sin caché"** → Global Accelerator
- **"Servicio global"** → IAM, CloudFront, Route 53
- **"Centros de datos aislados dentro de una región"** → Zonas de Disponibilidad

---

### 📊 Diagrama: Árbol de Decisión para Preguntas del Examen

```mermaid
flowchart TD
    Q["❓ Pregunta sobre<br/>Infraestructura Global"]

    Q --> A{"¿El escenario habla de<br/>DISPONIBILIDAD o<br/>RESILIENCIA?"}
    A -->|"Dentro de<br/>una región"| HA["✅ Multi-AZ<br/>(Zonas de Disponibilidad)"]
    A -->|"Ante desastres<br/>regionales"| DR["✅ Multi-Región"]
    A -->|"No"| B

    B{"¿Habla de LATENCIA<br/>o entrega de<br/>contenido?"}
    B -->|"CDN / caché<br/>contenido"| CF["✅ CloudFront<br/>+ Edge Locations"]
    B -->|"Baja latencia<br/>en una ciudad"| LZ["✅ Local Zones"]
    B -->|"Latencia ultrabaja<br/>5G / móvil"| WL["✅ Wavelength"]
    B -->|"Optimizar ruta<br/>de red (sin caché)"| GA["✅ Global Accelerator"]
    B -->|"No"| C

    C{"¿Habla de CONECTIVIDAD<br/>on-premises ↔ AWS?"}
    C -->|"Conexión privada<br/>dedicada"| DX["✅ Direct Connect"]
    C -->|"Conexión cifrada<br/>por internet"| VPN["✅ AWS VPN"]
    C -->|"AWS en mi<br/>datacenter"| OP["✅ Outposts"]
    C -->|"No"| D

    D{"¿Habla de conectar<br/>VPCs entre sí?"}
    D -->|"2 VPCs"| PEER["✅ VPC Peering"]
    D -->|"Muchas VPCs<br/>+ on-premises"| TGW["✅ Transit Gateway"]
    D -->|"Acceso privado<br/>a servicios AWS"| PL["✅ PrivateLink"]
    D -->|"No"| E

    E{"¿Habla de elegir<br/>REGIÓN?"}
    E -->|"Regulación /<br/>soberanía datos"| REG["✅ Cumplimiento<br/>(primer factor)"]
    E -->|"Servicio global<br/>(no atado a región)"| GLOB["✅ IAM, CloudFront,<br/>Route 53"]

    style Q fill:#FF9900,stroke:#232F3E,color:#232F3E
    style A fill:#232F3E,stroke:#FF9900,color:#FFFFFF
    style B fill:#232F3E,stroke:#FF9900,color:#FFFFFF
    style C fill:#232F3E,stroke:#FF9900,color:#FFFFFF
    style D fill:#232F3E,stroke:#FF9900,color:#FFFFFF
    style E fill:#232F3E,stroke:#FF9900,color:#FFFFFF
    style HA fill:#0d904f,stroke:#232F3E,color:#FFFFFF
    style DR fill:#0d904f,stroke:#232F3E,color:#FFFFFF
    style CF fill:#0d904f,stroke:#232F3E,color:#FFFFFF
    style LZ fill:#0d904f,stroke:#232F3E,color:#FFFFFF
    style WL fill:#0d904f,stroke:#232F3E,color:#FFFFFF
    style GA fill:#0d904f,stroke:#232F3E,color:#FFFFFF
    style DX fill:#0d904f,stroke:#232F3E,color:#FFFFFF
    style VPN fill:#0d904f,stroke:#232F3E,color:#FFFFFF
    style OP fill:#0d904f,stroke:#232F3E,color:#FFFFFF
    style PEER fill:#0d904f,stroke:#232F3E,color:#FFFFFF
    style TGW fill:#0d904f,stroke:#232F3E,color:#FFFFFF
    style PL fill:#0d904f,stroke:#232F3E,color:#FFFFFF
    style REG fill:#0d904f,stroke:#232F3E,color:#FFFFFF
    style GLOB fill:#0d904f,stroke:#232F3E,color:#FFFFFF
```
