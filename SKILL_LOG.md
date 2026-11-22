# Registro de diseño de skills

Este archivo registra las seis skills de 4Geeks acordadas: cuatro del bloque inicial y dos opcionales. Las skills anteriores `gestor-tareas` y `asistente-clases` se conservan y no forman parte de este registro.

Para cada skill registraremos:
- **Prompt en lenguaje natural:** solicitud que la activa.
- **Descripción breve:** propósito, sin copiar el contenido completo de la skill.
- **Resultados obtenidos:** pruebas y resultados reales; indicar explícitamente cuando esté pendiente.
- **Endpoints de la API:** rutas utilizadas y métodos HTTP, cuando correspondan.

## Skills

1. `autenticar-4geeks` — implementada; prueba integral pendiente.
2. `proyectos-4geeks` — implementada; prueba real del endpoint pendiente.
3. `pendientes-4geeks` — implementada; prueba real del endpoint pendiente.
4. `summary-progress-4geeks` — implementada; prueba real de la combinación de endpoints pendiente.
5. `proyectos-rechazados-4geeks` — implementada; consulta real y disponibilidad de feedback pendientes de verificar.
6. `actividad-reciente-4geeks` — implementada; consulta real pendiente de verificar.

## Consulta inicial del perfil 4Geeks — 2026-11-22

- **Prompt natural:** «Llama a la API y muéstrame qué JSON te devuelve».
- **Descripción breve:** consulta autenticada del perfil actual para inspeccionar la estructura de la respuesta y descubrir los datos académicos asociados.
- **Endpoint:** `GET https://breathecode.herokuapp.com/v1/admissions/user/me`.
- **Resultado real:** HTTP 200; la respuesta JSON incluye secciones de perfil, cohortes, roles, permisos y configuración. Se omiten del registro los valores personales y metadatos específicos de la cuenta.
- **Muestra segura de estructura** (valores personales y cohortes individuales omitidos):

```json
{
  "id": "[REDACTADO]",
  "email": "[REDACTADO]",
  "date_joined": "[OMITIDO]",
  "profile": { "...": "campos de perfil omitidos" },
  "cohorts": [
    {
      "id": "[ID DE INSCRIPCIÓN OMITIDO]",
      "role": "STUDENT",
      "educational_status": "ACTIVE",
      "cohort": {
        "id": "[ID DE COHORTE OMITIDO]",
        "slug": "[SLUG OMITIDO]",
        "academy": "[OMITIDO]"
      }
    }
  ],
  "roles": [
    { "academy": "[OMITIDO]", "role": "[OMITIDO]" }
  ],
  "permissions": [ "[lista de permisos omitida]" ],
  "settings": "[OMITIDO]"
}
```

- La muestra está deliberadamente depurada y no representa todos los 28 elementos; no almacenar ni publicar identificadores de usuario/inscripción, email, perfil, nombres ni slugs de cohortes individuales.

## 1. Autenticar-4Geeks (`skills/autenticar-4geeks/SKILL.md`)

- **Prompt en lenguaje natural:** «Comprueba si mi token de estudiante de 4Geeks es válido y si puedo acceder a mi perfil».
- **Descripción breve:** consulta el endpoint de perfil con el token configurado y comunica si fue aceptado, sin exponer el cuerpo de la respuesta. No verifica sesión de navegador.
- **Resultado obtenido:** skill creada. Prueba previa del endpoint: HTTP 200. La prueba validó acceso API en ese momento; no demuestra sesión activa en navegador. Prueba integral de la skill pendiente.
- **Endpoint:** `GET https://breathecode.herokuapp.com/v1/admissions/user/me`.


## 2. Proyectos-4Geeks (`skills/proyectos-4geeks/SKILL.md`)

- **Prompt en lenguaje natural:** «Muéstrame mis proyectos de 4Geeks y el estado actual de cada uno».
- **Descripción breve:** lista los proyectos asignados al usuario autenticado y resume su estado; es de solo lectura y no entrega ni modifica tareas.
- **Resultados obtenidos:** skill creada tras aprobación. No se ha probado este flujo contra la API; queda pendiente validar la respuesta real y la paginación. Interpretación según referencia: `PENDING` pendiente, `DONE` completado/entregado pero no necesariamente calificado, `APPROVED` aprobado y `REJECTED` revisado con correcciones requeridas.
- **Endpoint previsto:** `GET https://breathecode.herokuapp.com/v1/assignment/user/me/task`, con `task_type=PROJECT` y paginación mediante `limit`/`offset` cuando corresponda.


## 3. Pendientes-4Geeks (`skills/pendientes-4geeks/SKILL.md`)

- **Prompt en lenguaje natural:** «Dime qué tareas o ejercicios tengo pendientes en 4Geeks y qué me falta completar».
- **Descripción breve:** consulta las asignaciones pendientes del usuario autenticado y resume sus datos y requisitos concretos cuando la API los proporciona; no modifica ni entrega tareas.
- **Resultados obtenidos:** skill creada tras aprobación. La respuesta real de este flujo aún no se ha probado; la skill no inferirá requisitos ausentes de la respuesta.
- **Endpoint previsto:** `GET https://breathecode.herokuapp.com/v1/assignment/user/me/task`, con `task_status=PENDING` y paginación mediante `limit`/`offset` cuando corresponda.


## 4. Summary-Progress-4Geeks (`skills/summary-progress-4geeks/SKILL.md`)

- **Prompt en lenguaje natural:** «Dame un resumen de cuánto he avanzado en el curso de 4Geeks».
- **Descripción breve:** resume el avance a partir de asignaciones y actividad disponibles, con contexto de cohorte activa cuando sea inequívoco. No modifica datos.
- **Resultados obtenidos:** skill creada tras aprobación. La combinación de endpoints aún no se ha probado; tampoco se ha confirmado una medida oficial de progreso curricular. La skill separa datos observados de cálculos propios y no presenta métricas parciales como porcentaje global.
- **Endpoints previstos:** `GET https://breathecode.herokuapp.com/v1/assignment/user/me/task`; `GET https://breathecode.herokuapp.com/v1/activity/me`; opcionalmente `GET https://breathecode.herokuapp.com/v1/admissions/academy/cohort/me` para contexto de cohorte.


## 5. Proyectos-Rechazados-4Geeks (`skills/proyectos-rechazados-4geeks/SKILL.md`)

- **Prompt en lenguaje natural:** «Muéstrame mis proyectos rechazados de 4Geeks y el feedback recibido».
- **Descripción breve:** lista los proyectos propios en estado `REJECTED` y resume el feedback solo si viene incluido en la respuesta; no los modifica ni vuelve a entregar.
- **Resultados obtenidos:** skill creada. Consulta real pendiente; aún no se ha confirmado si el endpoint devuelve feedback en el listado.
- **Endpoint previsto:** `GET https://breathecode.herokuapp.com/v1/assignment/user/me/task`, con `task_type=PROJECT`, `task_status=REJECTED` y paginación mediante `limit`/`offset` cuando corresponda.

## 6. Actividad-Reciente-4Geeks (`skills/actividad-reciente-4geeks/SKILL.md`)

- **Prompt en lenguaje natural:** «Resume mi actividad reciente en 4Geeks».
- **Descripción breve:** resume los registros de actividad de aprendizaje propios que devuelva la API; permite acotar fechas si el usuario las indica y no modifica datos.
- **Resultados obtenidos:** skill creada. Consulta real pendiente; sin fechas solicitadas, la skill no inventa el rango y aclara que el período depende del comportamiento de la API.
- **Endpoint previsto:** `GET https://breathecode.herokuapp.com/v1/activity/me`; query opcional `date_start`, `date_end` y `cohort` cuando corresponda.
