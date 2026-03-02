# AWS CERTIFIED CLOUD PRACTITIONER (CLF‑C02) — 60 PREGUNTAS
simulación para reforzar conceptos: Con el objetivo de reforzar conceptops , mirar hareas para reforzar, se hacen las siguiente preguntas.

## Bloque 1 — Fundamentos de Cloud y AWS (10 preguntas)
1. ¿Cuál es una ventaja clave del cloud computing frente al on‑premise?
A) Mayor inversión inicial
B) Escalabilidad bajo demanda
C) Requiere gestionar hardware
D) No permite automatización

2. ¿Qué es una Región en AWS?
A) Un centro de datos individual
B) Un conjunto de Availability Zones
C) Un Edge Location
D) Un servicio de red

3. ¿Qué es una Availability Zone?
A) Un grupo de regiones
B) Un centro de datos aislado dentro de una región
C) Un servicio de backup
D) Un servicio de red

4. ¿Qué servicio permite ejecutar máquinas virtuales?
A) Lambda
B) EC2
C) S3
D) RDS

5. ¿Qué tipo de almacenamiento es persistente para EC2?
A) Instance Store
B) EBS
C) S3
D) Glacier

6. ¿Qué modelo describe AWS en el Shared Responsibility Model?
A) AWS es responsable de todo
B) El cliente es responsable de todo
C) AWS: seguridad de la nube; cliente: seguridad en la nube
D) Cliente: seguridad de la nube; AWS: seguridad en la nube

7. ¿Qué servicio es ideal para almacenar objetos?
A) EBS
B) EFS
C) S3
D) DynamoDB

8. ¿Qué significa alta disponibilidad?
A) Ejecutar más instancias de lo necesario
B) Capacidad de recuperarse ante fallos
C) Reducir costos
D) Ejecutar en una sola AZ

9. ¿Qué servicio permite distribuir contenido globalmente?
A) CloudFront
B) Route 53
C) S3
D) EC2

10. ¿Qué es el elasticity?
A) Capacidad de reducir costos
B) Capacidad de ajustar recursos automáticamente
C) Capacidad de replicar datos
D) Capacidad de auditar acciones

11. ¿Qué es una VPC?
A) Una red privada virtual dentro de AWS
B) Un servicio de almacenamiento
C) Un balanceador
D) Un firewall

12. ¿Qué subnet puede tener acceso directo a Internet?
A) Privada
B) Pública
C) Aislada
D) Local

13. ¿Qué componente permite acceso a Internet?
A) NAT Gateway
B) Internet Gateway
C) VPC Endpoint
D) Route Table

14. ¿Qué hace un NAT Gateway?
A) Permite acceso público a instancias
B) Permite que subnets privadas salgan a Internet
C) Bloquea tráfico entrante
D) Cifra tráfico

15. ¿Qué diferencia a un Security Group de una NACL?
A) SG es stateless
B) NACL es stateful
C) SG es stateful
D) Ambos son stateless

16. ¿Qué es un VPC Endpoint?
A) Un túnel VPN
B) Un acceso privado a servicios AWS sin Internet
C) Un firewall
D) Un balanceador

17. ¿Qué es VPC Peering?
A) Conexión privada entre dos VPCs
B) Un túnel VPN
C) Un servicio de DNS
D) Un balanceador

18. ¿Qué es un ENI?
A) Un firewall
B) Una tarjeta de red virtual
C) Un balanceador
D) Un endpoint

19. ¿Qué opción permite acceder a instancias privadas sin exponerlas?
A) Internet Gateway
B) Bastion Host
C) NAT Gateway
D) S3 Endpoint

20. ¿Qué rompe más frecuentemente la conectividad en VPCs?
A) Falta de roles
B) Route Tables mal configuradas
C) Falta de etiquetas
D) Falta de logs

21. ¿Qué es un IAM User?
A) Una identidad humana o técnica
B) Un rol temporal
C) Un grupo de permisos
D) Un servicio de auditoría

22. ¿Qué es un IAM Role?
A) Un usuario
B) Una identidad con permisos temporales
C) Un grupo
D) Un servicio de red

23. ¿Qué servicio genera credenciales temporales?
A) IAM
B) STS
C) Organizations
D) Access Analyzer

24. ¿Qué es una policy?
A) Un archivo JSON con permisos
B) Un usuario
C) Un rol
D) Un grupo

25. ¿Qué es una trust policy?
A) Define permisos
B) Define quién puede asumir un rol
C) Define logs
D) Define costos

26. ¿Qué es least privilege?
A) Dar permisos ilimitados
B) Dar permisos mínimos necesarios
C) Dar permisos temporales
D) Dar permisos solo a roles

27. ¿Qué tipo de policy es administrada por AWS?
A) Inline
B) Managed
C) Trust
D) Custom

28. ¿Qué herramienta detecta permisos excesivos?
A) CloudTrail
B) IAM Access Analyzer
C) Config
D) GuardDuty

29. ¿Qué NO deberías hacer con un root user?
A) Activar MFA
B) Crear usuarios
C) Usarlo para tareas diarias
D) Cambiar billing

30. ¿Qué tipo de credenciales usa un rol?
A) Permanentes
B) Temporales
C) De root
D) De usuario

31. ¿Qué controla una bucket policy?
A) Acceso a objetos individuales
B) Acceso a todo el bucket
C) Acceso a roles
D) Acceso a usuarios

32. ¿Qué hace Block Public Access?
A) Cifra objetos
B) Evita acceso público
C) Elimina objetos públicos
D) Permite acceso privado

33. ¿Qué es versioning?
A) Cifrado
B) Control de versiones de objetos
C) Replicación
D) Compresión

34. ¿Qué tipo de replicación copia objetos entre regiones?
A) Same‑Region Replication
B) Cross‑Region Replication
C) Multi‑AZ Replication
D) Global Replication

35.¿Qué método de cifrado NO requiere claves del cliente?
A) SSE‑S3
B) SSE‑KMS
C) SSE‑C
D) Client‑Side Encryption

36. ¿Qué servicio conecta on‑premise con AWS mediante Internet?
A) Direct Connect
B) VPN Site‑to‑Site
C) VPC Peering
D) Transit Gateway

37. ¿Qué servicio conecta on‑premise con AWS mediante enlace dedicado?
A) VPN
B) Direct Connect
C) Peering
D) NAT

38. ¿Qué Load Balancer opera en capa 7?
A) NLB
B) ALB
C) CLB
D) Route 53

39. ¿Qué Load Balancer opera en capa 4?
A) ALB
B) NLB
C) CLB
D) S3

40. ¿Qué es un Target Group?
A) Un firewall
B) Un grupo de instancias detrás de un LB
C) Un rol
D) Un endpoint

41. ¿Qué hace Auto Scaling?
A) Cifra datos
B) Ajusta capacidad automáticamente
C) Crea roles
D) Gestiona costos

42. ¿Qué es User Data?
A) Datos del usuario
B) Script que se ejecuta al iniciar EC2
C) Logs
D) Métricas

43. ¿En qué capa OSI está TCP?
A) 3
B) 4
C) 5
D) 7

44. ¿En qué capa OSI está HTTP?
A) 3
B) 4
C) 5
D) 7

45. ¿Qué servicio permite conectar múltiples VPCs y on‑premise?
A) NAT
B) Transit Gateway
C) VPC Endpoint
D) S3

46. ¿Qué tipo de base es DynamoDB?
A) SQL
B) NoSQL clave‑valor
C) Data warehouse
D) Cache

47. ¿Qué mejora la disponibilidad en RDS?
A) Read Replicas
B) Multi‑AZ
C) Backups
D) Parameter Groups

48. ¿Qué servicio es un cache administrado?
A) DynamoDB
B) ElastiCache
C) Aurora
D) S3

49. ¿Qué motor es compatible con Aurora?
A) Oracle
B) SQL Server
C) MySQL y PostgreSQL
D) MongoDB

50. ¿Qué es DAX?
A) Un motor SQL
B) Un acelerador de DynamoDB
C) Un balanceador
D) Un firewall

51. ¿Qué servicio registra acciones en la cuenta?
A) CloudWatch
B) CloudTrail
C) Config
D) GuardDuty

52. ¿Qué servicio evalúa cumplimiento de reglas?
A) CloudTrail
B) Config
C) CloudWatch
D) Inspector

53. ¿Qué servicio gestiona métricas y alarmas?
A) CloudTrail
B) CloudWatch
C) Config
D) IAM

54. ¿Qué servicio detecta actividad sospechosa?
A) GuardDuty
B) CloudTrail
C) Config
D) S3

55. ¿Qué servicio permite dashboards de métricas?
A) CloudTrail
B) CloudWatch
C) Config
D) IAM

56. ¿Qué servicio usa colas?
A) SNS
B) SQS
C) Lambda
D) API Gateway

57. ¿Qué servicio expone APIs?
A) Lambda
B) API Gateway
C) EventBridge
D) SQS

58. ¿Qué servicio ejecuta código sin servidores?
A) EC2
B) Lambda
C) ECS
D) Batch

59. ¿Qué arquitectura es típica en serverless?
A) EC2 → RDS
B) API Gateway → Lambda → DynamoDB
C) S3 → EC2 → RDS

60.¿Qué arquitectura es típica en serverless?
A) EC2 → RDS
B) API Gateway → Lambda → DynamoDB 
C) S3 → EC2 → RDS 
D) VPC → NAT → EC2

### mis respuestas 
b,b,b,b,b,c,c,b,a,b,a,b,b,b,c,b,a,b,d,b,a,b,b,a,b,b,b,b,c,b,a,b,b,b,a,b,b,b,b,b,b,b,b,d,b,b,c,b,c,b,a,d,a,c,d,b,b,b,b

**Total de fallos: 10**: seguiremos practicando y emepzaremos ha realizar simulacros AWS parecidos al examen

### Respuestas correctas 
1 B 2 B 3 B 4 B 5 B 6 C 7 C 8 B 9 A 10 B 11 A 12 B 13 B 14 B 15 C 16 B 17 A 18 B 19 B 20 B 21 A 22 B 23 B 24 A 25 
B 26 B 27 B 28 B 29 C 30 B 31 B 32 B 33 B 34 B 35 A 36 B 37 B 38 B 39 B 40 B 41 B 42 B 43 B 44 D 45 B 46 B 47 B 48 B 49 C 50 B 51 B 52 C 53 B 54 A 55 B 56 B 57 B 58 B 59 B 60 B



