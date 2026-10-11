# Límites de los servicios

> **Estado:** diseño planificado. Los límites pueden ajustarse mediante un ADR.

Un **límite de servicio** define qué datos y qué reglas pertenecen a un servicio y cuáles no. Respetarlos
es lo que permite que cada equipo trabaje, pruebe y despliegue su servicio de forma independiente.

## Identity Service — obligatorio

**Responsable de:**

- Usuarios del sistema y sus credenciales (almacenadas con *hash* seguro, nunca en texto plano).
- Autenticación (inicio de sesión) y emisión de tokens.
- Roles y permisos.

**No es responsable de:** datos de negocio de clientes, tickets ni proyectos.

**Expone:** autenticación, consulta de usuarios/roles para quien tenga permiso.

**Depende de:** ningún otro servicio.

## Client Service — obligatorio

**Responsable de:**

- Alta, consulta, modificación y desactivación de clientes (organizaciones y sus contactos).
- Reglas de validación de los datos de cliente.

**No es responsable de:** usuarios ni credenciales (Identity) ni tickets (Ticket).

**Expone:** API de clientes, incluida la consulta de existencia/estado de un cliente por su id.

**Depende de:** Identity solo para validar tokens (ver nota sobre "token" en la matriz).

## Ticket Service — obligatorio

**Responsable de:**

- Tickets y su ciclo de vida (estados y transiciones válidas).
- Prioridades.
- Asignación de tickets a agentes.
- Comentarios en tickets.

**No es responsable de:** los datos del cliente ni del usuario. Guarda solo sus **identificadores**
(`clientId`, `assigneeId`, `authorId`).

**Expone:** API de tickets, estados, asignaciones y comentarios.

**Depende de:** Client Service (verificar que el cliente existe y está activo, vía REST); Identity para
validar tokens; Notification Service si existe.

## Project Service — opcional

**Responsable de:** proyectos y la asociación de tickets o clientes a proyectos mediante identificadores.

**No es responsable de:** el ciclo de vida de los tickets.

**Se implementa solo** si el número de estudiantes lo permite.

## Notification Service — opcional

**Responsable de:** generar y enviar notificaciones (p. ej. cambio de estado o asignación de un ticket)
y registrar su envío.

**No es responsable de:** decidir cuándo cambia un ticket; solo reacciona a las peticiones que recibe.

**Se implementa solo** si el número de estudiantes lo permite. Ticket Service debe funcionar
correctamente aunque Notification Service no exista o no esté disponible.

## Matriz de dependencias

| Origen → Destino | Identity | Client | Ticket | Project | Notification |
|------------------|:-------:|:------:|:------:|:-------:|:------------:|
| **Frontend**     | ✔ | ✔ | ✔ | opcional | — |
| **Client**       | token | — | — | — | — |
| **Ticket**       | token | ✔ REST | — | — | opcional |
| **Project**      | token | opcional | opcional | — | — |

"token" = solo validación del token de acceso. Según el mecanismo que se elija
([decisiones pendientes](technical-decisions-pending.md#d7-autenticación)), la validación puede hacerse
**localmente** (p. ej. verificando la firma de un JWT) sin llamar a Identity en cada petición. Cualquier nueva dependencia entre servicios se documenta
en esta matriz y, si es relevante, en un ADR.

## Por qué un servicio no accede a la base de datos de otro

Está **prohibido** que un servicio lea o escriba directamente en las tablas de otro servicio. Motivos:

1. **Acoplamiento oculto.** Si Ticket lee la tabla de clientes, cualquier cambio en esa tabla (renombrar
   una columna, dividirla) rompe Ticket sin que nadie lo note en revisión. La API es un contrato
   explícito y versionable; el esquema de la base de datos no lo es.
2. **Se pierde el despliegue independiente.** Los dos servicios quedan obligados a cambiar y desplegarse
   juntos: se vuelve un monolito distribuido, con los inconvenientes de ambos mundos.
3. **Se saltan las reglas de negocio.** Las validaciones y reglas viven en el servicio dueño. Escribir
   directamente en su tabla puede dejar datos inválidos.
4. **Se rompe la seguridad.** La autorización se aplica en la API del servicio dueño. Un acceso directo
   a la base de datos la evita por completo y además obliga a compartir credenciales.
5. **Se pierde la autonomía de los equipos.** Cada equipo debe poder evolucionar su modelo de datos
   libremente detrás de su API.

**Qué hacer en su lugar:**

- Pedir el dato por **API REST** al servicio dueño.
- Guardar solo el **identificador** de la entidad externa.
- Si el rendimiento lo exige, guardar una **copia mínima** del dato asumiendo que puede estar
  desactualizado, y documentarlo en un ADR.
