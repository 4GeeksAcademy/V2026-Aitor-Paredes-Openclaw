# TOOLS.md - Servicios conectados mediante Zapier

Inventario comprobado el 21-11-2026. Describe los servicios y capacidades visibles en Zapier; no es un inventario exhaustivo campo por campo.

Antes de cada operación, inspecciona el esquema actual de la acción para conocer parámetros, valores dinámicos y salida.

## Reglas generales

- Primero llama a `inspect_zapier_actions` sin argumentos para comprobar qué aplicaciones están habilitadas. Después inspecciona la aplicación y acción concretas y su esquema mediante `tool_name`. No adivines nombres de acción ni parámetros.
- Las lecturas y búsquedas se ejecutan con `execute_zapier_read_action`; los cambios con `execute_zapier_write_action`.
- Usa la cuenta predeterminada omitiendo `connection_id`, salvo que Aitor indique otra cuenta. Si menciona una cuenta alternativa, consulta las conexiones y confirma si hay más de una coincidencia plausible.
- No guardes IDs de conexión en este documento.
- Resuelve campos dinámicos como carpetas, calendarios, hojas, listas y destinatarios inspeccionando la acción con sus parámetros previos. No inventes valores.
- Confirma datos del destinatario, documento, evento o registro cuando haya ambigüedad. No asumas que el primer resultado de una búsqueda es el correcto.
- Gmail: enviar, responder o reenviar correo es una acción externa. Hazlo solo cuando Aitor lo solicite claramente y confirma destinatarios y contenido si son ambiguos. Usa un borrador cuando solicite redactar sin indicar envío explícito.
- Google Drive: antes de compartir fuera del entorno conectado, crear un enlace público, borrar o modificar un documento existente, pide confirmación explícita para esa acción. La frase requerida es `Confirmo este envío apb`.
- No incluyas datos personales innecesarios de estudiantes.
- Si un archivo o información sale del entorno de Drive, envía después un correo de seguimiento a la cuenta vinculada de Aitor resumiendo qué se envió y a quién. Las lecturas internas no requieren ese aviso.
- Trata borrados, cambios de permisos, sobrescrituras y modificaciones como operaciones delicadas. Confirma primero cuando no se hayan solicitado explícitamente. Evita borrados permanentes salvo solicitud inequívoca.

## Servicios

### Google Docs

- Uso: buscar y leer documentos; crear documentos desde texto o plantilla; insertar o anexar texto; buscar y reemplazar; aplicar formato; actualizar propiedades e imágenes.
- Estado: hay dos conexiones disponibles y una está predeterminada.
- Acciones relevantes: Find a Document, Get Document Content/Tabs Content, Create Document From Text/Template, Append Text, Insert Text, Find and Replace, Format Text, Update Document Properties.
- Valores por defecto: usar la conexión predeterminada. Identificar y validar la plantilla o el documento por nombre o ID antes de actualizarlo.

### Google Calendar

- Uso: consultar eventos, calendarios y periodos ocupados; crear, actualizar o mover eventos; gestionar asistentes.
- Estado: hay dos conexiones disponibles y una está predeterminada.
- Acciones relevantes: Find Events, Find Calendars, Get Calendar Information, Find Busy Periods, Retrieve Event by ID, Create Detailed Event/Quick Add Event, Update Event, Move Event, Add/Remove Attendees, Delete Event.
- Valores por defecto: usar el calendario y la cuenta predeterminados solo si la petición no especifica otros. Usa la zona horaria del calendario consultando sus propiedades y conserva horas exactas con zona indicada. No añadas invitados ni borres eventos sin una petición clara.

### Gmail

- Uso: buscar y leer correos y conversaciones; recuperar adjuntos; etiquetar o archivar; crear borradores y enviar, responder o reenviar.
- Estado: hay una conexión disponible y está predeterminada.
- Acciones relevantes: Find Email, Get Conversation, Get Attachment by Filename, Add/Remove Label, Archive Email, Create Draft/Reply, Send Email, Reply to Email, Forward Email.
- Valores por defecto: usar el buzón predeterminado. Crear borrador si se solicita redactar pero no enviar.
- Limitación: las búsquedas activadas por Zapier tienen limitaciones temporales indicadas por la propia acción. No confundas un trigger con una búsqueda histórica.

### Google Drive

- Uso: buscar y recuperar archivos y carpetas; consultar permisos; crear carpetas o archivos; copiar, mover, exportar, actualizar metadatos o gestionar permisos.
- Estado: hay una conexión disponible y está predeterminada.
- Acciones relevantes: Find a File/Folder, Retrieve File or Folder by ID, Get File Permissions, Create File From Text, Create Folder, Copy/Move File, Export File, Update Metadata/Name, Add/Remove File Sharing Preference.
- Valores por defecto: usar la cuenta predeterminada y conservar el archivo original cuando sea posible.
- Un enlace de uso compartido puede ampliar el acceso. Sigue siempre la regla de confirmación de Drive. La acción `Delete File (Permanent)` es irreversible y no debe usarse sin solicitud inequívoca y confirmación explícita.

### Google Tasks

- Uso: buscar tareas incompletas y listar tareas por lista; crear o actualizar tareas y listas; detectar tareas nuevas o completadas.
- Estado: hay una conexión disponible llamada Google Tasks.
- Acciones relevantes: Find Task, Get Tasks by List, Create Task, Update Task, Create Task List.
- Valores por defecto: usar la lista predeterminada si no se especifica otra. No crees una nueva lista salvo que se solicite o sea imprescindible y se confirme el nombre.

### Google Sheets

- Uso: buscar hojas y filas; leer rangos o lotes; crear o actualizar filas; crear hojas o columnas; copiar rangos; ordenar y aplicar formato o validación.
- Estado: hay una conexión disponible y está predeterminada.
- Acciones relevantes: Find Spreadsheet/Worksheet, Get Data Range, Get Many Spreadsheet Rows, Lookup Spreadsheet Row(s), Create/Update Spreadsheet Row(s), Create Spreadsheet, Create Worksheet, Copy Range, Format, Sort Range.
- Valores por defecto: identifica el archivo y la hoja por nombre o ID antes de realizar cambios. Respeta los encabezados y columnas existentes. No limpies ni elimines filas u hojas salvo petición explícita. Algunas acciones de limpieza y borrado son permanentes.

### Microsoft Excel

- Uso disponible: leer celdas, filas y hojas; localizar filas; añadir o actualizar filas; crear libros o hojas; borrar contenido o filas; cambiar hojas.
- Estado: las acciones aparecen habilitadas, pero Zapier informa que hay 0 conexiones. No está listo para ejecutar hasta conectar una cuenta y establecerla como predeterminada.
- Valores por defecto: ninguno configurado. Solicita la conexión antes de operar. No asumas que la conexión de Google Sheets sirve para Excel.
