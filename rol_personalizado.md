# Laboratorio: Modificación de Roles IAM y Prueba de Acceso S3 desde EC2
**Objetivo**: Aprender a modificar permisos IAM, crear políticas personalizadas y validar el acceso desde una instancia EC2 mediante AWS CLI.

## 1. Preparación del entorno
### 1.1 Encender la instancia EC2
- Ir a la consola de AWS → EC2.
- Seleccionar la instancia.
- Hacer clic en Iniciar.
- Esperar hasta que el estado sea running (verde).

### 1.2 Actualizar la IP en el Security Group
Como la IP pública cambia cada día si apagas la EC2:
- Ir a Security Groups.
- Editar la regla de entrada del puerto 22 (SSH).
- Seleccionar My IP para actualizarla.
- Guardar los cambios.
- estos pasos se pueden encontrar en [creaciondeVPC_EC2](creaciondeVPC_EC2)

### 1.3 Conectarse por SSH
En Windows:

```bash
cd C:\Users\Usuario\aws
ssh -i llave-briyit.pem ec2-user@IP_PUBLICA
```

## 2. Modificar el Rol IAM para añadir permisos de escritura
### 2.1 Ubicar el rol existente
- Ir a IAM → Roles.
- ![](imagenes/a1.png)
- Buscar el rol: Rol_EC2_S3_SoloLectura.
- ![](imagenes/a2.png)
- Entrar en la pestaña Permisos.

## 3. Crear una Política Administrada Personalizada (Lectura + Escritura)
### 3.1 Crear la política
- Ir a IAM → Políticas → Crear política.
- Seleccionar Servicio: S3.
- ![](imagenes/a3.png)
- Marcar las acciones necesarias:
  -  ListBucket → ver contenido
  -  GetObject → leer/descargar
  -  PutObject → subir archivos

- ![](imagenes/a4.png)
- ![](imagenes/a5.png)


### 3.2 Restringir la política al bucket específico
En Recursos → Añadir ARN:

Bucket
Bucket name: laboratorio-briyit-2026

- ![](imagenes/a6.png)
  
Objetos
Bucket name: laboratorio-briyit-2026

Object name: marcar Any (*)

- ![](imagenes/a7.png)

### 3.3 Verificar la política en JSON
Debe verse así:

- 
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "s3:ListBucket"
            ],
            "Resource": "arn:aws:s3:::laboratorio-briyit-2026"
        },
        {
            "Effect": "Allow",
            "Action": [
                "s3:GetObject",
                "s3:PutObject"
            ],
            "Resource": "arn:aws:s3:::laboratorio-briyit-2026/*"
        }
    ]
}
```

- ![](imagenes/a8.png)
### 3.4 Guardar la política
Politica_S3_LecturaEscritura_Briyit

- Crear política.
- ![](imagenes/a9.png)

## 4. Adjuntar la política al rol EC2
- Ir a IAM → Roles.
- Abrir Rol_EC2_S3_SoloLectura.
- ![](imagenes/a10.png)
- Clic en Agregar permisos → Adjuntar políticas.
- Buscar Politica_S3_LecturaEscritura_Briyit.
- ![](imagenes/a11.png)
- Seleccionarla y agregarla.
- ![](imagenes/a12.png)
##  Retirar la política de solo lectura
- Ubicar AmazonS3ReadOnlyAccess.
- Hacer clic en Quitar (o la X).


## 5. Justificación técnica (Buenas prácticas AWS)
Se utilizó una Política Administrada por el Cliente en lugar de una política Online porque:
- **Reutilización**: puede aplicarse a futuras EC2 sin reescribir JSON.
- **Orden**: IAM mantiene una librería clara de permisos.
- **Seguridad**: si se corrige la política, el cambio se replica en todos los recursos que la usan.
- **Auditoría**: más fácil de revisar y versionar.

## 6. Prueba final desde la EC2
### 6.1 Subir un archivo al bucket
En la terminal de la instancia:

```bash
aws s3 cp prueba.txt s3://laboratorio-briyit-2026/
```
- ![](imagenes/a13.png)

A parece un error, reviso que el nombre del bucket esté bien escrito.
(En este caso faltaba una r).
- ![](imagenes/a14.png)
- verificamos si el archivo esta en S3
- ![](imagenes/a15.png)
- ![](imagenes/a16.png)
- ![](imagenes/a17.png)
- ![](imagenes/a18.png)
- ![](imagenes/a19.png)



### 6.2 Prueba de seguridad: intentar borrar el archivo
Para validar si la politica permite borrar algun archivo :

```bash
aws s3 rm s3://laboratorio-briyit-2026/prueba.txt
```
- ![](imagenes/a20.png)
- Si falla, la política está correctamente restringida.
- Si funciona, significa que la  política permite DeleteObject y deberías revisarl
