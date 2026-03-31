# 🤖 Procesador de RUT — Automatización con IA

Pipeline automatizado que recibe documentos RUT colombianos (PDF), extrae datos estructurados usando modelos de visión IA, normaliza la información y la enruta al endpoint correspondiente según si la entidad es un **cliente** o un **usuario**.

Construido completamente en **n8n** e integrado con **OpenAI GPT-4o** para inteligencia documental.

---

## 🧩 Arquitectura — 3 Workflows

```
┌─────────────────────────────────────────────────────┐
│           1. WORKFLOW PRINCIPAL (Orquestador)        │
│  Recibe solicitudes entrantes (cliente o usuario)   │
│  Enruta al Procesador de PDF → normaliza datos      │
│  Envía al endpoint correcto (cliente / usuario)     │
└────────────────────┬────────────────────────────────┘
                     │
         ┌───────────▼───────────┐
         │  2. PROCESADOR PDF    │
         │  Recibe el PDF        │
         │  Extrae datos con     │
         │  GPT-4o Vision API    │
         │  Retorna JSON         │
         │  estructurado         │
         └───────────────────────┘

┌─────────────────────────────────────────────────────┐
│              3. MANEJADOR DE ERRORES                │
│  Se activa ante cualquier fallo en los workflows    │
│  Envía alerta instantánea a Telegram                │
└─────────────────────────────────────────────────────┘
```

---

## ⚙️ Cómo funciona

1. Se recibe un documento RUT (PDF) junto con una indicación de si la entidad es **cliente** o **usuario**
2. El **Workflow Principal** recibe la solicitud y dispara el **Procesador de PDF**
3. El **Procesador de PDF** envía el documento a **GPT-4o Vision**, que extrae los campos clave: NIT, DV, razón social, dirección, régimen tributario y cédula
4. Los datos extraídos regresan al **Workflow Principal**, donde son normalizados y validados
5. La información normalizada se envía al endpoint correspondiente — **endpoint de clientes** o **endpoint de usuarios** — que registra el log
6. Si algún paso falla, el **Manejador de Errores** se activa de inmediato y envía una alerta detallada al canal de **Telegram**

---

## 🛠️ Stack Tecnológico

| Capa | Tecnología |
|---|---|
| Motor de automatización | n8n |
| IA / Extracción documental | OpenAI GPT-4o Vision API |
| Formato de documento | PDF (RUT DIAN Colombia) |
| Almacenamiento de datos | Endpoint REST API → log |
| Monitoreo de errores | Bot de Telegram |

---

## 📁 Archivos de Workflows

| Archivo | Descripción |
|---|---|
| `01_workflow-principal.json` | Orquestador — recibe solicitudes, normaliza datos y enruta a endpoints |
| `02_procesador-pdf.json` | Procesa el PDF con GPT-4o y retorna datos estructurados |
| `03_manejador-errores.json` | Monitorea fallos en todos los workflows y envía alertas por Telegram |

---

## 📊 Resultados

- ✅ **0 intervención manual** en el proceso de ingesta de RUT
- ✅ **~80% de reducción** en errores de carga de datos vs. proceso manual
- ✅ Visibilidad de errores en tiempo real vía Telegram
- ✅ Maneja entidades de tipo cliente y usuario en un único pipeline unificado

---

## 👤 Autor

**Diego Andrés Navarro Cano**  
Desarrollador Backend & Especialista en Automatización  
[linkedin.com/in/desarrolladorbackend-automatizaciones](https://www.linkedin.com/in/desarrolladorbackend-automatizaciones/)
