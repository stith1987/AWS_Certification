# Servicios de Red de AWS - Examen CLF-C02

Basado en las tres fuentes proporcionadas (Sequeira, Piper/Clinton y Kankaria), he analizado los Servicios de Red de AWS.

En el contexto del examen **AWS Certified Cloud Practitioner (CLF-C02)**, este tema es fundamental para el **Dominio 3: Tecnología y Servicios en la Nube**, específicamente la **Declaración de Tarea 3.5: Identificar servicios de red de AWS**.

A continuación, presento un análisis detallado estructurado para el examen:

---

## 1. Amazon Virtual Private Cloud (VPC)

El VPC es el **componente fundamental** de la red en AWS. El examen evalúa si entiende que es una red virtual lógicamente aislada donde usted define su propio espacio de direcciones IP.

- **Alcance:** Un VPC existe dentro de una **Región** de AWS, pero puede abarcar **todas las Zonas de Disponibilidad (AZ)** dentro de esa región.
- **Bloques CIDR:** Definen el rango de direcciones IP (ej. 10.0.0.0/16). Este rango es personalizado por el usuario.
- **Subredes (Subnets):** Dividen el VPC. Debe saber que una subred reside en **una sola Zona de Disponibilidad**.
  - **Subred Pública:** Tiene una ruta directa a un **Internet Gateway (IGW)**, permitiendo tráfico hacia y desde Internet.
  - **Subred Privada:** No tiene acceso directo a Internet. Para que las instancias en una subred privada accedan a Internet (por ejemplo, para actualizaciones de software) sin ser accesibles desde fuera, se utiliza un **NAT Gateway**.

> **Tip de examen:** "Red virtual aislada en AWS" = **VPC**. "Subred en una sola AZ" es una regla clave. Las subredes públicas tienen IGW; las privadas usan NAT Gateway para salida.

### 📊 Diagrama: Arquitectura de un VPC

```mermaid
flowchart TD
    subgraph REGION["🌐 Región AWS"]
        subgraph VPC["🏗️ VPC (ej. 10.0.0.0/16)"]
            IGW["🚪 Internet Gateway"]

            subgraph AZ1["AZ-a"]
                PUB1["🌐 Subred Pública<br/>10.0.1.0/24<br/>Servidores web, ALB"]
                PRIV1["🔒 Subred Privada<br/>10.0.3.0/24<br/>BD, backend"]
            end

            subgraph AZ2["AZ-b"]
                PUB2["🌐 Subred Pública<br/>10.0.2.0/24"]
                PRIV2["🔒 Subred Privada<br/>10.0.4.0/24"]
            end

            NAT["📤 NAT Gateway<br/>(en subred pública)"]

            IGW <--> PUB1
            IGW <--> PUB2
            PUB1 --> NAT
            NAT -.->|"Solo salida<br/>(actualizaciones)"| PRIV1
            NAT -.->|"Solo salida"| PRIV2
        end
    end

    INTERNET["🌍 Internet"] <--> IGW

    style REGION fill:#232F3E,stroke:#FF9900,color:#FFFFFF
    style VPC fill:#1a73e8,stroke:#FF9900,color:#FFFFFF
    style AZ1 fill:#0d904f,stroke:#FFFFFF,color:#FFFFFF
    style AZ2 fill:#e8710a,stroke:#FFFFFF,color:#FFFFFF
    style IGW fill:#FF9900,stroke:#232F3E,color:#232F3E
    style NAT fill:#FF9900,stroke:#232F3E,color:#232F3E
    style INTERNET fill:#FFFFFF,stroke:#232F3E,color:#232F3E
```

---

## 2. Seguridad de Red: La Comparación Clave

Uno de los temas **más frecuentes** en el examen es la diferencia entre Grupos de Seguridad y NACLs. Las tres fuentes enfatizan esta distinción crítica:

### Grupos de Seguridad (Security Groups)

- Actúan a nivel de **Instancia** (EC2).
- Son **Con Estado (Stateful):** Si permite una solicitud entrante (inbound), la respuesta saliente (outbound) se permite automáticamente, independientemente de las reglas de salida.
- Por defecto, **bloquean todo el tráfico entrante** y permiten todo el saliente.
- Solo soportan reglas de **Permitir (Allow)**.

### Listas de Control de Acceso de Red (Network ACLs - NACLs)

- Actúan a nivel de **Subred**.
- Son **Sin Estado (Stateless):** Debe crear reglas explícitas tanto para el tráfico entrante como para el saliente; la respuesta **no** se permite automáticamente.
- Procesan reglas en **orden numérico** y soportan reglas de **"Denegar" (Deny)**, a diferencia de los Security Groups que solo permiten.
- Por defecto, **permiten todo** el tráfico entrante y saliente.

> **Tip de examen:** **Security Group** = nivel de instancia + stateful + solo Allow. **NACL** = nivel de subred + stateless + Allow y Deny.

### 📊 Diagrama: Security Groups vs NACLs

```mermaid
flowchart TD
    subgraph COMPARISON["🆚 Security Groups vs NACLs"]
        direction LR

        subgraph SG["🛡️ Security Groups"]
            SG1["📍 Nivel: INSTANCIA"]
            SG2["🔄 Stateful<br/>(respuesta automática)"]
            SG3["✅ Solo reglas ALLOW"]
            SG4["🚫 Default: bloquea<br/>todo entrante"]
        end

        subgraph NACL["🧱 NACLs"]
            N1["📍 Nivel: SUBRED"]
            N2["➡️ Stateless<br/>(reglas explícitas ida y vuelta)"]
            N3["✅❌ Reglas ALLOW y DENY"]
            N4["✅ Default: permite<br/>todo"]
        end
    end

    subgraph FLOW["📦 Flujo del tráfico"]
        direction LR
        INET["🌍 Internet"] --> NACL_F["🧱 NACL<br/>(filtra en subred)"]
        NACL_F --> SG_F["🛡️ Security Group<br/>(filtra en instancia)"]
        SG_F --> EC2["🖥️ EC2"]
    end

    style COMPARISON fill:#232F3E,stroke:#FF9900,color:#FFFFFF
    style SG fill:#0d904f,stroke:#FFFFFF,color:#FFFFFF
    style NACL fill:#e8710a,stroke:#FFFFFF,color:#FFFFFF
    style FLOW fill:#232F3E,stroke:#FF9900,color:#FFFFFF
    style NACL_F fill:#e8710a,stroke:#232F3E,color:#FFFFFF
    style SG_F fill:#0d904f,stroke:#232F3E,color:#FFFFFF
    style EC2 fill:#FF9900,stroke:#232F3E,color:#232F3E
```

---

## 3. Conectividad Híbrida y entre VPCs

El examen presenta escenarios sobre cómo conectar su infraestructura local con AWS o cómo conectar VPCs entre sí.

- **AWS Direct Connect:** Proporciona una conexión de red **dedicada y física** desde sus instalaciones a AWS. No utiliza el internet público, ofreciendo mayor seguridad, velocidad y consistencia que una VPN.
- **AWS Site-to-Site VPN:** Conecta su red local a su VPC a través del **internet público** mediante un túnel encriptado (IPsec). Es más rápido de configurar y más barato que Direct Connect, pero depende de la calidad de su internet.
- **VPC Peering:** Conecta **dos VPCs** entre sí, permitiendo que se comporten como una sola red utilizando direcciones IP privadas. Funciona incluso entre **diferentes cuentas y regiones**.
- **AWS Transit Gateway:** Actúa como un **"hub" central** para conectar múltiples VPCs y redes locales, simplificando la topología de red (topología hub-and-spoke) en lugar de tener muchas conexiones punto a punto complejas.

> **Tip de examen:** "Conexión privada dedicada (sin internet)" = **Direct Connect**. "Conexión cifrada por internet" = **VPN**. "Conectar 2 VPCs" = **VPC Peering**. "Conectar muchas VPCs + on-premises" = **Transit Gateway**.

### 📊 Diagrama: Conectividad Híbrida - ¿Cómo me conecto a AWS?

```mermaid
flowchart TD
    Q["🤔 ¿Cómo conecto mi<br/>infraestructura a AWS?"]

    Q --> T{"¿Qué tipo de<br/>conexión necesito?"}

    T -->|"On-premises → AWS<br/>PRIVADA y dedicada"| DX["🔌 Direct Connect<br/>Conexión física dedicada<br/>Alta velocidad, consistente<br/>⏱️ Semanas de aprovisionamiento"]
    T -->|"On-premises → AWS<br/>CIFRADA por internet"| VPN["🔐 Site-to-Site VPN<br/>Túnel IPsec por internet<br/>Rápido de configurar<br/>💰 Más barato"]
    T -->|"VPC ↔ VPC<br/>(solo 2)"| PEER["🤝 VPC Peering<br/>Conexión directa<br/>Entre cuentas/regiones<br/>IP privadas"]
    T -->|"Muchas VPCs<br/>+ on-premises"| TGW["🔀 Transit Gateway<br/>Hub central<br/>Topología hub-and-spoke<br/>Simplifica la red"]

    subgraph VS["🆚 Direct Connect vs VPN"]
        direction LR
        DXC["🔌 Direct Connect<br/>✅ Privada, consistente<br/>❌ Semanas de setup<br/>❌ Más cara"]
        VPNC["🔐 VPN<br/>✅ Rápida, barata<br/>✅ Cifrada<br/>❌ Depende de internet"]
    end

    style Q fill:#FF9900,stroke:#232F3E,color:#232F3E
    style T fill:#232F3E,stroke:#FF9900,color:#FFFFFF
    style DX fill:#1a73e8,stroke:#232F3E,color:#FFFFFF
    style VPN fill:#0d904f,stroke:#232F3E,color:#FFFFFF
    style PEER fill:#e8710a,stroke:#232F3E,color:#FFFFFF
    style TGW fill:#FF9900,stroke:#232F3E,color:#232F3E
    style VS fill:#232F3E,stroke:#FF9900,color:#FFFFFF
    style DXC fill:#1a73e8,stroke:#FFFFFF,color:#FFFFFF
    style VPNC fill:#0d904f,stroke:#FFFFFF,color:#FFFFFF
```

---

## 4. Entrega de Contenido y Optimización

Debe distinguir entre **acelerar contenido estático** y **optimizar el tráfico de red global**.

- **Amazon CloudFront:** Es una **red de entrega de contenido (CDN)**. Almacena en caché contenido (videos, imágenes, APIs) en **Ubicaciones de Borde (Edge Locations)** repartidas por todo el mundo para reducir la latencia de los usuarios finales.
- **AWS Global Accelerator:** Mejora la disponibilidad y el rendimiento de las aplicaciones para usuarios globales utilizando la **red interna de AWS** y direcciones IP estáticas (anycast). A diferencia de CloudFront (que cachea contenido), Global Accelerator **optimiza la ruta de red** hacia sus aplicaciones (TCP/UDP).

> **Tip de examen:** "CDN / caché de contenido / baja latencia al entregar archivos" = **CloudFront**. "Optimizar ruta de red sin caché / IPs estáticas anycast" = **Global Accelerator**.

### 📊 Diagrama: CloudFront vs Global Accelerator

```mermaid
flowchart LR
    USER["👤 Usuario<br/>Global"]

    USER --> CF_PATH
    USER --> GA_PATH

    subgraph CF_PATH["☁️ Amazon CloudFront (CDN)"]
        direction TB
        EL["📡 Edge Location<br/>(600+ en el mundo)"]
        CACHE{"¿En caché?"}
        EL --> CACHE
        CACHE -->|"✅ HIT"| RESP1["⚡ Respuesta<br/>inmediata"]
        CACHE -->|"❌ MISS"| ORIGIN1["🌐 Origen<br/>(S3, ALB)"]
    end

    subgraph GA_PATH["🌍 Global Accelerator"]
        direction TB
        ANYCAST["📍 IPs estáticas<br/>anycast"]
        AWSNET["🔗 Red privada<br/>de AWS"]
        ANYCAST --> AWSNET
        AWSNET --> APP["🖥️ Aplicación<br/>(en cualquier región)"]
    end

    subgraph DIFF["🆚 Diferencia Clave"]
        D1["☁️ CloudFront<br/>CACHEA contenido<br/>Estático + dinámico"]
        D2["🌍 Global Accelerator<br/>OPTIMIZA ruta de red<br/>Sin caché (TCP/UDP)"]
    end

    style CF_PATH fill:#1a73e8,stroke:#FF9900,color:#FFFFFF
    style GA_PATH fill:#0d904f,stroke:#FF9900,color:#FFFFFF
    style DIFF fill:#232F3E,stroke:#FF9900,color:#FFFFFF
    style USER fill:#FF9900,stroke:#232F3E,color:#232F3E
    style D1 fill:#1a73e8,stroke:#FFFFFF,color:#FFFFFF
    style D2 fill:#0d904f,stroke:#FFFFFF,color:#FFFFFF
```

---

## 5. Sistema de Nombres de Dominio (DNS)

### Amazon Route 53

- Es el servicio de **DNS escalable** de AWS.
- **Funciones clave:**
  - **Registro de dominios:** Comprar y gestionar nombres de dominio.
  - **Enrutamiento de tráfico (DNS routing):** Traducir nombres de dominio a direcciones IP.
  - **Comprobación de estado (Health Checks):** Monitorear la salud de los recursos y redirigir si fallan.
- **Políticas de Enrutamiento** que suelen aparecer en el examen:

| Política | Descripción | Caso de uso |
|---|---|---|
| **Simple** | Un registro, un recurso | Sitio web básico |
| **Failover** | Redirige a recurso secundario si el primario falla | Alta disponibilidad |
| **Latencia** | Envía al usuario a la región más rápida | Usuarios globales |
| **Geolocalización** | Basado en la ubicación geográfica del usuario | Contenido regional, regulaciones |
| **Weighted (Ponderado)** | Distribuye tráfico según porcentajes asignados | Testing A/B, migraciones graduales |
| **Multivalue** | Múltiples recursos, con health checks | Balanceo de carga básico |

> **Tip de examen:** "DNS de AWS" = **Route 53**. "Enrutar por latencia/geolocalización" = Route 53 con la **política de enrutamiento** correspondiente. Route 53 es un servicio **global**.

### 📊 Diagrama: Políticas de Enrutamiento de Route 53

```mermaid
flowchart TD
    USER["👤 Usuario solicita<br/>www.ejemplo.com"]
    USER --> R53["🔗 Amazon Route 53<br/>(DNS Global)"]

    R53 --> POL{"¿Qué política<br/>de enrutamiento?"}

    POL -->|"Simple"| SIM["📍 Un solo recurso<br/>(IP fija)"]
    POL -->|"Failover"| FAIL["🔄 Primario → Secundario<br/>Si health check falla,<br/>redirige automáticamente"]
    POL -->|"Latencia"| LAT["⚡ Región más rápida<br/>Mide latencia al usuario<br/>y elige la mejor"]
    POL -->|"Geolocalización"| GEO["🌍 Por ubicación<br/>del usuario<br/>(país, continente)"]
    POL -->|"Weighted"| WGT["⚖️ Por porcentaje<br/>70% → v1, 30% → v2<br/>(testing A/B)"]

    style USER fill:#FF9900,stroke:#232F3E,color:#232F3E
    style R53 fill:#FF9900,stroke:#232F3E,color:#232F3E
    style POL fill:#232F3E,stroke:#FF9900,color:#FFFFFF
    style SIM fill:#0d904f,stroke:#232F3E,color:#FFFFFF
    style FAIL fill:#FF4444,stroke:#232F3E,color:#FFFFFF
    style LAT fill:#1a73e8,stroke:#232F3E,color:#FFFFFF
    style GEO fill:#e8710a,stroke:#232F3E,color:#FFFFFF
    style WGT fill:#7B1FA2,stroke:#232F3E,color:#FFFFFF
```

---

## Resumen para el Candidato

Para las preguntas de Servicios de Red en el examen CLF-C02:

| Escenario en el examen | Respuesta |
|---|---|
| Red virtual aislada en AWS | **VPC** |
| Nivel de instancia + Stateful | **Security Group** |
| Nivel de subred + Stateless | **NACL** |
| Acceso a internet desde subred pública | **Internet Gateway** |
| Acceso a internet desde subred privada (solo salida) | **NAT Gateway** |
| Conexión privada física (sin internet) | **Direct Connect** |
| Conexión cifrada por internet | **Site-to-Site VPN** |
| Conectar 2 VPCs | **VPC Peering** |
| Conectar múltiples VPCs y VPNs centralizadamente | **Transit Gateway** |
| CDN / Caché en el borde | **CloudFront** |
| Optimizar ruta de red global (sin caché) | **Global Accelerator** |
| DNS / Enrutamiento por latencia o geo | **Route 53** |

### Palabras clave que debes asociar

- **"Red virtual aislada"** → VPC
- **"Stateful + nivel instancia"** → Security Group
- **"Stateless + nivel subred + Deny"** → NACL
- **"Subred pública ↔ Internet"** → Internet Gateway
- **"Subred privada → Internet (solo salida)"** → NAT Gateway
- **"Conexión dedicada física"** → Direct Connect
- **"Túnel cifrado por internet"** → VPN
- **"Conectar 2 VPCs"** → VPC Peering
- **"Hub central para muchas VPCs"** → Transit Gateway
- **"CDN / caché / Edge Locations"** → CloudFront
- **"Optimizar ruta sin caché / IPs anycast"** → Global Accelerator
- **"DNS / registro de dominios"** → Route 53

---

### 📊 Diagrama: Árbol de Decisión para Preguntas del Examen

```mermaid
flowchart TD
    Q["❓ Pregunta sobre<br/>Servicios de Red"]

    Q --> A{"¿Sobre qué aspecto<br/>de la red pregunta?"}

    A -->|"Seguridad /<br/>filtrado de tráfico"| B{"¿A qué nivel?"}
    A -->|"Conectividad<br/>on-premises ↔ AWS"| C{"¿Tipo de<br/>conexión?"}
    A -->|"Conectar VPCs<br/>entre sí"| D{"¿Cuántas VPCs?"}
    A -->|"Entrega de contenido<br/>/ rendimiento global"| E{"¿Cachea<br/>contenido?"}
    A -->|"DNS / Dominios"| R53["✅ Route 53"]
    A -->|"Red virtual<br/>aislada"| VPC["✅ VPC"]

    B -->|"Instancia<br/>(Stateful)"| SG["✅ Security Group"]
    B -->|"Subred<br/>(Stateless)"| NACL["✅ NACL"]

    C -->|"Privada dedicada<br/>(sin internet)"| DX["✅ Direct Connect"]
    C -->|"Cifrada por<br/>internet"| VPN["✅ Site-to-Site VPN"]

    D -->|"Solo 2 VPCs"| PEER["✅ VPC Peering"]
    D -->|"Muchas VPCs<br/>+ on-premises"| TGW["✅ Transit Gateway"]

    E -->|"Sí, CDN /<br/>Edge Locations"| CF["✅ CloudFront"]
    E -->|"No, optimiza<br/>ruta de red"| GA["✅ Global Accelerator"]

    VPC --> F{"¿Acceso a<br/>Internet?"}
    F -->|"Subred pública<br/>↔ Internet"| IGW["✅ Internet Gateway"]
    F -->|"Subred privada<br/>→ Internet (salida)"| NATGW["✅ NAT Gateway"]

    style Q fill:#FF9900,stroke:#232F3E,color:#232F3E
    style A fill:#232F3E,stroke:#FF9900,color:#FFFFFF
    style B fill:#232F3E,stroke:#FF9900,color:#FFFFFF
    style C fill:#232F3E,stroke:#FF9900,color:#FFFFFF
    style D fill:#232F3E,stroke:#FF9900,color:#FFFFFF
    style E fill:#232F3E,stroke:#FF9900,color:#FFFFFF
    style F fill:#232F3E,stroke:#FF9900,color:#FFFFFF
    style SG fill:#0d904f,stroke:#232F3E,color:#FFFFFF
    style NACL fill:#0d904f,stroke:#232F3E,color:#FFFFFF
    style DX fill:#0d904f,stroke:#232F3E,color:#FFFFFF
    style VPN fill:#0d904f,stroke:#232F3E,color:#FFFFFF
    style PEER fill:#0d904f,stroke:#232F3E,color:#FFFFFF
    style TGW fill:#0d904f,stroke:#232F3E,color:#FFFFFF
    style CF fill:#0d904f,stroke:#232F3E,color:#FFFFFF
    style GA fill:#0d904f,stroke:#232F3E,color:#FFFFFF
    style R53 fill:#0d904f,stroke:#232F3E,color:#FFFFFF
    style VPC fill:#0d904f,stroke:#232F3E,color:#FFFFFF
    style IGW fill:#0d904f,stroke:#232F3E,color:#FFFFFF
    style NATGW fill:#0d904f,stroke:#232F3E,color:#FFFFFF
```
