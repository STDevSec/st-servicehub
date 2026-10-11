# Auditoría técnica del bootstrap — ST ServiceHub

| Campo | Valor |
|-------|-------|
| **Fecha** | 2026-10-10 |
| **Repositorio** | <https://github.com/STDevSec/st-servicehub> |
| **Commit auditado** | `283a1d508b4a3ee4136bb7c181a3f09b5e6c8ce8` (`main`, sincronizado con `origin/main`) |
| **Fase del proyecto** | Bootstrap (sin implementación funcional) |
| **Realizada por** | Claude Code, asistente de IA con revisión humana pendiente (ver [política de IA](../learning/ai-development-policy.md)) |

---

## 1. Resumen ejecutivo

El repositorio está en **buen estado para un bootstrap**. La estructura coincide con la arquitectura
definida, no hay código de negocio ni dependencias, no se han encontrado secretos ni en el árbol de
trabajo ni en el historial, y las plantillas de GitHub son válidas.

Se detectaron **28 hallazgos**: 0 CRITICAL, 2 HIGH, 7 MEDIUM, 10 LOW y 9 INFORMATIONAL. La mayoría eran
**contradicciones entre documentos**, huecos de proceso (revisión en equipos pequeños, fase IMPLEMENT
en la plantilla SDD) y detalles de configuración. Se corrigieron **19**; los **9 restantes** dependen de
decisiones institucionales o de cambios de configuración remota en GitHub que no se aplicaron sin
autorización.

Los dos hallazgos HIGH se deben a que el repositorio **ya es público**: no tiene licencia y no dispone
de un canal privado operativo para reportar vulnerabilidades.

## 2. Alcance

**Revisado:**

- Todos los archivos rastreados (33) y el historial de Git (1 commit).
- Documentación: README, CONTRIBUTING, SECURITY, LICENSE-DECISION, arquitectura, ADR, specs, API,
  guías de aprendizaje e infraestructura.
- Configuración: `.gitignore`, `.gitattributes`, `.editorconfig`, `.env.example`.
- Plantillas de GitHub: Issues, Pull Request y CODEOWNERS.
- Configuración remota de GitHub (solo lectura): visibilidad, protección de `main`, etiquetas, opciones
  de merge, funciones de seguridad y errores de CODEOWNERS.
- Enlaces internos y externos y diagramas Mermaid.

**Fuera de alcance:**

- Implementación de servicios, frontend, Docker o pipelines (no existen todavía, por diseño).
- Cambios en la configuración remota de GitHub y en el historial de Git.
- Revisión legal de la licencia.
- Infraestructura de `stdevsec.com` (solo se registra lo observado).

## 3. Inventario

**Estado inicial:**

- Ruta: `/run/media/rootatkali/2A29735162DEE30D/Programming/st-servicehub`, en un sistema de archivos NTFS.
  Todos los modos de archivo son `100644`.
- Rama `main` en el commit `283a1d5`, sincronizada con `origin` (`https://github.com/STDevSec/st-servicehub.git`).
- Árbol de trabajo limpio: sin cambios locales, sin archivos sin seguimiento y sin archivos ignorados presentes.
- 33 archivos rastreados: 26 de contenido y 7 `.gitkeep`.

```text
.github/            CODEOWNERS, PULL_REQUEST_TEMPLATE.md, ISSUE_TEMPLATE/{feature,bug_report,config}.yml
docs/architecture/  overview.md, system-context.md, service-boundaries.md
docs/decisions/     README.md (con plantilla ADR), ADR-0001-monorepo.md
docs/specs/         README.md, feature-template.md
docs/api/           README.md
docs/learning/      student-guide.md, ai-development-policy.md
frontend/angular-app/                .gitkeep
services/<5 servicios>/              .gitkeep
infrastructure/{docker,compose,jenkins}/README.md
scripts/                             .gitkeep
raíz: .editorconfig .gitattributes .gitignore .env.example README.md CONTRIBUTING.md SECURITY.md LICENSE-DECISION.md
```

**Configuración remota observada:**

| Elemento | Valor |
|----------|-------|
| Visibilidad | **Pública** |
| Repositorio plantilla | No |
| Protección de `main` | PR obligatorio, 1 aprobación, descartar aprobaciones obsoletas, resolver conversaciones, sin force push, sin borrado. `enforce_admins`: desactivado. Sin status checks |
| Etiquetas | Las 9 por defecto de GitHub (incluye `bug` y `enhancement`) |
| Opciones de merge | squash, merge commit y rebase permitidos; `delete_branch_on_merge`: desactivado |
| Secret scanning / push protection / Dependabot | Desactivados |
| Private vulnerability reporting | Desactivado |
| Errores de CODEOWNERS | Ninguno |

**Estructura:** coherente. Los servicios opcionales están identificados como opcionales en todos los
documentos. No hay duplicados ni archivos redundantes. Los directorios vacíos están rastreados con `.gitkeep`
y los de infraestructura tienen un README que explica su propósito futuro.

## 4. Hallazgos

Estados: **Corregido**, **Pendiente**, **Documentado** (se acepta y queda explicado).

| ID | Severidad | Archivo | Descripción | Impacto | Estado | Corrección |
|----|-----------|---------|-------------|---------|--------|------------|
| A-01 | HIGH | `LICENSE-DECISION.md` (remoto) | Repositorio **público** sin licencia. | Sin licencia no está claro qué derechos tienen terceros sobre el código ni sobre las contribuciones de los estudiantes. | Pendiente | Requiere decisión de STDevSec (ver R-1). |
| A-02 | HIGH | `SECURITY.md` (remoto) | No existe canal privado operativo para reportar vulnerabilidades: el reporte privado de GitHub está desactivado y no hay un contacto definido. El documento decía "si está habilitado". | Quien encuentre una vulnerabilidad podría publicarla en un Issue. | Pendiente (documento corregido) | `SECURITY.md` refleja el estado real e indica el contacto provisional; falta activar el reporte privado o publicar un contacto (R-2). |
| A-03 | MEDIUM | Configuración remota | Secret scanning y push protection desactivados en un repositorio público (son gratuitos). | Un secreto subido por error no se bloquearía ni se detectaría. | Pendiente | Recomendado en `SECURITY.md`; requiere autorización (R-3). |
| A-04 | MEDIUM | `CONTRIBUTING.md` | Afirmaba que la protección de `main` **no** estaba aplicada, cuando ya lo está. | Información falsa sobre un control de seguridad. | Corregido | Tabla con el estado real de cada regla y comando para consultarlo. |
| A-05 | MEDIUM | `CONTRIBUTING.md`, `student-guide.md` | No había política de revisión para equipos de 1 a 3 personas. | Equipos unipersonales sin revisor posible, o revisiones no independientes. | Corregido | Política nueva: compañero no coautor → equipo revisor par → instructor; prohibida la autoaprobación. |
| A-06 | MEDIUM | `docs/specs/feature-template.md` | La plantilla saltaba de TASKS a VERIFY: faltaba la fase **IMPLEMENT**. | El ciclo SDD documentado no coincidía con la plantilla; no se registraban desviaciones ni uso de IA. | Corregido | Nueva sección 5 (IMPLEMENT); secciones renumeradas; nuevo ítem en la Definition of Done. |
| A-07 | MEDIUM | `.gitattributes` | `package-lock.json` tenía `-diff`, que oculta sus cambios en `git diff`. | Impide revisar cambios de dependencias localmente (riesgo de cadena de suministro). | Corregido | Se eliminó `-diff` y se mantuvo `linguist-generated`. |
| A-08 | MEDIUM | `README.md`, `overview.md` | Las decisiones técnicas estaban solo enumeradas, sin alternativas ni criterio. | La semana 1 no tenía base para decidir; riesgo de elegir versiones al azar. | Corregido | Nuevo [`technical-decisions-pending.md`](../architecture/technical-decisions-pending.md) (D1–D14), sin fijar versiones. |
| A-09 | MEDIUM | `README.md` | El roadmap no coincidía con los objetivos oficiales de cada semana: Docker Compose en la semana 4, y faltaban despliegue y defensa técnica. | Expectativas de entrega desalineadas con el curso. | Corregido | Roadmap reescrito según las semanas oficiales. |
| A-10 | LOW | `CONTRIBUTING.md` / `student-guide.md` | El plazo para reportar un bloqueo era contradictorio: "más de un día" frente a "más de medio día". | Instrucciones inconsistentes. | Corregido | Unificado en medio día. |
| A-11 | LOW | `README.md` | Las versiones se confirmaban "al inicio de la semana 2" según los requisitos y "en la semana 1" según el roadmap. | Contradicción. | Corregido | Unificado en la semana 1. |
| A-12 | LOW | `ADR-0001-monorepo.md` | Hablaba de un "curso de tres semanas"; el curso dura cuatro (tres de desarrollo). | Imprecisión. | Corregido | Corrección editorial. No cambia la decisión, que sigue siendo válida. |
| A-13 | LOW | `PULL_REQUEST_TEMPLATE.md` | Faltaba la sección "Cambios realizados". | Revisión menos guiada. | Corregido | Sección añadida; "Especificación" pasa a llamarse "Especificación (SDD)". |
| A-14 | LOW | PR template, `feature-template.md` | Espacios al final de línea en los marcadores de posición (`- `, `- [ ] `). | Ruido en los diffs; contradice `.editorconfig`. | Corregido | Sustituidos por `<...>` y `CA-1: <criterio>`. |
| A-15 | LOW | `.gitignore` | No excluía archivos de bases de datos locales (`*.db`, `*.sqlite`, `*.mv.db`). | Podrían subirse datos locales. | Corregido | Reglas añadidas. Se descartó `data/` porque excluiría recursos legítimos como `src/main/resources/data/`. |
| A-16 | LOW | `student-guide.md` | Recomendaba `git rebase` sin explicar que una rama ya subida requiere `--force-with-lease`. | Estudiantes bloqueados o usando `--force`. | Corregido | Se recomienda `merge` y se explica `--force-with-lease`. |
| A-17 | LOW | `service-boundaries.md` | "Depende de Identity para validar tokens" sugería una llamada en tiempo de ejecución. | Confusión sobre el acoplamiento entre servicios. | Corregido | Se aclara que la validación puede ser local (enlace a D7). |
| A-18 | LOW | Configuración remota | `delete_branch_on_merge` está desactivado, pero CONTRIBUTING indica que la rama se elimina al integrar. | Acumulación de ramas obsoletas. | Pendiente | Activarlo requiere autorización (R-4). |
| A-19 | LOW | `README.md`, `student-guide.md` | La regla 2+1 no decía que las dos slices son **propias** (individuales). | Ambigüedad en la evaluación. | Corregido | Redacción ajustada. |
| A-20 | INFO | Configuración remota | `enforce_admins` desactivado: los administradores pueden saltarse la protección. | Aceptable con un único administrador. | Documentado | Explicado en CONTRIBUTING. |
| A-21 | INFO | Configuración remota | No hay status checks obligatorios. | Esperado: no existe CI todavía. | Documentado | Añadir cuando exista Jenkins (D12). |
| A-22 | INFO | `.github/CODEOWNERS` | Todas las reglas están comentadas; sintaxis válida (la API no devuelve errores). | Sin asignación automática de revisores. | Pendiente | Completar con equipos reales (R-5). |
| A-23 | INFO | Configuración remota | El repositorio no está marcado como plantilla. | Reutilización manual entre generaciones. | Pendiente | Ver la [sección 9](#9-estrategia-para-futuras-generaciones). |
| A-24 | INFO | `README.md` (enlace institucional) | `https://stdevsec.com` falla de forma intermitente en el handshake TLS (`tlsv1 alert internal error`). Por HTTP y `https://www.stdevsec.com` responde 200. | El enlace puede fallar a algunos visitantes. Problema externo al repositorio. | Documentado | Revisar la configuración TLS del dominio raíz. El enlace se mantiene porque es la URL oficial. |
| A-25 | INFO | `.editorconfig` | El patrón `[{pom.xml,*.xml}]` era redundante. | Ninguno funcional. | Corregido | Simplificado a `[*.xml]`. |
| A-26 | INFO | `CONTRIBUTING.md` | El *squash merge* recomendado agrupa los commits individuales en `main`, lo que puede chocar con la trazabilidad 2+1. | Evaluadores buscando commits en `main`. | Corregido | Se aclara que la evidencia de trazabilidad es el PR, que conserva los commits. |
| A-27 | INFO | `overview.md` | Usaba el presente ("es") para servicios no implementados. | Podía sugerir que existen. | Corregido | Cambiado a futuro. |
| A-28 | INFO | `docs/specs/README.md` | No dejaba explícito que SDD no consiste en generar código con IA. | Riesgo de que la spec se use solo como prompt. | Corregido | Nueva sección "SDD no es generar código con IA". |

**Verificado sin hallazgos:**

- Separación de servicios obligatorios y opcionales.
- Propiedad de datos y prohibición de acceso directo a la base de datos de otro servicio.
- Uso de REST y OpenAPI.
- Autorización en el backend y *guards* de Angular como simple ayuda de interfaz.
- Ausencia de Kubernetes, mensajería o gateway obligatorios.
- "Monorepo no es monolito".
- `.env.example` sin secretos.
- Plantillas de Issues válidas.
- Política de IA con la regla de explicar y defender el código.

## 5. Cambios realizados

| Archivo | Cambio | Hallazgos |
|---------|--------|-----------|
| `README.md` | Roadmap alineado con las semanas oficiales; confirmación de versiones en la semana 1; regla 2+1 "propias"; estado de la protección de `main`; enlaces a decisiones pendientes y a esta auditoría; `docs/audits/` añadido al árbol | A-08, A-09, A-11, A-19 |
| `CONTRIBUTING.md` | Política de revisión para equipos pequeños; tabla con el estado real de la protección; nota sobre squash y trazabilidad; plazo de bloqueo de medio día | A-04, A-05, A-10, A-26 |
| `SECURITY.md` | Estado real del canal de reporte; recomendación de controles automáticos de GitHub | A-02, A-03 |
| `.github/PULL_REQUEST_TEMPLATE.md` | Secciones "Especificación (SDD)" y "Cambios realizados"; marcadores sin espacios finales | A-13, A-14 |
| `docs/specs/feature-template.md` | Sección IMPLEMENT; renumeración; nuevo ítem en la DoD; marcadores sin espacios finales | A-06, A-14 |
| `docs/specs/README.md` | Paso de implementación; sección "SDD no es generar código con IA" | A-06, A-28 |
| `docs/learning/student-guide.md` | `merge` frente a `rebase` y `--force-with-lease`; política de revisión; 2+1 "propias"; IMPLEMENT; referencia a la tabla 6.1 | A-05, A-06, A-16, A-19 |
| `docs/architecture/overview.md` | Redacción en futuro; enlace a decisiones pendientes | A-08, A-27 |
| `docs/architecture/service-boundaries.md` | Aclaración sobre la validación local del token | A-17 |
| `docs/decisions/ADR-0001-monorepo.md` | Corrección editorial de la duración del curso | A-12 |
| `.gitattributes` | Eliminado `-diff` de `package-lock.json` | A-07 |
| `.gitignore` | Exclusión de bases de datos locales | A-15 |
| `.editorconfig` | Patrón XML simplificado | A-25 |
| **Nuevo:** `docs/architecture/technical-decisions-pending.md` | D1–D14 con contexto, alternativas, ventajas, desventajas, recomendación, riesgos y estado | A-08 |
| **Nuevo:** `docs/audits/bootstrap-audit.md` | Este informe | — |

No se modificó la configuración remota ni el historial de Git, y no se hizo commit ni push.

## 6. Validaciones

Ejecutadas después de las correcciones, salvo que se indique otra cosa.

| # | Validación | Comando / método | Resultado |
|---|------------|------------------|-----------|
| V1 | YAML y esquema básico de las plantillas de Issues | `python3` + `yaml.safe_load`: claves obligatorias, tipos de campo admitidos, `id` únicos, `default` dentro de rango, `contact_links` completos | ✅ 3/3 OK |
| V2 | Las etiquetas usadas en las plantillas existen | `gh label list` comparado con `labels:` | ✅ `bug` y `enhancement` existen |
| V3 | JSON | `git ls-files '*.json'` | No aplica: 0 archivos JSON |
| V4 | Enlaces relativos y anclas Markdown | Script `python3` que resuelve rutas y anclas con slugs de GitHub | ✅ 0 rotos, verificado con este informe ya creado (primera pasada: 2 rotos, ambos hacia este informe aún inexistente) |
| V5 | Enlaces externos | `curl -L` a cada URL | ✅ 10/11 con HTTP 200. ⚠️ `https://stdevsec.com`: error TLS intermitente (A-24) |
| V6 | Finales de línea | `git ls-files --eol` | ✅ Todo LF (los `.gitkeep` están vacíos) |
| V7 | Espacios al final de línea | `grep -nE '[[:space:]]+$'` | ✅ Ninguno (antes: 4, A-14) |
| V8 | Salto de línea final | `tail -c1` en cada archivo | ✅ Todos |
| V9 | Diagramas Mermaid | `mermaid.parse()` de `mermaid@11.4.1` + `jsdom`, instalados localmente fuera del repositorio; control negativo con un diagrama inválido | ✅ 3/3 válidos; el control negativo falla como se espera |
| V10 | Secretos en el árbol de trabajo | `grep` de asignaciones `password/secret/token/api_key` con valor y de patrones de claves (PEM, AWS, GitHub, Slack, `sk-`) | ✅ Sin coincidencias |
| V11 | Secretos en el historial | `git log -p --all` con los mismos patrones | ✅ Sin coincidencias (1 commit) |
| V12 | Comportamiento de `.gitignore` | `git check-ignore --no-index` sobre rutas de ejemplo | ✅ Ignora `.env`, `.env.local`, `target/`, `node_modules/`, `*.pem`, `*.mv.db`, `application-local.yml`. Rastrea `.env.example`, `maven-wrapper.jar` y `src/main/resources/data/` |
| V13 | Git rastrea los archivos nuevos y modificados | `git add -A -n` | ✅ 13 modificados y 2 nuevos rastreables |
| V14 | Ausencia de código de negocio y dependencias | Búsqueda de `*.java`, `*.ts`, `pom.xml`, `package.json`, `Dockerfile`, `Jenkinsfile`, compose | ✅ Ninguno |
| V15 | Sintaxis de CODEOWNERS | `gh api .../codeowners/errors` | ✅ `{"errors":[]}`. Se valida la versión publicada; este cambio no modifica CODEOWNERS |
| — | Renderizado visual de las plantillas en la interfaz de GitHub | — | **NO EJECUTADA** — requiere publicar los cambios y revisarlos en el navegador |
| — | *Lint* de estilo Markdown (markdownlint) | — | **NO EJECUTADA** — la herramienta no está instalada y no se autorizó instalarla |
| — | Ortografía | — | **NO EJECUTADA** — no hay corrector disponible; se hizo solo una revisión manual (no cuenta como prueba automatizada) |

## 7. Riesgos pendientes

| ID | Riesgo | Requiere |
|----|--------|----------|
| R-1 | Repositorio público sin licencia (A-01) | Decisión de STDevSec Academy ([LICENSE-DECISION.md](../../LICENSE-DECISION.md)) |
| R-2 | Sin canal privado de vulnerabilidades (A-02) | Activar *Private vulnerability reporting* o publicar un contacto de seguridad |
| R-3 | Sin secret scanning ni push protection (A-03) | Autorización para activarlos en *Settings → Code security* |
| R-4 | Ramas obsoletas tras integrar (A-18) | Autorización para activar `delete_branch_on_merge` |
| R-5 | CODEOWNERS vacío (A-22) | Crear los equipos en GitHub y asignar responsables |
| R-6 | Decisiones técnicas D1–D14 abiertas | Cerrarlas en la semana 1 mediante ADR |
| R-7 | Los administradores pueden saltarse la protección (A-20) | Activar `enforce_admins` cuando haya más revisores |
| R-8 | Términos de participación de los estudiantes en un repositorio público (datos personales en commits, autoría) | Decidir si los estudiantes usan un correo `noreply` de GitHub y si se requiere aceptación de términos o DCO |

## 8. Recomendaciones

1. **Antes de abrir el repositorio a estudiantes:** resolver R-1, R-2 y R-3.
2. **Semana 1:** cerrar D1–D7 (versiones, base de datos, build y autenticación) como ADR-0002 en adelante;
   asignar equipos y equipos revisores pares; completar CODEOWNERS.
3. **Antes de la semana 3:** tener disponible el servidor Jenkins (D12) y añadir sus checks como
   obligatorios en `main`.
4. Recomendar a los estudiantes la opción *Keep my email address private* de GitHub y su correo
   `noreply` en `git config user.email`.
5. Repetir esta auditoría al final del curso como base para la siguiente generación.

## 9. Estrategia para futuras generaciones

**Diagnóstico:** el repositorio es apto como **plantilla**: es solo documentación y estructura, no
contiene soluciones ni datos de estudiantes. Aún no está marcado como plantilla.

**Propuesta (no aplicada; requiere autorización):**

1. **Base estable:** este repositorio (`st-servicehub`) se mantiene como **repositorio plantilla**
   (*Settings → Template repository*). Solo contiene el bootstrap y se versiona con etiquetas
   (`template-v1.0`, `template-v1.1`, …). Las mejoras entre generaciones llegan por PR revisado.
2. **Un repositorio por generación,** creado con *Use this template*, por ejemplo
   `st-servicehub-2026-g1`. Así las entregas de una generación nunca se mezclan con las de otra y cada
   generación tiene su propio historial, Issues y PRs.
3. **Materiales del instructor separados:** soluciones de referencia, rúbricas y especificaciones
   modelo viven en un **repositorio privado distinto**, por ejemplo `st-servicehub-instructor`, al que no
   acceden los estudiantes. Nunca en una rama ni en una carpeta del repositorio de los estudiantes:
   el historial de Git conservaría las respuestas.
4. **Evitar publicar respuestas antes de tiempo:** los repositorios de generación se crean privados
   durante el curso y solo se publican (si se decide) al terminar la evaluación. Los enunciados de
   evaluación se publican como Issues en la fecha correspondiente, no antes.
5. **Trazabilidad individual:** cada estudiante usa su propia cuenta de GitHub, la cadena
   Issue → PR → Review queda en su repositorio de generación, y al cerrar el curso se archiva
   (*Archive*) el repositorio, que queda en solo lectura.
6. **Retroalimentación a la plantilla:** al terminar cada generación, las mejoras de documentación o
   proceso se trasladan a la plantilla por PR, nunca código de soluciones.

> Esta estrategia supone un cambio sobre el uso actual del repositorio, donde `st-servicehub` sería el
> repositorio de trabajo de la primera generación. Si se adopta, debería registrarse como ADR.

## 10. Criterio de preparación

| Nivel | ¿Listo? | Justificación |
|-------|---------|---------------|
| **1. Plantilla académica** | **Sí, con condiciones** | Estructura, documentación, plantillas y políticas completas y coherentes. Antes de usarlo como plantilla pública conviene resolver la licencia (R-1) y marcarlo como plantilla. |
| **2. Scaffolding técnico** | **No** | Faltan D1–D6: versiones de Java, Spring Boot, Angular y Node.js, motor de base de datos y gestor de dependencias. Generar proyectos ahora fijaría versiones sin decisión. D7 (autenticación) debe cerrarse antes de crear Identity Service. |
| **3. Contribuciones estudiantiles** | **No, todavía** | Requiere el nivel 2, además de equipos y revisores pares asignados, CODEOWNERS (R-5), canal de seguridad (R-2), licencia o términos de participación (R-1, R-8) y secret scanning recomendado (R-3). |
