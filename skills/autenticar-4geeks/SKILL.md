---
name: autenticar-4geeks
description: Comprueba si el token guardado de 4Geeks es aceptado por el endpoint de perfil y comunica el estado de autenticación sin exponer datos personales.
---

# Autenticar 4Geeks

## Objetivo

Comprobar si la credencial configurada permite autenticarse ante el endpoint de perfil de 4Geeks en el momento de la consulta. Esta comprobación no verifica una sesión activa en el navegador ni la validez del token para otros servicios o endpoints.

## Activación

Usar cuando Aitor solicite comprobar, validar o verificar la autenticación/token de 4Geeks.

## Procedimiento

1. Leer la credencial desde el gestor de secretos configurado para `BREATHECODE_STUDENT_TOKEN`. Nunca pedir que se pegue en el chat, imprimirla, registrarla ni incluirla en argumentos visibles.
2. Si la credencial no está disponible, informar que no se pudo realizar la comprobación y solicitar configurar la credencial de forma protegida.
3. Hacer una petición `GET` autenticada a `https://breathecode.herokuapp.com/v1/admissions/user/me`, usando el esquema de autenticación documentado/configurado para esta credencial.
4. No mostrar, persistir ni registrar el cuerpo de la respuesta. Descartar los datos personales recibidos; solo evaluar el código HTTP y, cuando proceda, metadatos mínimos de error.
5. Informar el resultado con precisión:
   - `200`: «El token fue aceptado por el endpoint de perfil en esta comprobación. Esto no confirma una sesión activa en el navegador.»
   - `401`: «El endpoint rechazó la autenticación; el token no fue aceptado.» No afirmar la causa exacta ni imprimir la respuesta.
   - `403`: «La autenticación no fue autorizada para este recurso.» No etiquetar automáticamente el token como inválido.
   - Otros estados, error de red o timeout: informar que no se pudo confirmar la autenticación e indicar el estado/error técnico no sensible si está disponible.

## Límites

- No crear, renovar, modificar ni revocar credenciales.
- No consultar tareas, cohortes u otros datos con fines distintos a verificar que el endpoint acepta la autenticación.
- No afirmar que hay una sesión activa en navegador: este endpoint solo comprueba acceso API en el momento de la llamada.
- No exponer ni conservar JSON, perfil, identificadores, correo, token, cabeceras de autorización ni otros datos personales.

## Endpoint

- `GET https://breathecode.herokuapp.com/v1/admissions/user/me`
