# Tic Tac Agency - Servidor Central de Automatización (n8n)

Servidor de orquestación de flujos de automatización e inteligencia artificial para **Tic Tac Agency Performance SAS**. Esta instancia autoalojada de **n8n** desplegada en **Render** gestiona los flujos conversacionales, bots de atención al cliente, cotizadores automáticos e integraciones de IA (LLMs) para las marcas y clientes de la agencia.

---

## 🏗️ Arquitectura del Sistema

El proyecto utiliza una arquitectura de microservicios en la nube:

* **Orquestador Principal:** [n8n](https://n8n.io/) desplegado en [Render Cloud Platform](https://render.com/) mediante contenedor Docker y base de datos **PostgreSQL en Render**.
* **Integración WhatsApp Directa:** Meta WhatsApp Business Cloud API (Directa desde Meta Graph API `v20.0`).
  * **Nombre de la App de Meta:** `Tic Tac Agency N8N` (App ID: `1533701941786947` - Estado: **Publicada / Live**).
  * **Cuenta WABA:** `Tic Tac Agency Bot` (WABA ID: `1023085523781668`).
  * **Identificador de Número de Teléfono (Phone ID):** `1209428932251589` (`+57 311 8849896`).
* **Soporte de Coexistencia / BSP:** [YCloud](https://ycloud.com/) (API Key activa para compatibilidad con la App móvil de WhatsApp Business en smartphone).
* **Inteligencia Artificial:** Google Gemini (1.5 Flash / 2.0 Flash) en Google AI Studio para clasificación de intenciones y procesamiento multimodal.
* **Base de Datos / Inventario & CRM:** Google Sheets / PostgreSQL Render.

---

## 🔌 Webhook Endpoints en Render

El servidor n8n expone dos puntos de enlace (Webhooks) seguros bajo la URL de producción `https://n8n-gafas-antioquia.onrender.com`:

| Endpoint | Origen | Descripción |
| :--- | :--- | :--- |
| `/webhook/whatsapp` | Meta Developers (Cloud API Directa) | Recibe reto de verificación GET (`hub.challenge`) y eventos POST de mensajes entrantes directo desde servidores de Meta. |
| `/webhook/ycloud` | YCloud API | Recibe eventos de mensajes entrantes en modo Coexistencia. |

---

## 🤖 Flujo de Atención Conversacional (Workflow)

1. **Recepción del Mensaje Entrante:**
   * El cliente escribe a la línea de WhatsApp `+57 311 8849896`.
   * Meta Cloud API o YCloud envía el payload del mensaje entrante al webhook de n8n.
2. **Evaluación de Intención con IA (Gemini):**
   * El modelo de lenguaje analiza el mensaje entrante y clasifica la necesidad del usuario (consultas, asesoría de servicios de la agencia, cotizaciones).
3. **Respuesta Automática en Tiempo Real:**
   * n8n procesa la respuesta y realiza la llamada POST de retorno a la API de WhatsApp para responderle al cliente en menos de 1 segundo.
4. **Transferencia a Asesor Humano:**
   * Si se detecta intención de cierre de contrato o consulta especializada, el flujo etiqueta la conversación y transfiere la atención a la App móvil de WhatsApp Business.

---

## 🛠️ Variables de Entorno (.env & Render)

Las siguientes variables de entorno deben estar configuradas en la consola de **Render** y en el archivo local `.env`:

```bash
# Nombre del Servicio
SERVICE_NAME=n8n-tictac-agency

# Base de datos PostgreSQL en Render
DB_TYPE=postgresdb
DB_POSTGRESDB_HOST=dpg-xxxx-a.render.com
DB_POSTGRESDB_PORT=5432
DB_POSTGRESDB_DATABASE=n8n_tictac_db
DB_POSTGRESDB_USER=n8n_tictac_user
DB_POSTGRESDB_PASSWORD=xxxx

# Llave de Encriptación n8n y API Key
N8N_ENCRYPTION_KEY=xxxx
N8N_API_KEY=xxxx

# Configuración Meta Cloud API & YCloud
META_PHONE_NUMBER_ID=1209428932251589
META_WABA_ID=1023085523781668
YCLOUD_API_KEY=76bceb57f1aea93641540fe98a2091f5

# Mantenimiento de Ejecuciones
EXECUTIONS_DATA_PRUNE=true
EXECUTIONS_DATA_MAX_AGE=168
```

---

## 🐙 Despliegue en Render Cloud

El servicio se despliega automáticamente mediante Git Push al repositorio oficial:
`https://github.com/dsancalderon/tictac-agency-n8n.git`

El archivo `render.yaml` define la infraestructura del Web Service de Render y la base de datos PostgreSQL asociada.