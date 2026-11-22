---
name: proyectos-rechazados-4geeks
description: Lista los proyectos del usuario autenticado de 4Geeks con estado REJECTED y muestra el feedback disponible, sin modificarlos ni volver a entregarlos.
---

# Proyectos rechazados 4Geeks

## Objetivo

Consultar los proyectos rechazados del usuario autenticado y resumir el feedback de revisión si está incluido en los datos devueltos.

## Activación

Usar cuando Aitor pida ver proyectos rechazados, proyectos que requieren correcciones o el feedback asociado a ellos.

## Procedimiento

1. Leer `BREATHECODE_STUDENT_TOKEN` desde el gestor de secretos configurado. No pedir, mostrar ni registrar el token.
2. Consultar `GET https://breathecode.herokuapp.com/v1/assignment/user/me/task` con `Authorization: Token <token>`, `task_type=PROJECT` y `task_status=REJECTED`.
3. Recuperar páginas adicionales mediante `limit` y `offset` cuando corresponda.
4. Para cada resultado, mostrar título/nombre, estado, feedback y fecha relevante solo si esos campos están presentes. Resumir fielmente el feedback, sin inferir motivos de rechazo.
5. Si el listado no contiene feedback, no hacer consultas de detalle adicionales salvo que se necesite y esté claramente permitido por la documentación; indicar que la respuesta no proporciona el feedback.
6. Si no hay resultados, comunicar que no se encontraron proyectos con ese estado. Ante errores, indicar que no se pudo completar la consulta.

## Límites

- Solo lectura: no editar, borrar, entregar ni cambiar el estado de proyectos.
- No consultar datos de otros usuarios.
- No afirmar que existe feedback si no aparece en la respuesta.
- No exponer token, cabeceras ni datos personales innecesarios.

## Endpoint

- `GET https://breathecode.herokuapp.com/v1/assignment/user/me/task`
- Query: `task_type=PROJECT`, `task_status=REJECTED`; paginación opcional con `limit` y `offset`.
