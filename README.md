# 📧 Clasificador de Emails con IA para Clínicas

Automatización con **N8N + Claude API** que clasifica emails entrantes de una clínica dental en tiempo real, envía alertas por Telegram cuando hay una urgencia, y responde automáticamente al paciente sin intervención humana.

---

## 🎯 Problema que resuelve

Las clínicas reciben decenas de emails al día mezclados: pacientes con dolor urgente, consultas de precio, facturas de proveedores y spam. Alguien tiene que leerlos todos para saber cuáles son urgentes — y a veces esos emails se pierden horas o días.

Con este sistema, la IA lo clasifica en segundos y responde sola.

---

## ⚙️ Cómo funciona

```
Gmail (nuevo email sin leer)
        ↓
Claude API
  → Clasifica en 4 categorías
  → Redacta respuesta automática si corresponde
        ↓
¿Es URGENTE?
  ├── SÍ → Alerta Telegram inmediata
  └── NO → Continúa
        ↓
¿Tiene respuesta redactada?
  ├── SÍ → Envía email automático al remitente
  └── NO → Continúa
        ↓
Google Sheets → Registro de todos los emails
```

### Categorías de clasificación

| Categoría | Descripción | Telegram | Respuesta automática |
|-----------|-------------|----------|----------------------|
| 🚨 URGENTE | Dolor, emergencia, necesita atención hoy | ✅ Sí | ✅ Sí — "Le llamamos en menos de 1 hora" |
| 📅 CITA | Pedir hora, cancelar, consultar servicios | ❌ No | ✅ Sí — Invita a llamar o responder |
| 📋 ADMIN | Proveedor, factura, tema interno | ❌ No | ❌ No |
| 🗑️ SPAM | Publicidad, irrelevante | ❌ No | ❌ No |

---

## 🛠️ Stack tecnológico

- **N8N** — Orquestación del workflow
- **Claude API (Anthropic)** — Clasificación + redacción de respuesta
- **Gmail API** — Lectura y envío de emails
- **Telegram Bot API** — Alertas de urgencia en tiempo real
- **Google Sheets** — Registro y trazabilidad de todos los emails

---

## 🚀 Instalación

### Requisitos previos
- N8N instalado (cloud o self-hosted)
- Cuenta Gmail con OAuth2 en N8N
- API Key de Anthropic
- Bot de Telegram creado con @BotFather
- Google Sheets con columnas: `Fecha | Categoria | Remitente | Asunto | Resumen | Respuesta Enviada`

### Pasos

**1. Importar el workflow**
- N8N → menú superior derecho → Import from file
- Selecciona `email_classifier_v2.json`

**2. Configurar credenciales**

| Nodo | Credencial | Cómo obtenerla |
|------|-----------|----------------|
| Gmail Trigger + Envío | Gmail OAuth2 | N8N → Credentials → New → Gmail OAuth2 |
| Claude API | HTTP Header Auth | Header: `x-api-key` / Valor: tu API key de console.anthropic.com |
| Telegram | Telegram API | Token de @BotFather → Chat ID de @userinfobot |
| Google Sheets | Google Sheets OAuth2 | N8N → Credentials → New → Google Sheets |

**3. Configurar Google Sheets**
- Crea una hoja nueva
- Añade en la fila 1: `Fecha | Categoria | Remitente | Asunto | Resumen | Respuesta Enviada`
- Copia el ID de la URL (el trozo entre `/d/` y `/edit`) y pégalo en el nodo de Sheets

**4. Activar y probar**
- Mándate un email de prueba con texto tipo: "Llevo 3 días con un dolor horrible en la muela"
- Ejecuta el workflow manualmente desde N8N
- Verifica que llega la alerta a Telegram y el email de respuesta
- Si todo funciona, activa el trigger automático

---

## 📱 Ejemplo de alerta Telegram

```
🚨 EMAIL URGENTE RECIBIDO

👤 De: paciente@gmail.com
📧 Asunto: Dolor horrible, necesito cita urgente
🕐 Hora: 24/04/2026 12:30

💬 Mensaje:
Llevo 3 días con un dolor insoportable en...

✅ Respuesta automática enviada al paciente
```

## 📨 Ejemplo de respuesta automática enviada al paciente

```
Asunto: Re: Dolor horrible, necesito cita urgente

Estimado/a paciente,

Hemos recibido su mensaje y entendemos que está pasando 
un momento difícil. Nuestro equipo le contactará 
telefónicamente en menos de 1 hora para atender su urgencia.

Si necesita asistencia inmediata, puede llamarnos al 922 000 000.

Un saludo,
Clínica Dental Demo - Atención al Paciente
```

---

## 🔧 Personalización

Cambia el prompt en el nodo de Claude para adaptarlo a cualquier negocio:
- **Inmobiliarias** — leads, consultas, propietarios, visitas
- **Gestorías** — urgencias fiscales, clientes, documentación
- **E-commerce** — reclamaciones, pedidos, devoluciones, consultas

---

## 👤 Autor

Aday — Automation Developer  
Stack: N8N · Claude API · Python · Google Sheets
