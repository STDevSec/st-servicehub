# Contratos de API

> **Estado:** todavía no existe ningún contrato. Se crearán a partir de la semana 2.

Este directorio contiene los contratos **OpenAPI** de cada servicio. El contrato es el acuerdo entre el
equipo que **produce** la API y los equipos que la **consumen** (frontend u otros servicios).

## Convenciones

- Un archivo por servicio: `<servicio>.yaml` (p. ej. `ticket-service.yaml`).
- Versión de la especificación OpenAPI: pendiente de confirmar según la versión de las librerías
  elegidas (se recomienda 3.x).
- Rutas en plural y en inglés, con prefijo común: `/api/<recurso>` (p. ej. `/api/tickets`).
- JSON en `camelCase`.
- Formato de error común a todos los servicios (pendiente; se recomienda *Problem Details*, RFC 9457).
- Toda operación declara su requisito de seguridad (token y roles).

## Flujo recomendado (*contract-first*)

1. Definir o modificar el contrato en este directorio como parte de la spec.
2. Revisarlo con los equipos consumidores **antes** de implementar.
3. Implementar el servicio respetando el contrato.
4. Verificar que la documentación generada por el servicio coincide con el contrato.

## Cambios de contrato

- Un cambio **compatible** (añadir un campo opcional, un endpoint nuevo) se declara en el PR.
- Un cambio **incompatible** (eliminar o renombrar campos, cambiar tipos o códigos de respuesta) requiere
  acuerdo con los consumidores y, si afecta a varios equipos, un ADR.
