# EC2 Hardened Node Deployment
Proyecto completo de administración de servidores Linux en AWS: instalación y hardening de Apache, monitoreo, creación de AMI, plantillas de lanzamiento y Auto Scaling Group. 
Incluye script de autocuración y despliegue de un CV digital.
Autora: Briyit Rodríguez

##  Vista previa del resultado CV digital 

![](imagenes/_21.png)

## 1. Inicio del proyecto
Este proyecto combina dos líneas de trabajo:
- Elastic Compute Nodes (EC2): creación, administración y automatización de instancias Linux.
- Servidor Apache: instalación, configuración, hardening y despliegue de un sitio web (CV digital).

## 2. Acceso a la instancia EC2
### 2.1. Encender la instancia
Desde la consola de AWS:
- Seleccionar la instancia creada previamente
- Hacer clic en Start Instance
- ![](imagenes/_1.png)
- ![](imagenes/_2.png)
- ![](imagenes/_3.png)
  
### 2.2. Conexión desde Windows mediante SSH
- Abrir CMD
```Codigo
Windows + R
```
- cmd
- Ir a la carpeta donde está la clave .pem
```bash
cd Desktop
cd aws
```
Conectarse a la instancia
```bash
ssh -i "nombre.pem" ec2-user@IP_PUBLICA
```
- ![](imagenes/_4.png)
## 3. Instalación del servidor Apache
Una vez dentro de la instancia Amazon Linux 2023:

### 3.1. Intento inicial (incorrecto en Amazon Linux)
```bash
sudo apt update
```
(Imagen 5, 6)

### 3.2. Instalación correcta (familia Red Hat)
```bash
sudo dnf install httpd -y
```
7 
## 4. Administración del servicio Apache
- Iniciar Apache
```bash
sudo systemctl start httpd
```

- Comandos usados
  - Estado:

  ```bash
  sudo systemctl status httpd
  ```
  - Reiniciar:
  
  ```bash
  sudo systemctl restart httpd
  ```
  
  - Logs de error:
  
  ```bash
  sudo tail -f /var/log/httpd/error_log
  ```
  
  - Logs de acceso:
  
  ```bash
  sudo tail -f /var/log/httpd/access_log
  ```
  (Imagen 8)

## 5. Hardening del servidor Apache
### 5.1. Ocultar información del servidor
Editar:

```bash
sudo nano /etc/httpd/conf/httpd.conf
```

Añadir:

```bash
ServerTokens Prod
ServerSignature Off
```

Reiniciar:

```bash
sudo systemctl restart httpd
```bash

### 5.2. Desactivar listado de directorios
Buscar:

```bash
<Directory "/var/www/html">
```

Cambiar:

```bash
Options Indexes FollowSymLinks
```

Por:

```bash
Options -Indexes +FollowSymLinks
```
- Guardar y salir:
- Ctrl + O, Enter → Ctrl + X

(Imagen 9, 10, 11)

## 5.3. Configuración del Security Group
En AWS:

- Abrir Security Groups
- Editar reglas de entrada

Añadir:

```bash
Tipo: HTTP
Puerto: 80
Origen: 0.0.0.0/0
```
(Imagen 12–16)

## 6. Creación del sitio web (index.html)
### 6.1. Crear archivo
```bash
sudo nano /var/www/html/index.html
```
### 6.2. Contenido
Incluye tu CV digital con HTML + CSS generado parcialmente con IA para agilizar el maquetado.

(Imagen 17)

## 7. Prueba del sitio web
Copiar la IP pública y abrir:

```bash
http://IP_PUBLICA
```
no carga:
- Revisar SG
- Revisar Apache
- ```bash
  sudo systemctl start httpd
  sudo systemctl status httpd
  ```

(Imagen 18–21)

## 8. Script de autocuración (Auto‑Recovery Script)
### 8.1. Crear archivo
```bash
nano check_apache.sh
```

### 8.2. Versión final del script
```bash
#!/bin/bash
# Monitor de Autocuracion - Biset Rodriguez

if ! systemctl is-active --quiet httpd; then
  echo " ALERTA: Apache esta caido. Iniciando protocolo de recuperacion..."
  sudo systemctl start httpd
  echo " REPARADO: El servicio Apache ha sido levantado exitosamente."
else
  echo "SISTEMA OK: Apache funcionando correctamente."
fi
- 22
- primera version
- ```bahs
  chmod +x check_apache.sh
  ```
- 23
- cambio de version para que cuando se ejecute salga en la terminal que se ejecuto 

### 8.3. Prueba manual
Detener Apache:

```bash
sudo systemctl stop httpd
```

Ejecutar script:

```bash
./check_apache.sh
```
Verificar:

```bash
sudo systemctl status httpd
```
(Imagen 23–24- 25 . 26 -27 28)

## 9. Supervisión de logs
```bash
sudo tail -f /var/log/httpd/access_log
```
Probar rutas 404:

```bash
http://IP/admin
http://IP/wp-login.php
http://IP/.env
http://IP/config.php
```
(Imagen 29)

## 10. Creación de la AMI personalizada
### 10.1. Crear imagen
EC2 → Instancias → Acciones → Imágenes → Crear imagen

```text
Nombre: CV-Liset-Hardened-v1

Descripción: Apache + CV + Hardening + Auto‑Recovery

(Imagen 33–35)

10.2. Esperar estado “available”
(Imagen 36)

11. Creación de la Launch Template
EC2 → Launch Templates → Crear plantilla

AMI: CV-Liset-Hardened-v1

Tipo: t3.micro

Key Pair: tu .pem

SG: el que permite puerto 80
```

(Imagen 30–32)

## 12. Creación del Auto Scaling Group
### 12.1. Nueva versión de la plantilla
Ajustar SG correcto

- Verificar VPC correcta (...bd1a4)
(Imagen 43)

### 12.2. Configuración del ASG
```text
Min: 1

Max: 3

Desired: 1
```
(Imagen 44–45)

- Métrica de escalado
- CPU Utilization
- Target: 70%
(Imagen 46)

(Imagen 47–50)

## 13. Verificación del Auto Scaling
- EC2 → Instancias
- Aparece nueva instancia generada automáticamente
- Probar IP pública
(Imagen 51)

- no carga:

- Revisar Apache
- Revisar SG
(Imagen 52)

## 14. Corrección del estado “disabled”

```bash
sudo systemctl start httpd
sudo systemctl enable httpd
```
(Imagen 53–54)

## 15. Problemas detectados y soluciones
1. Instancia sin SG correcto
Verificar pestaña Security  
(Imagen 55)

2. Instancia sin el CV
La AMI usada no contenía el CV.
Solución: crear AMI desde la instancia correcta.

3. Auto Scaling recrea instancias
ASG detecta que la capacidad mínima no se cumple.

## 16. Cómo detener el Auto Scaling Group
Auto Scaling Groups → Seleccionar → Editar:
```text
Código
Desired: 0
Min: 0
Max: 0
```
(Imagen 61–63)

- Luego terminar instancias manualmente.
(Imagen 64)

## 17. Mejoras pendientes
- Crear AMI final con CV + hardening + script
- Configurar User Data para automatizar despliegue
- Integrar monitoreo con CloudWatch

## 18. Costos y uso
Créditos restantes: 119,25 USD  
Días restantes: 137  
(Imagen 65 66)

### Estado actual del proyecto
✔ EC2 configurado
✔ Apache instalado y endurecido
✔ CV desplegado
✔ Script de autocuración funcionando
✔ Logs supervisados
✔ AMI creada
✔ Launch Template creada
✔ Auto Scaling Group configurado
✔ Proyecto documentado paso a paso
🔎Pendiente: AMI final + User Data + IAM + despliegue automático
