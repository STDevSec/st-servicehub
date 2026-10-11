# Política de seguridad — ST ServiceHub

ST ServiceHub es un proyecto **educativo**, pero se desarrolla con prácticas de seguridad profesionales.
Esta política aplica a todo el código, la configuración y la documentación del repositorio.

## 1. Manejo de secretos

Se considera secreto cualquier contraseña, token, clave API, clave privada, certificado, cadena de
conexión con credenciales o secreto de firma de tokens.

- **Nunca** se suben secretos al repositorio, ni siquiera "temporalmente" ni en ramas personales.
  Un secreto que llegó a un commit se considera **comprometido** aunque se elimine después: el
  historial de Git lo conserva.
- Los secretos se proporcionan mediante **variables de entorno** o el gestor de credenciales del
  entorno de CI (p. ej. *Credentials* de Jenkins).
- No compartas secretos por Issues, PRs, comentarios, capturas de pantalla ni herramientas de IA.

## 2. Variables de entorno

- [`.env.example`](.env.example) documenta las variables **sin valores sensibles**.
- Cada persona copia ese archivo a `.env` localmente. `.env` está excluido en [`.gitignore`](.gitignore).
- Las aplicaciones Spring Boot deben leer la configuración sensible desde variables de entorno
  (p. ej. `${TICKET_DB_PASSWORD}`), nunca desde valores fijos en `application.yml`.
- Si añades una variable nueva, actualiza `.env.example` en el mismo PR.

## 3. Prohibición de subir credenciales

Antes de cada commit:

```bash
git status
git diff --staged
```

Revisa que no aparezcan `.env`, claves, *keystores* ni valores sensibles. Si subiste un secreto por error:

1. **Avisa de inmediato** al equipo docente (en privado, no en un Issue público).
2. **Rota/revoca** el secreto: cambiarlo es obligatorio; borrarlo del código no basta.
3. Elimina el secreto del código en un nuevo commit y sigue las indicaciones del equipo docente sobre
   la limpieza del historial.

## 4. Reporte responsable de vulnerabilidades

Si encuentras una vulnerabilidad:

- **No abras un Issue público** con los detalles.
- Repórtala de forma privada mediante
  [GitHub Private Vulnerability Reporting](https://docs.github.com/es/code-security/security-advisories/guidance-on-reporting-and-writing-information-about-vulnerabilities/privately-reporting-a-security-vulnerability)
  (pestaña *Security* del repositorio, si está habilitado) o directamente al equipo docente de
  STDevSec Academy por los canales del curso.
- Incluye: componente afectado, pasos para reproducir, impacto estimado y, si la tienes, una propuesta
  de corrección.
- No explotes la vulnerabilidad más allá de lo necesario para demostrarla, y nunca fuera del entorno
  local del proyecto.

> **Pendiente:** a fecha de la última auditoría ([bootstrap-audit.md](docs/audits/bootstrap-audit.md)),
> el *Private Vulnerability Reporting* **no está habilitado** en el repositorio y no existe un canal de
> contacto oficial de seguridad. Mientras tanto, contacta en privado con el equipo docente.
> STDevSec Academy debe habilitar uno de los dos antes de abrir el repositorio a contribuciones.

### Controles automáticos de GitHub

Se recomienda activar en *Settings → Code security* (gratuitos en repositorios públicos):
*Secret scanning*, *Push protection*, *Dependabot alerts* y *Private vulnerability reporting*.
Su activación requiere permisos de administración y está pendiente de decisión.

## 5. Revisión de dependencias

- Toda dependencia nueva se **declara y justifica** en el PR.
- Usa versiones explícitas y mantenidas; evita versiones `SNAPSHOT`, rangos abiertos o librerías abandonadas.
- Prefiere las capacidades de Spring Boot, Angular y la librería estándar antes que añadir dependencias.
- Revisa las dependencias con las herramientas disponibles (p. ej. `npm audit`, Dependabot, OWASP
  Dependency-Check) cuando se integren en el pipeline.

## 6. Autorización en el backend

- **La autorización se aplica siempre en el backend.** Ocultar un botón en Angular no es control de acceso.
- Cada endpoint debe declarar qué roles o permisos lo pueden invocar; por defecto se **deniega**.
- Verifica la **propiedad del recurso** (p. ej. que un usuario solo vea los tickets que le corresponden),
  no solo el rol.
- Los servicios validan el token recibido; no confían en cabeceras o identificadores enviados por el cliente
  sin verificarlos.

## 7. Validación de entradas

- Toda entrada externa (cuerpo, parámetros, cabeceras) se valida en el backend con **Bean Validation**.
- Usa consultas parametrizadas / JPA; nunca concatenes entradas en SQL.
- Las respuestas de error no deben exponer trazas de pila, consultas SQL ni detalles internos.
- En el frontend, la validación con Reactive Forms mejora la experiencia de usuario, pero **no** sustituye
  la validación del backend.

## 8. Datos de prueba

- Usa **solo datos ficticios** (nombres, correos con dominios reservados como `example.com`, teléfonos falsos).
- **Prohibido** usar datos personales reales de personas o clientes, ni copiar datos de sistemas reales.
- Las credenciales de usuarios de prueba no se escriben en el repositorio; se documenta cómo generarlas localmente.
- Los datos de prueba no deben cargarse automáticamente en entornos que no sean locales o de pruebas.
