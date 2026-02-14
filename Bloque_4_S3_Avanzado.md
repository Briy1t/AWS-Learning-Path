## Bloque 4 — S3 Avanzado

## Bucket Policies
Una bucket policy es una policy JSON aplicada directamente a un bucket S3.
Controla quién puede entrar y qué puede hacer dentro del bucket.

Diferencias clave:
- IAM Policy → qué puede hacer un usuario o rol dentro de tu cuenta
- Bucket Policy → quién puede acceder al bucket, incluso desde otras cuentas o servicios externos

**S3** es un servicio global y compartido, así que necesita reglas claras para:

- Acceso público
- Acceso entre cuentas
- Acceso desde servicios externos
- Acceso desde roles de otras cuentas

→IAM controla permisos dentro de tu cuenta.
→Bucket Policy controla quién entra al bucket, venga de donde venga.

**Reglas importantes:**

- Primero se evalúa Deny
- Luego Allow

Deny siempre gana

**Elementos de una bucket policy:**

- Principal → quién puede acceder (esto no existe en IAM policies)
- Action → lo que puede hacer (ej: s3:GetObject)
- Resource → bucket u objetos
Condition → opcional

## ACLs (Access Control Lists)
Antes de las bucket policies, S3 usaba ACLs para controlar acceso.
Hoy casi no se usan porque son muy limitadas:

- No tienen conditions
- No tienen acciones detalladas
- No permiten granularidad fina
- No se integran bien con IAM moderno

Existen dos niveles:

→ ACL del bucket
→ ACL del objeto (puede ser distinta a la del bucket)

**Permisos típicos:**

- Read
- Write
- Read_acp
- Write_acp
- Full_control

Bucket policies son más seguras, claras y auditables.
Además existe Block Public Access, que bloquea cualquier intento de hacer público un bucket, incluso si una ACL o policy dice **“allow”**.

**ACLs** solo se usan en dos casos:

- cuando subes objetos desde otra cuenta sin roles (bucket-owner-full-control)
- servicios antiguos o integraciones heredadas

**Regla de oro:**
" Si puedes usar bucket policies, no uses ACLs. "

## Versioning
El versionado permite guardar cada cambio de un objeto sin borrar el anterior.

Estados:

1. Unversioned → sin versionado (por defecto)
2. Enabled → cada cambio crea una nueva versión
3. Suspended → mantiene versiones antiguas, pero no crea nuevas

**Detalles importantes:**

- Cada versión ocupa espacio
- Borrar un objeto crea un delete marker
- Puedes recuperar versiones anteriores
- Es una protección contra errores humanos

Muchas empresas combinan versioning + lifecycle para borrar versiones antiguas y ahorrar costes.

## MFA Delete
Protección extra muy poco usada porque es muy restrictiva:

- Solo se activa desde CLI
- Solo el root puede activarlo
- No funciona con roles

## Cifrado en S3
Tres opciones:

1. **SSE-S3 →** AWS gestiona la clave
2. **SSE-KMS →** tú gestionas la clave KMS (se cobra por uso)
3. **SSE-C →** tú aportas la clave (menos usado)

## Block Public Access
Si está activado, ningún objeto puede ser público, aunque una policy diga allow.
Se creó para evitar errores humanos.

Replicación entre buckets
Dos tipos:

1. **SRR (Same Region Replication) →** misma región
2. **CRR (Cross Region Replication) →** regiones distintas

**Costes:**

- PUT de la réplica
- almacenamiento en el bucket destino
- Sirve para disponibilidad, resiliencia y cumplimiento.

## Lifecycle Rules
Son una forma de automatizar el ciclo de vida de los objetos en **S3** para reducir costos, archivar datos y manejar versiones antiguas sin intervención manual.

Permiten definir:
-Qué objetos afectan (filtro)
-Qué acción realizar (mover, archivar, borrar)
-Cuándo hacerlo (días desde la creación)

Se usan para mover datos a clases de almacenamiento más baratas (IA, One Zone‑IA, Glacier, Deep Archive), 
borrar versiones antiguas, limpiar delete markers y controlar el crecimiento del bucket cuando hay versioning.

El flujo típico es:
- Standard → Standard‑IA → Glacier → Deep Archive → borrar.

Son muy usadas en logs, backups, datos históricos y cualquier archivo que no se consulta frecuentemente, porque el ahorro frente a **S3 Standar**d puede ser enorme.


|Día	|Acción |	Clase |
|:--|:--|:--|
|30	|Mover |	Standard‑IA |
|90	|Mover	| Glacier Instant Retrieval |
|365	|Mover	 | Glacier Deep Archive |
|730	|Borrar|	—|

## Las storage classes 
Son los tipos de almacenamiento que ofrece S3. 
Cada una tiene un costo distinto, una velocidad de acceso distinta y un propósito diferente. 
Las lifecycle rules existen justamente para mover objetos entre estas clases y optimizar costos.

**Clases principales:**
- **S3 Standard** 
Acceso frecuente. Es la clase por defecto. Rápida y más cara.

- **S3 Standard‑IA (Infrequent Access)**
Para datos que lees poco. Más barata, pero cobra por lectura.

- **S3 One Zone‑IA**
Igual que IA pero en una sola AZ. Más barata, menos resiliente.

- **S3 Glacier Instant Retrieval**  
Para archivos que casi no se consultan, pero necesitas recuperar rápido.

- **S3 Glacier Flexible Retrieval**
Más barato que Instant. Recuperación más lenta.

-**S3 Glacier Deep Archive**
La opción más barata. Recuperación muy lenta. Ideal para archivo a largo plazo.

Por qué importan tanto:

- Determinan el costo por GB
- Determinan la velocidad de recuperación
- Determinan si hay costo por lectura
- Determinan si puedes usarlas para archivos activos o archivados



|Clase	|Costo	|Velocidad|Uso|
|:--|:--|:--|:--|
|Standard	|Alto |	Inmediato|Uso diario|
|Standard‑IA	|Medio|	Inmediato	|Acceso ocasional|
|One Zone‑IA|	Bajo	|Inmediato|	Datos recreables|
|Glacier Instant	|Muy-medio|	Rápido	|Archivado accesible|
|Glacier Flexible	|Muy bajo|	Minutos‑horas	|Archivado|
|Glacier Deep Archive	|Bajísimo	|Horas	|Archivado largo plazo|
