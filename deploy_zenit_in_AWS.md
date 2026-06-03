# Despliegue de ZENIT en AWS
Infraestructura, Seguridad, Servicios y Troubleshooting

Este documento describe el despliegue completo de la aplicación ZENIT en AWS, incluyendo la creación de la instancia, configuración del entorno Linux, instalación de servicios, despliegue de la aplicación, protección de datos sensibles, resolución de errores y endurecimiento del servidor.
El enfoque es profesional, siguiendo buenas prácticas de SysAdmin, DevOps y seguridad.

## 1. Objetivo del proyecto
El objetivo principal es aprender a trabajar en un entorno AWS real, desplegando ZENIT sobre una arquitectura estable y segura, utilizando:

- EC2
- Nginx como reverse proxy
- Uvicorn + FastAPI
- Python 3.11 compilado
- SQLite como base local
- Variables de entorno
- Seguridad por capas
- Backups automáticos
- Cifrado de volumen EBS
- Este despliegue constituye la base para futuras iteraciones más avanzadas (S3, RDS, ALB, Auto Scaling, etc.).

## 2. Arquitectura seleccionada
Para esta primera fase se eligió una arquitectura simple pero profesional:

- EC2 Ubuntu Server 22.04 LTS
- Nginx como reverse proxy
- Uvicorn ejecutado mediante systemd
- SQLite como base de datos local
- Archivos estáticos servidos desde /var/www/zenit
- Security Group minimalista
- SSH (22) → solo mi IP
- HTTP (80) → público
- HTTPS (443) → público
- Sin Docker, sin S3, sin RDS en esta fase

## 3. Creación de la instancia EC2
AMI seleccionada:***Ubuntu Server 22.04 LTS***
- Elegida por estabilidad, soporte extendido y compatibilidad con Nginx + FastAPI.

- Tipo de instancia :t3.micro

- Par de claves :Se reutiliza el keypair existente.

- VPC :Se utiliza la VPC previamente creada.

## 4. Problemas con Security Groups y MSSQL
Durante el lanzamiento, AWS generaba automáticamente la regla:

img1

```Código
Permitir tráfico MSSQL desde 0.0.0.0/0
```

Esto provocaba el error:


Microsoft SQL Server is not supported for the instance type ‘t3.micro’
Causas identificadas
El asistente estaba heredando configuraciones de una instancia previa creada con una AMI de SQL Server.

AWS intentaba asociar SQL Server aunque no se necesitara.

El Security Group se estaba creando en otra VPC, lo cual es incompatible.


## 5. Solución definitiva: Launch Template limpio
Para evitar configuraciones heredadas:

Pasos realizados
- Ir a Plantillas de lanzamiento
- Crear plantilla nueva: zenit-template
- Seleccionar:
  - Ubuntu 22.04
  - t3.micro
  - Keypair existente
  - VPC correcta
  - Security Group nuevo: zenit-sg
  - SSH (22) → My IP
  - HTTP (80) → Anywhere IPv4
  - HTTPS (443) → Anywhere IPv4
  - Almacenamiento:30 GB - gp3 - No cifrado

- Lanzar instancia desde la plantilla

Con esto se eliminó completamente el error de MSSQL.

## 6. Configuración de seguridad del servidor (Ubuntu)
Actualizar sistema
```bash
sudo apt update && sudo apt upgrade -y
```
Instalar firewall interno (UFW)
```bash
sudo apt install ufw -y
sudo ufw allow OpenSSH
sudo ufw allow 80
sudo ufw allow 443
sudo ufw enable
```
Deshabilitar acceso root por SSH
```bash
sudo nano /etc/ssh/sshd_config
PermitRootLogin no
sudo systemctl restart ssh
Instalar Fail2Ban
```
Protección contra ataques de fuerza bruta.

## 7. Instalación de Nginx
```bash
sudo apt install nginx -y
systemctl status nginx
```
Comprobación desde navegador:
http://IP_PUBLICA

## 8. Estructura del proyecto ZENIT
```bash
sudo mkdir -p /var/www/zenit
sudo chown -R ubuntu:ubuntu /var/www/zenit
```

Aquí se alojan:

- index.html
- css/
- js/
- imágenes
- dashboard

## 9. Configuración de Nginx para ZENIT
```bash
sudo nano /etc/nginx/sites-available/zenit
```
```Código
server {
    listen 80;
    server_name _;

    root /var/www/zenit;
    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }
}
```
Activar:

```bash

sudo ln -s /etc/nginx/sites-available/zenit /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl restart nginx
```

## 10. Subida de archivos ZENIT
Métodos disponibles:

- SFTP (FileZilla)
- SCP
- GitHub + git clone

Se utilizó GitHub.

## 11. Instalación de Python y FastAPI
El servidor tenía Python 3.14, incompatible con varias dependencias.

Solución: Compilar Python 3.11 desde código fuente
Se instaló Python 3.11 manualmente.

## 12. Creación del entorno virtual
El primer entorno se creó con Python 3.14 por error.

Corrección
```bash
deactivate
rm -rf venv
python3.11 -m venv venv
source venv/bin/activate
```
## 13. Instalación de dependencias
Error inicial:

```Código
bcrypt not found
```
Solución:

```bash
pip install bcrypt
```
## 14. Error 502 — Diagnóstico
El error 502 no era de Nginx ni del disco cifrado.

Causa real
FastAPI fallaba al arrancar por un error de sintaxis:

```Código
app.add_middleware(SessionMiddleware, secret_key=os.getenv"ZENIT_SECRET_KEY")
``` 
Faltaban paréntesis:

```Código
os.getenv("ZENIT_SECRET_KEY")```
El servicio systemd arrancaba → fallaba → quedaba “running” sin proceso → Nginx devolvía 502.

## 15. Servicio systemd para ZENIT
Se creó un servicio dedicado para Uvicorn.

Errores corregidos:

- Ruta incorrecta
- Python equivocado
- Entorno virtual mal creado
- Variables de entorno duplicadas

## 16. Protección de la base de datos
Permisos estrictos
```bash
chmod 600 zenit.db
```
Instalar SQLite3
```bash
sudo apt install sqlite3
sqlite3 zenit.db
.tables
```
- Mover secret_key a variables de entorno
- Se eliminó del código fuente.

## 17. Cifrado del volumen EBS
No se puede cifrar un volumen en uso.

Procedimiento correcto
- Crear snapshot
- Crear volumen cifrado desde snapshot
- Detener instancia
- Desmontar volumen viejo
- Montar volumen nuevo
- Encender instancia

## 18. Script de backup automático
Se planificó:

- Script backup_zenit.sh
- Carpeta /home/ubuntu/zenit_backups
- Cron job diario
- Retención de 7 días

## 19. Endurecimiento final del servidor
- SSH sin root
- SSH sin contraseña
- Fail2Ban
- Firewall doble (AWS + UFW)
- Permisos estrictos
- Variables de entorno
- Revisión de logs
- Snapshot estable

## 20. Roadmap futuro (alta disponibilidad)
- S3 para estáticos
- AMI personalizada
- Launch Template
- Auto Scaling Group
- Load Balancer (ALB)
- Migración a RDS PostgreSQL
- Arquitectura multi-AZ

## Conclusión
El despliegue de ZENIT en AWS permitió trabajar con un entorno real, enfrentando problemas auténticos de infraestructura, seguridad, compatibilidad y servicios.
El resultado es una aplicación funcionando sobre una arquitectura estable, segura y preparada para escalar en futuras iteraciones.


## En proceso / Próximos pasos
El despliegue actual de ZENIT en AWS se encuentra operativo y estable, pero forma parte de un proyecto en evolución. Las siguientes tareas están planificadas para las próximas iteraciones, alineadas con la integración de mi portafolio personal y la transición hacia una arquitectura más robusta:

### 1. Integración del portafolio
Finalizar la creación de mi portafolio profesional

Integrarlo dentro de la misma infraestructura

Preparar rutas, estáticos y estructura para despliegue conjunto

### 2. Migración de estáticos a S3
Mover imágenes, CSS y JS a Amazon S3

Configurar políticas de acceso

Optimizar tiempos de carga

Preparar Nginx para servir contenido desde S3

### 3. Variables de entorno y seguridad
Consolidar .env

Mover claves sensibles fuera del código

Revisar permisos y rutas internas

### 4. Backups automáticos
Implementar script backup_zenit.sh

Crear rotación de 7 días

Proteger backups con permisos estrictos

### 5. Cifrado completo del volumen EBS
Crear snapshot estable

Generar volumen cifrado

Migrar la instancia al volumen seguro

### 6. Arquitectura avanzada (futuro)
Crear AMI personalizada

Crear Launch Template

Configurar Auto Scaling Group

Añadir Load Balancer (ALB)

Migrar base de datos a RDS PostgreSQL

Preparar arquitectura multi-AZ
