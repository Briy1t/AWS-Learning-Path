# Bloque 7 — Monitoreo y Observabilidad (CloudWatch, CloudTrail, AWS Config)
Este bloque cubre los servicios que permiten monitorear, auditar y controlar cambios dentro de AWS. Son esenciales para entender cómo se comportan tus recursos, quién hace cambios y si tu infraestructura cumple buenas prácticas.

## 1. Amazon CloudWatch
Servicio de monitoreo y rendimiento.
- Monitorear métricas (CPU, RAM, latencia, requests, errores)
- Recolectar logs de servicios (Lambda, API Gateway, EC2, RDS…)
- Crear alarmas basadas en métricas
- Crear dashboards visuales
- Activar acciones automáticas (ej: Auto Scaling)
--- 
Componentes principales:

**1. CloudWatch Metrics**
Números que cambian en el tiempo.
Ejemplos:
- CPU de EC2
- Requests de API Gateway
- Latencia de Lambda
- Lecturas de DynamoDB

**2. CloudWatch Logs**
Registros de aplicaciones.
Ejemplos:
- Logs de Lambda
- Logs de API Gateway
- Logs de ECS
- Logs de apps en EC2
---
Se organizan en:
- Log Groups
- Log Streams

---
**3. CloudWatch Alarms**
Alarmas que se disparan cuando una métrica supera un umbral.
Ejemplos:
- CPU > 80%
- Errores 5xx > 10
- Latencia > 200 ms
---
Pueden:
- Enviar notificaciones (SNS)
- Ejecutar acciones (Auto Scaling)

---

**Ejemplos típicos del examen**
- “Tu EC2 está al 90% de CPU, ¿qué servicio te avisa?” → CloudWatch Alarm
- “Quieres ver logs de Lambda” → CloudWatch Logs
- “Quieres ver métricas de DynamoDB” → CloudWatch Metrics
- “Quieres un panel visual” → CloudWatch Dashboard
---

## AWS CloudTrail
Servicio de auditoría.
Registra quién hizo qué en tu cuenta AWS.
Registra:
- Acciones de consola
- Acciones de CLI
- Acciones de API
- Cambios en recursos
- Accesos fallidos y exitosos
---
**Tipos de eventos**
- Management Events → cambios en recursos
- Data Events → acceso a datos (ej: GetObject en S3)
- Insights → comportamientos anómalos
---
## AWS Config
Servicio que registra cambios y verifica cumplimiento.
- Registra cambios en recursos
- Verifica cumplimiento con reglas (Config Rules)
- Guarda historial de configuraciones
---
**Ejemplos:**
- “¿Todos los buckets están cifrados?” → Config Rule
- “¿Cómo era este Security Group hace 1 semana?” → Config
- “¿Quién borró un bucket?” → CloudTrail, no Config

###  Diferencias clave

|Servicio |	¿Para qué sirve? |
|:--|:--|
|CloudWatch |	Métricas, logs, alarmas |
|CloudTrail |	Auditoría: quién hizo qué |
|AWS Config	|Historial de cambios + cumplimiento |

---
## AWS Trusted Advisor
Recomendaciones automáticas sobre:
- Costos
- Seguridad
- Rendimiento
- Fault tolerance
- Service limits

## AWS Health Dashboard
- Service Health Dashboard → estado general de AWS
- Personal Health Dashboard → estado de tus recursos
