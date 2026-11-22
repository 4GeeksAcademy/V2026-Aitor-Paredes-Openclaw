---
name: summary-progress-4geeks
description: Resume el avance del usuario autenticado en 4Geeks a partir de sus asignaciones y actividad disponible, sin modificar datos.
---

# Resumen de progreso 4Geeks

## Objetivo

Ofrecer una visión general del avance del usuario en el curso usando únicamente los datos de asignaciones, actividad y, cuando sea útil, cohorte activa que devuelva la API.

## Activación

Usar cuando Aitor solicite un resumen o visión general de cuánto ha avanzado en el curso de 4Geeks.

## Procedimiento

1. Leer `BREATHECODE_STUDENT_TOKEN` desde el gestor de secretos configurado. Nunca pedir que se pegue en el chat ni mostrar o registrar el token.
2. Consultar las asignaciones propias en `GET https://breathecode.herokuapp.com/v1/assignment/user/me/task`, usando `Authorization: Token <token>` y paginación (`limit`/`offset`) si corresponde.
3. Consultar la actividad propia en `GET https://breathecode.herokuapp.com/v1/activity/me`. Usar `cohort` solo si se puede determinar con seguridad la cohorte relevante; `date_start`/`date_end` solo cuando se requiera resumir un período concreto.
4. Opcionalmente consultar `GET https://breathecode.herokuapp.com/v1/admissions/academy/cohort/me` para identificar la inscripción/cohorte activa. Si hay varias cohortes activas o la asociación no es clara, no elegir una arbitrariamente: indicar la ambigüedad y presentar datos globales del usuario si es posible.
5. Resumir cantidades por estado y tipo cuando esos campos existan: `PENDING`, `DONE`, `APPROVED`, `REJECTED`. Mantener visible la diferencia entre completado/entregado (`DONE`) y revisado/aprobado (`APPROVED`).
6. Añadir actividad reciente solo en la medida en que la respuesta permita describirla de manera verificable. Separar las métricas observadas de cualquier cálculo derivado.
7. Calcular un porcentaje únicamente si la respuesta permite conocer un denominador completo y definir explícitamente el criterio. Presentarlo como cálculo propio sobre asignaciones consultadas, nunca como porcentaje oficial de progreso curricular.
8. Si los datos no permiten establecer progreso curricular global, decirlo claramente y dar solo el resumen de asignaciones/actividad disponible. Informar de fallos parciales y no inferir valores ausentes.

## Límites

- Solo lectura: no actualizar, entregar, borrar ni calificar tareas.
- No consultar información de otros estudiantes o cohortes más allá de la propia información devuelta en el contexto autenticado.
- No presentar métricas parciales como progreso total del curso.
- No exponer el JSON completo, el token, cabeceras de autorización ni datos personales innecesarios.

## Endpoints

- `GET https://breathecode.herokuapp.com/v1/assignment/user/me/task` — asignaciones propias.
- `GET https://breathecode.herokuapp.com/v1/activity/me` — actividad propia.
- `GET https://breathecode.herokuapp.com/v1/admissions/academy/cohort/me` — opcional, contexto de cohorte activa.
