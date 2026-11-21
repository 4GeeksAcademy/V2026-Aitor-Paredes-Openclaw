---
name: gestor-tareas
description: Crea tareas en Google Tasks a partir de mensajes de Telegram y puede crear recordatorios opcionales en Google Calendar. No clasifica automaticamente y siempre pregunta la lista.
---

# Gestor de tareas

## Objetivo

Crear tareas generales en Google Tasks sin clasificar automaticamente su contenido y sin modificar tareas ni crear recordatorios sin confirmacion.

## Activacion

Aceptar mensajes como:

```text
Crea una tarea Hacer ejercicios Inmovilizado - Antonio Sanchez para el sabado 22
Crea una tarea Corregir ejercicios - Ana Perez para el domingo 23
```

## Entradas

Extraer e interpretar:

- Titulo de la tarea
- Descripcion o detalles adicionales
- Fecha limite
- Hora limite si se indica
- Lista de Google Tasks
- Recordatorio opcional
- Fecha y hora del recordatorio, si se solicita
- Duracion del recordatorio o bloqueo en Google Calendar
- Zona horaria

No inferir ni asignar automaticamente una lista. Las listas disponibles son:

- Trabajo
- Clases
- Cursos
- Administración

## Preparacion

Antes de escribir:

1. Ejecutar `inspect_zapier_actions` sin argumentos.
2. Inspeccionar las acciones concretas de Google Tasks y Google Calendar.
3. Inspeccionar los esquemas de las acciones antes de ejecutar parametros.
4. Resolver las listas disponibles y comprobar que la seleccion existe.
5. Resolver el calendario y la zona horaria si se solicita recordatorio.
6. Convertir fechas relativas en fechas exactas usando la fecha actual y confirmar la interpretacion si existe ambigüedad.

Usar `execute_zapier_read_action` para consultar y `execute_zapier_write_action` para escribir.

Usar la cuenta predeterminada omitiendo `connection_id`, salvo que Aitor indique otra.

## Flujo

### 1. Interpretar el mensaje

Separar:

- Titulo
- Descripcion
- Fecha limite
- Lista solicitada
- Recordatorio solicitado

No ampliar el titulo con etiquetas de clasificacion.

Ejemplo:

```text
Titulo: Hacer ejercicios Inmovilizado - Antonio Sanchez
Descripcion: Detalles incluidos en el mensaje
Fecha limite: sabado 22
Lista: pendiente de confirmar
Recordatorio: pendiente de confirmar
```

### 2. Solicitar datos faltantes

Preguntar solo lo necesario:

- Si no se indica lista: mostrar las cuatro listas y pedir una.
- Si no se indica fecha y se necesita: preguntar si tiene fecha limite.
- Si no se indica recordatorio: preguntar si desea uno.
- Si se solicita recordatorio sin hora: preguntar fecha y hora exactas.
- Si hay ambigüedad en una fecha relativa: mostrar la fecha interpretada y pedir confirmacion.

No asignar automaticamente ninguna lista.

### 3. Mostrar resumen

Antes de crear la tarea, mostrar:

```text
Titulo:
Descripcion:
Lista:
Fecha limite:
Hora limite:
Recordatorio:
Fecha y hora del recordatorio:
Duracion:
Zona horaria:
```

Preguntar si crea la tarea.

No crear una tarea sin confirmacion.

### 4. Crear la tarea

Tras confirmar:

- Crear la tarea en la lista seleccionada.
- Usar el titulo exactamente como lo haya indicado Aitor, salvo correccion ortografica menor aceptada por el usuario.
- Usar la descripcion como notas.
- Añadir la fecha limite exacta si fue indicada.
- Añadir la hora limite solo si fue indicada y la accion la admite.
- Mantener la tarea en estado pendiente.

Si Google Tasks no permite guardar el campo indicado, informar y mostrar el efecto real antes de continuar.

### 5. Gestionar recordatorio

Un recordatorio se crea solo si Aitor lo solicita.

Primero comprobar el calendario:

- Buscar conflictos dentro del intervalo del recordatorio.
- Si existe conflicto, mostrarlo y esperar instruccion.
- Si no existe conflicto, mostrar la propuesta completa.

Opciones disponibles:

- Crear el evento.
- Elegir otra hora.
- No crear el evento.

La duracion predeterminada puede ser 30 minutos solo si Aitor confirma el valor o ya lo definio.

El recordatorio debe añadirse al evento de Google Calendar, no a una tarea si la accion no ofrece soporte nativo verificable.

No crear Meet ni invitados salvo que Aitor lo solicite expresamente.

### 6. Confirmar la operacion final

Mostrar:

- Titulo creado.
- Lista donde fue creada.
- Fecha limite.
- Descripcion guardada.
- Recordatorio creado o no creado.

Si el recordatorio no se pudo crear, informar claramente y mantener intacta la tarea creada.

## Valores por defecto

- Listas: Trabajo, Clases, Cursos, Administración.
- Clasificacion: ninguna; siempre pedir la lista.
- Titulo: texto indicado por Aitor.
- Descripcion: detalles incluidos en el mensaje.
- Fecha limite: sin fecha hasta que se indique o se confirme.
- Estado: pendiente.
- Recordatorio: no crear salvo solicitud expresa.
- Zona horaria: Europe/Madrid si se crea recordatorio y no se indica otra.
- Duracion del recordatorio: preguntar antes de crear si no se indica.
- Cuenta: predeterminada.

## Reglas de seguridad

- No eliminar ni modificar tareas existentes sin autorizacion expresa.
- No cambiar listas de forma automatica.
- No crear eventos ni recordatorios sin confirmacion separada.
- No agregar Meet, invitados ni permisos a un recordatorio salvo solicitud expresa.
- No asumir la hora o fecha de una expresion relativa si hay ambigüedad.
- No guardar datos personales innecesarios en el titulo o la descripcion.

## Criterios de prueba

Probar sin datos reales:

1. Crear tarea con titulo, lista y fecha.
2. Crear tarea sin fecha limite.
3. Crear tarea sin detalles descriptivos.
4. Pedir una lista cuando no se indique.
5. Rechazar una lista inexistente.
6. Solicitar un recordatorio con fecha y hora.
7. Solicitar recordatorio sin hora.
8. Detectar un conflicto de calendario.
9. Elegir otra hora para el recordatorio.
10. Crear tarea y rechazar el recordatorio.
11. Crear recordatorio y verificar su titulo y hora.
12. Rechazar la creacion completa.
