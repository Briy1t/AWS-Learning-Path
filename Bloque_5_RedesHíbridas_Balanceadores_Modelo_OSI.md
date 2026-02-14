# Bloque 5 Redes Híbridas, Balanceadores, Auto Scaling y Modelo OSI

Este bloque reúne los conceptos que conectan con AWS, además de cómo se distribuye tráfico,
cómo se escalan instancias y cómo funcionan las capas de red.

## 1. VPN Site‑to‑Site

Una **VPN Site‑to‑Site** conecta tu infraestructura local con AWS mediante un túnel cifrado.
  - Usa **IPsec**: para cifrar los datos.
  - La latencia depende de Internet.
  - Es más económica que **Direct Connect**
  - Ideal para oficinas pequeñas o medianas
  - Sirve para unir tu servidor local con AWS de forma segura.

## 2. Direct Connect

Direct Connect es una conexión física entre tu empresa y AWS.
  - **No** usa Internet
  - Latencia muy baja
  - Ancho de banda alto
  - Más costoso
  - Instalación más lenta
  - Puedes combinarlo con **VPN** para doble seguridad.

**Tabla comparativa**
| Característica |	VPN	Direct | Connect |
|:--|:--|:--|
| Usa Internet |	Sí| No |
| Cifrado	| Sí (IPsec) |	Opcional |
| Latencia	| Variable |	Muy baja |
| Ancho de banda |	Limitado | Alto |
| Costo |	Bajo| Alto |
| Instalación	| Rápida | Lenta |
| Uso típico |	Pymes	| Grandes empresas |

## 3. Arquitectura básica de VPC

```text
VPC
├── Subnet Pública (con IGW)
│     └── Load Balancer
│
├── Subnet Privada (con NAT Gateway)
│     └── EC2 / Fargate / App
│
└── Subnet Privada (sin Internet)
      └── RDS / DynamoDB ```

Ejemplo con CIDR:


VPC 10.0.0.0/16
│
├── Subnet Pública (10.0.1.0/24)
│      Route Table:
│      0.0.0.0/0 → IGW
│
└── Subnet Privada (10.0.2.0/24)

       Route Table:
       0.0.0.0/0 → NAT Gateway
```
## 4. Componentes de EC2

| Componente |	Qué es |	Qué guarda |
|:--|:--|:--|
|AMI	| Plantilla del SO	| SO + configuraciones |
|EBS	| Disco duro |	Archivos, apps, contenedores |
|ENI	| Tarjeta de red	| IP privada/pública |
|Instancia |	Máquina virtual |	Usa AMI + EBS + ENI |


## 5. Dónde va cada componente en la VPC

| Componente |	Dónde va	| Por qué |
|:--|:--|:--|
| ALB |	Subnet pública |	Recibe tráfico de Internet |
|EC2	| Subnet privada |	Seguridad |
|NAT Gateway |	Subnet pública |	Permite salida a Internet desde privadas |
|ALB → EC2 |	Comunicación interna |	No necesita NAT |


## 6. User Data

Script que se ejecuta al iniciar la instancia.
Sirve para:
  - instalar software
  - configurar servicios
  - automatizar despliegues
  - iniciar aplicaciones
Es clave para automatizar servidores sin intervención humana.

## 7. Modelo OSI (las 7 capas)

| Capa |	Nombre |	Qué hace |	Relevancia en AWS |
|:--|:--|:--|:--|
|7 |	Aplicación|	HTTP, HTTPS, APIs	ALB,| routing avanzado |
|6 |	Presentación |	Formatos, cifrado	| TLS/SSL |
|5 | Sesión |	Mantiene sesiones	| Cookies, WebSockets |
|4 |Transporte |	TCP/UDP |	NLB, puertos |
|3 | Red |IP | routing	VPC, subnets | route tables |
|2 |Enlace |	MAC, switches	| ENI, seguridad de red |
|1 |Física | Cables, señales	| Direct Connect |

Énfasis en examen
 - Capa 7 → ALB
 - Capa 4 → NLB


## Listeners

- 80 (HTTP)
- 443 (HTTPS)

## Target Group

Lista de destinos:
- EC2 (IPs privadas)
- Lambdas
- ECS Tasks

Incluye:
  - health checks
  - routing
  - integración con Auto Scaling


| Componente |	Función |
|:--|:--|
|ALB |	Recibe tráfico desde Internet |
|Target Group |Lista de instancias que reciben tráfico |
|EC2	| Procesan las peticiones |
|Health Check	|Verifica si están sanas|

## 8. Auto Scaling Group (ASG)
El ASG permite alta disponibilidad y escalado automático.
- Si una instancia falla, el ASG la reemplaza.

Tres valores clave:

- Minimum
- Desired
- Maximum

Puede escalar por:

  - CPU
  - tráfico
  - peticiones
  - métricas personalizadas

Flujo típico:

```text 
Internet
   ↓
ALB (público)
   ↓
TARGET GROUP
   ↓
AUTO SCALING GROUP
   ↓       ↓       ↓
 EC2 A   EC2 B   EC2 C

 ```


## 10. Resumen final del bloque

- Este bloque cubre:
- VPN y Direct Connect
- Arquitectura real de VPC
- Componentes de EC2
- Load Balancers (ALB y NLB)
- Target Groups
- Auto Scaling Group
- User Data
- Modelo OSI completo (con énfasis en capas 4 y 7)
