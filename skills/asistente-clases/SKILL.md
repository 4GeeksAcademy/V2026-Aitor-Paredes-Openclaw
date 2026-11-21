---
name: asistente-clases
description: Programa clases online o presenciales en Google Calendar, consulta al alumno en Google Sheets y envia el enlace de Meet por Gmail solo tras confirmacion. Usar cuando Aitor indique una clase por nombre de alumno, fecha y hora.
---

# Asistente de clases

## Objetivo

Crear una clase en el Google Calendar de Aitor sin adivinar datos, sin sobrescribir eventos y sin enviar correos antes de recibir autorizacion.

## Entradas

Aceptar mensajes como:

```text
Clase con Ana García el lunes 30 a las 16:00
```

Interpretar:

- Nombre completo del alumno
- Fecha
- Hora de inicio
- Duracion: 90 minutos por defecto
- Modalidad: online por defecto
- Zona horaria: Europe/Madrid
- Libro de alumnos: `Data Alumnos`
- Hoja de alumnos: `BBDD Alumnos`
- Columnas: `Nombre` y `Correo`

Si el mensaje indica presencial, duracion distinta o zona horaria distinta, usar los valores indicados.

## Fuentes

Usar las herramientas Zapier descritas en `TOOLS.md`.

Antes de ejecutar una accion:

1. Ejecutar `inspect_zapier_actions` sin argumentos.
2. Inspeccionar la accion concreta y su esquema.
3. Usar `execute_zapier_read_action` para consultas.
4. Usar `execute_zapier_write_action` solo despues de confirmar la accion.
5. Usar la cuenta predeterminada omitiendo `connection_id` salvo que Aitor indique otra.

## Flujo

### 1. Extraer la solicitud

Convertir el mensaje en una estructura interna:

- Alumno
- Fecha
- Hora de inicio
- Duracion
- Modalidad
- Contexto adicional

Si falta el alumno, fecha, hora o modalidad por si quiere presencial, preguntar solo por los datos que falten.

### 2. Buscar al alumno

Buscar el libro `Data Alumnos` y la hoja `BBDD Alumnos`.

La coincidencia en la columna `Nombre` debe ser exacta con el nombre completo proporcionado.

- Si no hay coincidencia: informar y pedir el nombre exacto o el correo.
- Si hay varias coincidencias: mostrar todas las coincidencias disponibles sin exponer datos innecesarios y pedir elegir una.
- Si hay una coincidencia: leer el correo de la columna `Correo` y validar que tenga formato de correo.

No asumir la primera coincidencia ni enviar un correo si no se puede identificar inequívocamente al alumno.

### 3. Buscar disponibilidad

Consultar Google Calendar para el intervalo completo:

- Inicio: hora indicada.
- Fin: inicio mas la duracion predeterminada de 90 minutos.
- Zona horaria: Europe/Madrid.
- Calendario: predeterminado, salvo que Aitor indique otro.

Mostrar el resultado:

- Hora libre: pasar a la propuesta.
- Conflicto: listar el titulo, hora y participantes visibles del evento existente. No modificar ni crear nada.

### 4. Gestionar conflictos

Si existe un conflicto, ofrecer estas opciones:

- Reservar igualmente.
- Mover el evento existente a otro hueco.
- Eliminar el evento existente.
- Elegir otro horario para la clase.

Interpretar instruccion posterior en Telegram y mostrar una propuesta concreta antes de ejecutar.

No eliminar permanentemente un evento salvo solicitud inequívoca y confirmacion separada.

No mover un evento si no puede calcularse un destino valido y confirmar la nueva hora.

### 5. Mostrar propuesta antes de crear

Mostrar un log resumido:

```text
Alumno:
Nombre completo:
Correo:
Fecha:
Hora:
Duracion:
Modalidad:
Calendario:
Recordatorio: 30 minutos antes
Conflictos:
Accion prevista:
```

Pedir confirmacion para crear el evento.

No crear el evento sin confirmacion expresa.

### 6. Crear la clase

Despues de confirmar:

- Crear el evento en el calendario predeterminado.
- Usar titulo descriptivo como `Clase - [Nombre completo]`.
- Usar 90 minutos salvo duracion indicada.
- Usar zona horaria Europe/Madrid.
- Añadir Google Meet si la modalidad es online.
- Añadir al alumno como invitado solo si el correo fue validado.
- Configurar recordatorio de 30 minutos antes del inicio.

Si la accion de Calendar no devuelve un enlace Meet valido, no continuar con el envio. Informar del problema y ofrecer crear el evento sin Meet solo si Aitor confirma.

### 7. Preparar correo de aviso

Despues de crear el evento, mostrar el borrador que se enviaria:

- Destinatario: correo obtenido de Data Alumnos.
- Asunto: `Clase online - [Nombre] - [Fecha]`.
- Contenido breve:
  - Fecha y hora.
  - Duracion.
  - Zona horaria.
  - Enlace de Google Meet.
  - Recordatorio de que la clase comienza 30 minutos despues del aviso en el calendario.

No incluir datos personales innecesarios ni adjuntos.

Mostrar el cuerpo completo y pedir una segunda confirmacion explicita para enviarlo.

Si Aitor solo pide crear el evento, no preparar ni enviar correo.

### 8. Enviar y comprobar

Solo despues de la confirmacion:

- Enviar o crear el borrador segun lo solicitado.
- Confirmar que la accion devuelve resultado correcto.
- Informar brevemente de destinatario, asunto, enlace incluido y resultado.

Gmail es una accion externa. No responder, reenviar ni enviar nada sin solicitud clara.

## Valores por defecto

- Libro: `Data Alumnos`
- Hoja: `BBDD Alumnos`
- Nombre completo: coincidencia exacta
- Correo: columna `Correo`
- Duracion: 90 minutos
- Modalidad: online
- Zona horaria: Europe/Madrid
- Calendario: predeterminado
- Recordatorio: 30 minutos
- Cuenta: predeterminada
- Envio: no automatico; requiere confirmacion separada

## Reglas de seguridad

- No guardar correos ni IDs de conexion en esta skill.
- No compartir datos de alumnos fuera del entorno conectado.
- No modificar permisos, enlaces publicos ni documentos externos.
- No borrar, mover, sobrescribir o eliminar eventos sin mostrar la propuesta y recibir confirmacion.
- No enviar correo si hay ambigüedad en el alumno, destinatario, hora o enlace.
- No usar coincidencias parciales del nombre salvo que Aitor confirme explicitamente la fila.

## Ejemplo de salida

```text
Alumno: Ana García
Correo: validado
Fecha: lunes 30
Hora: 16:00
Duracion: 90 minutos
Modalidad: online
Calendario: predeterminado
Meet: por generar
Recordatorio: 30 minutos
Conflicto: ninguno

¿Creo el evento y genero el enlace de Meet?
```

## Criterios de prueba

Probar, sin datos reales:

1. Nombre exacto existente y sin conflictos.
2. Nombre inexistente.
3. Nombre duplicado.
4. Fecha ocupada por un evento.
5. Solicitud de reservar igualmente.
6. Solicitud de mover el evento existente.
7. Solicitud de eliminar el evento existente.
8. Solicitud de elegir otro horario.
9. Solicitud presencial sin Meet.
10. Creacion correcta del enlace Meet.
11. Fallo al generar el enlace Meet.
12. Autorizacion de envio y rechazo del envio.
