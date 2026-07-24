# Gafas Antioquia - Asistente Virtual de WhatsApp

Asistente automatizado de servicio al cliente y ventas para Gafas Antioquia. Este proyecto integra un flujo conversacional inteligente utilizando n8n, modelos de lenguaje grande (LLMs) y la API oficial de WhatsApp Business a través de YCloud para gestionar consultas de inventario, cotizaciones y derivación a agentes humanos.

## Tecnologías y Arquitectura

El proyecto utiliza una arquitectura de automatización *self-hosted* y servicios en la nube:

*   **Orquestador:** [n8n](https://n8n.io/) (Autoalojado vía Docker).
*   **Proveedor de WhatsApp API:** [YCloud](https://ycloud.com/) (Webhook para recepción y HTTP Requests para envío).
*   **Inteligencia Artificial:** Gemini 1.5 Flash / 2.0 Flash (Google AI Studio) - Seleccionado por su alta velocidad, eficiencia de costos y capacidad de análisis multimodal (imágenes).
*   **Base de Datos / Inventario:** Google Sheets.

## Estructura de la Base de Datos (Inventario)

Para que el modelo pueda identificar y cotizar correctamente los productos, la tabla de inventario requiere las siguientes columnas:

| Columna | Descripción | Ejemplo |
| :--- | :--- | :--- |
| `Modelo` | Nombre interno/comercial de las gafas. | Gafas Aviador Lujo |
| `Categoría` | Tipo de gafas. | de lujo, deportivas, de sol, formuladas |
| `Precio` | Valor comercial. | $150.000 |
| `URL Imagen` | Enlace público a la foto del producto. | `https://.../foto.jpg` |
| `Descripción Visual` | Atributos físicos para el análisis multimodal de la IA. | Marco metálico dorado delgado, lentes circulares oscuros. |

## Flujo de Trabajo (Workflow en n8n)

El bot sigue un embudo de atención estructurado para filtrar intenciones y optimizar el tiempo del equipo de ventas:

### 1. Recepción y Clasificación (Triage)
*   El cliente envía un mensaje a la línea de WhatsApp.
*   Ycloud reponde preguntandole al cliente con un saludo y pregunta en que le podemos ayudar 
*   El cliente responde.
*   YCloud dispara un Webhook hacia n8n.
*   Gemini (1.5/2.0 Flash) analiza el mensaje y lo clasifica en una de 4 categorías: `de lujo`, `deportivas`, `de sol`, o `formuladas`.
*   Si no clasifica el mensaje en las 4 categorías sigue hablando con el cliente hasta determinar  unas de estas 4 categorías

### 2. Derivación de Casos Clínicos
*   Si la categoría es **formuladas**:
    *   n8n envía una solicitud a YCloud para etiquetar el chat/contacto como "Gafas Formuladas".
    *   El bot detiene su intervención y envía una alerta para que un asesor humano tome el control de la conversación.

### 3. Consulta de Catálogo y Envío de Opciones
*   Para las categorías `de lujo`, `deportivas`, o `de sol`:
    *   n8n consulta la hoja de cálculo (Google Sheets) buscando productos disponibles bajo la categoría solicitada.
    *   A través de YCloud, el bot envía al cliente las imágenes (URLs) de los modelos disponibles, preguntando cuál le interesa.

### 4. Detección de Selección del Cliente
El bot está diseñado para interpretar la selección del cliente en tres escenarios posibles:
*   **Escenario A (Imágenes):** El cliente toma un pantallazo de un modelo y lo envía. Gemini analiza visualmente la imagen entrante, la compara con las "Descripciones Visuales" del catálogo y extrae el nombre del modelo.
*   **Escenario B (Cita de Mensaje):** El cliente responde deslizando una de las fotos enviadas. n8n lee el `context.message_id` del payload de YCloud para identificar matemáticamente qué modelo seleccionó.
*   **Escenario C (Lenguaje Natural):** El cliente responde con adjetivos (ej. "las azules delgadas"). Gemini utiliza la memoria de la conversación para deducir a qué modelo se refiere.

### 5. Cotización y Cierre
*   Una vez identificado el modelo, n8n extrae el precio de la base de datos y se lo envía al cliente.
*   Gemini evalúa la respuesta del cliente tras ver el precio. Si detecta intención de compra (ej. "¿Cómo pago?", "Quiero esas"):
    *   n8n etiqueta al contacto en YCloud como "Cliente Potencial".
    *   El flujo se transfiere a un humano para procesar el pago y el envío.

## Instalación y Despliegue Local

Sigue estos pasos para configurar y levantar el entorno de desarrollo:

### 1. Configurar Variables de Entorno
Crea tu archivo `.env` copiando el archivo de ejemplo [.env.example](file:///c:/Users/Santi/GafasAntioquia/.env.example):
```bash
cp .env.example .env
```
Abre el archivo `.env` recién creado y define las variables:
*   `WEBHOOK_URL`: Debe apuntar a la URL pública de tu instancia de n8n para recibir eventos de YCloud (ver sección de Webhooks abajo).
*   `GENERIC_TIMEZONE` y `TZ`: Define tu zona horaria (por defecto `America/Bogota`).

### 2. Levantar los Servicios (Docker Compose)
Levanta la instancia de n8n utilizando el archivo [docker-compose.yml](file:///c:/Users/Santi/GafasAntioquia/docker-compose.yml) configurado:
```bash
docker compose up -d
```
Una vez levantado el contenedor, puedes ingresar a la interfaz gráfica de n8n desde tu navegador en: [http://localhost:5678](http://localhost:5678).

### 3. Exposición de Webhooks con Túnel (ngrok)
Dado que YCloud necesita enviar webhooks a tu instancia local de n8n, debes exponer tu puerto local `5678` a internet. Si usas **ngrok**, ejecuta el siguiente comando:
```bash
ngrok http 5678
```
Copia la URL HTTPS segura generada por ngrok (ejemplo: `https://tu-subdominio.ngrok-free.app`) y asígnala en la variable `WEBHOOK_URL` de tu archivo `.env`. Luego, reinicia los contenedores para aplicar el cambio en n8n:
```bash
docker compose down && docker compose up -d
```

### 4. Configurar Credenciales en n8n
Una vez dentro del panel de n8n, configura las credenciales necesarias para los servicios externos:
1.  **Google Sheets**: Configura el acceso OAuth2 o usa una Service Account para interactuar con la hoja del catálogo de inventario.
2.  **Google Gemini (Google AI Studio)**: Crea una credencial de tipo *Google API* con tu clave de API de Gemini.
3.  **YCloud**: Configura el nodo de YCloud o usa nodos de tipo HTTP Request autorizando tus llamadas mediante tu Token de API de YCloud.