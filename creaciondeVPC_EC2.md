# CREACIÓN DE VPC Y EC2 
El objetivo de este ejercicio es familiarizarse con el entorno de AWS , para ello vamos a crear nuestra VPC y lanzaremos nuestra primera EC2 , ya que en el ejercicio anterior creamos usuarios y role para EC2.

 ## El Orden Lógico de Construcción (The "Cloud Blueprint")
1. Capa de Identidad (IAM): (Lo que hicimos en el ejercicio anterior ). Primero defines quién tiene permiso. Creas los Roles, Grupos y Usuarios.
2. Capa de Red (VPC): (Nivel 5). Diseñas tu red,subnets, la puerta de entrada (Internet Gateway) y las reglas de tráfico (Route Tables). Sin red, no hay nada.
3. Capa de Seguridad de Red (Security Groups): Antes de meter la máquina, . Definir qué puertos están abiertos (ej. el puerto 80 para web o 22 para SSH).
4. Capa de Cómputo (EC2): (Nivel 3). lanzar la máquina dentro de la red creada y se cuelga el Rol que se hizo en el paso 1.
5. Capa de Almacenamiento (S3): (Nivel 4). Crear el Bucket para guardar los datos que tu EC2 va a procesar.
6. Capa de Automatización (CLI/CloudFormation): (Nivel 6).  

- Es importante saber crear tu propia VPC asi tendras mas control en la seguridad

---

## El Objetivo del Proyecto: "La Fortaleza Segura"
Escenario: Quieres una computadora en la nube (EC2) que sea capaz de guardar y leer archivos de un baúl digital (S3), pero que nadie en internet pueda entrar a ella, excepto tú desde tu casa.

### La Arquitectura que vamos a construir:
- `Red (VPC)`: Un espacio privado y aislado en la nube.
- `Subnet Pública`: un IP que te permite conectar a  Internet.
- `Internet Gateway`: La puerta principal que permite que entre y salga internet.
- `Security Group` (Firewall): Define los parámetros de que IP puede entrar
- `Instancia EC2` (T2.micro T3.micro): Tu computadora (gratis).
- `IAM Role`: El permiso que se asignaron a la computadora para que hable con S3.
- `VPC`: Crear una VPC es gratis.
- `Subnets e Internet Gateway`: Son gratis.
- `EC2`: Usaremos la t2.micro (o t3.micro según tu región), que te da 750 horas al mes gratis.
- `S3`: Los primeros 5 GB son gratis.
- `Cuidado con`: El "NAT Gateway". Es una herramienta de red que cobra por hora. NO la usaremos. Usaremos alternativas gratuitas para nuestro laboratorio.

### Paso 1: Creación de la VPC
vamos a la consola.
- Busca VPC en el buscador de arriba.
- ![](imagenes/ii_1.png)
- ![](imagenes/ii_2.png)
- Dale al botón naranja Create VPC.
- ![](imagenes/ii_3.png)
- Selecciona la opción solo la VPC (importante para aprender manualmente).
- ![](imagenes/ii_4.png)
- Name tag: VPC-Laboratorio-Briyit
- IPv4 CIDR block: Escribe 10.0.0.0/16.
- ![](imagenes/ii_5.png)
-  Create VPC.
-  ![](imagenes/ii_6.png)
  
- Ahora mismo es una caja vacía, sin salida a internet y sin subnets. El siguiente paso es dividir este espacio."
#### Nombres de host de DNS: Desactivado
Si no se activa cuando crees tu computadora (EC2), AWS no le dará un nombre público (tipo ec2-54-xx-xx.compute-1.amazonaws.com). Solo tendrá una IP numérica.
Por qué lo queremos activado: Es mucho más fácil entrar a tu servidor usando un nombre que recordando una cadena de números.

- **Cómo activarlo**
- Arriba a la derecha de tu pantalla, haz clic en el botón naranja Acciones (Actions).
- Selecciona Editar configuración de la VPC (Edit VPC settings).
- ![](imagenes/ii_7.png)
-  check en Habilitar nombres de host de DNS (Enable DNS hostnames) y márcalo.
- Click a Guardar.

## Creación de subnets

- En el menú de la izquierda (debajo de VPC), clic en Subnets.
- Create subnet.
- ![](imagenes/ii_8.png)
- Selecciono tu VPC (VPC-LAB-BRIYIT).
- ![](imagenes/ii_9.png)
- subredes selecionamos subnet-Publica-1
- Zona de disponibilidad elegimos la primera opción
- ![](imagenes/ii_10.png)
- ![](imagenes/ii_11.png)
- realizamos la particion correspontiente 10.0.1.0/24
- click en guardar cambios 
- ![](imagenes/ii_12.png)
- En la subnet creada en la barra superopr en el boton de acciones le damos click en configurarción de subred
- selecionamos la opcion de habilitar la asignación automatica de IP
- ![](imagenes/ii_13.png)
- ![](imagenes/ii_14.png)
- Si no hicieras esto, tendrías que asignar IPs manuales (Elastic IPs) cada vez, lo cual es más lento y a veces tiene costo si no se usan bien.

## Internet Gateway (IGW)
- En el menú de la izquierda, buscar Puertas de enlace de Internet (Internet Gateways).
- ![](imagenes/ii_15.png)
- Clic en el botón naranja Crear puerta de enlace de Internet.
- Poner un nombre: IGW-Laboratorio-Briyit.
- ![](imagenes/ii_16.png)
- Click en Crear.
- clic en el botón Acciones.
- Conectar a VPC (Attach to VPC).
- ![](imagenes/ii_17.png)
- Elige la VPC-LAB-BRIYIT de la lista.
- ![](imagenes/ii_18.png)
- Click en conectar gateway de Internet.
- ![](imagenes/ii_19.png)


## Configurar la Tabla de Enrutamiento (Route Table)
- En el menú de la izquierda, clic en Tablas de enrutamiento (Route Tables).
- ![](imagenes/ii_20.png)
- Buscar la que pertenece a tu VPC (VPC-LAB-BRIYIT o el ID).
- ![](imagenes/ii_21.png)
- Haz clic en el ID de la tabla de enrutamiento (el link azul que empieza por rtb-...).
- Ve a la pestaña de abajo que dice Rutas (Routes) click en el botón Editar rutas (Edit routes).
- ![](imagenes/ii_22.png)
- Editar rutas.
- Destino: 0.0.0.0/0 (Esto le dice a la red: "Para cualquier lugar que no sea esta oficina, mira hacia aquí").
- ![](imagenes/ii_23.png)
- Target (Destino): Selecciona Internet Gateway y IGW-Laboratorio-Briyit.
- ![](imagenes/ii_24.png)
- Click en Guardar cambios.
- ![](imagenes/ii_25.png)

## EC2
- EC2 en la barra superior.Vamos a "Lanzar una instancia" (crear tu servidor virtual).
- ![](imagenes/ii_26.png)
- ![](imagenes/ii_27.png)
- Nombre:Servidor-Web-01.
- ![](imagenes/ii_28.png)
- Imagen de Amazon (AMI): Amazon Linux 2023 (es la que viene marcada por defecto y entra en la Capa Gratuita)
- Tipo de instancia: Asegúrate de que diga t3.micro (en Estocolmo, la t3.micro suele ser la gratuita).
- Par de claves (Key pair): "Crear nuevo par de claves".
- ![](imagenes/ii_29.png)
- Ponle un nombre (ej: llave-estocolmo).
 - Formato de archivo de clave privada:
 -  Tipo de par de claves: RSA. Es el estándar más compatible.
 - Elegimos .pem
 - Si vas a usar la terminal de Windows (PowerShell) o Mac/Linux, elige .pem.
 - Si prefieres usar el programa PuTTY, elige .ppk.
 - crear par de llaves
- Descárgala y guárdala. Si la pierdes, no podrás entrar a tu servidor.

## Segurity Groups 
- Por defecto, viene activado el SSH (Puerto 22) para que tú entres desde tu terminal.
- ![](imagenes/ii_30.png)
- Origen:  "Mi IP" (My IP). Así solo TÚ desde tu casa podrás intentar entrar, y nadie más en el mundo.
- ![](imagenes/ii_31.png)
- En el desplegable de Red, selecciona  vpc-0184236ea18afbd80 | VPC-LAB-BRIYIT.
- En Subred, seleccionar  Subnet-Publica-1.
- Confirmar que  Asignar automáticamente la IP pública esté en Habilitar.
- Click en lanzar instancia
- ![](imagenes/ii_32.png)
- ![](imagenes/ii_33.png)
- ![](imagenes/ii_34.png)
- En el servidor Web que creamos lo vamos a detener para no consumir recursos
- ![](imagenes/ii_35.png)
- Seleccionamos la instancia en la barra superior encontramos Estado de instancia damos click 
- Seleccionamos DEtener Instancia 
- ![](imagenes/ii_36.png)
- ![](imagenes/ii_37.png)
- Click en detener instancia 

  








