# Especificaciones (Spec-Driven Development)

En ST ServiceHub **no se escribe código sin una especificación**. La spec obliga a pensar el problema,
acordar el alcance y definir cómo se verificará *antes* de implementar. También es el mejor contexto que
se le puede dar a un asistente de IA.

## El proceso

```text
INTENT → SPEC → PLAN → TASKS → IMPLEMENT → VERIFY
```

| Fase | Pregunta | Resultado |
|------|----------|-----------|
| **INTENT** | ¿Qué problema de negocio resolvemos y para quién? | Issue con objetivo e historia de usuario |
| **SPEC** | ¿Qué debe hacer exactamente el sistema? | Alcance, reglas, contrato API, seguridad, criterios de aceptación |
| **PLAN** | ¿Cómo lo construiremos? | Plan técnico por capas y componentes |
| **TASKS** | ¿En qué pasos pequeños se divide? | Lista de tareas verificables |
| **IMPLEMENT** | Construir | Commits en la rama del Issue |
| **VERIFY** | ¿Cumple la spec? | Pruebas, evidencias y PR revisado |

## Cómo crear una spec

1. Copia [`feature-template.md`](feature-template.md) en la carpeta del servicio correspondiente:

   ```text
   docs/specs/<servicio>/<issue-id>-<descripcion>.md
   ```

   Ejemplo: `docs/specs/ticket-service/12-create-ticket.md`.
   Para cambios del frontend usa `docs/specs/frontend/`; para trabajo transversal, `docs/specs/cross-cutting/`.
2. Completa como mínimo las secciones hasta **Criterios de aceptación** y súbela en un PR propio
   (`docs/<issue-id>-...`) o como primer commit de la rama de la feature.
3. Revisa los criterios de aceptación con tu equipo antes de implementar.
4. Completa **Plan**, **Tareas**, **Implementación** (desviaciones y uso de IA), **Pruebas** y **Evidencias**
   a medida que avanzas.
5. Si la implementación revela que la spec era incorrecta, **actualiza la spec**: debe reflejar lo entregado.

## SDD no es "generar código con IA"

La spec no es un *prompt* para que una herramienta escriba el código. Es el acuerdo que **tú** debes
entender y poder defender: la IA puede ayudarte a redactarla o a implementarla, pero cada criterio de
aceptación se verifica con pruebas que comprendes, y en la defensa técnica tendrás que explicar
cualquier decisión. Ver la [política de IA](../learning/ai-development-policy.md).

## Buenas prácticas

- Una spec por vertical slice. Si crece demasiado, divídela.
- Criterios de aceptación **verificables** (Dado / Cuando / Entonces), no ambiguos.
- Escribe explícitamente lo que **queda fuera** del alcance.
- La sección de seguridad nunca se deja vacía: como mínimo, quién puede invocar la funcionalidad.
