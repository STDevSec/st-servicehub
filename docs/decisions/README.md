# Architecture Decision Records (ADR)

Un **ADR** documenta una decisión de arquitectura relevante: el contexto, la decisión tomada, las
alternativas consideradas y sus consecuencias. Sirve para que cualquier persona (incluidas futuras
generaciones del curso) entienda **por qué** el sistema es como es.

## Cuándo escribir un ADR

- Elegir o cambiar una tecnología, versión o motor de base de datos.
- Añadir o eliminar un servicio, o mover una responsabilidad entre servicios.
- Introducir una dependencia nueva entre servicios.
- Decidir un patrón transversal (autenticación, formato de errores, API Gateway, mensajería).
- Elegir la licencia del repositorio.

## Reglas

- Nombre del archivo: `ADR-NNNN-titulo-corto.md` (número correlativo de 4 dígitos).
- Un ADR **aceptado no se edita** en lo sustancial. Si la decisión cambia, se crea un ADR nuevo que lo
  **sustituye**, y el anterior se marca como `Sustituido por ADR-XXXX`.
- Estados posibles: `Propuesto`, `Aceptado`, `Rechazado`, `Obsoleto`, `Sustituido por ADR-XXXX`.
- Un ADR se propone mediante Pull Request y se acepta al integrarse tras la revisión.

## Índice

| ADR | Título | Estado |
|-----|--------|--------|
| [ADR-0001](ADR-0001-monorepo.md) | Utilizar un monorepositorio académico | Aceptado |

## Plantilla

Copia el siguiente bloque en un archivo nuevo:

```markdown
# ADR-NNNN: <Título de la decisión>

- **Estado:** Propuesto
- **Fecha:** AAAA-MM-DD
- **Autores:** <nombres o usuarios>
- **Issue relacionada:** #<id>

## Contexto

<¿Qué problema o necesidad motiva la decisión? ¿Qué restricciones existen?>

## Decisión

<La decisión, enunciada de forma clara y en presente: "Utilizaremos...".>

## Alternativas consideradas

1. **<Alternativa A>:** <ventajas / inconvenientes / por qué se descarta>
2. **<Alternativa B>:** <...>

## Consecuencias

**Positivas:**

- <...>

**Negativas / riesgos:**

- <...>

**Acciones derivadas:**

- <...>
```
