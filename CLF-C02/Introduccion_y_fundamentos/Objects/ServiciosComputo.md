Basado en las tres fuentes proporcionadas (Sequeira, Piper/Clinton y Kankaria), he analizado los Servicios de Cómputo de AWS.
En el contexto del examen AWS Certified Cloud Practitioner (CLF-C02), este tema es el núcleo del Dominio 3: Tecnología y Servicios en la Nube (34% del examen), abordando específicamente la Declaración de Tarea 3.3: Identificar servicios de cómputo de AWS. Además, tiene un fuerte solapamiento con el Dominio 4 (Facturación) debido a los modelos de precios de cómputo.
A continuación, presento un análisis detallado estructurado para el examen:
1. Amazon Elastic Compute Cloud (EC2) - IaaS
El servicio fundamental de Infraestructura como Servicio (IaaS). El examen evalúa su capacidad para seleccionar la configuración correcta según el caso de uso.
• Definición: Proporciona capacidad de cómputo redimensionable (máquinas virtuales) en la nube. Ofrece control total a nivel de sistema operativo (root/admin).
• Imágenes de Máquina de Amazon (AMIs): Son las "plantillas" preconfiguradas que contienen el sistema operativo y el software necesario para lanzar una instancia. Las fuentes destacan cuatro categorías de AMIs: Quick Start (inicio rápido), Mis AMIs (personalizadas), Marketplace (de proveedores externos) y Comunitarias.
• Tipos de Instancias (Familias): Debe memorizar las categorías generales para elegir la instancia adecuada según el escenario:
    ◦ Propósito General (Ej. T, M): Equilibrio de recursos (servidores web, repositorios de código).
    ◦ Optimizadas para Cómputo (Ej. C): Alto rendimiento de procesador (procesamiento por lotes, transcodificación de medios).
    ◦ Optimizadas para Memoria (Ej. R, X): Cargas de trabajo que procesan grandes conjuntos de datos en memoria.
    ◦ Cómputo Acelerado (Ej. P, G): Usan GPU para aprendizaje automático o gráficos.
    ◦ Optimizadas para Almacenamiento (Ej. I, D): Acceso secuencial de lectura/escritura muy alto.
2. Modelos de Precios de Cómputo
Aunque es parte del Dominio 4, es inseparable de EC2. Las fuentes coinciden en cuatro modelos clave que siempre aparecen en el examen:
1. Bajo Demanda (On-Demand): El más flexible, sin compromisos a largo plazo. Ideal para cargas de trabajo a corto plazo, irregulares o impredecibles que no pueden ser interrumpidas. Es el más costoso por hora.
2. Instancias Reservadas (RI) y Savings Plans: Ofrecen descuentos significativos (hasta 72%) a cambio de un compromiso de 1 o 3 años. Ideales para cargas de trabajo de estado estable y uso predecible.
3. Instancias de Spot: Permiten ofertar por capacidad no utilizada con descuentos de hasta el 90%. AWS puede reclamarlas con solo 2 minutos de aviso. Solo para cargas de trabajo flexibles, sin estado o tolerantes a fallos.
4. Hosts Dedicados (Dedicated Hosts): Servidores físicos dedicados para su uso. Se usan para cumplir con requisitos de licencias de software existentes (BYOL) o normativas de cumplimiento estrictas.
3. Servicios de Contenedores
El examen distingue entre la orquestación (gestión) y el cómputo (dónde se ejecutan) de los contenedores.
• Amazon Elastic Container Service (ECS): Servicio de orquestación de contenedores altamente escalable que soporta Docker. Es la forma "nativa" de AWS para ejecutar contenedores.
• Amazon Elastic Kubernetes Service (EKS): Servicio gestionado para ejecutar Kubernetes en AWS. Elija esto si la pregunta menciona migrar cargas de trabajo de Kubernetes existentes o usar herramientas de código abierto.
• AWS Fargate: Es un motor de cómputo serverless para contenedores. Funciona tanto con ECS como con EKS. Con Fargate, no tiene que aprovisionar ni gestionar servidores (no ve las instancias EC2 subyacentes), solo paga por los recursos que consume el contenedor.
4. Cómputo Serverless (Sin Servidor)
Un tema crítico para la arquitectura moderna en la nube.
• AWS Lambda: Ejecuta código sin aprovisionar ni gestionar servidores. Se factura por milisegundos de tiempo de cómputo y número de solicitudes. Es basado en eventos (se activa por cambios en S3, DynamoDB, etc.).
    ◦ Límite clave para el examen: Las funciones tienen un tiempo de espera máximo de 15 minutos.
5. Servicios de Cómputo Gestionados y Especializados
El examen a menudo presenta escenarios buscando la solución con "menor carga administrativa".
• AWS Elastic Beanstalk (PaaS): Servicio fácil de usar para desplegar y escalar aplicaciones web. Usted sube el código y Elastic Beanstalk maneja automáticamente el despliegue (aprovisionamiento de capacidad, equilibrio de carga, auto-escalado). Usted mantiene el control de la configuración si lo desea.
• Amazon Lightsail: Servidores privados virtuales (VPS) simplificados. Incluye todo lo necesario (cómputo, almacenamiento, redes) por un precio mensual bajo y predecible. Ideal para sitios web simples o desarrolladores principiantes que no necesitan las funciones avanzadas de EC2.
• AWS Batch: Permite ejecutar cargas de trabajo de computación por lotes a cualquier escala. Aprovisiona dinámicamente la cantidad y el tipo óptimos de recursos de cómputo (como instancias optimizadas para memoria o CPU).
6. Cómputo Híbrido y en el Borde
Las fuentes más recientes (Piper/Clinton y Kankaria) destacan extensiones de la infraestructura que aparecen en la versión CLF-C02 del examen:
• AWS Outposts: Lleva la infraestructura y los servicios de AWS a su centro de datos local (on-premises) para una experiencia híbrida consistente.
• AWS Wavelength: Despliega servicios de cómputo y almacenamiento en el borde de las redes 5G de telecomunicaciones para aplicaciones de latencia ultrabaja.
• AWS Local Zones: Extiende la infraestructura de AWS a áreas metropolitanas específicas para acercar el cómputo a los usuarios finales y reducir la latencia.
Resumen para el Candidato
Para aprobar las preguntas de Servicios de Cómputo:
1. IaaS y Control Total = EC2.
2. Código sin servidores (Eventos/ <15 min) = Lambda.
3. Fácil despliegue web (PaaS) = Elastic Beanstalk.
4. VPS simple / Precio fijo = Lightsail.
5. Contenedores Docker = ECS (nativo) o EKS (Kubernetes).
6. Contenedores sin gestionar servidores = Fargate.