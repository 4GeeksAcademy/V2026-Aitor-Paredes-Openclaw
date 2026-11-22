---
name: pendientes-4geeks
description: Consulta las asignaciones pendientes del usuario autenticado de 4Geeks y resume qué falta completar sin modificar ni entregar tareas.
---

# Pendientes 4Geeks

## Objetivo

Indicar qué asignaciones tiene pendientes el usuario autenticado y qué información concreta proporciona la API sobre lo que falta completar. Solo lectura.

## Activación

Usar cuando Aitor solicite consultar sus tareas, ejercicios, proyectos u otras asignaciones pendientes de 4Geeks, o pregunte qué le falta completar.

## Procedimiento

1. Leer `BREATHECODE_STUDENT_TOKEN` desde el gestor de secretos configurado. Nunca pedir que se pegue en el chat ni mostrar o registrar el token.
2. Consultar las tareas del usuario mediante `GET https://breathecode.herokuapp.com/v1/assignment/user/me/task`, con `Authorization: Token <token>` y `task_status=PENDING`.
3. Recuperar las páginas siguientes usando `limit` y `offset` si la respuesta está paginada.
4. Mostrar cada asignación con título/nombre, tipo, estado y fecha límite si esos campos están disponibles. Resumir instrucciones o requisitos concretos solo cuando aparezcan en la respuesta; no inferir qué pasos faltan basándose solo en el título.
5. Si la respuesta incluye `REJECTED`, no presentarlo como `PENDING`: señalarlo en una sección distinta como revisado/requiere correcciones, conservando el estado original.
6. Si no hay resultados, decir que la consulta no devolvió asignaciones pendientes. Si falla la petición o no se reconoce la respuesta, informar de que no se pudo completar la consulta sin inventar datos.

## Límites

- No actualizar, entregar, borrar ni calificar asignaciones.
- No consultar datos de otros usuarios.
- No asumir que `DONE` significa pendiente ni que una tarea está calificada sin estado que lo indique.
- No mostrar el JSON completo, credenciales, cabeceras de autorización ni datos personales innecesarios.

## Endpoint

- `GET https://breathecode.herokuapp.com/v1/assignment/user/me/task`
- Query: `task_status=PENDING`; paginación opcional con `limit` y `offset`.
