# AWS Certified Cloud Practitioner(CLF-C02)
Documentación del aprendizaje previo para presentar el examen.

En el siguiente plan de estudio se dividira en bloques. 

# Objetivo

Tener buenos fundamentos para presentar el examen , el objetivo no es solo obtener la certificación si no tambien aprender de forma profunda,
sabiendo que la idea principal no es solo obtener el certificado sino saber trabajar en la nube. 
mediante esta documentación podre consolidar conceptos e interiorizarlo.

## División del temario 

Como bien es sabido el examen de AWS certified cloud practitioner(CLF-C02) tiene una guia de lo que se tienen que aprender para poder 
aprobar.

Por ello dividiremos en temario en 9 bloques.

---

## Bloque 1 — Fundamentos de Cloud y AWS

Este bloque introduce los conceptos esenciales para entender qué es la nube y cómo funciona AWS.  
Aquí se construyen las bases necesarias para avanzar hacia servicios más específicos y comprender cómo se organiza la infraestructura en la nube.

En este bloque encontrarás:

- Qué es la computación en la nube y por qué se utiliza hoy en día  
- Cómo AWS estructura sus servicios y regiones  
- La diferencia entre on‑premise y cloud  
- Qué es una instancia EC2 y cómo se clasifica  
- Los tipos de almacenamiento más comunes (EBS, Instance Store, S3)  
- El modelo de responsabilidad compartida  
- Conceptos clave de alta disponibilidad y escalabilidad  

El objetivo es que al terminar este bloque tengas una visión clara de cómo funciona AWS a nivel fundamental y puedas avanzar con seguridad hacia los siguientes temas.

### Documentación completa del Bloque 1

Puedes leer el contenido completo aquí: [Documentación del Bloque 1](Bloque_1_Fundamentos_de_la_nube.md)

## Bloque 2 – Redes en AWS (Fundamentos)

Esta primera parte del bloque introduce los conceptos esenciales para entender cómo funciona la red dentro de una VPC. El objetivo es comprender cómo se conectan las instancias, cómo fluye el tráfico y qué componentes controlan la seguridad y el acceso.

En esta sección encontrarás:

- Qué es una VPC y cómo se estructura  
- Tipos de subnets y sus diferencias (pública, privada, aislada)  
- Cómo funcionan NAT Gateway e Internet Gateway  
- Qué son las Route Tables y cómo determinan el flujo  
- Seguridad en red: Security Groups y NACLs  
- Puertos esenciales para comunicación  
- Flujo de tráfico básico dentro de la VPC  
- Introducción a Endpoints para acceso privado a servicios de AWS  

Esta parte sienta las bases para la sección avanzada, donde veremos VPC Peering, Bastion Host, flujo entre VPCs, ENIs y servicios privados.

### Fundamentos
Puedes leer el contenido completo aquí: [Documentación del Bloque 2](Boque_2_Redes_AWS.md)

### Subtema 
- **Redes avanzadas** nivel 1 
Este subtema reúne los conceptos esenciales para entender cómo funcionan las conexiones privadas dentro de AWS: cómo tu VPC accede a servicios sin internet, cómo se comunican varias VPC entre sí, cómo se gestionan las tarjetas de red (ENI) y cuáles son las dos formas de acceder a instancias privadas (Bastion vs SSM). También incluye los errores típicos que suelen romper la conectividad en arquitecturas reales.

### Redes avanzadas
Puedes leer el contenido completo aquí: [Sub_tema_Resdes_avanzadas_nivel_1](Bloque_2_sub_tema_Resdes_avanzadas_Nivel_1.md)

## Bloque 3 — IAM (Identity and Access Management)

Este bloque introduce los conceptos básicos de IAM, explicando cómo se gestionan identidades y permisos en AWS mediante usuarios, grupos, roles y policies. Resume cómo funcionan las credenciales temporales (STS), las trust policies, el principio de least privilege y las diferencias entre permisos managed e inline. También incluye buenas prácticas de seguridad, el uso del Access Analyzer y una visión de los roles que se verán más adelante al trabajar con otros servicios.

### IAM
Puedes leer el contenido completo aquí: [Bloque 3 — IAM](Bloque_3_IAM.md)

 ## Bloque 4 — S3 Avanzado
Este bloque profundiza en las funciones avanzadas de S3: cómo se controla el acceso mediante bucket policies, por qué las ACLs casi no se usan hoy, cómo funciona el versioning para proteger datos, las opciones de cifrado, el impacto de Block Public Access y los distintos tipos de replicación entre buckets. Es un subtema centrado en seguridad, control de acceso y protección de datos dentro de S3.

 ### S3 Avanzado
 Puedes leer el contenido completo aquí: [Bloque 4 — S3 Avanzado](Bloque_4_S3_Avanzado.md)

## Bloque 5 — Redes Híbridas, Load Balancers, Auto Scaling y Modelo OSI
Este bloque explica cómo conectar tu infraestructura local con AWS mediante VPN o Direct Connect, cómo se organiza una VPC real, y cómo funcionan los componentes clave para manejar tráfico y escalar aplicaciones: ALB, NLB, Target Groups, Auto Scaling Groups y User Data.
También incluye las 7 capas del modelo OSI, con énfasis en las capas más usadas en AWS (4 y 7).

### Redes Híbridas 
Puedes leer el contenido completo aquí: [Bloque 5 — Redes Híbridas, Load Balancers, Auto Scaling y Modelo OSI](Bloque_5_RedesHíbridas_Balanceadores_Modelo_OSI.md)

## Bloque 6 — Bases de Datos en AWS
Este bloque resume los servicios de bases de datos más usados en AWS. Explica las diferencias entre bases SQL y NoSQL, cómo funciona RDS (Multi‑AZ, Read Replicas), el modelo clave‑valor de DynamoDB, las ventajas de Aurora y cuándo usar cachés como ElastiCache o DAX para mejorar el rendimiento.
### Bases de Datos en AWS
Puedes leer el contenido completo aquí: [Bloque 6 — Bases de Datos en AWS](Bloque_6_base_de_datos_y_cache_aws.md)

## Bloque 7 — Monitoreo y Observabilidad (CloudWatch, CloudTrail, AWS Config)
Este bloque explica los servicios que permiten monitorear, auditar y controlar cambios dentro de AWS. Aquí aprenderás cómo ver métricas, logs, alarmas, historial de acciones y cumplimiento de reglas. Es uno de los bloques más preguntados en el examen.
En este bloque encontrarás:
- CloudWatch: métricas, logs, alarmas, dashboards y acciones automáticas
- CloudTrail: auditoría, quién hizo qué, registro de acciones
- AWS Config: historial de cambios, cumplimiento de reglas, recursos NON‑COMPLIANT
- Diferencias claras entre CloudWatch vs CloudTrail vs Config
- Ejemplos típicos del examen
### Monitoreo y Observabilidad
Puedes leer el contenido completo aquí: [Bloque_7_Monitoreo_y_Obeservabilidad.md)


## Bloque 8 — Integraciones y Serverless (API Gateway, EventBridge, SNS, SQS, Lambda)
Este bloque cubre los servicios que permiten construir arquitecturas serverless y conectar servicios entre sí mediante eventos, colas y notificaciones. Es clave para entender cómo funcionan las aplicaciones modernas en AWS.
En este bloque encontrarás:
- API Gateway: exponer APIs, autenticación, throttling, REST vs HTTP
- EventBridge: bus de eventos, reglas, cron jobs, integraciones SaaS
- SNS: publicación/suscripción (uno a muchos)
- SQS: colas (uno a uno), desacoplamiento, retención de mensajes
- Lambda: función serverless que ejecuta código bajo demanda
- Arquitectura típica: API Gateway → Lambda → DynamoDB
- Ejemplos típicos del examen
### Integraciones y Serverless
Puedes leer el contenido completo aquí: [Bloque_8_Integrasiones_y_Serverless.md)


## Bloque 9 — Costos, Facturación y Arquitectura (Billing, Pricing, Well‑Architected)
Este bloque resume cómo se calculan los costos en AWS, cómo controlar el gasto y cómo aplicar buenas prácticas de arquitectura. Es un bloque teórico pero muy importante para el examen.
En este bloque encontrarás:
- Modelos de precios: On‑Demand, Reserved Instances, Savings Plans, Spot
- Herramientas de costos: Billing Dashboard, Cost Explorer, Budgets, Pricing Calculator
- Free Tier y tipos de niveles gratuitos
- Well‑Architected Framework: los 6 pilares
- Shared Responsibility Model
- TCO (Total Cost of Ownership)
- Organizations, Control Tower, etiquetas de costos
- Ejemplos típicos del examen
### Costos, Facturación y Arquitectura
Puedes leer el contenido completo aquí: [Bloque_9_Costos_Facturacion_arquitectura.md)

