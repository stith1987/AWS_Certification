
Pregunta 30:
Un becario de una empresa de TI aprovisionó una instancia EC2 bajo demanda basada en Linux con facturación por segundos, pero la canceló a los 30 segundos porque quería aprovisionar otro tipo de instancia. ¿Cuál es la duración por la que se facturaría la instancia?

300 segundos

Tu respuesta es incorrecta
600 segundos

30 segundos

Respuesta correcta
60 segundos

Explicación general
Opción correcta:

60 segundos

Existe una tarifa mínima de un minuto para las instancias EC2 basadas en Linux, por lo que ésta es la opción correcta.

Opciones incorrectas:

30 segundos

300 segundos

600 segundos

Estas tres opciones contradicen los detalles proporcionados anteriormente en la explicación, por lo que son incorrectas.

Referencia:

https://aws.amazon.com/blogs/aws/new-per-second-billing-for-ec2-instances-and-ebs-volumes/

Temática
Facturación y precios

Pregunta 31:
Una empresa quiere identificar la configuración óptima de recursos de AWS para sus cargas de trabajo, de modo que la empresa pueda reducir costes y aumentar el rendimiento de las cargas de trabajo. ¿Cuál de los siguientes servicios puede utilizarse para cumplir este requisito?

AWS Budgets

Tu respuesta es correcta
AWS Compute Optimizer

AWS Cost Explorer

AWS Systems Manager

Explicación general
Opción correcta:

AWS Compute Optimizer

AWS Compute Optimizer recomienda los recursos de AWS óptimos para tus cargas de trabajo con el fin de reducir costes y mejorar el rendimiento, utilizando el aprendizaje automático para analizar las métricas de utilización históricas. Un aprovisionamiento excesivo de recursos puede generar costes de infraestructura innecesarios, y un aprovisionamiento insuficiente puede dar lugar a un rendimiento deficiente de las aplicaciones. Compute Optimizer te ayuda a elegir configuraciones óptimas para tres tipos de recursos de AWS: instancias de Amazon EC2, volúmenes de Amazon EBS y funciones de AWS Lambda, basándose en tus datos de utilización.

Compute Optimizer recomienda hasta 3 opciones de entre más de 140 tipos de instancias EC2, así como una amplia gama de opciones de configuración de volúmenes EBS y funciones Lambda, para dimensionar correctamente tus cargas de trabajo. Compute Optimizer también proyecta cuál habría sido la utilización de la CPU, la utilización de la memoria y el tiempo de ejecución de tu carga de trabajo con las opciones de recursos de AWS recomendadas. Esto te ayuda a comprender cómo habría funcionado tu carga de trabajo con las opciones recomendadas antes de poner en práctica las recomendaciones.

Cómo funciona AWS Compute Optimizer:  vía - https://aws.amazon.com/compute-optimizer/

Opciones incorrectas:

AWS Systems Manager - AWS Systems Manager es el centro de operaciones de AWS. Systems Manager proporciona una interfaz de usuario unificada para que puedas rastrear y resolver problemas operativos en todas tus aplicaciones y recursos de AWS desde un lugar central. Con Systems Manager, puedes automatizar tareas operativas para instancias de Amazon EC2 o instancias de Amazon RDS. También puedes agrupar recursos por aplicación, ver datos operativos para monitorizar y solucionar problemas, implementar flujos de trabajo de cambios preaprobados y auditar cambios operativos para tus grupos de recursos. Systems Manager simplifica la gestión de recursos y aplicaciones, acorta el tiempo para detectar y resolver problemas operativos, y facilita el funcionamiento y la gestión de tu infraestructura a escala. Systems Manager no puede utilizarse para identificar la configuración óptima de recursos para las cargas de trabajo que se ejecutan en AWS.

AWS Budgets - AWS Budgets te permite establecer presupuestos personalizados para realizar un seguimiento de tu coste y uso desde los casos de uso más sencillos a los más complejos. Con AWS Budgets, puedes elegir que se te avise por correo electrónico o notificación SNS cuando el coste y el uso reales o previstos superen el umbral de tu presupuesto, o cuando la utilización o cobertura reales de tu RI y planes de ahorro caigan por debajo del umbral deseado. Con las acciones de AWS Budgets, también puedes configurar acciones específicas para responder al estado de coste y uso en tus cuentas, de modo que si tu coste o uso supera o se prevé que supere tu umbral, se puedan ejecutar acciones automáticamente o con tu aprobación para reducir el gasto excesivo involuntario.

AWS Cost Explorer - AWS Cost Explorer tiene una interfaz fácil de usar que te permite visualizar, comprender y administrar tus costes y uso de AWS a lo largo del tiempo. Cost Explorer Resource Rightsizing Recommendations y Compute Optimizer utilizan el mismo motor de recomendaciones. El motor de recomendaciones Compute Optimizer ofrece recomendaciones para ayudar a los clientes a identificar los tipos de instancia EC2 óptimos para sus cargas de trabajo. La consola y la API del Explorador de Costes muestran un subconjunto de estas recomendaciones que pueden suponer un ahorro de costes, y las aumenta con información de costes y ahorros específica del cliente (por ejemplo, información de facturación, créditos disponibles, RI y Planes de Ahorro) para ayudar a los propietarios de la Gestión de Costes a identificar rápidamente las oportunidades de ahorro mediante el redimensionamiento de la infraestructura. La consola Compute Optimizer y su API ofrecen todas las recomendaciones independientemente de las implicaciones de costes.

Referencia:

https://aws.amazon.com/compute-optimizer/

Temática
Tecnología

Pregunta 32:
¿Qué servicio de AWS puede utilizarse para mitigar un ataque de denegación de servicio distribuido (DDoS)?

Tu respuesta es correcta
AWS Shield

Amazon CloudWatch

AWS Key Management Service (AWS KMS)

AWS Systems Manager

Explicación general
Opción correcta:

AWS Shield

AWS Shield es un servicio gestionado de protección contra la Denegación de Servicio Distribuida (DDoS) que protege las aplicaciones que se ejecutan en AWS. AWS Shield proporciona detección siempre activa y mitigaciones automáticas en línea que minimizan el tiempo de inactividad y la latencia de las aplicaciones, por lo que no es necesario contratar a AWS Support para beneficiarse de la protección DDoS. Hay dos niveles de AWS Shield: Standard y Advanced.

Todos los clientes de AWS se benefician de las protecciones automáticas de AWS Shield Standard, sin cargo adicional. AWS Shield Standard te defiende contra los ataques DDoS más comunes y frecuentes de la capa de red y transporte dirigidos a tu sitio web o aplicaciones. Cuando utilizas AWS Shield Standard con Amazon CloudFront y Amazon Route 53, recibes una protección de disponibilidad completa contra todos los ataques conocidos a la infraestructura (Capas 3 y 4).

Para obtener mayores niveles de protección contra los ataques dirigidos a tus aplicaciones que se ejecutan en Amazon Elastic Compute Cloud (EC2), Elastic Load Balancer (ELB), Amazon CloudFront, AWS Global Accelerator y los recursos de Amazon Route 53, puedes suscribirte a AWS Shield Advanced. Además de las protecciones de red y de la capa de transporte que vienen con Standard, AWS Shield Advanced proporciona detección y mitigación adicionales contra ataques DDoS grandes y sofisticados, visibilidad casi en tiempo real de los ataques e integración con AWS WAF, un firewall de aplicaciones web.

Opciones incorrectas:

Amazon CloudWatch - Amazon CloudWatch es un servicio de monitorización y observabilidad creado para ingenieros DevOps, desarrolladores, ingenieros de fiabilidad de sitios (SRE) y administradores de TI. CloudWatch proporciona datos y perspectivas procesables para monitorizar aplicaciones, responder a cambios de rendimiento en todo el sistema, optimizar la utilización de recursos y obtener una visión unificada de la salud operativa. Es un servicio excelente para construir sistemas resistentes.

AWS Systems Manager - AWS Systems Manager te proporciona visibilidad y control de tu infraestructura en AWS. Systems Manager proporciona una interfaz de usuario unificada para que puedas ver los datos operativos de varios servicios de AWS y te permite automatizar tareas operativas en todos tus recursos de AWS. Con Systems Manager, puedes agrupar recursos, como instancias de Amazon EC2, buckets de Amazon S3 o instancias de Amazon RDS, por aplicación, ver datos operativos para monitorizar y solucionar problemas, y tomar medidas sobre tus grupos de recursos.

AWS Key Management Service (AWS KMS) - AWS Key Management Service (KMS) te facilita la creación y administración de claves criptográficas y el control de su uso en una amplia gama de servicios de AWS y en tus aplicaciones. AWS KMS es un servicio seguro y resistente que utiliza módulos de seguridad de hardware que han sido validados según FIPS 140-2, o están en proceso de validación, para proteger tus claves.

Referencia:

https://aws.amazon.com/shield/

Temática
Seguridad y normativa

Pregunta 33:
¿Cuál de los siguientes planes de AWS Support proporciona acceso únicamente a las comprobaciones básicas de las comprobaciones de buenas prácticas de AWS Trusted Advisor? (Selecciona dos)

Tu selección es correcta
AWS Basic Support

AWS Enterprise Support

AWS Business Support

Tu selección es correcta
AWS Developer Support

AWS Enterprise On-Ramp Support

Explicación general
Opciones correctas:

AWS Basic Support

El plan AWS Basic Support sólo proporciona acceso a lo siguiente:

Servicio de atención al cliente y comunidades - Acceso 24x7 al servicio de atención al cliente, documentación, Whitepapers y foros de soporte. AWS Trusted Advisor - Acceso a las comprobaciones básicas de Trusted Advisor y orientación para aprovisionar tus recursos siguiendo las mejores prácticas para aumentar el rendimiento y mejorar la seguridad. Salud de AWS - El panel de salud de tu cuenta : Una vista personalizada de la salud de tus servicios AWS, y alertas cuando tus recursos se vean afectados.

AWS Developer Support

Deberías utilizar el plan AWS Developer Support si estás probando o realizando desarrollos iniciales en AWS y deseas tener la posibilidad de obtener soporte técnico por correo electrónico durante el horario laboral, así como orientación general sobre arquitectura a medida que construyes y pruebas. Este plan proporciona acceso sólo a las comprobaciones básicas de Trusted Advisor de la Cuota de Servicio y a las comprobaciones básicas de Seguridad.

Alerta de examen:

Revisa las diferencias entre los planes AWS Developer Support, AWS Business Support, AWS Enterprise On-Ramp Support y AWS Enterprise Support, ya que puedes esperar al menos un par de preguntas en el examen:







vía - https://aws.amazon.com/premiumsupport/plans/

Opciones incorrectas:

AWS Enterprise Support - El plan AWS Enterprise Support proporciona a los clientes un servicio similar al de un conserje, en el que el objetivo principal es ayudar al cliente a conseguir sus resultados y encontrar el éxito en el Cloud. Con AWS Enterprise Support, obtienes soporte técnico 24x7 de ingenieros de alta calidad, herramientas y tecnología para administrar automáticamente el estado de tu entorno, orientación arquitectónica consultiva y un administrador técnico de cuentas (TAM) designado para coordinar el acceso a programas proactivos/preventivos y a expertos en la materia de AWS. También obtienes acceso completo a las comprobaciones de buenas prácticas de AWS Trusted Advisor.

AWS Business Support - Deberías utilizar el plan AWS Business Support si tienes cargas de trabajo de producción en AWS y quieres acceso telefónico, por correo electrónico y por chat 24x7 a soporte técnico y orientación sobre arquitectura en el contexto de tus casos de uso específicos. También obtienes acceso completo a las comprobaciones de buenas prácticas de AWS Trusted Advisor.

AWS Enterprise On-Ramp Support - Deberías utilizar el plan AWS Enterprise On-Ramp Support si tienes cargas de trabajo de producción/negocio críticas en AWS y quieres acceso 24x7 a soporte técnico y necesitas orientación experta para crecer y optimizar en el Cloud. Obtendrás acceso completo a las comprobaciones de buenas prácticas de AWS Trusted Advisor.

Referencia:

https://aws.amazon.com/premiumsupport/plans/

Temática
Conceptos del Cloud

Pregunta 34:
Una empresa utiliza instancias EC2 reservadas en varias unidades y cada unidad tiene su propia cuenta de AWS. Sin embargo, algunas de las unidades infrautilizan sus instancias reservadas, mientras que otras unidades necesitan más instancias reservadas. Como Cloud Practitioner, ¿cuál de las siguientes recomendarías como la solución más rentable?

Utiliza AWS Trusted Advisor para gestionar las cuentas de AWS de todas las unidades y luego comparte las instancias EC2 reservadas entre todas las unidades

Respuesta correcta
Utiliza AWS Organizations para gestionar las cuentas de AWS de todas las unidades y luego comparte las instancias EC2 reservadas entre todas las unidades

Tu respuesta es incorrecta
Utiliza AWS Systems Manager para gestionar las cuentas de AWS de todas las unidades y luego comparte las instancias EC2 reservadas entre todas las unidades

Utiliza AWS Cost Explorer para gestionar las cuentas de AWS de todas las unidades y luego comparte las instancias EC2 reservadas entre todas las unidades

Explicación general
Opción correcta:

Utiliza AWS Organizations para gestionar las cuentas de AWS de todas las unidades y luego comparte las instancias EC2 reservadas entre todas las unidades

AWS Organizations te ayuda a administrar centralmente la facturación; controlar el acceso, la normativa y la seguridad; y compartir recursos entre tus cuentas de AWS. Con AWS Organizaciones, puedes automatizar la creación de cuentas, crear grupos de cuentas que reflejen tus necesidades empresariales y aplicar políticas a estos grupos para su gobernanza. También puedes simplificar la facturación configurando un único método de pago para todas tus cuentas de AWS. AWS Organizaciones está disponible para todos los clientes de AWS sin coste adicional.

Características principales de las organizaciones de AWS:  vía - https://aws.amazon.com/organizations/

Opciones incorrectas:

Utiliza AWS Trusted Advisor para gestionar las cuentas de AWS de todas las unidades y luego comparte las instancias EC2 reservadas entre todas las unidades - AWS Trusted Advisor es una herramienta online que te proporciona orientación en tiempo real para ayudarte a aprovisionar tus recursos siguiendo las mejores prácticas de AWS sobre optimización de costes, seguridad, tolerancia a fallos, límites de servicio y mejora del rendimiento. No puedes utilizar Trusted Advisor para compartir las instancias EC2 reservadas entre varias cuentas de AWS.

Cómo funciona Trusted Advisor:  vía - https://aws.amazon.com/premiumsupport/technology/trusted-advisor/

Utiliza AWS Cost Explorer para gestionar las cuentas de AWS de todas las unidades y luego comparte las instancias EC2 reservadas entre todas las unidades - AWS Cost Explorer te permite explorar tus costes y uso de AWS tanto a un alto nivel como a un nivel detallado de análisis, y te faculta para profundizar utilizando varias dimensiones de filtrado (por ejemplo, servicio de AWS, región, cuenta vinculada). No puedes utilizar AWS Cost Explorer para compartir las instancias EC2 reservadas entre varias cuentas de AWS.

Utiliza AWS Systems Manager para gestionar las cuentas de AWS de todas las unidades y luego comparte las instancias EC2 reservadas entre todas las unidades - Systems Manager proporciona una interfaz de usuario unificada para que puedas ver los datos operativos de varios servicios de AWS y te permite automatizar tareas operativas en todos tus recursos de AWS. Con Systems Manager, puedes agrupar recursos, como instancias de Amazon EC2, buckets de Amazon S3 o instancias de Amazon RDS, por aplicación, ver datos operativos para monitorizar y solucionar problemas, y tomar medidas sobre tus grupos de recursos. No puedes utilizar Systems Manager para compartir las instancias EC2 reservadas entre varias cuentas de AWS.

Cómo funciona AWS Systems Manager:  vía - https://aws.amazon.com/systems-manager/

Referencias:

https://aws.amazon.com/organizations/

https://aws.amazon.com/premiumsupport/technology/trusted-advisor/

https://aws.amazon.com/systems-manager/

Temática
Seguridad y normativa

Pregunta 35:
¿Cuál de las siguientes opciones es CORRECTA en relación con la eliminación de una cuenta de AWS Organizaciones AWS?

La cuenta de AWS no debe tener ninguna Política de Control de Servicios (SCP) asociada. Sólo entonces se puede eliminar de las organizaciones de AWS

La cuenta de AWS puede eliminarse de AWS Systems Manager

Respuesta correcta
La cuenta de AWS debe poder funcionar como cuenta independiente. Sólo entonces se puede eliminar de las organizaciones de AWS

Tu respuesta es incorrecta
Envía un ticket de soporte a AWS Support para eliminar la cuenta

Explicación general
Opción correcta:

La cuenta de AWS debe poder funcionar como cuenta independiente. Sólo entonces se puede eliminar de las organizaciones de AWS

Puedes eliminar una cuenta de tu organización sólo si la cuenta tiene la información necesaria para funcionar como cuenta independiente. Para cada cuenta que quieras convertir en autónoma, debes aceptar el Acuerdo de Cliente de AWS, elegir un plan de soporte, proporcionar y verificar la información de contacto requerida y proporcionar un método de pago actual. AWS utiliza el método de pago para cobrar cualquier actividad facturable (no de la capa gratuita de AWS) de AWS que se produzca mientras la cuenta no esté vinculada a una organización.

Opciones incorrectas:

Envía un ticket de soporte a AWS Support para eliminar la cuenta - AWS Support no necesita ayudarte a eliminar una cuenta de AWS de las Organizaciones de AWS.

La cuenta de AWS puede eliminarse de AWS Systems Manager - AWS Systems Manager te proporciona visibilidad y control de tu infraestructura en AWS. Systems Manager proporciona una interfaz de usuario unificada para que puedas ver los datos operativos de varios servicios de AWS y te permite automatizar tareas operativas como ejecutar comandos, administrar parches y configurar servidores en el Cloud de AWS, así como en la infraestructura local. Systems Manager no puede utilizarse para eliminar una cuenta de AWS de AWS Organizations.

La cuenta de AWS no debe tener ninguna Política de Control de Servicios (SCP) asociada. Sólo entonces se puede eliminar de las organizaciones de AWS - Esto no es un requisito previo para eliminar la cuenta de AWS. Los principales de la cuenta de AWS ya no se ven afectados por ninguna política de control de servicios (SCP) que se haya definido en la organización. Esto significa que las restricciones impuestas por esas SCP han desaparecido, y los usuarios y roles de la cuenta pueden tener más permisos de los que tenían antes.

Referencia:

https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_accounts_remove.html

Temática
Conceptos del Cloud

