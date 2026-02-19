# Bloque 9 — Costos, Facturación y Arquitectura (Billing, Pricing, Well‑Architected)
Este bloque explica cómo se calculan los costos en AWS, cómo controlar el gasto y cómo aplicar buenas prácticas de arquitectura. Es un bloque teórico pero muy importante en el examen AWS CCP, porque AWS quiere que entiendas cómo se paga, cómo se optimiza y cómo se diseña correctamente en la nube.

## Principios de precios en AWS
AWS usa tres principios básicos:

**1. Pay as you go (paga por uso)**
Pagas solo por lo que consumes.
**Ejemplos:**
- EC2 → por hora/segundo
- Lambda → por invocación
- S3 → por GB almacenado

**2. Pay less when you use more (descuentos por volumen)**
Mientras más usas, más barato sale.
**Ejemplo:** S3 baja de precio al aumentar los TB almacenados.

**3. Pay less when you commit (descuentos por compromiso)**
Si te comprometes a usar un servicio por 1 o 3 años, pagas menos.
Ejemplo: Reserved Instances, Savings Plans.

## Modelos de compra (muy preguntado)
**On‑Demand**
- Sin compromiso
- Más caro
- Ideal para cargas impredecibles
---
**Reserved Instances (RI)**
- Compromiso 1–3 años
- Hasta 72% más barato
- Para cargas estables
  
**Savings Plans**
- Igual que RI pero más flexible
- Te comprometes a gastar X USD/hora
- Funciona con EC2, Fargate y Lambda
---
**Spot Instances**
- Hasta 90% más baratas
- AWS puede quitarlas
- Para cargas interrumpibles (batch, ML, big data)

## Herramientas de costos
### Billing Dashboard
- Vista general de tu factura
- Cost Explorer
- Analiza gastos por:
  - servicio
  - región
  - cuenta
  - tiempo

### AWS Budgets
Alertas cuando te acercas a un límite de gasto.

**Ejemplo:**“Avísame si paso de 10€ este mes.”

### AWS Pricing Calculator
Calcula cuánto costará una arquitectura antes de crearla.

### TCO Calculator
Compara costos on‑premise vs AWS.

##  Free Tier (nivel gratuito)
Tres tipos:

|Tipo	| Ejemplo |
|:--|:--|
|Siempre gratis |	Lambda 1M invocaciones/mes |
|12 meses gratis |	EC2 t2.micro, S3 5GB |
|Pruebas puntuales	| Redshift 2 meses |

## Well‑Architected Framework
**Los 6 pilares:**

1. ****Operational Excellence**  :Automatización, monitoreo, operaciones.
2. **Security**  :Cifrado, IAM, protección de datos.
3. **Reliability**  :Alta disponibilidad, recuperación ante fallos.
4. **Performance** Efficiency  :Uso eficiente de recursos, escalado.
5. **Cost Optimization**  :Evitar gastos innecesarios.
6. **Sustainability**  :Impacto ambiental.

## Shared Responsibility Model
**AWS es responsable de:**
  - Seguridad DE la nube:
  - Centros de datos
  - Hardware
  - Red física
  - Hipervisor
  - Seguridad física

**Tú eres responsable de:**
  - Seguridad EN la nube:
  - Configuración de S3
  - Security Groups
  - IAM
  - Cifrado
  - Parches del SO en EC2

**Ejemplos típicos**
- “Un bucket S3 quedó público” → responsabilidad del cliente
- “¿Quién protege los centros de datos?” → AWS
- “¿Quién parchea EC2?” → cliente
- “¿Quién parchea Lambda?” → AWS

## Cost Optimization 
- Auto Scaling
- Spot Instances
- Savings Plans / RI
- S3 Lifecycle (mover datos a Glacier)
- Eliminar recursos no usados (EBS, IPs, snapshots)
- Usar CloudFront para reducir costos de transferencia

##  Organizations y Control Tower
AWS Organizations
  - Gestionar múltiples cuentas

**Consolidar facturación**

-**Aplicar políticas (SCPs)**
  - Políticas que bloquean acciones a nivel de organización
  - No dan permisos, solo restringen
  - Se aplican a cuentas completas
  - Separar entornos
  Usar cuentas distintas para prod, dev, test
  - Aumenta seguridad y control
  - Se gestiona con AWS Organizations
  Separar entornos (prod, dev, test)

**AWS Control Tower**
Crea una estructura multi‑cuenta con buenas prácticas
  - Configura landing zones
  - Automatiza gobernanza

## Cost Allocation Tags
Etiquetas que permiten asignar costos a:
  - proyectos
  - equipos
  - departamentos
Ejemplo:
**Environment**= Production  
**Project** = App1

## Costos de transferencia de datos
**Gratis**
  - Tráfico dentro de la misma AZ
  - Tráfico entre servicios dentro de la misma región (en muchos casos)

**Se cobra**
  - Tráfico hacia Internet
  - Tráfico entre regiones
  - Tráfico entre VPCs (dependiendo del método)

## Preguntas típicas del examen
- “¿Qué herramienta te avisa si te acercas a un límite de gasto?” → `AWS Budgets`
- “¿Qué herramienta analiza gastos históricos?” → `Cost Explorer`
- “¿Qué opción es más barata para cargas interrumpibles?” → `Spot Instances`
- “¿Qué pilar se enfoca en ahorrar dinero?” → `Cost Optimization`
- “¿Quién parchea el SO en EC2?” → `El cliente`
- “¿Qué herramienta calcula costos antes de desplegar?” → `Pricing Calculator`
