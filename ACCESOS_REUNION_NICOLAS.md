# Accesos y definiciones para completar el generador de informes

## 1. HubSpot

### Acceso

- [ ] Crear una Private App para la automatización.
- [ ] Entregar el access token por un canal seguro.
- [ ] Confirmar el Portal / Account ID.
- [ ] Autorizar lectura de contactos.
- [ ] Autorizar lectura de empresas.
- [ ] Autorizar lectura de negocios, si las ventas están en Deals.
- [ ] Autorizar lectura de propietarios.
- [ ] Autorizar lectura de esquemas y propiedades personalizadas.

No se requieren permisos de escritura durante la primera fase.

### Definiciones

- [ ] ID del pipeline utilizado.
- [ ] IDs y significado de las etapas.
- [ ] Propiedad que identifica el proyecto o cliente.
- [ ] Propiedad utilizada para MQL.
- [ ] Propiedad utilizada para No Nicho.
- [ ] Propiedad utilizada para Pendiente de clasificar.
- [ ] Propiedad utilizada para Venta.
- [ ] Propiedad o criterio de lead caliente.
- [ ] Regla para contar leads duplicados.
- [ ] Fecha utilizada para incluir un registro en el informe.

## 2. Meta Ads

### Acceso

- [ ] Acceso al Business Manager.
- [ ] ID del Business Manager.
- [ ] ID de la cuenta publicitaria.
- [ ] Token de usuario de sistema o integración.
- [ ] Permiso `ads_read`.
- [ ] Permiso para consultar insights y campañas.
- [ ] Confirmar campañas asociadas a Proyecto Pekín.

### Definiciones

- [ ] Campañas de Vivienda.
- [ ] Campañas de Inversionistas.
- [ ] Campañas de Apartaestudio.
- [ ] Campañas de Reconocimiento / interacción.
- [ ] Métrica que se considera lead.
- [ ] Moneda y zona horaria de la cuenta.
- [ ] Desgloses requeridos: fecha, edad, género, plataforma y ubicación.

## 3. Google Ads

### Acceso

- [ ] Customer ID de la cuenta.
- [ ] Login Customer ID, si se utiliza una cuenta administradora MCC.
- [ ] Developer Token de Google Ads API.
- [ ] OAuth conectado a una cuenta con acceso.
- [ ] Confirmar acceso a las campañas de Proyecto Pekín.

### Definiciones

- [ ] Campañas PMAX incluidas.
- [ ] Acción de conversión que debe contabilizarse.
- [ ] Métricas: inversión, impresiones, clics, conversiones y costo por conversión.
- [ ] Datos de subasta requeridos.
- [ ] Zona horaria y moneda.

## 4. Google Sheets y archivos operativos

- [ ] Enlace de la hoja de metas.
- [ ] Compartir la hoja con permiso de lectura al usuario OAuth de n8n.
- [ ] Nombre de cada pestaña relevante.
- [ ] Columnas que contienen cliente, periodo, meta e inversión.
- [ ] Enlace de la carpeta de destino de informes.
- [ ] Permiso de edición sobre la carpeta de destino.
- [ ] Correo de las personas que deben poder abrir los informes.
- [ ] Ejemplos de capturas o gráficos que se actualizan semanalmente.

## 5. IA estratégica

- [ ] Definir Gemini u OpenAI.
- [ ] API key por un canal seguro.
- [ ] Cuenta responsable de consumo y facturación.
- [ ] Modelo autorizado.
- [ ] Límite mensual de uso.
- [ ] Ejemplos de redacción aprobada por Nicolás.
- [ ] Palabras, afirmaciones o recomendaciones que deben evitarse.

## 6. Reglas del informe

- [ ] Día y hora de corte semanal.
- [ ] Zona horaria oficial.
- [ ] Periodo exacto: últimos 7 días o semana calendario.
- [ ] Lista inicial de clientes y aliases.
- [ ] Metas por cliente y campaña.
- [ ] Convención del nombre del archivo.
- [ ] Carpeta de destino por cliente.
- [ ] Entrega como enlace, PDF, presentación o combinación.
- [ ] Destinatarios autorizados por WhatsApp.
- [ ] Tratamiento cuando falten datos.
- [ ] Persona que aprueba el informe antes de enviarlo al cliente.

## 7. Seguridad

- [ ] No compartir contraseñas personales.
- [ ] Usar OAuth, Private Apps, usuarios de sistema o tokens revocables.
- [ ] Aplicar permisos mínimos de lectura.
- [ ] Definir responsable de rotar tokens.
- [ ] Guardar secretos únicamente en credenciales de n8n o variables de Render.
- [ ] No subir secretos al repositorio.
