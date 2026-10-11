# infrastructure/jenkins

> **Estado:** vacío. No existe todavía ningún pipeline.

## Propósito futuro

Pipelines de integración continua (`Jenkinsfile`) para ST ServiceHub.

Principios previstos:

- **Un pipeline por servicio** (y otro para el frontend), que se ejecuta solo cuando cambian archivos de
  su directorio. Así se respeta el despliegue independiente aunque el código esté en un monorepo
  ([ADR-0001](../../docs/decisions/ADR-0001-monorepo.md)).
- Etapas mínimas: compilar → pruebas → análisis/dependencias → construir imagen Docker.
- Ejecución en cada Pull Request; el resultado se reporta a GitHub como verificación requerida.
- Credenciales gestionadas con *Jenkins Credentials*, nunca en el `Jenkinsfile`.

## Pendiente

- Servidor Jenkins disponible y su integración con GitHub.
- Ubicación final de los `Jenkinsfile` (este directorio o la raíz de cada servicio).
