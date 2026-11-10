# SalesFlow AI | Preentrega 1

**Curso:** AI Automation Avanzado, Coderhouse  
**Proyecto:** AI Lead Qualification & Sales Manager  
**Módulo:** M1, Agente base en n8n  
**Empresa de ejemplo:** Nova Automation (ficticia)

## Descripción del proyecto

SalesFlow AI es un asistente comercial basado en inteligencia artificial, desarrollado en n8n para responder consultas iniciales sobre servicios de automatización e IA, consultar un catálogo de servicios y orientar a potenciales clientes.

Esta primera versión establece la base técnica de un proyecto que evolucionará hacia un sistema de calificación de leads y gestión comercial con múltiples agentes.

## Objetivos del M1

- Configurar un agente con un System Prompt que defina rol, ámbito, objetivos, reglas y criterios de escalamiento.
- Conectar un modelo de lenguaje al agente.
- Incorporar una herramienta de consulta del catálogo comercial.
- Limitar la cantidad de iteraciones del agente.
- Registrar internamente las consultas, respuestas y uso de herramientas para observabilidad.

## Arquitectura

```text
Chat Trigger
     |
     v
  AI Agent ------------------ Groq Chat Model
     |
     +----------------------- Google Sheets Tool (lectura de catálogo)
     |
     v
Gmail (registro interno de ejecución)
```

**Componentes:**

1. **Chat Trigger:** recibe las consultas del usuario.
2. **AI Agent:** interpreta la consulta y decide si necesita una herramienta.
3. **Groq Chat Model:** modelo `openai/gpt-oss-20b` utilizado por el agente.
4. **Google Sheets Tool:** consulta de solo lectura al catálogo de servicios de Nova Automation.
5. **Gmail:** envía un registro interno con el mensaje recibido, la respuesta y un resumen de las herramientas ejecutadas.

## Catálogo de ejemplo

| Servicio | Precio desde (USD) |
| --- | ---: |
| Chatbot IA | 300 |
| Automatización CRM | 500 |
| Automatización de marketing | 400 |
| Consultoría IA | 150 |

Los precios son datos ficticios de demostración y no constituyen una oferta comercial real.

## Guardrails y supervisión

- Máximo de **7 iteraciones** por ejecución.
- Activación de **Return Intermediate Steps** para registrar el uso de herramientas.
- Prohibición de inventar precios, características, descuentos o condiciones no verificadas.
- Prohibición de confirmar reuniones, enviar solicitudes o prometer derivaciones que el agente no puede ejecutar.
- Identificación de solicitudes que requieren revisión humana, como descuentos especiales o condiciones comerciales excepcionales.
- La derivación humana es **conceptual** en M1: el agente reconoce cuándo se necesita un asesor, pero no realiza todavía la transferencia automáticamente.

## Pruebas funcionales

| Caso | Consulta de ejemplo | Comportamiento esperado |
| --- | --- | --- |
| 1. Saludo | «Hola, ¿cómo estás?» | Responder sin consultar el catálogo. |
| 2. Precio | «¿Cuánto cuesta implementar un chatbot de IA?» | Consultar Google Sheets e informar el precio verificado. |
| 3. Consulta comercial | «Tengo una empresa de 20 empleados y necesito automatizar mi CRM.» | Identificar el servicio y hacer preguntas de calificación. |
| 4. Reunión | «Reservame una reunión mañana.» | No confirmar una reserva que no puede realizar. |
| 5. Escalamiento | «Necesito un descuento del 40 % y hablar con un responsable comercial.» | Reconocer que requiere aprobación humana, sin afirmar que la derivación se realizó. |

Estas pruebas fueron realizadas durante el desarrollo. El JSON exportado contiene la configuración del workflow, no el historial de ejecuciones.

## Cómo importar y configurar

1. Importar `checkpoint1_romina_marchetti.json` en n8n.
2. Configurar credenciales propias para Groq, Google Sheets y Gmail.
3. Crear o seleccionar una hoja de cálculo con un catálogo equivalente y configurar el nodo de Google Sheets.
4. Configurar el destinatario interno del registro en el nodo Gmail.
5. Ejecutar las pruebas desde Chat Trigger y verificar las respuestas y los registros.

## Seguridad y privacidad

El workflow publicado está **sanitizado para uso público**: no incluye secretos, tokens, credenciales conectadas ni identificadores privados de recursos. Los valores de ejemplo deben reemplazarse al importar el archivo. No se deben publicar archivos `.env`, contraseñas ni capturas con información sensible.

## Próximos pasos

En el M2, evolucionar esta base hacia un **Manager** que coordine dos **Workers** mediante subworkflows y contratos de datos estructurados. En módulos posteriores se prevé incorporar memoria, integraciones comerciales y otras capacidades.

## Entregable

- `checkpoint1_romina_marchetti.json`: exportación del workflow n8n correspondiente a la Preentrega 1.
