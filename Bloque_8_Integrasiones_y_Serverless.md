# Bloque 8 — Integraciones y Serverless (API Gateway, EventBridge, SNS, SQS, Lambda)
Este bloque reúne los servicios clave para construir arquitecturas serverless y conectar componentes dentro de AWS mediante eventos, colas y notificaciones.
Es uno de los bloques más preguntados en el examen CCP porque combina integración, automatización y comunicación entre servicios.

## 1. API Gateway
Servicio que expone APIs de forma segura, escalable y sin servidores.

- Crear APIs REST
- Crear APIs HTTP (más baratas y rápidas)
- Crear APIs WebSocket (tiempo real)
- Conectar clientes con Lambda, EC2, DynamoDB, etc.
- Manejar autenticación (IAM, Cognito, API Keys)
- Aplicar throttling (límite de peticiones)
- Hacer caché opcional
- Transformar requests/responses

#### Arquitectura típica
```text
Cliente → API Gateway → Lambda → DynamoDB
```

---
**Tipos de API**

- **REST API →** más completa, más cara
- **HTTP API →** más simple, más barata, ideal para Lambda
- **WebSocket API →** comunicación en tiempo real

---

**Ejemplos típicos del examen**
- “Exponer una Lambda como endpoint HTTP” → API Gateway
- “Limitar peticiones por segundo” → Throttling (API Gateway)
- “Autenticar usuarios con Cognito antes de acceder a una API” → API Gateway + Cognito

## EventBridge
Bus de eventos de AWS.
Permite conectar servicios entre sí mediante reglas basadas en eventos.
- Reaccionar a eventos de AWS
- Integrarse con aplicaciones SaaS (Shopify, Zendesk, Auth0…)
- Recibir eventos personalizados
- Crear cron jobs (tareas programadas)
---
**Ejemplos de eventos**
- “Se subió un archivo a S3”
- “EC2 cambió de estado”
- “Ejecutar Lambda cada 5 minutos”
- “Un usuario se registró en Cognito”
---
**Ejemplos típicos del examen**
- “Ejecutar una Lambda cada 5 minutos” → EventBridge (regla programada)
- “Procesar automáticamente archivos subidos a S3” → EventBridge
- “Conectar un evento de Shopify con Lambda” → EventBridge (SaaS)

## SNS (Simple Notification Service)
Servicio de publicación/suscripción (pub/sub).

**Características**
- Un mensaje → muchos destinos
- Destinos posibles:
- Email
- SMS
- Lambda
- SQS
- HTTP endpoints
---
**Uso típico**
- Notificaciones masivas
- Alertas
- Disparar varias Lambdas a la vez
- “Enviar un mensaje a varios sistemas al mismo tiempo” → SNS

## SQS (Simple Queue Service)
Servicio de colas.
---
**Características:**
- Un mensaje → un consumidor
- Desacopla sistemas
- Garantiza entrega
- Retiene mensajes hasta 14 días
- Escala automáticamente
---
**Uso típico**
- Procesamiento en orden
- Tareas que no deben perderse
- Sistemas desacoplados
- “Procesar mensajes uno por uno sin perderlos” → SQS

###  Diferencia clave: SNS vs SQS
|Servicio	|Tipo	|Ejemplo |
|:--|:--|:--|
|SNS	|Pub/Sub	| Un mensaje → muchos destinos|
|SQS	|Cola |	Un mensaje → un consumidor |

---
**Pregunta típica**
- “Quieres enviar una notificación por email y SMS al mismo tiempo” → SNS
- “Quieres desacoplar productores y consumidores” → SQS

## AWS Lambda
Servicio serverless que ejecuta código bajo demanda.

- Escala automáticamente
- Paga por invocación
- No requiere servidores
- Integración nativa con:
- API Gateway
- EventBridge
- DynamoDB
- S3
- “Ejecutar código sin administrar servidores” → Lambda
- “Responder a un evento de S3” → Lambda + EventBridge
- “Backend serverless para una API” → API Gateway + Lambda

# Arquitectura serverless típica
```text
Cliente → API Gateway → Lambda → DynamoDB
```
Esta arquitectura es:
- Escalable
- Barata
- Sin servidores
- Muy usada en el examen

## Resumen rápido del bloque
- API Gateway → expone APIs
- EventBridge → reacciona a eventos
- SNS → notificaciones (uno a muchos)
- SQS → colas (uno a uno)
- Lambda → ejecuta código serverless
