# Modelo de Responsabilidad Compartida de AWS - Examen CLF-C02

Basado en las tres fuentes proporcionadas (Sequeira, Piper/Clinton y Kankaria), he analizado el Modelo de Responsabilidad Compartida de AWS.

En el contexto del examen **AWS Certified Cloud Practitioner (CLF-C02)**, este es posiblemente el concepto más importante del **Dominio 2: Seguridad y Cumplimiento**, que representa el **30% de la puntuación total del examen**. Específicamente, aborda la **Declaración de Tarea 2.1: Comprender el modelo de responsabilidad compartida de AWS**.

A continuación, presento un análisis detallado estructurado para maximizar su preparación para el examen.

---

## 1. La Definición Fundamental: "DE" vs. "EN" la Nube

El examen evalúa constantemente si puede distinguir entre estas dos preposiciones. Las tres fuentes coinciden en la definición central que debe memorizar:

| Responsable | Alcance | Qué incluye |
|---|---|---|
| **AWS** | Seguridad **"DE"** la Nube (Security OF the Cloud) | Hardware, software, redes e instalaciones físicas |
| **Cliente** | Seguridad **"EN"** la Nube (Security IN the Cloud) | Datos, configuración de aplicaciones, controles de acceso |

> **Tip de examen:** Esta distinción "DE" vs. "EN" es la regla de oro. Si la puedes aplicar correctamente, responderás la mayoría de las preguntas de responsabilidad compartida.

### Modelo visual: "DE" vs "EN" la nube

```mermaid
flowchart TB
    subgraph CLIENTE["🔵 CLIENTE: Seguridad EN la nube"]
        direction TB
        C1["📊 Datos del cliente"]
        C2["🔐 Cifrado (reposo + tránsito)"]
        C3["👤 IAM: usuarios, roles, MFA"]
        C4["🛡️ Security Groups + NACLs"]
        C5["💻 Parcheo del SO (en EC2)"]
        C6["⚙️ Configuración de aplicaciones"]
    end

    subgraph AWS["🟠 AWS: Seguridad DE la nube"]
        direction TB
        A1["🖥️ Hardware: servidores, storage, red"]
        A2["🏗️ Infraestructura global: Regiones, AZ, Edge"]
        A3["🔧 Software de virtualización (hipervisor)"]
        A4["🏢 Seguridad física de centros de datos"]
        A5["🗑️ Destrucción segura de hardware"]
    end

    CLIENTE ~~~ AWS

    style CLIENTE fill:#1a73e8,color:#fff
    style AWS fill:#FF9900,color:#fff
```

---

## 2. Responsabilidades de AWS (Lo que hereda el cliente)

Para el examen, debe identificar qué tareas **nunca** son responsabilidad del cliente:

- **Seguridad física:** Protección de los centros de datos, control de acceso físico, vigilancia, suministro eléctrico y refrigeración.
- **Infraestructura global:** Mantenimiento de Regiones, Zonas de Disponibilidad y Ubicaciones de Borde (Edge Locations).
- **Hardware de base:** Servidores físicos, dispositivos de red, almacenamiento físico.
- **Software de virtualización:** Mantenimiento y parcheo del hipervisor (la capa que permite ejecutar máquinas virtuales).
- **Infraestructura de red:** Cables, switches, balanceadores de carga internos y conectividad entre AZ.
- **Disposición de hardware:** Descomisión y destrucción segura de discos y equipos al final de su vida útil.

> **Tip de examen:** Si la pregunta menciona "acceso físico", "hardware", "hipervisor", "destrucción de discos" o "centro de datos", la respuesta es **siempre AWS**.

---

## 3. Responsabilidades del Cliente (Lo que usted configura)

El examen presentará escenarios donde algo sale mal (ej. una filtración de datos) y preguntará quién tuvo la culpa. El cliente **siempre** es responsable de:

- **Datos del cliente:** La encriptación (en reposo y en tránsito) y la integridad de los datos.
- **Gestión de Identidad y Acceso (IAM):** Configurar usuarios, roles, grupos, políticas de contraseñas y MFA.
- **Configuración de red:** Grupos de Seguridad (Security Groups), NACLs (Network ACLs) y configuraciones de VPC.
- **Sistemas operativos (en IaaS):** Si usa EC2, usted es responsable de parchear y actualizar el sistema operativo invitado (Guest OS).
- **Configuración del firewall:** Reglas de entrada y salida en Security Groups y NACLs.
- **Cifrado del lado del cliente:** Decidir qué datos cifrar y cómo hacerlo.
- **Protección del tráfico de red:** Configurar SSL/TLS, VPN y cifrado en tránsito.

> **Tip de examen:** Regla general de Piper y Clinton: **"Si puedes editarlo, eres dueño de él."** Si tienes acceso para configurar algo, es tu responsabilidad hacerlo correctamente.

---

## 4. El "Deslizamiento" de la Responsabilidad según el Servicio

Este es un punto crítico para el examen y donde muchos candidatos fallan. La línea de responsabilidad **se mueve** dependiendo de si el servicio es IaaS, PaaS o SaaS.

### Comparación por tipo de servicio

| Capa | IaaS (EC2) | PaaS (RDS, Elastic Beanstalk) | Serverless (Lambda) | SaaS (Amazon Connect) |
|---|---|---|---|---|
| **Datos** | Cliente | Cliente | Cliente | Cliente |
| **Aplicación** | Cliente | Cliente | Cliente (código) | AWS |
| **Sistema operativo** | Cliente | AWS | AWS | AWS |
| **Parches del OS** | Cliente | AWS | AWS | AWS |
| **Red / Firewall** | Cliente | Cliente (parcial) | AWS (parcial) | AWS |
| **Infraestructura** | AWS | AWS | AWS | AWS |

### Ejemplos detallados

- **EC2 (IaaS):** El cliente tiene la **mayor carga**. Debe gestionar el sistema operativo, parches de seguridad del OS, actualizaciones de aplicaciones y el firewall.
- **RDS (PaaS):** AWS gestiona el sistema operativo y los parches del motor de base de datos. El cliente solo es responsable de gestionar los datos, el acceso y la configuración de la base de datos.
- **Lambda (Serverless):** AWS gestiona toda la infraestructura de cómputo subyacente. El cliente solo asegura su **código** y los **datos**.
- **S3:** AWS gestiona la infraestructura. El cliente es responsable de las **políticas de bucket**, el **cifrado** y los **permisos de acceso**.

> **Tip de examen:** Cuanto más **gestionado** sea el servicio, **menos** responsabilidad tiene el cliente. Lambda = mínima responsabilidad del cliente. EC2 = máxima responsabilidad del cliente.

### Deslizamiento de responsabilidad por tipo de servicio

```mermaid
flowchart LR
    subgraph EC2["🖥️ IaaS (EC2)\nMáxima responsabilidad\ndel cliente"]
        direction TB
        E1["👤 Cliente:\nDatos + App + SO\n+ Parches + Firewall"]
        E2["🟠 AWS:\nHardware + Red\n+ Hipervisor"]
    end

    subgraph RDS["🗄️ PaaS (RDS)\nResponsabilidad\ncompartida"]
        direction TB
        R1["👤 Cliente:\nDatos + Config BD\n+ Acceso"]
        R2["🟠 AWS:\nSO + Parches BD\n+ Hardware"]
    end

    subgraph LAMBDA["⚡ Serverless (Lambda)\nMínima responsabilidad\ndel cliente"]
        direction TB
        L1["👤 Cliente:\nCódigo + Datos"]
        L2["🟠 AWS:\nTodo lo demás\n(SO, runtime, infra)"]
    end

    EC2 -->|"Más gestionado →"| RDS -->|"Más gestionado →"| LAMBDA

    style EC2 fill:#FF4444,color:#fff
    style RDS fill:#e8710a,color:#fff
    style LAMBDA fill:#00AA00,color:#fff
```

---

## 5. Clasificación de Controles de TI

Sequeira introduce una clasificación específica de controles que puede aparecer en el examen:

### Los 3 tipos de controles

| Tipo de control | Definición | Ejemplo |
|---|---|---|
| **Controles Heredados** | El cliente los recibe totalmente de AWS | Seguridad física, protección ambiental, controles de infraestructura |
| **Controles Compartidos** | Aplican a ambas partes en contextos separados | Parcheo (AWS parchea infraestructura, cliente parchea su OS), gestión de configuración, conciencia y entrenamiento |
| **Controles Específicos del Cliente** | Totalmente responsabilidad del cliente según la aplicación | Cifrar una columna en una BD, enrutamiento de datos dentro de zonas de seguridad |

### Ejemplos de controles compartidos

- **Parcheo:** AWS parchea la infraestructura subyacente; el cliente parchea su sistema operativo y aplicaciones.
- **Gestión de configuración:** AWS configura sus dispositivos de infraestructura; el cliente configura sus bases de datos, aplicaciones y Security Groups.
- **Conciencia y entrenamiento:** AWS entrena a sus empleados; el cliente entrena a los suyos.

> **Tip de examen:** Los controles **compartidos** son los más confusos en el examen. Recuerda que "compartido" significa que ambos hacen la misma actividad pero en su propio contexto.

### Los 3 tipos de controles

```mermaid
flowchart TD
    CTR["🔒 Controles de TI\nen AWS"] --> H["🟠 Heredados\n(100% AWS)"]
    CTR --> S["🟡 Compartidos\n(AWS + Cliente)"]
    CTR --> C["🔵 Específicos\ndel Cliente"]

    H --> H1["Seguridad física\nProtección ambiental\nInfraestructura de red"]

    S --> S1["Parcheo:\nAWS → infraestructura\nCliente → SO y apps"]
    S --> S2["Configuración:\nAWS → dispositivos\nCliente → BD y apps"]
    S --> S3["Entrenamiento:\nAWS → sus empleados\nCliente → sus empleados"]

    C --> C1["Cifrar columna en BD\nEnrutamiento de datos\nZonas de seguridad"]

    style CTR fill:#FF9900,color:#fff
    style H fill:#232F3E,color:#fff
    style S fill:#e8710a,color:#fff
    style C fill:#1a73e8,color:#fff
```

---

## 6. Servicios de Seguridad Clave para el Examen

Aunque pertenecen al Dominio 2 en general, estos servicios están directamente relacionados con la responsabilidad del cliente:

| Servicio | Función | Responsable de usarlo |
|---|---|---|
| **IAM** | Gestión de usuarios, roles, políticas y MFA | Cliente |
| **AWS KMS** | Gestión de claves de cifrado | Cliente (AWS gestiona la infraestructura de KMS) |
| **AWS Shield** | Protección contra DDoS | AWS (Standard es automático), Cliente (Advanced es opcional) |
| **AWS WAF** | Firewall de aplicaciones web | Cliente (configura las reglas) |
| **Security Groups** | Firewall a nivel de instancia (stateful) | Cliente |
| **NACLs** | Firewall a nivel de subred (stateless) | Cliente |
| **AWS CloudTrail** | Registro de llamadas a la API (auditoría) | Cliente (debe habilitarlo y revisarlo) |
| **Amazon GuardDuty** | Detección inteligente de amenazas | Cliente (debe activarlo) |
| **AWS Config** | Auditoría de configuración de recursos | Cliente |

> **Tip de examen:** Security Groups = **stateful** (recuerdan el estado de la conexión). NACLs = **stateless** (evalúan cada paquete independientemente). Esta distinción aparece frecuentemente en el examen.

### Capas de seguridad: Defensa en profundidad

```mermaid
flowchart TD
    U["👤 Usuario / Atacante"] --> EDGE["🌐 Edge Location\n🛡️ Shield (DDoS)\n🔥 WAF (SQL injection, XSS)"]
    EDGE --> VPC["🏗️ VPC\n📋 NACLs (Stateless)\nFirewall a nivel de subred"]
    VPC --> SG["🖥️ Instancia EC2\n🔒 Security Groups (Stateful)\nFirewall a nivel de instancia"]
    SG --> APP["⚙️ Aplicación\n👤 IAM (autenticación)\n📜 Políticas (autorización)"]
    APP --> DATA["📊 Datos\n🔐 KMS (cifrado en reposo)\n🔒 TLS (cifrado en tránsito)"]

    EDGE -.-> AWSR["🟠 AWS gestiona\nShield Standard"]
    VPC -.-> CLIR["🔵 Cliente configura\nreglas NACL"]
    SG -.-> CLI2["🔵 Cliente configura\nreglas SG"]
    APP -.-> CLI3["🔵 Cliente gestiona\nIAM y permisos"]
    DATA -.-> CLI4["🔵 Cliente decide\nqué cifrar"]

    style EDGE fill:#FF9900,color:#fff
    style VPC fill:#e8710a,color:#fff
    style SG fill:#1a73e8,color:#fff
    style APP fill:#232F3E,color:#fff
    style DATA fill:#0d904f,color:#fff
```

---

## Resumen para el Candidato

Para aprobar las preguntas sobre el Modelo de Responsabilidad Compartida:

| Escenario | Responsable |
|---|---|
| Acceso físico al centro de datos | **AWS** |
| Destrucción de discos al descomisionar | **AWS** |
| Parcheo del hipervisor | **AWS** |
| Mantenimiento de la infraestructura global | **AWS** |
| Encriptación de datos del cliente | **Cliente** |
| Permisos de usuarios (IAM) | **Cliente** |
| Parches del sistema operativo en EC2 | **Cliente** |
| Configuración de Security Groups | **Cliente** |
| Parches del OS en RDS | **AWS** |
| Parches del OS en Lambda | **AWS** |
| Parcheo de infraestructura de red | **AWS** |
| Habilitar MFA para usuarios | **Cliente** |

### Regla rápida por servicio

- **EC2** → "Parchear OS" = **Cliente**
- **RDS** → "Parchear OS" = **AWS**
- **Lambda** → "Parchear OS" = **AWS**
- **S3** → "Políticas de acceso y cifrado" = **Cliente**

### Palabras clave que debes asociar

- **"Seguridad física / hardware / hipervisor"** → Responsabilidad de AWS
- **"Datos / cifrado / IAM / permisos"** → Responsabilidad del cliente
- **"Parchear sistema operativo"** → Depende del servicio (EC2 = cliente, RDS/Lambda = AWS)
- **"Configuración de firewall"** → Responsabilidad del cliente (Security Groups, NACLs)
- **"¿Quién tiene la culpa de la filtración?"** → Casi siempre el cliente (mala configuración)

### Árbol de decisión para preguntas del examen

```mermaid
flowchart TD
    Q["❓ Pregunta sobre\nResponsabilidad Compartida"] --> K1{"¿Menciona hardware,\ncentro de datos\no hipervisor?"}
    Q --> K2{"¿Menciona datos,\ncifrado o IAM?"}
    Q --> K3{"¿Menciona parchear\nel sistema operativo?"}
    Q --> K4{"¿Menciona firewall\no reglas de red?"}
    Q --> K5{"¿Menciona una filtración\no brecha de seguridad?"}

    K1 -->|Sí| A1["🟠 AWS\nSeguridad DE la nube\nInfraestructura física"]
    K2 -->|Sí| A2["🔵 Cliente\nSeguridad EN la nube\nDatos y acceso"]
    K3 -->|Sí| A3{"¿Qué servicio?"}
    K4 -->|Sí| A4["🔵 Cliente\nSecurity Groups\nNACLs, WAF"]
    K5 -->|Sí| A5["🔵 Casi siempre\nel Cliente\n(mala configuración)"]

    A3 -->|"EC2"| B1["🔵 Cliente\nparchea el SO"]
    A3 -->|"RDS / Lambda"| B2["🟠 AWS\nparchea el SO"]

    style Q fill:#FF9900,color:#fff
    style A1 fill:#FF9900,color:#fff
    style A2 fill:#1a73e8,color:#fff
    style A4 fill:#1a73e8,color:#fff
    style A5 fill:#1a73e8,color:#fff
    style B1 fill:#1a73e8,color:#fff
    style B2 fill:#FF9900,color:#fff
```
