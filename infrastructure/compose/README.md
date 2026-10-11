# infrastructure/compose

> **Estado:** vacío. No existe todavía un archivo Docker Compose porque no hay aplicaciones que ejecutar.

## Propósito futuro

Levantar **en local** el conjunto de ST ServiceHub con un solo comando:

- Una base de datos por servicio (respetando la propiedad de datos).
- Los servicios implementados.
- El frontend.
- Herramientas de apoyo opcionales (p. ej. un servidor SMTP de pruebas si existe Notification Service).

Docker Compose es una herramienta de **desarrollo local**; no representa el despliegue en producción.

## Reglas

- Las credenciales se leen de variables de entorno (`.env`), nunca se escriben en el archivo Compose.
- Solo se añaden servicios que existan y funcionen.
- Las personalizaciones locales van en `docker-compose.override.yml` (ignorado por Git).

## Pendiente

- Motor de base de datos.
- Puertos definitivos (ver propuesta en `.env.example`).
