# Bloque 1

# Fundamentos de cloud y AWS

**Cloud:** Hoy en día en el mundo se manejan grandes volúmenes de datos que no es eficiente ni escalable gestionar solo en infraestructuras locales.  
Por ello, los datos se encuentran en la nube: centros de datos administrados donde se pueden cargar, almacenar, copiar y procesar información.  
La nube permite acceder a recursos de computación, almacenamiento y redes bajo demanda, sin necesidad de infraestructura física propia.

**AWS (Amazon Web Services):** Es una plataforma que ofrece más de 200 servicios en la nube, donde AWS se encarga de la parte física y tú de la configuración.  
A lo largo del estudio veremos las diferentes ventajas de su uso.

---

## Tipos de servicios AWS (Modelos de servicio)

### Modelos de servicio

1. **IaaS:** Infraestructura (por ejemplo, EC2, VPC, EBS)
2. **PaaS:** Plataformas administradas (por ejemplo, RDS, Elastic Beanstalk)
3. **SaaS:** Software completo (por ejemplo, aplicaciones listas para usar)
4. **Serverless:** Modelo de ejecución donde se ejecuta código sin gestionar servidores (por ejemplo, Lambda)

### Tipos de implementación de la nube

- **Nube pública:** Los recursos (servidores, almacenamiento) se comparten entre múltiples organizaciones a través de Internet. La infraestructura es propiedad del proveedor (por ejemplo, AWS).
- **Nube privada:** Infraestructura dedicada a una sola empresa. Ofrece mayor control, pero menor flexibilidad y escalabilidad.
- **Nube híbrida:** Conecta la nube pública (como AWS) con nubes privadas, permitiendo la portabilidad de datos y cargas de trabajo.

---

## On‑premise vs Cloud

| On‑premise | Cloud |
| :-- | :-- |
| Servidores físicos propios de la empresa | Servidores externos gestionados por proveedores |
| Requiere alta inversión inicial | Pago por uso |
| Mantenimiento por parte de la empresa | Mantenimiento por parte del proveedor |
| Escalabilidad lenta | Escalabilidad rápida |
| Máximo control físico | Máximo control lógico, pero menos control físico |
| Acceso limitado, suele requerir VPN | Acceso desde cualquier lugar con Internet |
| Actualizaciones por parte de la empresa | Actualizaciones por parte del proveedor |

---

## Regiones

Una **región** es una ubicación física donde AWS agrupa varias zonas de disponibilidad (AZ).  
Se usan para reducir latencia y cumplir requisitos legales y de residencia de datos.

### Zona de Disponibilidad (AZ – Availability Zone)

Una **AZ** puede tener uno o varios centros de datos independientes dentro de una región.  
Están diseñadas para ofrecer alta disponibilidad: si una AZ falla, otra puede seguir funcionando.

---

## Servicios globales vs regionales

| Global | Regional |
| :-- | :-- |
| No dependen de una región específica | Existen dentro de una región |
| IAM, Route 53, CloudFront, Organizations | EC2, S3, RDS, VPC, Lambda |

---

# Instancias

Las instancias se crean dentro del servicio **EC2**.  
Una instancia es una máquina virtual que utiliza volúmenes **EBS** para almacenar datos en bloques.

## Tipos de instancia

Existen varias familias de instancias:

- **t:** económicas, con CPU burst (acumulan créditos), ideales para cargas ligeras.
- **m:** balanceadas (CPU + RAM).
- **c:** optimizadas para cómputo (más CPU).
- **r:** optimizadas para memoria (más RAM).
- **g/p:** optimizadas para GPU y cómputo de alto rendimiento (ML, gráficos, HPC).

### Tamaños de instancia

Tamaños comunes:

- nano  
- micro  
- small  
- medium  
- large  
- xlarge  
- 2xlarge  
- 4xlarge  
- 8xlarge  
- 12xlarge  
- 16xlarge  
- 24xlarge  
- 32xlarge  

**Idea clave:**  
A mayor tamaño → más CPU, más RAM, más red → más costoso.

**Ejemplo dentro de la familia t2:**

- `t2.micro` → pequeño, barato  
- `t2.small` → un poco más grande  
- `t2.medium` → más CPU y RAM  
- `t2.large` → mucho más potente y más caro  

Esto funciona igual en las demás familias (m, c, r, g…).

Lo especial de la familia **t** es el sistema de créditos de CPU (*burstable performance*).

---

## Tipos de compra de EC2

- **On‑Demand:** flexible, pagas por uso, sin compromiso.
- **Reserved Instances:** hasta ~75% más barato, compromiso de 1–3 años.
- **Savings Plans:** hasta ~72% más barato, compromiso de gasto por hora.
- **Spot Instances:** hasta ~90% más barato, pero AWS puede interrumpirlas.
- **Dedicated Host:** servidor físico completo dedicado a un solo cliente, útil para requisitos de licencias o cumplimiento.

---

## EBS

**EBS (Elastic Block Store)** es el “disco duro” de la instancia EC2. Puede almacenar:

- El sistema operativo (Linux o Windows, Ubuntu, ect), **AMI** tiene la plantilla del SO de la instancia.
- Tus aplicaciones y archivos.

Características:

- Se puede desconectar de una instancia y adjuntar a otra.
- Se cobra por GB al mes.
- Es **persistente**: los datos no se borran al detener la instancia (a menos que se configure lo contrario).

### AMI

Una **AMI (Amazon Machine Image)** es la plantilla que contiene:

- El sistema operativo (Linux, Windows, etc.)
- La configuración base

Ejemplos de AMI:

- Amazon Linux  
- Ubuntu  
- Red Hat  
- Windows Server  

Cuando creas una instancia, la AMI se copia al volumen EBS.

---

## Esquema

```text
EC2 (servicio)
 └─ Instancia (máquina virtual)
      ├─ AMI (plantilla con SO)
      └─ EBS (disco duro persistente) 
```

---

## Instance Store

**Instance Store** es almacenamiento temporal que viene del hardware físico del servidor.

Características:

- Muy rápido  
- No persistente  
- Se borra si la instancia se detiene o falla  

Usos:

- Datos temporales  
- Cachés  
- Archivos intermedios  
- Procesamiento intensivo  

Para datos importantes → usar **EBS** o **S3**.

---

## S3

**S3** es un servicio de almacenamiento de objetos (no es un disco duro).

Un objeto incluye:

- Archivo (foto, documento, vídeo, JSON, etc.)  
- Metadatos (tamaño, tipo, etiquetas, fecha, etc.)  
- Clave (ruta/nombre)  

S3 es:

- Duradero (11 nueves: 99.999999999%)  
- Seguro  
- Escalable  
- Económico  

---

### Costos de S3

**S3 cobra por:**

- Almacenamiento  
- Peticiones **PUT** (subir)  
- Peticiones **GET** (descargar)  
- Copias de objetos  
- Transferencia de datos hacia fuera de AWS  

**S3 NO cobra por:**

- Eliminar objetos  
- Transferencia dentro de la misma región  
- Listar objetos (LIST) → muy barato  

---

### Versionado en S3

Permite:

- Guardar cada versión de un archivo  
- Recuperar versiones anteriores  
- Evitar pérdidas por sobrescritura  
- Añadir *delete markers* cuando se “borra” un objeto  

> Un bucket de S3 es un contenedor lógico donde guardas objetos (archivos + metadatos).  
> No es un contenedor Docker, no ejecuta nada.  
> Está fuera de EC2 y solo sirve para almacenar datos.

---

## Modelo de responsabilidad compartida

**AWS** se encarga de la infraestructura física y virtual de la nube (*Security OF the Cloud*).  
**Tú** te encargas de la configuración y seguridad dentro de la nube (*Security IN the Cloud*).

- **AWS →** Infraestructura y servicios administrados (RDS, DynamoDB, S3).  
- **Tú →** Configuración, datos, permisos, redes, aplicaciones.

Ejemplos:

- En **EC2**, tú parcheas el sistema operativo.  
- En **RDS**, AWS parchea el sistema operativo.

---

## Edge Locations

Son puntos de presencia distribuidos por el mundo para entregar contenido con baja latencia.

Servicios que usan Edge Locations:

- CloudFront (CDN)  
- Route 53 (DNS global)  
- AWS Global Accelerator  

CloudFront almacena copias en caché, no todo S3.

---

## Datos importantes

### Alta disponibilidad

- Múltiples AZ  
- Load Balancer  
- Auto Scaling  
- Evitar un único punto de fallo  

### Escalabilidad

- **Vertical:** más CPU/RAM  
- **Horizontal:** más instancias  

### Load Balancer

- Reparte tráfico  
- Evita que una instancia se sobrecargue  
- Detecta instancias sanas  

Ayuda a **alta disponibilidad** y **escalabilidad horizontal**.

### Auto Scaling Group

- Crea más instancias cuando hay más carga  
- Elimina instancias cuando baja la carga  
- Mantiene un número mínimo de instancias  

Ayuda a **escalabilidad horizontal** y **resiliencia**.

### Múltiples AZ

- Si una AZ falla, otra sigue funcionando  
- Tus instancias están duplicadas en varias zonas  

Esto es **alta disponibilidad**.

