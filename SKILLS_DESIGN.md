# SKILLS_DESIGN.md

## Asistente de clases

1. ¿Qué hace y qué no hace?

   - Hace: interpreta una petición de clase escrita por Telegram, busca al alumno por nombre completo en el archivo `Data Alumnos`, hoja `BBDD Alumnos`, obtiene su correo, consulta Google Calendar, crea una clase online o presencial y prepara el envío del enlace de Meet por Gmail.
   - Usa duración de 90 minutos y modalidad online por defecto.
   - Añade Google Meet para clases online y configura un recordatorio de 30 minutos antes.
   - Gestiona conflictos mostrando el evento existente y las opciones disponibles: reservar igualmente, mover el evento existente, eliminarlo o elegir otro horario.
   - No hace: no adivina el alumno si hay coincidencias múltiples o ninguna; no crea eventos ni Meet sin confirmación; no mueve, elimina ni sobrescribe eventos sin autorización expresa; no envía correos sin una segunda confirmación separada.

2. ¿Cómo se activa y qué necesita para funcionar?

   - Se activa con mensajes como `Clase con Ana García el lunes 30 a las 16:00`.
   - Necesita el nombre completo del alumno, fecha y hora de inicio.
   - Lee el libro `Data Alumnos`, hoja `BBDD Alumnos`, columna `Nombre` y columna `Correo` mediante Google Sheets.
   - Utiliza Google Calendar con duración de 90 minutos, modalidad online y zona horaria `Europe/Madrid` por defecto.
   - Consulta el calendario predeterminado y busca conflictos dentro del intervalo completo de la clase.
   - Si falta el nombre, fecha, hora o correo necesario, pide únicamente el dato que falte.

3. ¿Qué resultado produce, qué herramientas usa y cuándo pide confirmación?

   - Produce un resumen previo con alumno, correo validado, fecha, hora, duración, modalidad, calendario, recordatorio y conflictos.
   - Usa Google Sheets para localizar al alumno y su correo; Google Calendar para consultar disponibilidad y crear, mover o eliminar eventos; Gmail para preparar y enviar el aviso con el enlace Meet.
   - Pide confirmación antes de crear o modificar el evento.
   - Pide una confirmación separada después de crear el evento y mostrar el borrador completo antes de enviar el correo.
   - Si falta el enlace Meet, el alumno no puede identificarse inequívocamente o existe ambigüedad en la hora o destinatario, detiene el flujo y pide aclaración.

## Gestor de tareas

1. ¿Qué hace y qué no hace?

   - Hace: interpreta una petición escrita por Telegram, crea tareas en Google Tasks y puede añadir un recordatorio opcional en Google Calendar.
   - Conserva el titulo indicado por Aitor y añade detalles a la descripcion si se proporcionan.
   - Siempre pregunta a que lista debe añadirse: Trabajo, Clases, Cursos o Administración.
   - Puede crear una tarea con fecha limite y hora si se indican.
   - No hace: no clasifica automaticamente la tarea; no asume una lista, fecha, hora ni recordatorio; no modifica ni elimina tareas existentes; no crea recordatorios ni eventos sin confirmacion separada.

2. ¿Cómo se activa y qué necesita para funcionar?

   - Se activa con mensajes como `Crea una tarea corregir ejercicios - Ana Perez para el sabado 22`.
   - Necesita el titulo y pide la lista si no se indica.
   - Puede recibir una descripcion detallada, fecha limite, hora limite y solicitud de recordatorio.
   - Lee las listas disponibles de Google Tasks y consulta Google Calendar si se solicita un recordatorio.
   - Resuelve fechas relativas solo despues de mostrar la interpretacion y pedir confirmacion si existe ambigüedad.

3. ¿Qué resultado produce, qué herramientas usa y cuándo pide confirmación?

   - Produce un resumen previo con titulo, descripcion, lista, fecha limite, hora limite y recordatorio.
   - Usa Google Tasks para crear la tarea en la lista seleccionada y Google Calendar para crear un bloque de recordatorio opcional.
   - Pide confirmacion antes de crear la tarea.
   - Pide confirmacion separada antes de crear el recordatorio y verifica conflictos de calendario si es posible.
   - Si falta la lista, fecha necesaria, hora del recordatorio o existe ambigüedad, detiene el flujo y pide aclaracion.
