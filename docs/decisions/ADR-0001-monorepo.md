# ADR-0001: Utilizar un monorepositorio académico

- **Estado:** Aceptado
- **Fecha:** 2026-10-10
- **Autores:** STDevSec Academy
- **Issue relacionada:** N/A (decisión fundacional)

## Contexto

ST ServiceHub está compuesto por tres microservicios obligatorios, dos opcionales y una aplicación
Angular. Lo desarrollan varios equipos de estudiantes durante tres semanas, con supervisión docente.

Hay que decidir si cada componente vive en su propio repositorio (*polyrepo*) o si todos conviven en un
único repositorio (*monorepo*).

## Decisión

Utilizaremos **un único repositorio (`st-servicehub`)** que contiene todos los servicios, el frontend,
la infraestructura y la documentación.

Cada servicio mantiene su propio directorio, su propio proyecto Maven, su propia base de datos y su
propio pipeline. **Monorepo no significa monolito.**

## Motivos

- **Facilitar la enseñanza:** estudiantes y docentes ven el sistema completo en un solo lugar.
- **Centralizar contratos:** los contratos OpenAPI viven en `docs/api/`, visibles para productores y
  consumidores.
- **Simplificar el Code Review:** un cambio que afecta a la API y al frontend puede revisarse en un
  solo PR.
- **Coordinar frontend y backend:** evita versiones desalineadas entre repositorios.
- **Facilitar la CI:** una única configuración de Jenkins con pipelines por servicio.
- **Mantener la trazabilidad:** Issues, specs, ADR y código comparten historial y numeración.

## Alternativas consideradas

1. **Un repositorio por servicio (polyrepo):** refleja mejor la independencia real en producción, pero
   multiplica la configuración (permisos, plantillas, CI), dispersa la documentación y complica la
   revisión de cambios transversales. Excesivo para un curso de tres semanas.
2. **Monolito modular:** más sencillo de ejecutar, pero no cumple el objetivo educativo de practicar
   microservicios con despliegue independiente.

## Consecuencias

**Positivas:**

- Visión completa del sistema, documentación y contratos centralizados.
- Plantillas, reglas y convenciones únicas para todos los equipos.

**Negativas / riesgos:**

- **Mayor necesidad de reglas de colaboración:** muchas personas trabajan sobre el mismo `main`.
  Se mitiga con protección de rama, PR obligatorios, `CODEOWNERS` y convenciones de ramas/commits
  ([CONTRIBUTING.md](../../CONTRIBUTING.md)).
- **Posible crecimiento del repositorio:** se mitiga con `.gitignore` estricto (sin artefactos de
  compilación, `node_modules` ni binarios innecesarios).
- **Necesidad de pipelines específicos por servicio:** la CI debe construir y probar solo lo que cambió,
  para no acoplar los ciclos de los servicios.
- **Riesgo de acoplamiento accidental:** que el código esté cerca facilita, por error, compartir clases
  o acceder a otra base de datos. Está prohibido: ver
  [service-boundaries.md](../architecture/service-boundaries.md).

**Acciones derivadas:**

- Cada servicio será un proyecto Maven independiente, **sin** POM padre que acople sus versiones
  (cualquier excepción requiere un ADR).
- No se comparten clases de dominio entre servicios.
- Definir pipelines Jenkins por servicio.
