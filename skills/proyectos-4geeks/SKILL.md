---
name: proyectos-4geeks
description: Recupera los proyectos asignados al usuario autenticado de 4Geeks y muestra su estado actual, sin modificar ni entregar tareas.
---

# Proyectos 4Geeks

## Objetivo

Listar los proyectos asignados al usuario autenticado de 4Geeks e indicar el estado que devuelve la API. Es una consulta de solo lectura.

## Activación

Usar cuando Aitor solicite ver, listar o consultar sus proyectos de 4Geeks o su estado.

## Procedimiento

1. Leer `BREATHECODE_STUDENT_TOKEN` desde el gestor de secretos configurado. No pedir que se pegue en el chat ni mostrar o registrar el token.
2. Consultar el endpoint de tareas del usuario con `GET https://breathecode.herokuapp.com/v1/assignment/user/me/task`, enviando `Authorization: Token <token>` y el filtro `task_type=PROJECT`.
3. Recuperar las páginas siguientes con `limit` y `offset` si el endpoint pagina los resultados. Evitar peticiones repetidas innecesarias.
4. Presentar cada proyecto con los campos mínimos que permitan reconocerlo y mostrar su estado; no volcar innecesariamente la respuesta completa ni información personal.
5. Interpretar los estados documentados sin perder la distinción del valor original:
   - `PENDING`: pendiente.
   - `DONE`: completado/entregado por el estudiante; no implica que esté calificado.
   - `APPROVED`: revisado y aprobado.
   - `REJECTED`: revisado y requiere correcciones.
   - Estado desconocido: mostrar el valor original e indicar que su significado no está documentado.
6. Si la API falla o la forma de respuesta no coincide con la esperada, informar el error de forma segura y no inventar resultados.

## Límites

- Solo lectura: no actualizar, entregar, borrar ni calificar proyectos.
- No consultar proyectos de otros usuarios ni listar datos de cohortes ajenas.
- No afirmar “calificado” para `DONE`; ese estado solo indica completado/entregado según la referencia disponible.
- No exponer token, cabeceras de autorización ni datos personales innecesarios.

## Endpoint

- `GET https://breathecode.herokuapp.com/v1/assignment/user/me/task`
- Query: `task_type=PROJECT`; paginación opcional con `limit` y `offset`.
