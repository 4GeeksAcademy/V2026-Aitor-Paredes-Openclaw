---
name: actividad-reciente-4geeks
description: Resume la actividad reciente del usuario autenticado de 4Geeks a partir de los eventos que devuelve la API, sin modificar datos.
---

# Actividad reciente 4Geeks

## Objetivo

Ofrecer un resumen de la actividad de aprendizaje reciente del usuario autenticado, limitado a los registros disponibles en la API.

## Activación

Usar cuando Aitor pida consultar o resumir su actividad reciente en 4Geeks.

## Procedimiento

1. Leer `BREATHECODE_STUDENT_TOKEN` desde el gestor de secretos configurado. Nunca pedir, mostrar ni registrar el token.
2. Consultar `GET https://breathecode.herokuapp.com/v1/activity/me` con `Authorization: Token <token>`.
3. Si Aitor especifica un período, aplicar `date_start` y `date_end` en formato `YYYY-MM-DD`. Si no especifica fechas, consultar el endpoint sin inventar un rango y explicar que el resultado depende del período que el endpoint devuelva por defecto.
4. Usar el filtro `cohort` únicamente si se puede identificar inequívocamente la cohorte a la que Aitor se refiere; de otro modo, consultar la actividad propia sin ese filtro.
5. Resumir de forma concisa las fechas, tipos de actividad y métricas que efectivamente aparezcan. No inferir que la ausencia de registros significa inactividad fuera del alcance o retención de la API.
6. Si no hay datos o falla la consulta, distinguir entre «la API no devolvió actividad» y «no se pudo consultar».

## Límites

- Solo lectura; no registrar ni modificar actividad.
- No consultar ni revelar actividad de otros estudiantes.
- No inventar métricas, períodos ni acciones que la respuesta no incluya.
- No exponer token, cabeceras ni datos personales innecesarios.

## Endpoint

- `GET https://breathecode.herokuapp.com/v1/activity/me`
- Query opcional: `cohort`, `date_start`, `date_end`.
