# Guía del estudiante — ST ServiceHub

Esta guía explica, paso a paso, cómo trabajar en el repositorio. Complementa
[CONTRIBUTING.md](../../CONTRIBUTING.md), que contiene las reglas formales.

## Cómo clonar el repositorio

```bash
git clone https://github.com/STDevSec/st-servicehub.git
cd st-servicehub

# Configura tu identidad (una sola vez por computadora)
git config --global user.name "Tu Nombre"
git config --global user.email "tu-correo@example.com"

# Variables de entorno locales (nunca se suben al repositorio)
cp .env.example .env
```

> El repositorio puede ser privado: necesitas que te den acceso antes de clonarlo.

## Cómo trabajar con Issues

1. Ve a la pestaña **Issues** y busca una tarea de tu equipo sin asignar, o crea una nueva con la
   plantilla **Feature** o **Bug report**.
2. **Asígnatela** (*Assignees → assign yourself*) antes de empezar.
3. Usa el Issue como bitácora: preguntas, decisiones y bloqueos se comentan ahí.
4. Anota el número del Issue (`#12`): lo usarás en la rama, la spec, los commits y el PR.

## Cómo escribir una especificación

1. Copia la plantilla:

   ```bash
   mkdir -p docs/specs/ticket-service
   cp docs/specs/feature-template.md docs/specs/ticket-service/12-create-ticket.md
   ```

2. Completa **INTENT** y **SPEC** (alcance, exclusiones, reglas, contrato API, seguridad, criterios de
   aceptación y casos límite).
3. Revisa los criterios de aceptación con tu equipo antes de programar.
4. Completa **PLAN**, **TASKS**, **IMPLEMENT** y **VERIFY** durante el desarrollo.

Más detalles en [docs/specs/README.md](../specs/README.md).

## Cómo crear una rama

Siempre desde un `main` actualizado:

```bash
git switch main
git pull origin main
git switch -c feature/12-create-ticket
```

Convenciones: `feature/`, `fix/`, `docs/`, `test/` + `<issue-id>-descripcion`.

Si `main` avanza mientras trabajas, actualiza tu rama:

```bash
git fetch origin
git merge origin/main       # opción recomendada si ya subiste la rama
```

Si prefieres `git rebase origin/main` y la rama ya estaba en GitHub, tendrás que reescribir **tu**
rama remota con `git push --force-with-lease`. Nunca uses `--force` sobre ramas compartidas; sobre
`main` está bloqueado.

## Cómo realizar commits

```bash
git status                    # ¿Qué cambió?
git diff                      # Revisa el detalle
git add <archivos>            # Añade solo lo que corresponde (evita "git add ." a ciegas)
git diff --staged             # Revisa lo que vas a confirmar: ¿hay secretos o archivos .env?
git commit -m "feat(tickets): add ticket creation"
git push -u origin feature/12-create-ticket
```

Usa [Conventional Commits](https://www.conventionalcommits.org/es/v1.0.0/):
`feat`, `fix`, `test`, `docs`, `refactor`, `build`, `ci`, `chore`.

## Cómo documentar pruebas

- Cada criterio de aceptación de la spec debe estar cubierto por al menos una prueba.
- Completa la tabla **6.1 Pruebas** de la spec, indicando qué criterio cubre cada prueba.
- En el PR, en **Pruebas ejecutadas**, pega el comando y un resumen del resultado, por ejemplo:

  ```text
  ./mvnw test   → Tests run: 24, Failures: 0, Errors: 0, Skipped: 0
  ```

- En **Evidencias**, añade capturas o peticiones/respuestas relevantes **sin** tokens, contraseñas ni
  datos personales reales.

## Cómo abrir un Pull Request

1. Sube tu rama (`git push -u origin <rama>`).
2. En GitHub, pulsa **Compare & pull request** (base: `main`).
3. Título en formato Conventional Commits: `feat(tickets): add ticket creation`.
4. Completa **todas** las secciones de la plantilla y escribe `Closes #12` para enlazar el Issue.
5. Si aún no está listo, ábrelo como **Draft**.
6. Revisa tu propio diff en la pestaña *Files changed* antes de pedir revisión.

## Cómo solicitar revisión

1. En el PR, en **Reviewers**, añade al menos a una persona de tu equipo que no sea coautora. Si no
   hay nadie disponible, sigue la
   [política de revisión para equipos pequeños](../../CONTRIBUTING.md#política-de-revisión-para-equipos-pequeños)
   (equipo revisor par o instructor). Si tu cambio afecta al contrato o al servicio de otro equipo,
   añade también a alguien de ese equipo. No puedes aprobar tu propio PR.
2. Responde a cada comentario: corrige y sube un nuevo commit, o explica por qué no.
3. No marques como resueltas las conversaciones de otra persona sin su acuerdo.
4. Cuando el PR esté aprobado y la CI en verde (cuando exista), se integra con *squash merge*.

Revisar PRs de compañeros también es parte de tu trabajo: lee el código, ejecútalo si hace falta y
haz preguntas.

## Cómo reportar bloqueos

Estás bloqueado si no puedes avanzar por causas ajenas a ti (contrato no definido, dependencia de otro
equipo, error de entorno). Si llevas **más de medio día** sin avanzar:

1. Comenta en el Issue con este formato:

   ```text
   🚧 Bloqueo
   - Qué intento hacer:
   - Qué me lo impide:
   - Qué probé:
   - Qué necesito y de quién:
   ```

2. Menciona a la persona o al equipo que puede desbloquearte y avisa en el canal del curso.
3. Mientras tanto, avanza en otra tarea (pruebas, documentación, otra parte de la slice).

## Cómo cumplir la regla 2+1

Cada estudiante debe entregar:

- **2 vertical slices completas y propias** (individuales) dentro de su equipo. Una vertical slice es una funcionalidad de
  extremo a extremo (UI → API → dominio → persistencia → pruebas) que aporta valor por sí misma.
  Ejemplo: "crear un ticket" o "listar los clientes activos".
- **1 contribución transversal**, que beneficia a varios equipos. Ejemplos: configuración de seguridad
  compartida, pipeline de CI, Docker Compose, contratos OpenAPI, pruebas de integración entre servicios
  o formato común de errores.

Las contribuciones transversales **deben acordarse entre los equipos afectados** antes de empezar
(registra el acuerdo en el Issue) para evitar duplicidades o conflictos.

Cada una de las tres contribuciones debe ser **trazable**:

```text
Issue → Spec → Branch → Commits → Tests → PR → Review → CI → Merge
```

Para facilitar la evaluación, mantén esta tabla en el Issue principal de tu equipo:

| # | Tipo | Issue | Spec | PR | Estado |
|---|------|-------|------|----|--------|
| 1 | Vertical slice | #… | `docs/specs/…` | #… | … |
| 2 | Vertical slice | #… | `docs/specs/…` | #… | … |
| 3 | Transversal | #… | `docs/specs/…` | #… | … |

## Uso de IA

Puedes usar IA como asistente, siguiendo la [política de desarrollo con IA](ai-development-policy.md).
Recuerda: **debes poder explicar, modificar y defender cualquier código que entregues.**
