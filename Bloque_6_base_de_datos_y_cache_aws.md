# Bloque 6 — Bases de Datos en AWS (RDS, DynamoDB, Aurora, ElastiCache, DAX)
AWS divide sus bases de datos en dos grandes familias:

## 1. Bases de datos relacionales (SQL)
- MySQL
- PostgreSQL
- MariaDB
- Oracle
- SQL Server
- Aurora (motor propio de AWS)

## 2. Bases de datos NoSQL
- **DynamoDB**(clave‑valor, ultra rápida, serverless)

## 1. RDS (Relational Database Service)
RDS es un servicio gestionado por AWS, esto significa que
---
AWS se encarga de:
- Backups automáticos
- Parches del motor
- Alta disponibilidad
- Failover automático
- Escalado vertical (más CPU/RAM)
--- 
Tú solo te encargas de:
- Elegir el motor
- Elegir el tamaño
- Crear la base
- Conectarte desde tu aplicación
---
**Motores soportados**
 |Motor	|Tipo	|Comentario|
 |:--|:--|:--|
 |MySQL	|SQL	|Muy usado, fácil|
 |PostgreSQL |SQL|	Muy robusto |
 |MariaDB	|SQL	| Variante de MySQL |
 |Oracle	|SQL	|Empresas grandes|
 |SQL Server|	SQL |	Mundo Microsoft|
 
- Aurora	SQL:	Motor propio de AWS

--- 
Multi‑AZ vs Single‑AZ
- **Single‑AZ**:
- Una sola instancia
-Si falla → tu app se cae

- **Multi‑AZ**
- Copia completa en otra AZ
- Replicación síncrona
- Failover automático
- Es solo para alta disponibilidad

---
**Read Replicas**
- Copias solo lectura
- Sirven para escalar lectura
- Útiles para reportes, consultas pesadas
- No hacen failover automático
- No dan alta disponibilidad 

--- 

**Arquitectura básica**

- EC2 (app) → RDS (base de datos)
- RDS debe estar en subnets privadas
- EC2 puede estar en pública o privada
- El Security Group de RDS debe permitir tráfico desde el SG de EC2
- No se accede desde Internet, solo dentro de la VPC
--- 

¿Hace falta un role o route table especial?
- No.
- Solo necesitas Security Groups correctos.
- La route table ya permite comunicación interna dentro de la VPC.

--- 

## 2. DynamoDB (NoSQL)
Base de datos NoSQL, serverless, ultra rápida y totalmente gestionada.
- Características:
- Clave‑valor
- Sin tablas relacionales
- Sin joins
- Sin esquema rígido
- Escala automáticamente
- Latencia milisegundos
- Ideal para apps móviles, juegos, e‑commerce, microservicios

### Claves principales
-Partition Key (obligatoria): determina dónde se guarda el dato.
--- 
**Sort Key (opcional)**
Permite ordenar y agrupar datos bajo la misma partition key

---
### Modos de capacidad
1. **On‑Demand**:Pagas por request
- No configuras nada
- Escala solo
- Ideal para cargas impredecibles

2. **Provisioned**:Tú defines lecturas/escrituras
- Más barato si tienes tráfico estable
- Puedes activar Auto Scaling

### Tipos de lectura
- **Eventually Consisten**t → más barata
- **Strongly Consistent** → más precisa

### Seguridad
- IAM controla acceso
- Cifrado en reposo y en tránsito
- PITR (Point‑In‑Time Recovery)
--- 

**¿Qué es PITR?**
- Es la capacidad de restaurar la base a cualquier punto en el tiempo dentro de los últimos 35 días. 

- **Ejemplo:** “Devuélveme la base a como estaba ayer a las 3:42 PM”.

## 3. Aurora
Aurora es la base SQL más avanzada de AWS.

1.**Ventajas**
- Totalmente gestionada
- Alta disponibilidad por diseño
- Más rápida que MySQL/PostgreSQL tradicionales
- Más barata que Oracle/SQL Server
- Escala automáticamente
- Se repara sola
- Hasta **128 TB** de almacenamiento

--- 
**Arquitectura de Aurora**
- Aurora separa:Cómputo → instancias (writer + readers)
- Almacenamiento → cluster distribuido

- El almacenamiento tiene:
    - 6 copias
    - En 3 AZ

- Replicación automática
- Autoreparación

- Instancias
  - Writer (solo 1)
  - Lectura + escritura

- Si falla → failover a un reader
  - Readers (hasta 15)
  - Solo lectura

-Escalan lecturas
Failover rápido (segundos)

### Tipos de Aurora
- **Aurora MySQL** (compatible con MySQL)
- **Aurora PostgreSQL** (compatible con PostgreSQL)

## Aurora Serverless v2
- Escala automáticamente
- No pagas por instancias fijas
- Ideal para cargas variables

## 4. ElastiCache (Redis / Memcached)

Un caché es un lugar donde guardas datos muy usados para acceder a ellos más rápido.

---
ElastiCache evita:
- Consultas repetidas a RDS
- Saturación de la base
- Costos altos
- Latencia alta

### Motores

| Motor	|Persistencia	|¿Pierde datos al reiniciar?|	Uso típico|
|:--|:--|:--|:--|
|Memcached	| No	| Sí	|Caché simple, temporal|
|Redis |	 Sí (opcional)	| No (si está activado)	|Sesiones, tokens, colas, caché avanzado|

-  funciona con RDS o DynamoDB
1. Con **RDS**
- La app pide un dato
- Primero mira en ElastiCache
- Si está → lo devuelve rápido
- Si no está → va a RDS, lo guarda en caché y lo devuelve

2. Con **DynamoDB**:Funciona igual, pero DynamoDB tiene su propio caché:

## DAX (DynamoDB Accelerator)
- Caché especializado para DynamoDB
- Ultra rápido (microsegundos)
- Pagas por nodo
- No es serverless
- Reduce costos de lectura

--- 

**Comparación**

|Servicio |	¿Tiene costo?	|¿Por qué vale la pena?|
|:--|:--|:--|
|ElastiCache	| Sí	|Reduce carga en RDS y acelera apps|
|DAX	| Sí |	Reduce lecturas a DynamoDB |
|Caché en general |	 Sí	| Acelera lecturas y evita saturación|

### Diagramas
```text
RDS

┌──────────────────────────┐
│      RDS PRIMARY         │
│   (CPU, RAM, Storage)    │
└───────────┬──────────────┘
            │ Replicación síncrona
            ▼
┌──────────────────────────┐
│      RDS Multi-AZ        │
│   (Copia completa)       │
└──────────────────────────┘

Lecturas extra:
┌──────────────────────────┐
│     Read Replica 1       │
└──────────────────────────┘
┌──────────────────────────┐
│     Read Replica 2       │
└──────────────────────────┘

-------------------------------------
Aurora

┌──────────────────────┐
│      Writer           │
│ (solo 1 por cluster)  │
└───────────┬──────────┘
            │
┌───────────┴───────────┐
│                       │
│   Reader 1            │
│   Reader 2            │
└───────────┬───────────┘
            │
            ▼
┌──────────────────────────────────────────┐
│   ALMACENAMIENTO DISTRIBUIDO AURORA      │
│   6 copias en 3 Availability Zones       │
│   Escala automáticamente                 │
└──────────────────────────────────────────┘

```
