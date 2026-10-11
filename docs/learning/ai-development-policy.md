# Política de desarrollo asistido por IA

En ST ServiceHub se permite y se fomenta el uso de herramientas de inteligencia artificial (asistentes de
código, chats, agentes) **como apoyo**. El objetivo del curso es aprender a usarlas de forma profesional:
con criterio, verificación y responsabilidad.

> ### Regla fundamental
>
> **El estudiante debe poder explicar, modificar y defender cualquier código que entregue, aunque haya
> sido generado con asistencia de IA.**

## 1. La IA está permitida como asistente

Usos adecuados:

- Explicar conceptos, errores o código existente.
- Proponer alternativas de diseño para discutirlas.
- Generar borradores de código, pruebas o documentación a partir de **tu** especificación.
- Revisar tu código en busca de errores o mejoras.

La IA **no** sustituye la especificación, la revisión humana ni las pruebas.

## 2. Responsabilidad humana

- La autoría y la responsabilidad del código son **siempre de la persona que lo entrega**, no de la herramienta.
- "Lo generó la IA" no es una justificación válida para un error, una vulnerabilidad o una mala práctica.

## 3. Revisión obligatoria

- Todo código generado por IA se lee **línea por línea** antes de incorporarlo.
- Se pasa por el mismo Pull Request y Code Review que cualquier otro código.
- Presta especial atención a: dependencias inventadas o innecesarias, APIs inexistentes, código que
  ignora la arquitectura (p. ej. accesos a la base de datos de otro servicio) y controles de seguridad
  ausentes.

## 4. Verificación mediante pruebas

- El código asistido por IA debe estar cubierto por pruebas que **tú** entiendes y que comprueban los
  criterios de aceptación de la spec.
- Las pruebas generadas por IA también se revisan: una prueba que siempre pasa no verifica nada.

## 5. Prohibido exponer secretos

Nunca compartas con una herramienta de IA:

- Contraseñas, tokens, claves API, claves privadas o el contenido de `.env`.
- Datos personales reales.
- Información confidencial de STDevSec o de terceros.

Si necesitas ayuda con configuración, sustituye los valores por marcadores (`<DB_PASSWORD>`).

## 6. Revisar los cambios antes de aceptarlos

Si usas agentes o herramientas que modifican archivos o ejecutan comandos:

- Revisa cada cambio propuesto (`git diff`) antes de aceptarlo.
- No permitas que ejecuten acciones destructivas o remotas (`push`, `push --force`, borrado de ramas,
  cambios de configuración de GitHub) sin tu aprobación explícita.
- Trabaja siempre en tu rama, nunca directamente en `main`.

## 7. Registro del uso significativo de IA

Declara en la sección **Uso de IA** del Pull Request cuando la IA haya contribuido de forma
significativa (generó código, pruebas, diseño o documentación incorporados al PR):

- Herramienta utilizada.
- Para qué se utilizó.
- Qué revisaste, modificaste o descartaste.

No es necesario declarar usos menores (autocompletado de una línea, corrección ortográfica).
Declarar el uso de IA **no penaliza**; ocultarlo sí.

## 8. Capacidad de explicar el código

Durante las revisiones y la evaluación, el equipo docente puede pedirte que:

- Expliques qué hace una parte de tu código y por qué está diseñada así.
- Lo modifiques en el momento para cubrir un caso nuevo.
- Justifiques una decisión frente a alternativas.

Si no puedes hacerlo, la contribución no se considera completa, independientemente de cómo se generó.
