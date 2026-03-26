# Temas que hay que repasar 
## 1. CloudHSM vs ACM
- AWS Certificate Manager (ACM)
  - Gestiona certificados SSL/TLS.
  - AWS administra las llaves.
  - No usa hardware dedicado.

- AWS CloudHSM
  - HSM = Hardware Security Module.
  - Hardware físico dedicado solo para tu cuenta.
  - Cumple requisitos legales estrictos.
  - Tú administras las llaves.

- **Palabra clave: Hardware dedicado → CloudHSM.**

## 2. AWS Budgets vs Cost Explorer
- Cost Explorer
  - Analiza el pasado.
  - Muestra consumo histórico.
    
- AWS Budgets
  - Planifica el futuro.
  - Envía alertas antes de pasarte del presupuesto.

---
**Regla rápida**:
- Ver lo gastado = Cost Explorer
- Evitar gastar de más = Budgets

## 3. AWS Organizations
- Administra múltiples cuentas.
- Facturación consolidada.
- SCP (Service Control Policies).
- Compartir recursos.
- Estructura jerárquica.

- **Palabra clave: Multi‑cuenta → Organizations.**

## 4. Shield vs GuardDuty vs Inspector vs Macie

|Servicio	|Función	|Palabra Clave |
|:--|:--|:--|
|Shield |	Protege contra ataques masivos	|DDoS |
|GuardDuty |	Detecta comportamiento sospechoso	|Anomalías|
|Inspector	|Busca vulnerabilidades en EC2	|Vulnerabilidades|
|Macie	|Detecta datos sensibles en S3	|Datos privados|

**Regla rápida:**
- Comportamiento raro = GuardDuty
- DDoS = Shield
- Datos sensibles = Macie
- Fallos en servidores = Inspector

## 5. AWS Acceptable Use Policy
Documento legal que define acciones prohibidas en AWS.

- **Ejemplos:**
  - Spam.
  - Minería ilegal.
  - Ataques de hacking.
  - Actividades maliciosas.

- Si ves “prohibited actions”, “legal”, “conduct” → Acceptable Use Policy.

## 6. CloudTrail vs Config vs X-Ray vs Trusted Advisor
AWS CloudTrail
- Registra quién hizo qué.
- Auditoría.
---
AWS Config
- Historial de configuración.
- Cómo estaba antes.
---
AWS X-Ray
- Rastrea solicitudes dentro de una app.
- Diagnóstico de rendimiento.
---
AWS Trusted Advisor
- Consejos de seguridad, costos y límites.
- Te avisa si estás cerca del límite de servicio.

--- 

- Quién hizo esto → CloudTrail
- Cómo cambió la config → Config
- Mi app va lenta → X-Ray
- Me paso del límite → Trusted Advisor

## 7. Servicios gratuitos vs pagados
Servicios que no cobran por usarlos
AWS CloudFormation (solo pagas los recursos que crea).

### IAM.

- AWS Organizations.
- Elastic Beanstalk.
---
Servicios que sí cobran
- EC2.
- AWS Config.
- SNS.
- Cualquier recurso desplegado por CloudFormation o Beanstalk.

---
**Regla rápida**:
Herramientas de gestión suelen ser gratis; los recursos que crean, no.
