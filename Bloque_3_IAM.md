# Bloque 3 — IAM (Identity and Access Management)
En este bloque entramos en la parte de identidad y permisos dentro de AWS.
Aquí aparecen usuarios, grupos, roles y policies, que son la base de la seguridad.

## Usuarios
Un usuario es una identidad asignada a una persona real.
Se usa cuando alguien necesita entrar al panel de AWS o usar la CLI.

- Se les da una clave de acceso (si es necesario)
- No es buena idea crear un usuario para cada tarea repetitiva
- Para eso existen **los grupos**
- El usuario root no se usa nunca, solo para cosas críticas
- El root debe tener MFA obligatorio

## Grupos
Un grupo es una colección de usuarios que comparten los mismos permisos.

- Es más seguro dar permisos al grupo y no usuario por usuario
- Facilita la administración
- Evita errores y permisos innecesarios

**Ejemplo:**
Grupo “Desarrolladores” → todos tienen los mismos permisos.

## Roles
Los roles son los más usados en AWS.
No son para personas, sino para servicios.

Un rol sirve para que:

- EC2 acceda a S3
- Lambda acceda a DynamoDB
- ECS acceda a Secrets Manager
- SSM acceda a EC2

Y así con cualquier servicio que necesite permisos

`Los roles no tienen claves permanentes.`
Usan credenciales temporales generadas por**STS**, que caducan.
Esto los hace más seguros.

## Policies (políticas)
Las policies definen qué puede hacer una identidad.

Tienen cuatro partes:

- Action → qué puede hacer (ej: s3:GetObject)
- Resource → dónde lo puede hacer
- Effect → allow o deny
- Condition → condiciones opcionales

**"Deny"** siempre gana sobre **"Allow"**

---

La estructura de una policy
‎
```text
‎
‎`json
‎{
‎  "Version": "2012-10-17",
‎  "Statement": [
‎    {
‎      "Effect": "Allow",
‎      "Action": "s3:GetObject",
‎      "Resource": "arn:aws:s3:::mi-bucket/*"
‎    }
‎  ]
‎}
```
---
**Principio clave:**
**Least privilege →**dar solo lo necesario, ni más ni menos.

### Tipos de policies**

**Managed policies**

- Reutilizables
- Mantenidas por AWS
- Son las recomendadas

**Inline policies**

- Pegadas directamente a un usuario o rol
- No se reutilizan
- Si borras el recurso, se pierden

**Trust Policy**
Es la política que define quién puede asumir un rol.

**Ejemplo:**
“EC2 puede asumir este rol”
“Lambda puede asumir este rol”

**STS (Security Token Service)**
- Genera credenciales temporales.
- Son las que usan los roles.
- Más seguras que las claves permanentes.

## IAM Best Practices
 
1. No usar el usuario root para tareas diarias
2. Activar MFA en root, administradores y usuarios sensibles
3. No usar claves de acceso permanentes
4. Usar roles siempre para servicios
5. Aplicar el principio de least privilege
6. Rotar o eliminar claves si existen
7. Usar grupos para usuarios humanos
8. Revisar permisos con Access Analyzer
9. Usar roles para acceso entre cuentas (cross‑account)
10. Evitar inline policies cuando sea posible

Si un servicio necesita permisos, dale un rol.
Si una persona necesita permisos, dale un grupo.

## Access Analyzer
Servicio gratuito de AWS que analiza:

- si tus permisos están demasiado abiertos
- si estás exponiendo recursos sin querer
- si hay riesgos de seguridad
Es una herramienta para validar que lo que das está bien.

Lo que vendrá más adelante
Cuando avancemos con otros servicios, veremos roles más específicos:

- Roles para EC2
- Roles para Lambda
- Roles para ECS
- Roles para SSM
  Y más adelante, cuando lleguemos a Organizations:
- Service Control Policies (SCP)
- IAM Identity Center
