<!--
Gracias por tu contribución. Completa TODAS las secciones.
Escribe "N/A" cuando una sección no aplique; no la borres.
Guía: CONTRIBUTING.md
-->

## Issue relacionada

Closes #

## Descripción

<!-- Qué cambia y por qué. Breve y concreto. -->

## Especificación (SDD)

<!-- Enlace a la spec en docs/specs/ (obligatorio para features). -->

## Cambios realizados

<!-- Lista de cambios concretos: archivos/componentes y qué se hizo en cada uno.
     Indica si te desviaste de la spec y por qué. -->

- <...>

## Tipo de cambio

- [ ] `feat` — Nueva funcionalidad
- [ ] `fix` — Corrección de error
- [ ] `test` — Pruebas
- [ ] `docs` — Documentación
- [ ] `refactor` — Refactorización sin cambio de comportamiento
- [ ] `build` / `ci` — Construcción, dependencias o pipeline
- [ ] `chore` — Mantenimiento

## Componentes afectados

- [ ] identity-service
- [ ] client-service
- [ ] ticket-service
- [ ] project-service
- [ ] notification-service
- [ ] frontend (angular-app)
- [ ] infrastructure (docker / compose / jenkins)
- [ ] docs
- [ ] .github / configuración del repositorio

## Criterios de aceptación

<!-- Copia los criterios de la spec y marca los que se cumplen. -->

- [ ] CA-1: <criterio>

## Pruebas ejecutadas

<!-- Comandos ejecutados y resultado. Ej.: `./mvnw test` → 24 pruebas, 0 fallos. -->

## Evidencias

<!-- Capturas, salida de consola, respuestas de la API, enlaces a ejecuciones de CI.
     No incluyas tokens, contraseñas ni datos personales reales. -->

## Consideraciones de seguridad

<!-- Autenticación/autorización, validación de entradas, datos sensibles, secretos. -->

## Cambios de contrato

- [ ] Este PR **no** modifica contratos de API.
- [ ] Este PR modifica un contrato de API: OpenAPI actualizado en `docs/api/` y equipos consumidores avisados.

<!-- Si modifica contratos, describe el cambio y si es compatible hacia atrás. -->

## Dependencias nuevas

<!-- Nombre, versión y justificación. "N/A" si no hay. -->

## Riesgos pendientes

<!-- Limitaciones conocidas, deuda técnica, trabajo de seguimiento. -->

## Uso de IA

- [ ] No utilicé IA de forma significativa.
- [ ] Utilicé IA. Herramienta y propósito:

<!-- Ver docs/learning/ai-development-policy.md -->

---

## Definition of Done

- [ ] El Issue está enlazado y la spec está actualizada.
- [ ] Se cumplen todos los criterios de aceptación.
- [ ] Existen pruebas para el comportamiento nuevo o corregido y pasan localmente.
- [ ] La CI pasa (cuando exista).
- [ ] La autorización y la validación de entradas se aplican en el backend.
- [ ] No hay secretos, credenciales ni datos personales reales.
- [ ] No hay acceso directo a datos de otro servicio.
- [ ] La documentación (README, OpenAPI, ADR) está actualizada si corresponde.
- [ ] Los commits siguen Conventional Commits y la rama sigue la convención.
- [ ] Revisé mi propio diff antes de pedir revisión.
- [ ] Puedo explicar, modificar y defender todo el código de este PR.
