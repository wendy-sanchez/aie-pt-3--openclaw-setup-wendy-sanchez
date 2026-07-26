# Conecta tu Agente: Telegram, Google Drive y Calendario

## Objetivo

El objetivo de este proyecto fue configurar OpenClaw para utilizar el servidor MCP (Model Context Protocol) de Composio, autenticar la conexión mediante OAuth y verificar que el agente era capaz de interactuar con aplicaciones externas de Google, concretamente Google Docs y Google Calendar, ejecutando acciones reales desde Telegram.

## Desarrollo del proyecto

- El primer paso consistió en instalar y configurar OpenClaw en el entorno de trabajo. Una vez instalado, se verificó que el Gateway estuviera funcionando correctamente y que el agente pudiera responder desde Telegram utilizando el bot previamente configurado.

- A continuación, se procedió a integrar OpenClaw con Composio mediante su servidor MCP. Debido a que la documentación de la plataforma estaba basada en una versión anterior de Composio, fue necesario adaptar varios pasos a la versión actual de Composio Connect. Para ello se añadió manualmente el servidor MCP utilizando la URL oficial (CLI y login) y el sistema de autenticación OAuth.

- Después de registrar el servidor MCP, se realizó el proceso de autenticación iniciando sesión mediante OAuth, lo que permitió autorizar el acceso de OpenClaw a la cuenta de Composio.

- Posteriormente, se accedió al panel de Composio Connect para conectar las aplicaciones necesarias para el proyecto. Se habilitaron correctamente Google Docs y Google Calendar, verificando que ambas aparecieran con estado **Active**.

## Problema encontrado

- Aunque la configuración parecía correcta y tanto la autenticación como las conexiones de Google estaban activas, el agente seguía respondiendo como si utilizara la antigua integración de Composio. En lugar de acceder a las herramientas MCP disponibles, generaba respuestas relacionadas con instalaciones del CLI o migraciones internas que no correspondían al funcionamiento esperado.

- Para comprobar el estado de la integración se revisó la configuración del MCP mediante OpenClaw y se verificó que el servidor estaba correctamente registrado y que el comando `openclaw mcp probe` detectaba las herramientas disponibles.

- Tras analizar el comportamiento del agente, se identificó que OpenClaw estaba utilizando un runtime MCP almacenado en caché. La solución consistió en ejecutar el comando:

```bash
openclaw mcp reload
```

Este comando reconstruyó el runtime del servidor MCP, permitiendo que el agente comenzara a utilizar las herramientas realmente disponibles desde Composio Connect.

## Verificación de funcionamiento

- Una vez reconstruido el runtime, el comportamiento del agente cambió por completo. En lugar de intentar utilizar integraciones antiguas, comenzó a consultar dinámicamente las herramientas disponibles mediante MCP.

- Para validar la integración se realizó una búsqueda de las acciones disponibles para Google Docs. El agente localizó la herramienta correspondiente para crear documentos y generó automáticamente un documento llamado **"Prueba OpenClaw"** con contenido en formato Markdown. Posteriormente se verificó manualmente que el documento había sido creado correctamente dentro de Google Docs.

- Como segunda prueba se repitió el proceso utilizando Google Calendar. El agente localizó la acción necesaria para crear eventos, obtuvo el esquema de parámetros requerido y creó correctamente un nuevo evento en el calendario asociado a la cuenta de Google. Finalmente se comprobó que el evento aparecía correctamente en Google Calendar.

## Resultado

El proyecto finalizó con éxito, consiguiendo una integración completamente funcional entre OpenClaw y Composio MCP.

Se verificó correctamente:

- Configuración del servidor MCP.
- Autenticación mediante OAuth.
- Conexión con Google Docs.
- Conexión con Google Calendar.
- Comunicación con el agente desde Telegram.
- Creación real de documentos en Google Docs.
- Creación real de eventos en Google Calendar.
