# Configuración Inicial de AWS: Root User, IAM Users y Roles
Este documento describe el proceso completo para crear una cuenta en AWS, configurar autenticación multifactor (MFA), crear usuarios IAM con buenas prácticas de seguridad y asignar permisos mediante políticas y roles.

## Cómo registrarse en AWS y activar la cuenta gratuita (Free Tier)
Para crear una cuenta en AWS y acceder al Free Tier , es necesario completar un proceso de registro que incluye verificación de identidad y método de pago. A continuación se describen los pasos:

### 1. Registro inicial

- Acceder a la página oficial de AWS: https://aws.amazon.com
- Seleccionar Create an AWS Account.
- Ingresar un correo electrónico válido y crear una contraseña.
- Completar los datos de contacto solicitados.

### 2. Verificación del método de pago (cargo de 1 €)
AWS requiere verificar que el usuario es real y que el método de pago es válido.Para ello:
- Se debe introducir una tarjeta de débito o crédito válida.
- AWS realizará un cargo temporal de aproximadamente 1 € (o equivalente en tu moneda).
- Este cargo no es un pago, es solo una verificación y se devuelve automáticamente.
- Este paso es obligatorio incluso si solo se va a usar la capa gratuita.

### 3. Verificación de identidad
AWS solicita un segundo paso de verificación:
- Se puede elegir SMS o llamada telefónica.
- Se introduce el código enviado para confirmar la identidad.

### 4. Selección del plan
AWS mostrará tres opciones:
- Basic (gratuito)
- Developer
- Business
- Para el Free Tier, se debe seleccionar Basic Support – Free.

### 5. Activación de la cuenta
Una vez completados los pasos:
- AWS activará la cuenta en unos minutos.
- Se podrá acceder a la consola de AWS.
- El usuario tendrá acceso a los servicios incluidos en el Free Tier, como EC2, S3, Lambda, DynamoDB, etc.

--- 
Nota importante sobre el Free Tier
El Free Tier permite usar muchos servicios sin coste durante x meses, pero:
- Algunos servicios no están incluidos. Si se superan los límites gratuitos, se generan cargos.

- Es recomendable activar Billing Alerts para evitar sorpresas. 
- ![1](imagenes/i_1.png)
- ![](imagenes/i_2.png)
- ![](imagenes/i_3.png)
- ![](imagenes/i_5.png)


- ## 1. Creación de la Cuenta AWS (Root User)
Acceder a la página de registro de AWS.

- Ingresar un correo electrónico para la cuenta root.
- Confirmar el correo mediante el enlace enviado por AWS.
- ![](imagenes/1.png)
- ![](imagenes/2.png)
- Completar la segunda capa de verificación (MFA).
- ![](imagenes/3.png)
- Seleccionar Authenticator App como método MFA.
- ![](imagenes/4.png)
- Escanear el código QR con la aplicación y escribir los códigos generados.
- ![](imagenes/5.png)
- ![](imagenes/6.png)
--- 
Confirmar el mensaje: “Successfully assigned this virtual MFA device”.
- ![](imagenes/7.png)

## 2. Acceso al Servicio IAM
Dentro de la consola de AWS, buscar IAM desde la barra superior.
- Entrar al panel de IAM.
- seleccionar Users.
- ![](imagenes/8.png)
- ![](imagenes/9.png)
- Crear un usuario IAM siguiendo buenas prácticas:
  - La cuenta root solo debe usarse para tareas críticas.
  - El trabajo diario debe hacerse con usuarios IAM.

## 3. Creación del Usuario Administrador (admin_briyit)
- En IAM → Users → Create user.
- ![](imagenes/10.png)
- ![](imagenes/11.png)
- crear persona
- ![](imagenes/12.png)
- Nombre del usuario: admin_briyit.
- Habilitar acceso a la consola de AWS.
- Seleccionar Contraseña personalizada.
- Desactivar la opción “El usuario debe cambiar la contraseña al iniciar sesión”.
- ![](imagenes/13.png)
- En permisos, elegir:adjuntar politicas directamente
- ![](imagenes/14.png)
- Asignar la política:AdministratorAccess.
- ![](imagenes/15.png)
- ![](imagenes/16.png)
- Crear el usuario.
- ![](imagenes/17.png)
- ![](imagenes/18.png)


AWS proporcionará:

- URL de inicio de sesión para IAM
- Archivo CSV con credenciales (descargarlo)
- ![](imagenes/19.png)
- Cerrar sesión del root user.
- ![](imagenes/20.png)

## 4. Inicio de Sesión con admin_briyit
Iniciar sesión usando la URL proporcionada para IAM.

- ![](imagenes/21.png)
- ![](imagenes/22.png)
- Notar que este usuario no puede ver costos ni facturación, ya que esos permisos son exclusivos del root user.
- ![](imagenes/23.png)

## 5. Asignar Permisos de Facturación al Usuario Administrador
- Iniciar sesión nuevamente como root user.
- ![](imagenes/24.png)
- Ir a IAM → Users → admin_briyit.
- Buscar la sección: IAM user and role access to billing information.
- ![](imagenes/25.png)
- ![](imagenes/26.png)
- ![](imagenes/27.png)
- Activar el acceso.
- ![](imagenes/28.png)
- Guardar los cambios.
- Cerrar sesión del root user.
- ![](imagenes/29.png)
- Iniciar sesión con admin_briyit y verificar que ahora puede ver:
  - Costos del mes
  - Desglose de facturación
  - Cost Explorer
- ![](imagenes/30.png)

## 6. Creación del Usuario Desarrollador (desarrollador_s3)
En IAM → Users → Create user.
- ![](imagenes/31.png)
- Nombre del usuario: desarrollador_s3.
- ![](imagenes/32.png)
- Habilitar acceso a la consola.
- Contraseña personalizada.
- ![](imagenes/33.png)
- ![](imagenes/34.png)
- Asignar la política:AmazonS3ReadOnlyAccess.
- Crear el usuario.
- ![](imagenes/35.png)
- Descargar el archivo CSV con credenciales.

Este usuario será utilizado para proyectos que requieran acceso limitado a S3.

## 7. Creación de un Rol para EC2 con Acceso a S3
En IAM → Roles → Create role.
- ![](imagenes/36.png)
- ![](imagenes/37.png)

- Tipo de entidad confiable:AWS Service.
- Caso de uso: EC2.
- ![](imagenes/38.png)

- Adjuntar la política: AmazonS3ReadOnlyAccess.
- ![](imagenes/39.png)
- Continuar con la configuración por defecto.
- ![](imagenes/40.png)
- Crear el rol.
- Este rol permitirá que instancias EC2 accedan a S3 con permisos de solo lectura.
- ![](imagenes/41.png)

## 8. Resumen de Buenas Prácticas Aplicadas
- La cuenta root solo se usa para tareas críticas.
- Se creó un usuario administrador con permisos completos.
- Se creó un usuario desarrollador con permisos limitados.
- Se configuró MFA para la cuenta root.
- Se asignaron permisos de facturación al usuario administrador.
- Se creó un rol para EC2 con acceso restringido a S3.
- Se descargaron las credenciales de cada usuario de forma segura.

## 9. Conclusión
Este procedimiento establece una estructura segura y profesional para trabajar en AWS.
Permite separar responsabilidades, proteger la cuenta root y asignar permisos mínimos necesarios para cada usuario o servicio.
