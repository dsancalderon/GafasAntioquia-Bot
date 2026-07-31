# Configuración base de informes

## Workflows

1. Importar `tictac-router-informes.json`.
2. Importar `tictac-generador-informe-base.json`.
3. Abrir `gateway-whatsapp.json` en n8n y, en **Ejecutar Router de Informes**, seleccionar el workflow importado **Tic Tac - Router de Informes**.
4. En **Tic Tac - Router de Informes**, comprobar que **Ejecutar Generador en Segundo Plano** apunta a **Tic Tac - Generador Base de Informe**.
5. Mantener ese nodo con `Wait for Sub-Workflow Completion = false` y enviar:
   - `customerPhone`
   - `clientId`
   - `clientName`

Los IDs internos de workflows los asigna cada instancia de n8n al importarlos, por eso estas dos selecciones se realizan desde la interfaz.

## Datos ya configurados

- Usuarios autorizados: Nicolás, `+573222046768`, y número temporal de pruebas, `+573027697381`.
- Cliente inicial: `CLI-001`, Proyecto Pekín.
- Alias: `Proyecto Pekín`, `Proyecto Kinku`, `Pekín`, `Kinku` y `1`.
- Los textos se comparan sin distinguir mayúsculas ni tildes.

## Integraciones pendientes

Reemplazar los nodos simulados del generador cuando estén disponibles los accesos:

| Nodo actual | Integración definitiva |
| --- | --- |
| Cargar Configuración Temporal | Google Sheets / tabla maestra |
| Recolectar Datos Simulados | HubSpot y fuentes de campañas |
| Generar Análisis Simulado | Gemini u OpenAI con salida JSON |
| Preparar Placeholders de Slides | Google Drive copy + Google Slides batchUpdate |
| Preparar Notificación Final | HTTP Request a YCloud/WhatsApp |

## Variables pendientes

- ID de empresa o filtros de HubSpot.
- ID de la plantilla de Google Slides.
- ID de carpeta de Google Drive.
- Metas semanales.
- Credenciales de Google, HubSpot y Gemini/OpenAI en n8n.

## Pruebas mínimas

Desde el número autorizado:

- `#estado` debe continuar en el control administrativo.
- `Hola` debe mostrar el menú de informes.
- `1`, `Pekín`, `Kinku` o `genera el informe de Proyecto Pekín` deben confirmar el inicio.

Desde otro número:

- Ningún alias debe iniciar un informe.
