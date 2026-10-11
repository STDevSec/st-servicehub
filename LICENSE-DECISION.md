# Decisión de licencia — PENDIENTE

> **Estado:** sin decisión. Este repositorio **no tiene licencia** todavía.
> Mientras no exista un archivo `LICENSE`, se aplican por defecto todos los derechos de autor
> reservados: nadie más puede reutilizar, modificar ni redistribuir el código legalmente.

STDevSec Academy debe elegir una licencia **antes de publicar el repositorio** o de abrirlo a
personas ajenas al curso. Este documento resume las alternativas para facilitar la decisión.
No sustituye asesoría legal.

## Preguntas a responder

1. ¿El repositorio será **público** o **privado**?
2. ¿Se quiere que terceros (otras academias, empresas) puedan **reutilizar** el código?
3. ¿Se exige que las obras derivadas sigan siendo **abiertas**?
4. ¿La **documentación** y los materiales didácticos deben tener una licencia distinta del código?
5. ¿Quién es **titular** de los derechos de las contribuciones de los estudiantes? ¿Se necesita un
   acuerdo de contribución (CLA) o basta con un *Developer Certificate of Origin* (DCO)?

## Alternativas

| Opción | Tipo | Permite uso comercial | Exige compartir derivados | Comentario |
|--------|------|-----------------------|---------------------------|------------|
| **MIT** | Permisiva | Sí | No | La más simple. Máxima reutilización. |
| **Apache-2.0** | Permisiva | Sí | No | Como MIT, más concesión explícita de patentes y aviso de cambios. Común en el ecosistema Java/Spring. |
| **GPL-3.0** | Copyleft fuerte | Sí | Sí | Las obras derivadas distribuidas deben ser GPL. |
| **AGPL-3.0** | Copyleft de red | Sí | Sí, incluso si solo se ofrece como servicio | Cubre el uso como SaaS. |
| **Sin licencia / propietaria** | Todos los derechos reservados | No | — | Adecuada si el repositorio permanece privado y de uso interno del curso. |
| **CC BY 4.0 / CC BY-SA 4.0** (solo docs) | Contenido | Sí | BY-SA: sí | Para `docs/` y materiales didácticos; **no** recomendada para código. |

## Recomendación orientativa (no vinculante)

- Si el objetivo es **difundir** el proyecto como material educativo reutilizable:
  **Apache-2.0** para el código y **CC BY 4.0** para la documentación.
- Si el repositorio será **privado** y de uso exclusivo del curso: mantenerlo sin licencia pública y
  documentar los términos de uso para estudiantes.

## Cuando se tome la decisión

1. Registrar la decisión como ADR en [`docs/decisions`](docs/decisions/README.md).
2. Añadir el archivo `LICENSE` con el texto oficial de la licencia elegida.
3. Actualizar la sección de licencia del [README](README.md).
4. Sustituir este documento por una referencia al ADR, o eliminarlo.
