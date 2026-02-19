# Modelos de Precios y Facturación de AWS - Examen CLF-C02

Basado en las tres fuentes proporcionadas (Sequeira, Piper/Clinton y Kankaria), he analizado los Modelos de Precios y Facturación de AWS.

En el contexto del examen **AWS Certified Cloud Practitioner (CLF-C02)**, este tema es el núcleo del **Dominio 4: Facturación, Precios y Soporte**, que representa el **12% de la puntuación total**. Específicamente, aborda la **Declaración de Tarea 4.1: Comparar modelos de precios de AWS** y la **Declaración de Tarea 4.2: Comprender los recursos para facturación, presupuesto y gestión de costos**.

A continuación, presento un análisis detallado estructurado para el examen.

---

## 1. Filosofía General de Precios (Cloud Economics)

El examen evalúa si comprende cómo AWS cambia la estructura de costos fundamental de TI.

### CapEx vs. OpEx

Se pasa de grandes **gastos de capital iniciales (CapEx)** a **gastos operativos variables (OpEx)**. En lugar de invertir en centros de datos, paga solo cuando consume recursos.

### Pague por lo que usa (Pay-as-you-go)

Este modelo elimina la necesidad de sobreaprovisionar hardware. Si apaga una instancia, deja de pagar por ella. Este modelo medido permite cobrar desde centavos hasta miles de dólares según el uso exacto.

### Pague menos al usar más

AWS ofrece **descuentos por volumen**. A medida que aumenta el uso (por ejemplo, en almacenamiento S3 o transferencia de datos), el costo por unidad disminuye.

### Nivel Gratuito (Free Tier)

Fundamental para principiantes y para el examen. Se divide en tres categorías:

| Categoría | Descripción | Ejemplos |
|---|---|---|
| **Gratis para siempre** | Sin límite de tiempo | Lambda hasta 1M solicitudes/mes, DynamoDB hasta 25GB |
| **12 meses gratis** | Solo para cuentas nuevas | 750 horas de EC2 t2/t3.micro, 5GB en S3 estándar |
| **Pruebas (Trials)** | Ofertas a corto plazo | Servicios específicos por tiempo limitado |

### 📊 Diagrama: CapEx vs OpEx - El Cambio Fundamental

```mermaid
flowchart LR
    subgraph CAPEX["💰 Modelo Tradicional (CapEx)"]
        direction TB
        C1["🏢 Comprar servidores<br/>por adelantado"]
        C2["📈 Gran inversión inicial"]
        C3["⏳ Capacidad fija<br/>(sobreaprovisionamiento)"]
        C4["🔧 Mantenimiento<br/>a su cargo"]
        C1 --> C2 --> C3 --> C4
    end

    subgraph OPEX["☁️ Modelo AWS (OpEx)"]
        direction TB
        O1["🔄 Pague por lo que usa<br/>(pay-as-you-go)"]
        O2["📉 Sin inversión inicial"]
        O3["📊 Escala según demanda<br/>(elástico)"]
        O4["🤝 AWS gestiona<br/>infraestructura"]
        O1 --> O2 --> O3 --> O4
    end

    CAPEX -->|"☁️ Migración<br/>a la nube"| OPEX

    style CAPEX fill:#FF4444,stroke:#232F3E,color:#FFFFFF
    style OPEX fill:#0d904f,stroke:#232F3E,color:#FFFFFF
    style C1 fill:#FF4444,stroke:#FFFFFF,color:#FFFFFF
    style C2 fill:#FF4444,stroke:#FFFFFF,color:#FFFFFF
    style C3 fill:#FF4444,stroke:#FFFFFF,color:#FFFFFF
    style C4 fill:#FF4444,stroke:#FFFFFF,color:#FFFFFF
    style O1 fill:#0d904f,stroke:#FFFFFF,color:#FFFFFF
    style O2 fill:#0d904f,stroke:#FFFFFF,color:#FFFFFF
    style O3 fill:#0d904f,stroke:#FFFFFF,color:#FFFFFF
    style O4 fill:#0d904f,stroke:#FFFFFF,color:#FFFFFF
```

### 📊 Diagrama: Las 3 Categorías del Free Tier

```mermaid
flowchart TD
    FT["🆓 AWS Free Tier"]

    FT --> A["♾️ Gratis para Siempre<br/>(Always Free)"]
    FT --> B["📅 12 Meses Gratis<br/>(Free for 12 Months)"]
    FT --> C["🧪 Pruebas<br/>(Trials)"]

    A --> A1["Lambda: 1M solicitudes/mes"]
    A --> A2["DynamoDB: 25GB"]
    A --> A3["CloudWatch: 10 métricas"]

    B --> B1["EC2: 750h t2/t3.micro"]
    B --> B2["S3: 5GB estándar"]
    B --> B3["RDS: 750h db.t2.micro"]

    C --> C1["SageMaker: 2 meses"]
    C --> C2["Redshift: 2 meses"]

    style FT fill:#FF9900,stroke:#232F3E,color:#232F3E
    style A fill:#0d904f,stroke:#232F3E,color:#FFFFFF
    style B fill:#1a73e8,stroke:#232F3E,color:#FFFFFF
    style C fill:#7B1FA2,stroke:#232F3E,color:#FFFFFF
    style A1 fill:#0d904f,stroke:#FFFFFF,color:#FFFFFF
    style A2 fill:#0d904f,stroke:#FFFFFF,color:#FFFFFF
    style A3 fill:#0d904f,stroke:#FFFFFF,color:#FFFFFF
    style B1 fill:#1a73e8,stroke:#FFFFFF,color:#FFFFFF
    style B2 fill:#1a73e8,stroke:#FFFFFF,color:#FFFFFF
    style B3 fill:#1a73e8,stroke:#FFFFFF,color:#FFFFFF
    style C1 fill:#7B1FA2,stroke:#FFFFFF,color:#FFFFFF
    style C2 fill:#7B1FA2,stroke:#FFFFFF,color:#FFFFFF
```

---

## 2. Modelos de Precios de Cómputo (EC2)

Esta es el **área más evaluada** dentro de los precios. Debe ser capaz de elegir el modelo correcto basado en un escenario de uso.

### On-Demand (Bajo Demanda)

- **Características:** Sin compromiso a largo plazo ni pago inicial. Paga por hora o segundo. Es el modelo **más flexible pero el más costoso** por unidad de tiempo.
- **Caso de uso:** Cargas de trabajo a corto plazo, picos impredecibles o desarrollo/pruebas nuevas.

### Savings Plans e Instancias Reservadas (RIs)

- **Características:** Compromiso de uso por **1 o 3 años** a cambio de un gran descuento (hasta **72%** respecto a On-Demand).
- **Diferencia clave:** Las **Savings Plans** son el modelo recomendado por AWS sobre las RIs debido a su mayor flexibilidad (aplican a EC2, Fargate y Lambda), mientras que las RIs son específicas para instancias EC2.
- **Caso de uso:** Cargas de trabajo de **estado estable (steady-state)** y uso predecible a largo plazo.

### Spot Instances (Instancias de Subasta)

- **Características:** Compra capacidad no utilizada de AWS con descuentos de hasta el **90%**. Sin embargo, AWS puede **interrumpir/terminar** la instancia con solo **2 minutos** de aviso.
- **Caso de uso:** Cargas de trabajo **tolerantes a fallos**, flexibles en horario, procesamiento por lotes (batch processing) o análisis de big data.

### Dedicated Hosts (Hosts Dedicados)

- **Características:** Un servidor físico reservado exclusivamente para su uso. Es el modelo **más costoso**.
- **Caso de uso:** Cumplimiento de **licencias de software** existentes vinculadas al hardware (BYOL - Bring Your Own License) o requisitos regulatorios estrictos de aislamiento.

> **Tip de examen:** "Carga constante 3 años" = **Savings Plans / RIs**. "Tolerante a interrupciones, máximo ahorro" = **Spot**. "Sin compromiso, flexible" = **On-Demand**. "Licencias BYOL" = **Dedicated Hosts**.

### 📊 Diagrama: Modelos de Precios EC2 - Costo vs Flexibilidad

```mermaid
flowchart LR
    subgraph SPECTRUM["💰 Espectro de Precios EC2"]
        direction LR
        SPOT["🏷️ Spot<br/>Hasta 90% descuento<br/>⚠️ Puede interrumpirse"]
        RI["📋 Savings Plans / RIs<br/>Hasta 72% descuento<br/>📅 Compromiso 1-3 años"]
        OD["🔄 On-Demand<br/>Sin descuento<br/>✅ Máxima flexibilidad"]
        DH["🏢 Dedicated Hosts<br/>Más costoso<br/>🔒 Servidor físico exclusivo"]
    end

    SPOT ---|"Más barato →<br/>→ Más caro"| RI
    RI ---|"Menos flexible →<br/>→ Más flexible"| OD
    OD ---|"Requisitos<br/>especiales"| DH

    style SPECTRUM fill:#232F3E,stroke:#FF9900,color:#FFFFFF
    style SPOT fill:#0d904f,stroke:#FFFFFF,color:#FFFFFF
    style RI fill:#1a73e8,stroke:#FFFFFF,color:#FFFFFF
    style OD fill:#e8710a,stroke:#FFFFFF,color:#FFFFFF
    style DH fill:#FF4444,stroke:#FFFFFF,color:#FFFFFF
```

### 📊 Diagrama: ¿Qué modelo de precio elegir?

```mermaid
flowchart TD
    Q["🤔 ¿Qué modelo de<br/>precios EC2 necesito?"]

    Q --> A{"¿La carga de trabajo<br/>es predecible?"}

    A -->|"Sí, constante<br/>1-3 años"| B{"¿Necesito flexibilidad<br/>entre servicios?"}
    A -->|"No, es variable<br/>o temporal"| C{"¿Tolera<br/>interrupciones?"}
    A -->|"Tengo requisitos<br/>de licencias/hardware"| DH["✅ Dedicated Hosts<br/>(BYOL / regulatorio)"]

    B -->|"Sí (EC2 + Fargate<br/>+ Lambda)"| SP["✅ Savings Plans<br/>(más flexible)"]
    B -->|"No, solo EC2<br/>específico"| RI["✅ Reserved Instances<br/>(tipo específico)"]

    C -->|"Sí, es batch /<br/>procesamiento flexible"| SPOT["✅ Spot Instances<br/>(hasta 90% ahorro)"]
    C -->|"No, necesito<br/>disponibilidad garantizada"| OD["✅ On-Demand<br/>(pago por uso, sin compromiso)"]

    style Q fill:#FF9900,stroke:#232F3E,color:#232F3E
    style A fill:#232F3E,stroke:#FF9900,color:#FFFFFF
    style B fill:#232F3E,stroke:#FF9900,color:#FFFFFF
    style C fill:#232F3E,stroke:#FF9900,color:#FFFFFF
    style SP fill:#0d904f,stroke:#232F3E,color:#FFFFFF
    style RI fill:#0d904f,stroke:#232F3E,color:#FFFFFF
    style SPOT fill:#0d904f,stroke:#232F3E,color:#FFFFFF
    style OD fill:#0d904f,stroke:#232F3E,color:#FFFFFF
    style DH fill:#0d904f,stroke:#232F3E,color:#FFFFFF
```

---

## 3. Herramientas de Gestión de Costos

El examen presenta escenarios donde un gerente financiero o un administrador necesita visualizar, alertar o estimar costos. Debe distinguir entre estas herramientas.

### AWS Pricing Calculator

- **Función:** Permite **estimar los costos mensuales** de una arquitectura **antes de construirla**.
- **Clave:** No rastrea costos reales, solo proyecciones basadas en lo que usted ingresa.

### AWS Budgets

- **Función:** Permite establecer un **presupuesto personalizado** (de costos, uso o reservas) y recibir **alertas** (email, SNS) cuando se supera (o se prevé superar) el umbral definido.
- **Clave:** Es la herramienta para la **"acción proactiva"** ante sobrecostes.

### AWS Cost Explorer

- **Función:** Herramienta **visual** para analizar datos de costos **históricos y actuales**.
- **Clave:** Permite ver tendencias, filtrar por servicio/región/etiqueta, y ofrece **recomendaciones de optimización** (como comprar Reserved Instances). Responde: "¿Cuánto gasté en EC2 el mes pasado?"

### AWS Cost and Usage Reports (CUR)

- **Función:** Proporciona los datos de facturación **más granulares y detallados** disponibles (hasta nivel de hora o recurso individual).
- **Clave:** Generalmente se envía a un bucket S3 para ser analizado con herramientas como **Amazon Athena** o **Redshift**.

> **Tip de examen:** "Estimar costos antes de construir" = **Pricing Calculator**. "Alertas de presupuesto" = **Budgets**. "Analizar gastos pasados visualmente" = **Cost Explorer**. "Datos más granulares / detallados" = **CUR**.

### 📊 Diagrama: Herramientas de Gestión de Costos - ¿Cuándo usar cuál?

```mermaid
flowchart TD
    subgraph BEFORE["📐 ANTES del Despliegue"]
        PC["🧮 Pricing Calculator<br/>Estimar costos<br/>de una arquitectura<br/>ANTES de construirla"]
    end

    subgraph DURING["⚡ DURANTE la Operación"]
        BUD["🔔 AWS Budgets<br/>Alertas proactivas<br/>cuando se acerca<br/>al límite de gasto"]
    end

    subgraph AFTER["📊 DESPUÉS (Análisis)"]
        CE["📈 Cost Explorer<br/>Análisis VISUAL<br/>de tendencias y costos<br/>históricos"]
        CUR["📋 Cost & Usage Reports<br/>Datos MÁS GRANULARES<br/>hasta nivel de recurso<br/>individual por hora"]
    end

    BEFORE -->|"Despliega<br/>arquitectura"| DURING
    DURING -->|"Analiza<br/>resultados"| AFTER
    CUR -->|"Enviar a S3 →<br/>Athena / Redshift"| ANALYSIS["🔍 Análisis avanzado"]

    style BEFORE fill:#1a73e8,stroke:#FF9900,color:#FFFFFF
    style DURING fill:#e8710a,stroke:#FF9900,color:#FFFFFF
    style AFTER fill:#0d904f,stroke:#FF9900,color:#FFFFFF
    style PC fill:#1a73e8,stroke:#FFFFFF,color:#FFFFFF
    style BUD fill:#e8710a,stroke:#FFFFFF,color:#FFFFFF
    style CE fill:#0d904f,stroke:#FFFFFF,color:#FFFFFF
    style CUR fill:#0d904f,stroke:#FFFFFF,color:#FFFFFF
    style ANALYSIS fill:#7B1FA2,stroke:#FFFFFF,color:#FFFFFF
```

---

## 4. Estrategias de Optimización y Gobernanza

### Etiquetas de Asignación de Costos (Cost Allocation Tags)

- Etiquetas que, una vez activadas, permiten **rastrear costos detallados** por proyecto, departamento o centro de costos.
- Sin ellas, la factura es un total agregado difícil de desglosar.

### AWS Organizations y Facturación Consolidada

- Permite agrupar **múltiples cuentas de AWS** bajo una cuenta de administración (Management Account).
- **Beneficio clave:** La **Facturación Consolidada (Consolidated Billing)** combina el uso de todas las cuentas, lo que permite:
  - Alcanzar **niveles de descuento por volumen** más rápido.
  - Recibir **una sola factura** para toda la organización.

> **Tip de examen:** "Rastrear costos por departamento/proyecto" = **Cost Allocation Tags**. "Múltiples cuentas, una factura, descuentos por volumen" = **Organizations + Facturación Consolidada**.

### 📊 Diagrama: AWS Organizations - Facturación Consolidada

```mermaid
flowchart TD
    subgraph ORG["🏛️ AWS Organizations"]
        MGMT["👑 Cuenta de Administración<br/>(Management Account)<br/>📧 Recibe UNA sola factura"]

        subgraph OUS["📂 Unidades Organizativas (OUs)"]
            subgraph DEV["🔧 OU: Desarrollo"]
                DEV1["📦 Cuenta Dev-1<br/>Uso: $500"]
                DEV2["📦 Cuenta Dev-2<br/>Uso: $300"]
            end
            subgraph PROD["🚀 OU: Producción"]
                PROD1["📦 Cuenta Prod-1<br/>Uso: $2,000"]
                PROD2["📦 Cuenta Prod-2<br/>Uso: $1,500"]
            end
        end

        MGMT --> OUS
    end

    ORG --> BILL["💰 Facturación Consolidada<br/>Total: $4,300<br/>✅ Descuentos por volumen combinados<br/>✅ Una sola factura"]

    style ORG fill:#232F3E,stroke:#FF9900,color:#FFFFFF
    style MGMT fill:#FF9900,stroke:#232F3E,color:#232F3E
    style OUS fill:#232F3E,stroke:#FF9900,color:#FFFFFF
    style DEV fill:#1a73e8,stroke:#FFFFFF,color:#FFFFFF
    style PROD fill:#0d904f,stroke:#FFFFFF,color:#FFFFFF
    style DEV1 fill:#1a73e8,stroke:#FFFFFF,color:#FFFFFF
    style DEV2 fill:#1a73e8,stroke:#FFFFFF,color:#FFFFFF
    style PROD1 fill:#0d904f,stroke:#FFFFFF,color:#FFFFFF
    style PROD2 fill:#0d904f,stroke:#FFFFFF,color:#FFFFFF
    style BILL fill:#FF9900,stroke:#232F3E,color:#232F3E
```

---

## Resumen para el Candidato

Para las preguntas de facturación en el CLF-C02:

| Escenario en el examen | Respuesta |
|---|---|
| Estimación antes del despliegue | **Pricing Calculator** |
| Alertas cuando el gasto sube | **AWS Budgets** |
| Análisis visual y tendencias de costos | **Cost Explorer** |
| Datos más granulares / detallados | **Cost and Usage Reports (CUR)** |
| Descuento por compromiso (1-3 años) | **Savings Plans / Reserved Instances** |
| Máximo ahorro pero riesgo de interrupción | **Spot Instances** |
| Sin compromiso, pago por uso | **On-Demand** |
| Licencias de software (BYOL) | **Dedicated Hosts** |
| Rastrear costos por departamento | **Cost Allocation Tags** |
| Múltiples cuentas, una factura | **Organizations + Facturación Consolidada** |

### Palabras clave que debes asociar

- **"Estimar / proyectar costos antes"** → Pricing Calculator
- **"Alertas / presupuesto / umbral"** → AWS Budgets
- **"Analizar gastos / tendencias / visual"** → Cost Explorer
- **"Datos granulares / nivel de recurso"** → CUR
- **"Compromiso 1-3 años / steady-state"** → Savings Plans / RIs
- **"Interrumpible / batch / 90% ahorro"** → Spot
- **"Flexible / sin compromiso / corto plazo"** → On-Demand
- **"Licencias BYOL / servidor físico"** → Dedicated Hosts
- **"Etiquetas / rastrear por proyecto"** → Cost Allocation Tags
- **"Múltiples cuentas / volumen / una factura"** → Organizations

---

### 📊 Diagrama: Árbol de Decisión para Preguntas del Examen

```mermaid
flowchart TD
    Q["❓ Pregunta sobre<br/>Precios y Facturación"]

    Q --> A{"¿Es sobre PRECIOS<br/>de cómputo o<br/>HERRAMIENTAS de costos?"}

    A -->|"Precios /<br/>Modelos EC2"| B{"¿Qué escenario?"}
    A -->|"Herramientas<br/>de costos"| C{"¿Qué necesito hacer?"}
    A -->|"Gobernanza /<br/>Organización"| D{"¿Qué necesito?"}

    B -->|"Carga estable<br/>1-3 años"| SP["✅ Savings Plans / RIs<br/>(hasta 72% descuento)"]
    B -->|"Tolerante a fallos<br/>/ batch"| SPOT["✅ Spot Instances<br/>(hasta 90% descuento)"]
    B -->|"Corto plazo /<br/>impredecible"| OD["✅ On-Demand<br/>(sin compromiso)"]
    B -->|"Licencias BYOL /<br/>regulatorio"| DH["✅ Dedicated Hosts<br/>(servidor físico)"]

    C -->|"Estimar ANTES<br/>de construir"| PC["✅ Pricing Calculator"]
    C -->|"Alertas de<br/>presupuesto"| BUD["✅ AWS Budgets"]
    C -->|"Analizar gastos<br/>pasados (visual)"| CE["✅ Cost Explorer"]
    C -->|"Datos más<br/>detallados posibles"| CUR["✅ Cost & Usage Reports"]

    D -->|"Rastrear costos<br/>por departamento"| TAGS["✅ Cost Allocation Tags"]
    D -->|"Múltiples cuentas /<br/>una factura"| ORG["✅ Organizations +<br/>Facturación Consolidada"]

    style Q fill:#FF9900,stroke:#232F3E,color:#232F3E
    style A fill:#232F3E,stroke:#FF9900,color:#FFFFFF
    style B fill:#232F3E,stroke:#FF9900,color:#FFFFFF
    style C fill:#232F3E,stroke:#FF9900,color:#FFFFFF
    style D fill:#232F3E,stroke:#FF9900,color:#FFFFFF
    style SP fill:#0d904f,stroke:#232F3E,color:#FFFFFF
    style SPOT fill:#0d904f,stroke:#232F3E,color:#FFFFFF
    style OD fill:#0d904f,stroke:#232F3E,color:#FFFFFF
    style DH fill:#0d904f,stroke:#232F3E,color:#FFFFFF
    style PC fill:#0d904f,stroke:#232F3E,color:#FFFFFF
    style BUD fill:#0d904f,stroke:#232F3E,color:#FFFFFF
    style CE fill:#0d904f,stroke:#232F3E,color:#FFFFFF
    style CUR fill:#0d904f,stroke:#232F3E,color:#FFFFFF
    style TAGS fill:#0d904f,stroke:#232F3E,color:#FFFFFF
    style ORG fill:#0d904f,stroke:#232F3E,color:#FFFFFF
```
