# infrastructure/docker

> **Estado:** vacío. No existe todavía ningún Dockerfile.

## Propósito futuro

Guías y recursos Docker **comunes** a varios servicios, por ejemplo:

- Convenciones para escribir los Dockerfiles (imagen base, usuario no root, *multi-stage build*,
  `HEALTHCHECK`).
- Archivo `.dockerignore` de referencia.

Cada servicio tendrá **su propio `Dockerfile` dentro de su directorio** (`services/<servicio>/Dockerfile`,
`frontend/angular-app/Dockerfile`) para mantener su despliegue independiente.

## Pendiente

- Versiones de imágenes base (dependen de las versiones de Java y Node.js que se confirmen).
- Registro de imágenes, si se necesita.
