# Decisiones técnicas pendientes

> **Estado general:** ninguna de estas decisiones está tomada. Este documento **no fija versiones**:
> recoge alternativas y una recomendación orientativa para que STDevSec Academy decida durante la
> **semana 1**. Cada decisión aceptada se registrará como ADR en [`docs/decisions`](../decisions/README.md)
> y se actualizará aquí su estado.

**Criterios usados en las recomendaciones:**

- Curso de **cuatro semanas** (tres de desarrollo) con estudiantes de **niveles distintos**.
- Preferir lo **estable, documentado y con soporte vigente** durante todo el curso.
- **No añadir complejidad** que no aporte valor educativo (sin Kubernetes, mensajería ni API Gateway
  por defecto).
- Coherencia con la arquitectura definida: servicios independientes, datos por servicio, REST + OpenAPI,
  autorización en backend.

> Antes de fijar cualquier versión, compruébala en la fuente oficial en la fecha de la decisión:
> [Spring Boot — soporte](https://spring.io/projects/spring-boot#support),
> [Angular — versiones y soporte](https://angular.dev/reference/releases),
> [Node.js — releases](https://nodejs.org/en/about/previous-releases).

## Resumen

| ID | Decisión | Recomendación orientativa | Estado |
|----|----------|---------------------------|--------|
| D1 | Versión de Java | La LTS más reciente soportada por la versión de Spring Boot elegida | Pendiente |
| D2 | Versión de Spring Boot | Última versión GA con soporte OSS vigente hasta el final del curso | Pendiente |
| D3 | Versión de Angular | Última versión mayor estable con soporte activo | Pendiente |
| D4 | Versión de Node.js | La LTS activa compatible con la versión de Angular elegida | Pendiente |
| D5 | Motor de base de datos | PostgreSQL, una base de datos por servicio | Pendiente |
| D6 | Gestor de dependencias | Maven + Maven Wrapper; npm en el frontend | Pendiente (previsto en el stack) |
| D7 | Autenticación | JWT firmado con clave asimétrica emitido por Identity Service | Pendiente |
| D8 | Autorización | RBAC con roles en el token + comprobación de propiedad en cada servicio | Pendiente |
| D9 | Estructura de contratos OpenAPI | Un YAML por servicio en `docs/api/`, revisado en PR | Pendiente |
| D10 | Versionado de APIs | Prefijo `/api/v1` y reglas de compatibilidad | Pendiente |
| D11 | Estrategia de pruebas | Pirámide: unitarias + *slice tests* + integración selectiva | Pendiente |
| D12 | Estrategia de CI/CD | Jenkins con pipeline por servicio; despliegue de demo con Docker Compose | Pendiente |
| D13 | API Gateway | No incorporar inicialmente | Pendiente |
| D14 | Formato de errores | *Problem Details* (RFC 9457) | Pendiente |

---

## D1. Versión de Java

- **Contexto:** el backend usa Java LTS. La versión condiciona la de Spring Boot, las imágenes Docker
  y el JDK que cada estudiante instala.
- **Alternativas:** (a) la LTS más reciente; (b) la LTS anterior.
- **Ventajas:** (a) más soporte futuro y características modernas del lenguaje; (b) más material
  didáctico, más madurez en IDEs y herramientas.
- **Desventajas:** (a) algunas herramientas o plugins pueden tardar en soportarla; (b) menos vida de
  soporte restante.
- **Recomendación:** la **LTS más reciente que la versión de Spring Boot elegida (D2) soporte
  oficialmente** y que funcione en IntelliJ/VS Code y en las imágenes base de Docker. Debe ser **una sola
  versión para todos los servicios**.
- **Riesgos:** versiones distintas entre estudiantes generan errores de compilación difíciles de
  diagnosticar. Mitigación: fijarla en el `pom.xml` (`maven.compiler.release`) y en el Dockerfile.
- **Estado:** Pendiente.

## D2. Versión de Spring Boot

- **Contexto:** Spring Boot define las versiones de Spring Security, Spring Data JPA, Hibernate y
  Jakarta EE. Las ramas mayores tienen cambios incompatibles (p. ej. paquetes y configuración de seguridad).
- **Alternativas:** (a) la última versión mayor GA; (b) la última versión menor de la rama mayor
  anterior, si mantiene soporte OSS.
- **Ventajas:** (a) soporte más largo y documentación actualizada; (b) más ejemplos y respuestas
  disponibles en la comunidad.
- **Desventajas:** (a) parte del material en internet (y de las sugerencias de IA) corresponde a
  versiones anteriores y puede no compilar; (b) puede quedarse sin soporte OSS durante o poco después
  del curso.
- **Recomendación:** la **última versión GA cuyo soporte OSS cubra todo el curso**, generada con
  Spring Initializr. Fijar la versión exacta (*patch*) en todos los servicios.
- **Riesgos:** código sugerido por IA para otra versión mayor. Mitigación: indicar la versión en la spec
  y en el README de cada servicio, y verificar con pruebas.
- **Estado:** Pendiente.

## D3. Versión de Angular

- **Contexto:** Angular publica una versión mayor aproximadamente cada seis meses, con cambios en las
  APIs recomendadas (componentes *standalone*, *signals*, *runner* de pruebas por defecto).
- **Alternativas:** (a) la última versión mayor estable; (b) la versión mayor anterior con soporte LTS.
- **Ventajas:** (a) coincide con la documentación oficial actual de angular.dev; (b) más ejemplos de la
  comunidad.
- **Desventajas:** (a) menos ejemplos de terceros; (b) la documentación oficial ya muestra otra forma
  de hacer las cosas.
- **Recomendación:** la **última versión mayor estable**, creada con Angular CLI y usando las opciones
  por defecto de esa versión (componentes *standalone*, Reactive Forms, HttpClient, Router).
- **Riesgos:** mezclar estilos (NgModules frente a *standalone*) por copiar ejemplos antiguos.
  Mitigación: guía de estilo breve en el README del frontend.
- **Estado:** Pendiente.

## D4. Versión de Node.js

- **Contexto:** Angular CLI exige rangos concretos de Node.js.
- **Alternativas:** (a) la LTS activa; (b) la LTS en mantenimiento.
- **Ventajas:** (a) soporte más largo; (b) máxima compatibilidad con herramientas antiguas.
- **Desventajas:** (a) ninguna relevante si Angular la soporta; (b) puede salir de soporte durante el curso.
- **Recomendación:** la **LTS activa incluida en el rango soportado por la versión de Angular** (D3).
  Documentarla en un archivo `.nvmrc` y en `package.json` (`engines`) cuando se cree el frontend.
- **Riesgos:** estudiantes con Node.js del sistema muy antiguo o versión *Current* no LTS.
- **Estado:** Pendiente.

## D5. Motor de base de datos

- **Contexto:** cada servicio es dueño de sus datos; se necesita un motor relacional ejecutable en
  local con Docker.
- **Alternativas:** (a) PostgreSQL; (b) MySQL/MariaDB; (c) H2 en memoria.
- **Ventajas:** (a) libre, muy extendido, buena compatibilidad con JPA y con Testcontainers; (b) muy
  conocido; (c) sin instalación.
- **Desventajas:** (a) requiere Docker o instalación; (b) diferencias de comportamiento (mayúsculas,
  tipos) que confunden; (c) no es un motor real: oculta errores que aparecen en producción.
- **Recomendación:** **PostgreSQL** en Docker. En local, **un contenedor con una base de datos y un
  usuario distintos por servicio** (más ligero que un contenedor por servicio y respeta la propiedad de
  datos). H2 no se usa como base de datos principal.
- **Riesgos:** que se comparta un usuario o esquema entre servicios. Mitigación: credenciales separadas
  (ver [`.env.example`](../../.env.example)) y revisión en PR.
- **Decisión relacionada:** migraciones de esquema (Flyway o Liquibase) frente a `ddl-auto`. Se
  recomienda Flyway desde el inicio para que los cambios de esquema sean revisables.
- **Estado:** Pendiente.

## D6. Gestor de dependencias

- **Contexto:** el stack previsto ya menciona Maven.
- **Alternativas:** (a) Maven; (b) Gradle. Frontend: (a) npm; (b) pnpm/yarn.
- **Ventajas:** Maven: declarativo, el más usado en material educativo de Spring; npm: incluido con Node.js.
- **Desventajas:** Maven es más verboso; pnpm/yarn añaden una herramienta extra sin beneficio para el curso.
- **Recomendación:** **Maven con Maven Wrapper (`mvnw`) en cada servicio**, sin POM padre común
  ([ADR-0001](../decisions/ADR-0001-monorepo.md)); **npm** con `package-lock.json` versionado.
- **Riesgos:** cada servicio declara versiones distintas de las mismas librerías. Mitigación: usar el
  BOM de Spring Boot y no fijar versiones que ya gestiona.
- **Estado:** Pendiente (previsto en el stack, sin ADR).

## D7. Autenticación

- **Contexto:** Identity Service autentica; el resto de servicios necesita saber quién hace la petición
  sin acceder a la base de datos de Identity.
- **Alternativas:**
  1. JWT firmado con **clave simétrica** (HMAC) compartida.
  2. JWT firmado con **clave asimétrica** (RSA/EC): Identity firma con la clave privada; los demás
     validan con la pública.
  3. Tokens opacos con *introspection* (cada servicio consulta a Identity).
  4. Proveedor externo (p. ej. Keycloak).
  5. Sesiones con cookie.
- **Ventajas:** (1) simple; (2) solo Identity puede emitir tokens, validación local y sin acoplamiento
  en tiempo de ejecución; (3) revocación inmediata; (4) solución completa y estándar; (5) familiar.
- **Desventajas:** (1) cualquier servicio con el secreto puede **falsificar** tokens; (2) algo más de
  configuración; (3) Identity se convierte en dependencia de cada petición; (4) elimina gran parte del
  aprendizaje del Identity Service y añade infraestructura; (5) no encaja con varios servicios
  independientes.
- **Recomendación:** **opción 2**, con validación en cada servicio mediante el soporte de *resource
  server* de Spring Security, tokens de vida corta y sin *refresh tokens* en el alcance mínimo. Si el
  nivel del grupo lo requiere, la opción 1 es aceptable como paso intermedio documentado con un ADR.
  *Nota:* [`.env.example`](../../.env.example) contiene una variable `AUTH_TOKEN_SECRET` provisional;
  se ajustará según la decisión.
- **Riesgos:** almacenar el token en `localStorage` lo expone a XSS; claves en el repositorio.
  Mitigación: documentar el almacenamiento elegido en la spec y gestionar claves por variables de entorno.
- **Estado:** Pendiente.

## D8. Autorización

- **Contexto:** la autorización debe aplicarse en el backend. Los *guards* de Angular solo mejoran la
  experiencia de usuario; no son un control de seguridad.
- **Alternativas:** (a) RBAC (roles en el token); (b) permisos finos (*scopes*/permisos por acción);
  (c) motor de políticas externo.
- **Ventajas:** (a) simple y suficiente para pocos roles; (b) más flexible; (c) centralizado.
- **Desventajas:** (a) por sí solo no cubre la **propiedad** del recurso; (b) más diseño; (c) complejidad
  injustificada en el curso.
- **Recomendación:** **RBAC con los roles del token** + **comprobación de propiedad del recurso en el
  servicio dueño** (p. ej. un cliente solo ve sus tickets), denegando por defecto. Usar seguridad a
  nivel de método de Spring Security y probar los casos 401/403 en cada slice.
- **Riesgos:** confiar en identificadores enviados por el cliente (IDOR). Mitigación: derivar la
  identidad siempre del token validado.
- **Estado:** Pendiente.

## D9. Estructura de contratos OpenAPI

- **Contexto:** los contratos deben documentarse con OpenAPI y estar centralizados
  ([`docs/api`](../api/README.md)).
- **Alternativas:** (a) *contract-first*: YAML escrito a mano y revisado antes de implementar;
  (b) *code-first*: generado desde el código (p. ej. springdoc); (c) mixto.
- **Ventajas:** (a) obliga a acordar el contrato con los consumidores; (b) menos trabajo, siempre
  sincronizado con el código; (c) combina ambas.
- **Desventajas:** (a) puede desincronizarse del código; (b) el contrato se conoce tarde y se diseña
  "por accidente"; (c) requiere disciplina.
- **Recomendación:** **mixto**: un `docs/api/<servicio>.yaml` por servicio, escrito en la spec y revisado
  en PR (contrato acordado), y la documentación generada por el servicio para **verificar** que coincide.
  Sin generación de código a partir del YAML (añade complejidad).
- **Riesgos:** divergencia entre YAML y código. Mitigación: incluir la comparación en la checklist de PR
  y, si el tiempo lo permite, un *lint* del YAML en CI.
- **Estado:** Pendiente.

## D10. Versionado de APIs

- **Contexto:** varios equipos consumen APIs ajenas; un cambio incompatible rompe al consumidor.
- **Alternativas:** (a) prefijo en la ruta (`/api/v1/...`); (b) cabecera o *media type*; (c) sin
  versionado, solo cambios compatibles.
- **Ventajas:** (a) explícito y fácil de entender y probar; (b) URLs estables; (c) lo más simple.
- **Desventajas:** (a) duplicación si llega a existir `v2`; (b) más difícil de probar y de explicar;
  (c) no ofrece salida ante un cambio incompatible.
- **Recomendación:** **prefijo `/api/v1`** desde el inicio y la regla de que, durante el curso, solo se
  permiten cambios compatibles; un cambio incompatible requiere ADR. Si se acepta, actualizar las
  convenciones de [`docs/api/README.md`](../api/README.md) (hoy indican `/api/<recurso>`).
- **Riesgos:** ninguno relevante si se decide antes del primer endpoint.
- **Estado:** Pendiente.

## D11. Estrategia de pruebas

- **Contexto:** cada criterio de aceptación debe tener al menos una prueba; el tiempo es limitado.
- **Alternativas:** (a) solo pruebas unitarias; (b) pirámide (unitarias + *slice tests* + integración
  selectiva); (c) foco en pruebas *end-to-end*.
- **Ventajas:** (a) rápidas; (b) equilibrio entre coste y confianza; (c) prueban el sistema completo.
- **Desventajas:** (a) no detectan errores de configuración, seguridad ni persistencia; (b) requiere
  enseñar varios tipos de prueba; (c) lentas, frágiles y difíciles de diagnosticar.
- **Recomendación:** **pirámide**:
  - Backend: unitarias (JUnit 5 + Mockito) para reglas de negocio; *slice tests* de Spring para
    controladores (incluidos 400/401/403) y repositorios; integración con base de datos real mediante
    Testcontainers a partir de la semana 3, si el entorno lo permite.
  - Frontend: el *runner* de pruebas por defecto de la versión de Angular elegida, para servicios y
    componentes con lógica.
  - *End-to-end*: opcional, solo para el flujo principal en la semana 4.
- **Riesgos:** pruebas que no verifican nada (sobre todo si las genera la IA). Mitigación: la revisión
  de PR comprueba la relación prueba ↔ criterio de aceptación.
- **Estado:** Pendiente.

## D12. Estrategia de CI/CD

- **Contexto:** el stack prevé Jenkins; el monorepo exige pipelines por servicio (ADR-0001).
- **Alternativas:** (a) Jenkins con *Multibranch Pipeline* y etapas condicionadas por ruta; (b) GitHub
  Actions; (c) ambos.
- **Ventajas:** (a) es la herramienta del curso; muy usada en empresas; (b) integrado en GitHub, sin
  servidor propio y gratuito en repositorios públicos; (c) redundancia.
- **Desventajas:** (a) requiere un servidor disponible, accesible desde GitHub y mantenido; (b) no es la
  herramienta del temario; (c) duplica configuración.
- **Recomendación:** **Jenkins** con un `Jenkinsfile` por servicio (compilar → probar → construir
  imagen), ejecutado en cada PR y publicado en GitHub como *status check* obligatorio. **Despliegue
  (CD):** solo un entorno de demostración con Docker Compose en la semana 4; sin Kubernetes.
- **Riesgos:** que el servidor Jenkins no esté listo en la semana 3. Mitigación: decidir en la semana 1
  quién lo aprovisiona y, como plan B documentado en un ADR, GitHub Actions con las mismas etapas.
- **Estado:** Pendiente.

## D13. API Gateway

- **Contexto:** el frontend llama a cada servicio directamente.
- **Alternativas:** (a) sin gateway (CORS configurado por servicio); (b) Spring Cloud Gateway; (c) proxy
  inverso (p. ej. Nginx) solo para enrutar.
- **Ventajas:** (a) nada extra que construir; (b) punto único para enrutamiento y seguridad; (c) simple
  y útil para el despliegue de demo.
- **Desventajas:** (a) el frontend conoce varias URLs y hay que configurar CORS en cada servicio; (b) un
  componente más que mantener; (c) configuración adicional.
- **Recomendación:** **sin gateway** al inicio. Valorar (c) en la semana 4 para el despliegue de demo,
  mediante ADR.
- **Riesgos:** configuración de CORS demasiado permisiva (`*`). Mitigación: orígenes explícitos.
- **Estado:** Pendiente.

## D14. Formato de errores

- **Contexto:** el frontend y los servicios consumidores necesitan un formato de error común.
- **Alternativas:** (a) *Problem Details* (RFC 9457); (b) formato propio.
- **Ventajas:** (a) estándar y soportado de forma nativa por Spring; (b) libertad total.
- **Desventajas:** (a) hay que acordar las extensiones (p. ej. errores de validación por campo);
  (b) cada equipo acaba con un formato distinto.
- **Recomendación:** **Problem Details**, con una extensión común para errores de validación por campo,
  sin trazas ni detalles internos.
- **Riesgos:** filtrar información interna en los mensajes. Mitigación: revisión de seguridad en PR.
- **Estado:** Pendiente.
