# Bloque 2 – Redes en AWS

En este bloque se estudian los componentes fundamentales de red en AWS: VPC, Subnets, NAT, IGW, Route Tables, Security Groups, NACLs y Endpoints.  
El objetivo es entender cómo fluye el tráfico dentro de una VPC y cómo se conectan los distintos servicios.

---

## 1. VPC (Virtual Private Cloud)

Es tu red privada dentro de AWS. Tú controlas:

- Rangos de direcciones IP (CIDR)  
- Subnets  
- Route Tables  
- Security Groups  
- NACLs  
- Gateways (IGW, NAT, VPG, etc.)

Una VPC es como tu “datacenter” dentro de AWS.

---

## 2. Subnets

Son divisiones dentro de la VPC. Cada subnet está en una sola Availability Zone.

### Tipos de subnets

**Pública**
- Tiene una ruta hacia un Internet Gateway (IGW).  
- Las instancias pueden tener IP pública.

**Privada**
- No tiene ruta directa al IGW.  
- Puede tener ruta hacia un NAT Gateway para salir a Internet sin exponerse.

**Aislada**
- Sin IGW y sin NAT.  
- No tiene salida a Internet.  
- Se usa para datos sensibles o sistemas internos.

---

## 3. NAT (Network Address Translation)

Permite que recursos en subnets privadas salgan a Internet, pero sin recibir tráfico entrante.

- Solo permite tráfico de salida  
- Usa una IP pública  
- Se coloca en una subnet pública  
- Las instancias privadas salen a Internet usando la IP del NAT

Importante:  
Una instancia en subnet pública **sin IP pública** NO puede salir a Internet.

---

## 4. Internet Gateway (IGW)

Componente que permite la conexión entre la VPC e Internet.

- Se asocia a la VPC  
- Para usarlo necesitas una IP pública o una Elastic IP  
- AWS cobra por IPv4 públicas no utilizadas  
- IPv6 no tiene coste adicional

**Elastic IP**
- IP pública estática  
- No cambia al reiniciar  
- Se puede reasignar a otra instancia

---

## 5. Route Tables

Definen por dónde viaja el tráfico dentro de la VPC.

- Cada subnet está asociada a una Route Table  
- Las rutas determinan si la subnet es pública, privada o aislada  

Ejemplos:

- `0.0.0.0/0 → IGW` → subnet pública  
- `0.0.0.0/0 → NAT` → subnet privada  

---

## 6. Security Groups y NACLs

### Security Group (SG)
- Firewall a nivel de instancia  
- **Stateful**: si permites entrada, la salida correspondiente se permite sola  
- Por defecto permite todas las salidas  
- No permite reglas de “deny”, solo “allow”

### NACL (Network ACL)
- Firewall a nivel de subnet  
- **Stateless**: debes permitir entrada y salida explícitamente  
- Permite “allow” y “deny”  
- Se evalúan en orden numérico

**Resumen:**
- **Stateful** = recuerda la conexión  
- **Stateless** = no recuerda nada  

---

## Puertos esenciales

| Servicio | Puerto |
|:--|:--|
| HTTP | 80 |
| HTTPS | 443 |
| SSH | 22 |
| RDP | 3389 |
| MySQL | 3306 |
| PostgreSQL | 5432 |

---

## Flujo de tráfico

El flujo lo determinan:

- La route table de la subred  
- Los security groups  
- Las NACLs  
- Y, si aplica, un Load Balancer

### Application Load Balancer (ALB)

- Servicio regional  
- Recibe tráfico desde Internet (si está en subnet pública)  
- Reparte tráfico hacia instancias en subnets privadas  
- Permite alta disponibilidad y escalabilidad  
- Tiene coste por hora + por LCU  

Las instancias EC2 se conectan según:

- Su IP pública o privada  
- La ruta definida en la route table  
- El puerto permitido en el SG  
- El destino (base de datos, Internet, S3, etc.)

---

## Endpoints

Un **VPC Endpoint** permite que tu VPC se conecte a servicios de AWS **sin usar Internet**, sin NAT y sin IP pública.

### Tipos de endpoints

**Gateway Endpoint**  
Para S3 y DynamoDB  
- Gratis  
- Se añade a la route table  
- El tráfico nunca sale a Internet

**Interface Endpoint (PrivateLink)**  
Para servicios como: Secrets Manager, SSM, CloudWatch, SNS, SQS, EC2 API  
- Tiene coste  
- Crea ENIs privadas dentro de tu VPC

**ENI (Elastic Network Interface):**  
Es la tarjeta de red de una EC2.

---

# Ejercicios de flujo

## Ejercicio 1 — Usuario → ALB → Backend

1. Usuario en Internet  
2. Llega al ALB (subnet pública)  
3. El ALB reenvía al backend (subnet privada)

**Puertos:**

- Usuario → ALB: **80 o 443**  
- ALB → Backend: **80, 443, 8080, 3000…** (según la app)

---

## Ejercicio 2 — Backend privado → Internet

Una EC2 privada necesita instalar actualizaciones.

Ruta:

1. Backend (subnet privada)  
2. NAT Gateway (subnet pública)  
3. Internet Gateway  
4. Internet  

Sin NAT, una instancia privada no puede salir a Internet.

---

## Ejercicio 3 — Backend → Base de datos

Backend en subnet privada.  
Base de datos en subnet aislada.

Puertos:

- MySQL → **3306**  
- PostgreSQL → **5432**

---

## Ejercicio 4 — Backend → S3 sin Internet

Backend en subnet privada.  
No quieres usar NAT.

Recurso necesario:

**Gateway Endpoint para S3**

---

## Ejercicio 5 — EC2 pública sin Internet

Tienes:

- EC2 en subnet pública  
- Tiene IP pública  
- Route table con IGW  
- Pero no tiene Internet

Posibles causas:

- Security Group bloquea tráfico saliente  
- NACL bloquea tráfico entrante o saliente  
- La IP pública no está realmente asociada  
- Falta la ruta `0.0.0.0/0 → IGW` en la route table  

