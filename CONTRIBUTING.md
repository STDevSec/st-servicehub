# Guía de contribución — ST ServiceHub

Gracias por contribuir. Este documento define **cómo** se trabaja en el repositorio. Su objetivo es que
cada cambio sea pequeño, revisable y trazable de principio a fin.

> Si es tu primera vez, lee antes la [guía del estudiante](docs/learning/student-guide.md).

## Principios

- **Nada entra a `main` sin Pull Request y revisión.**
- **Sin especificación no hay implementación.** Ver [docs/specs](docs/specs/README.md).
- **Un PR = un propósito.** Cambios pequeños y enfocados en una vertical slice o una corrección.
- **Cada servicio es dueño de sus datos.** Ver [límites de los servicios](docs/architecture/service-boundaries.md).
- **Sin secretos en el repositorio.** Ver [SECURITY.md](SECURITY.md).
- **La IA asiste; tú respondes por el código.** Ver la [política de IA](docs/learning/ai-development-policy.md).

## Flujo de trabajo

```text
Issue → Spec → Branch → Commits → Tests → PR → Review → CI → Merge
```

1. **Crear o asignarse un Issue.** Usa las plantillas *Feature* o *Bug report*. Si el Issue ya existe,
   asígnatelo antes de empezar para evitar trabajo duplicado.
2. **Redactar la especificación.** Copia [`docs/specs/feature-template.md`](docs/specs/feature-template.md)
   y complétala. Los bugs simples pueden documentarse directamente en el Issue.
3. **Revisar los criterios de aceptación** con tu equipo (y con el equipo dueño del servicio, si tocas
   uno ajeno) antes de escribir código.
4. **Crear una rama** desde `main` actualizado, siguiendo la convención de nombres.
5. **Implementar los cambios** respetando la spec, Clean Code y SOLID.
6. **Ejecutar las pruebas** localmente. Añade o actualiza pruebas para todo comportamiento nuevo.
7. **Crear commits descriptivos** con Conventional Commits.
8. **Abrir un Pull Request** usando la plantilla. Enlaza el Issue (`Closes #<id>`) y la spec.
9. **Solicitar revisión** al menos a una persona que no sea autora del cambio (ver
   [política de revisión para equipos pequeños](#política-de-revisión-para-equipos-pequeños)).
10. **Verificar la CI.** Cuando exista el pipeline, todas las verificaciones deben pasar.
11. **Integrar los cambios.** Una vez aprobado y en verde, se integra mediante *squash merge* (recomendado)
    y se elimina la rama. El *squash* deja un único commit en `main`, que enlaza al PR; los commits
    individuales siguen visibles en el PR, que es la evidencia de trazabilidad para la evaluación.

## Convenciones de ramas

```text
feature/<issue-id>-descripcion
fix/<issue-id>-descripcion
docs/<issue-id>-descripcion
test/<issue-id>-descripcion
```

- `descripcion` en minúsculas, palabras separadas por guiones, breve.
- Ejemplos: `feature/12-create-ticket`, `fix/31-client-email-validation`, `docs/4-service-boundaries`.

## Convenciones de commits

Se usa [Conventional Commits](https://www.conventionalcommits.org/es/v1.0.0/):

```text
<tipo>(<ámbito>): <descripción en imperativo>
```

| Tipo | Uso |
|------|-----|
| `feat` | Nueva funcionalidad |
| `fix` | Corrección de un error |
| `test` | Añadir o corregir pruebas |
| `docs` | Solo documentación |
| `refactor` | Cambio interno sin alterar comportamiento |
| `build` | Dependencias o sistema de construcción |
| `ci` | Pipelines de integración continua |
| `chore` | Mantenimiento que no encaja en lo anterior |

**Ámbitos sugeridos:** `identity`, `clients`, `tickets`, `projects`, `notifications`, `frontend`,
`architecture`, `infra`, `ci`, `docs`.

Ejemplos:

```text
feat(tickets): add ticket creation
fix(clients): validate email
test(identity): cover invalid credentials
docs(architecture): describe service boundaries
```

Reglas:

- Descripción breve (≤ 72 caracteres), sin punto final.
- Un commit = un cambio lógico. Evita commits del tipo `fix`, `cambios` o `wip` en el PR final.
- Referencia el Issue en el cuerpo cuando aporte contexto: `Refs #12`.

## Pull Requests

- Usa la [plantilla de PR](.github/PULL_REQUEST_TEMPLATE.md) y completa **todas** las secciones
  (escribe "N/A" cuando no aplique).
- Mantén el PR pequeño (orientativo: < 400 líneas cambiadas, sin contar archivos generados).
- Si cambias un contrato de API, actualiza el OpenAPI en [`docs/api`](docs/api/README.md) y avisa al
  equipo consumidor.
- Declara las dependencias nuevas y justifícalas.
- Declara el uso significativo de IA.

### Revisión de código

Quien revisa comprueba, como mínimo:

- Que el cambio cumple la spec y sus criterios de aceptación.
- Que existen pruebas y son significativas.
- Que no hay secretos, credenciales ni datos personales reales.
- Que la autorización y la validación de entradas están en el backend.
- Que no se accede a datos de otro servicio de forma directa.
- Que el código es legible y la persona autora puede explicarlo.

Comenta con respeto y de forma concreta: describe el problema, por qué importa y una alternativa.

### Política de revisión para equipos pequeños

Los equipos tienen de una a tres personas, por lo que no siempre habrá otro integrante disponible
(o independiente) para revisar. Se aplica este orden:

1. **Otro integrante del mismo equipo** que no sea coautor del cambio.
2. Si no lo hay, **un estudiante de otro equipo**. Al inicio del curso, cada equipo tiene asignado
   un **equipo revisor par**; los equipos de una persona se emparejan obligatoriamente.
3. Si nadie puede revisar en **un día hábil**, o el cambio es sensible (seguridad, contratos
   compartidos, infraestructura), **el instructor**.

Reglas:

- **Nadie puede aprobar su propio Pull Request.** GitHub no cuenta la aprobación del autor, y la
  aprobación de un coautor tampoco se considera una revisión independiente.
- Si el PR cambia un **contrato de API**, la revisión debe incluir a alguien del equipo consumidor.
- La revisión cuenta para ambas personas: revisar con calidad forma parte de la evaluación.

## Protección de la rama `main`

Configuración **recomendada** para quien administre el repositorio en GitHub
(*Settings → Branches → Branch protection rules*, o *Rulesets*):

| Regla | Estado actual |
|-------|---------------|
| Prohibir push directo a `main` (requerir Pull Request) | ✅ Aplicada |
| Requerir al menos **una** aprobación | ✅ Aplicada |
| Descartar aprobaciones obsoletas cuando se suben nuevos commits | ✅ Aplicada |
| Requerir resolver las conversaciones antes de integrar | ✅ Aplicada |
| Bloquear *force push* | ✅ Aplicada |
| Bloquear la eliminación de la rama | ✅ Aplicada |
| Requerir que las verificaciones de CI pasen y que la rama esté actualizada | ⏳ Cuando exista el pipeline |
| Requerir revisión de *Code Owners* | ⏳ Cuando `CODEOWNERS` tenga responsables reales |
| Aplicar las reglas también a administradores (*enforce admins*) | ⏳ Desactivado: con un único administrador impediría integrar; activar cuando haya más revisores |

Para consultar la configuración vigente (requiere `gh` autenticado):

```bash
gh api repos/STDevSec/st-servicehub/branches/main/protection
```

## Bloqueos

Si te bloqueas más de medio día (dependencia de otro equipo, contrato indefinido, error de entorno),
comunícalo en el Issue con la etiqueta o mención correspondiente. Ver
[cómo reportar bloqueos](docs/learning/student-guide.md#cómo-reportar-bloqueos).
