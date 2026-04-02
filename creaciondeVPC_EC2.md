# CREACIÓN DE VPC Y EC2 
El objetivo de este ejercicio es familiarizarse con el entorno de AWS , para ello vamos a crear nuestra VPC y lanzaremos nuestra primera EC2 , ya que en el ejercicio anterior creamos usuarios y role para EC2.

 ## El Orden Lógico de Construcción (The "Cloud Blueprint")
1. Capa de Identidad (IAM): (Lo que hicimos en el ejercicio anterior ). Primero defines quién tiene permiso. Creas los Roles, Grupos y Usuarios.
2. Capa de Red (VPC): (Nivel 5). Diseñas tu red,subnets, la puerta de entrada (Internet Gateway) y las reglas de tráfico (Route Tables). Sin red, no hay nada.
3. Capa de Seguridad de Red (Security Groups): Antes de meter la máquina, . Define qué puertos están abiertos (ej. el puerto 80 para web o 22 para SSH).
4. Capa de Cómputo (EC2): (Nivel 3). lanzas la máquina dentro de la red creada y le cuelgas el Rol que hiciste en el paso 1.
5. Capa de Almacenamiento (S3): (Nivel 4). Creas el Bucket para guardar los datos que tu EC2 va a procesar.
6. Capa de Automatización (CLI/CloudFormation): (Nivel 6). Una vez que sabes hacer todo a mano, aprendes a hacerlo con código para repetirlo para ser más eficiente 

- Es importante saber crear tu propia VPC asi tendras mas control en la seguridad

---

## El Objetivo del Proyecto: "La Fortaleza Segura"
Escenario: Quieres una computadora en la nube (EC2) que sea capaz de guardar y leer archivos de un baúl digital (S3), pero que nadie en internet pueda entrar a ella, excepto tú desde tu casa.

### La Arquitectura que vamos a construir:
- Red (VPC): Un espacio privado y aislado en la nube.
- Subnet Pública: un IP que te permite conectar a  Internet.
- Internet Gateway: La puerta principal que permite que entre y salga internet.
- Security Group (Firewall): Define los parámetros de que IP puede entrar
- Instancia EC2 (T2.micro T3.micro): Tu computadora (gratis).
- IAM Role: El permiso que se asignaron a la computadora para que hable con S3.
- VPC: Crear una VPC es gratis.
- Subnets e Internet Gateway: Son gratis.
- EC2: Usaremos la t2.micro (o t3.micro según tu región), que te da 750 horas al mes gratis.
- S3: Los primeros 5 GB son gratis.
- Cuidado con: El "NAT Gateway". Es una herramienta de red que cobra por hora. NO la usaremos. Usaremos alternativas gratuitas para nuestro laboratorio.

### Paso 1: Creación de la VPC
vamos a la consola.
- Busca VPC en el buscador de arriba.
- Dale al botón naranja Create VPC.
- Selecciona la opción "VPC only" (importante para aprender manualmente).
- 1
- Name tag: VPC-Laboratorio-Briyit
- 2
- IPv4 CIDR block: Escribe 10.0.0.0/16.
- 3
. Create VPC.
  4
  5
  6
- Ahora mismo es una caja vacía, sin salida a internet y sin subnets. El siguiente paso es dividir este espacio."
#### Nombres de host de DNS: Desactivado
Si no se activa cuando crees tu computadora (EC2), AWS no le dará un nombre público (tipo ec2-54-xx-xx.compute-1.amazonaws.com). Solo tendrá una IP numérica.
Por qué lo queremos activado: Es mucho más fácil entrar a tu servidor usando un nombre que recordando una cadena de números.

**Cómo activarlo**
- Arriba a la derecha de tu pantalla, haz clic en el botón naranja Acciones (Actions).
- Selecciona Editar configuración de la VPC (Edit VPC settings).
- 7
- Busca el check que dice Habilitar nombres de host de DNS (Enable DNS hostnames) y márcalo.
- 8
- Dale a Guardar.

## Creación de subnets

En el menú de la izquierda (debajo de VPC), haz clic en Subnets.
Dale a Create subnet.
Selecciona tu VPC (VPC-LAB-BRIYIT).
Nombre: Subnet-Publica-Briyit.
Zona de disponibilidad: Elige us-east-1a (o la que prefieras, pero anótala).
Bloque CIDR IPv4: Escribe 10.0.1.0/24.


en processo ...
