<!--
PLANTILLA DE ESPECIFICACIÓN — ST ServiceHub
Copia este archivo a docs/specs/<servicio>/<issue-id>-<descripcion>.md
Sustituye todo lo que está entre <...> y elimina los comentarios de ayuda.
Escribe "N/A" (con una justificación breve) en las secciones que no apliquen.
Proceso: INTENT → SPEC → PLAN → TASKS → IMPLEMENT → VERIFY
-->

# SPEC-<issue-id>: <Título>

| Campo | Valor |
|-------|-------|
| **Identificador** | SPEC-<issue-id> |
| **Issue** | #<issue-id> |
| **Servicio(s)** | <identity-service / client-service / ticket-service / ... / frontend> |
| **Autor(es)** | <nombres> |
| **Revisor(es)** | <nombres> |
| **Estado** | Borrador / En revisión / Aprobada / Implementada |
| **Última actualización** | AAAA-MM-DD |

---

## 1. INTENT

### 1.1 Problema de negocio

<¿Qué problema existe hoy? ¿A quién afecta? ¿Qué pasa si no se resuelve?>

### 1.2 Historia de usuario

> Como **<rol>**,
> quiero **<acción>**,
> para **<beneficio>**.

---

## 2. SPEC

### 2.1 Alcance

<Qué incluye exactamente esta funcionalidad.>

- <...>

### 2.2 Exclusiones

<Qué NO incluye (se tratará en otro Issue o no se hará).>

- <...>

### 2.3 Reglas de negocio

| ID | Regla |
|----|-------|
| RN-1 | <...> |

### 2.4 Contrato API

<!-- Describe el contrato aquí y referencia el archivo OpenAPI en docs/api/. -->

- **Método y ruta:** `<MÉTODO> /api/<recurso>`
- **Archivo OpenAPI:** `docs/api/<servicio>.yaml`

**Petición:**

```json
{
}
```

**Respuestas:**

| Código | Situación | Cuerpo |
|--------|-----------|--------|
| <2xx> | <éxito> | <...> |
| 400 | Entrada inválida | <formato de error> |
| 401 | No autenticado | <...> |
| 403 | Sin permiso | <...> |

**Compatibilidad:** <¿Rompe algún contrato existente? ¿A qué consumidores afecta?>

### 2.5 Seguridad

- **Quién puede invocarla (roles/permisos):** <...>
- **Reglas de propiedad del recurso:** <p. ej. un cliente solo ve sus propios tickets>
- **Validación de entradas:** <campos, formatos, longitudes>
- **Datos sensibles:** <¿Se manejan? ¿Cómo se protegen? ¿Qué no debe registrarse en logs?>

### 2.6 Criterios de aceptación

<!-- Verificables. Cada criterio debe corresponder al menos a una prueba. -->

- [ ] **CA-1:** Dado <contexto>, cuando <acción>, entonces <resultado>.
- [ ] **CA-2:** Dado <contexto>, cuando <acción>, entonces <resultado>.

### 2.7 Casos límite

| Caso | Comportamiento esperado |
|------|-------------------------|
| <entrada vacía / duplicada / inexistente / sin permiso / servicio dependiente caído> | <...> |

---

## 3. PLAN

### 3.1 Plan técnico

<!-- Cómo se implementará en cada capa de la vertical slice. -->

- **Frontend:** <componentes, rutas, formularios, servicios HTTP>
- **API:** <controlador, DTO, validaciones>
- **Dominio:** <servicios, reglas>
- **Persistencia:** <entidades, cambios de esquema/migraciones>
- **Integraciones:** <llamadas a otros servicios>
- **Decisiones relevantes:** <¿Requiere un ADR?>

---

## 4. TASKS

- [ ] T1: <...>
- [ ] T2: <...>

---

## 5. IMPLEMENT

<!-- Registro breve de la implementación. No copies código aquí: enlaza commits y PR. -->

- **Rama:** `feature/<issue-id>-<descripcion>`
- **Commits principales:** <hash — mensaje>
- **Desviaciones de la spec:** <qué cambió respecto a lo planificado y por qué (y si se actualizó la spec)>
- **Decisiones tomadas durante la implementación:** <...>
- **Uso de IA:** <herramienta, para qué, qué revisaste o descartaste — ver docs/learning/ai-development-policy.md>

---

## 6. VERIFY

### 6.1 Pruebas

| Tipo | Qué se prueba | Criterio cubierto |
|------|---------------|-------------------|
| Unitaria | <...> | CA-1 |
| Integración | <...> | CA-2 |
| Frontend | <...> | <...> |
| Manual | <...> | <...> |

### 6.2 Evidencias

<!-- Salida de pruebas, capturas, peticiones/respuestas, enlace al PR y a la ejecución de CI.
     Sin tokens, contraseñas ni datos personales reales. -->

- PR: #<id>
- <...>

### 6.3 Riesgos

| Riesgo | Impacto | Mitigación |
|--------|---------|------------|
| <...> | <alto/medio/bajo> | <...> |

---

## 7. Definition of Done

- [ ] La spec está aprobada y refleja lo implementado.
- [ ] Todos los criterios de aceptación se cumplen y tienen al menos una prueba.
- [ ] La sección IMPLEMENT registra las desviaciones respecto a la spec.
- [ ] Los casos límite están cubiertos por pruebas o justificados.
- [ ] El contrato OpenAPI está actualizado (si aplica).
- [ ] La autorización y la validación se aplican en el backend.
- [ ] No hay secretos ni datos personales reales.
- [ ] El PR está revisado y aprobado, y la CI pasa (cuando exista).
- [ ] El uso significativo de IA está declarado en el PR.
- [ ] Puedo explicar, modificar y defender todo el código entregado.
