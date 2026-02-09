# Redes Avanzadas Nivel 1

En este sub_bloque nos centraremos en las redes avanzadas en AWS.

## 1. Endpoints
Conecta tu VPC con servicios de AWS sin usar internet.

**Gateway Endpoint**
- Se usan solo para S3 y DynamoDB
- Se configura en la route table
- Evita usar NAT/IGW
- Su uso es gratuito
- No pasa por internet
- No usa DNS privado, porque no crea ninguna ENI

**Interface Endpoint** (PrivateLink)
- Sirve para otros servicios de AWS (SSM, Secrets Manager, EC2 API, CloudWatch…) y servicios de terceros
- Crea una **ENI** en tu subnet
- Tiene costo
- Cobra por hora de la **ENI**y por tráfico
- Sí usa DNS privado para resolver el servicio dentro de la VPC


Lo que cobra AWS es:
la ENI creada por el endpoint
el tráfico que pasa por ella

No cobras por “usar el servicio”, sino por tener esa interfaz privada activa.

## 2. ENI (Elastic Network Interface)
Una ENI es la tarjeta de red virtual de una instancia EC2.

Lo importante:

- Puedes tener varias ENIs en una instancia
- Puedes mover una ENI de una instancia a otra
- Mantiene su IP privada, sus security groups y su configuración
- Esto permite alta disponibilidad
  
Muchos servicios crean ENIs sin que tú lo veas directamente:
Interface Endpoints, Lambda en VPC, ECS awsvpc, RDS Proxy, EKS, etc.

**IP elástica (EIP)**

- Es una IP pública fija
- Se asocia a una ENI
- Tiene costo solo cuando no la usas
- Son IPv4 porque son escasas

## 3. VPC Peering
Es la forma de conectar dos VPC de manera privada, como si fueran una sola red.

Características:
- Es punto a punto
- No hay trafico VPC A ↔ VPC B funciona, pero A → B → C no.

**Requiere rutas en ambas VPC:**
Crear el peering no basta, hay que actualizar las route tables.
- No usa internet
- No usa NAT
- No usa IGW

Las VPC no pueden tener CIDR solapados

Sirve para:
Que máquinas de una VPC accedan a máquinas de otra
Compartir servicios internos

## 4. Transit Gateway
Es como un “super‑peering”.

Diferencias con peering:

- Permite conectar muchas VPC entre sí
- Sí permite tránsito
- Es más escalable
- Se usa en arquitecturas grandes

Peering → conexión simple
Transit Gateway → conexión masiva

## El Bastion Host 
El Bastion Host es la forma clásica de entrar a una instancia privada cuando no quieres exponerla a internet.

Es básicamente una EC2 pública que actúa como puente para acceder a las EC2 privadas.
Es como una puerta de entrada controlada.

Cómo funciona:

- Te conectas por SSH (22) al bastion
- Desde el bastion entras a las EC2 privadas
- Las EC2 privadas no tienen IP pública
- No abres puertos al mundo, solo al bastion

Reglas importantes:

- El bastion va en subnet pública
- El backend o EC2 va en subnet privada
- El puerto 22 del bastion debe estar abierto solo para tu IP, nunca 0.0.0.0/0
- El backend no debe tener el puerto 22 abierto a internet

Por qué se sigue usando:

- Sistemas antiguos
- Empresas que aún no migran a SSM
-Arquitecturas donde SSH sigue siendo obligatorio


## Relación Bastion Host / SSM 

**SSM Session Manager** es como la versión moderna del bastion host.

- SSM no necesita abrir puertos
- No necesita SSH
- No necesita IP pública
- No necesita internet
- Es más seguro

Solo necesitas tener el agente SSM y permisos IAM

|Característica	|Bastion Host	|SSM Session Manager|
|:--|:--|:--|
|Tipo de acceso|	SSH directo	|Acceso sin SSH|
|Necesita IP pública|	Sí |	No |
|Necesita abrir puertos|	Sí (22)	|No|
|Seguridad|	Menos seguro (exposición del puerto 22)|	Más seguro (sin puertos abiertos)|
|Requisitos |	EC2 pública + SG	|Agente SSM + permisos IAM |
|Uso típico	|Sistemas antiguos o entornos donde SSH es obligatorio	|Arquitecturas modernas y seguras|
|Subnet |	Pública|	Privada o pública|
|Dependencia de internet |	Sí, para conectarte al bastion|	No, funciona sin internet|
|Mantenimiento|	Alto (actualizar, proteger, monitorear)|	Bajo (gestionado por AWS)|



## 6. Errores típicos

- Crear un endpoint pero no asociarlo a la route table
- Crear un peering pero no añadir rutas
- Intentar usar NAT o IGW para conectar VPCs (incorrecto)
- Intentar que el peering haga tránsito (A → B → C)
