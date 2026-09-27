# Conventional Commits & Pull Requests

Convenciones para redactar commits y pull requests. Aplica siempre que crees un
commit o abras/actualices un pull request en este proyecto.

## Conventional Commits

### Formato

```
<tipo>(<ámbito opcional>): <descripción breve>

<cuerpo opcional>

<footer opcional>
```

Reglas del **asunto** (primera línea):

- Escríbelo en **imperativo** y en minúscula: "agrega", no "agregado"/"agregando".
- Sin punto final. Máximo ~72 caracteres.
- El `<ámbito>` es opcional e indica la zona afectada (`auth`, `api`, `ingesta`,
  `infra`, `docs`...).

### Tipos permitidos

| Tipo | Uso |
|------|-----|
| `feat` | Nueva funcionalidad |
| `fix` | Corrección de un error |
| `docs` | Solo documentación |
| `style` | Formato/estilo sin cambio de lógica (espacios, comas) |
| `refactor` | Cambio de código que no corrige bug ni añade feature |
| `perf` | Mejora de rendimiento |
| `test` | Añadir o corregir pruebas |
| `build` | Sistema de build o dependencias |
| `ci` | Configuración/scripts de CI |
| `chore` | Tareas de mantenimiento sin impacto en producción |
| `revert` | Revierte un commit anterior |

### Ejemplos

Commit simple:

```
feat(ingesta): agrega carga incremental de ventas diarias
```

```
fix(api): corrige cálculo de total cuando el descuento es nulo
```

```
docs(readme): documenta variables de entorno de despliegue
```

Commit con cuerpo y referencia a ticket:

```
fix(auth): evita expiración prematura del token de sesión

El TTL se calculaba en segundos pero se comparaba en milisegundos, lo que
cerraba la sesión antes de tiempo. Se unifica la unidad a milisegundos.

Refs: ORV-482
```

### Breaking changes

Indica el cambio incompatible con `!` tras el tipo/ámbito y un footer
`BREAKING CHANGE:` describiendo la migración.

```
feat(api)!: renombra el campo "usuario" a "usuarioId" en la respuesta

BREAKING CHANGE: los consumidores deben leer "usuarioId" en vez de "usuario".
```

### Buenas prácticas de commit

- Un commit = un cambio lógico coherente; evita commits "cajón de sastre".
- El asunto responde a "¿qué hace este commit?"; el cuerpo, al "¿por qué?".
- No mezcles refactor y feature en el mismo commit.
- Referencia el ticket en el footer (`Refs:`, `Closes:`), no en el asunto.

## Pull Requests

### Título

Usa el mismo formato Conventional Commits que un commit; será el resumen del PR.

```
feat(ingesta): soporte de carga incremental para ventas
```

Manténlo bajo ~70 caracteres.

### Descripción

Estructura la descripción con estas secciones:

```markdown
## Resumen
Qué cambia y por qué, en 1-3 frases.

## Cambios
- Punto 1 del cambio
- Punto 2 del cambio

## Cómo se probó
- Pruebas ejecutadas / pasos de verificación / evidencia.

## Notas / Riesgos
- Impactos, breaking changes, feature flags, pasos de despliegue.

## Tickets
Closes ORV-482
```

### Ejemplo de descripción de PR

```markdown
## Resumen
Agrega la carga incremental de ventas para reducir el tiempo del proceso diario
de ~40 min a ~6 min al procesar solo particiones nuevas.

## Cambios
- Nuevo modo `incremental` en el job de ingesta (parámetro `--modo`).
- Lectura de la marca de agua (watermark) desde SSM Parameter Store.
- Pruebas unitarias del cálculo de particiones a procesar.

## Cómo se probó
- `pytest tests/ingesta/` en verde (12 casos).
- Ejecución en `dev` sobre 3 días de datos: resultados idénticos al modo full.

## Notas / Riesgos
- Requiere el parámetro SSM `/ventas/ingesta/watermark` (creado en este PR).
- Sin breaking changes; el modo `full` sigue siendo el valor por defecto.

## Tickets
Closes ORV-482
```

### Buenas prácticas de PR

- **Pequeños y enfocados**: un PR resuelve un objetivo; si crece demasiado,
  divídelo. Idealmente menos de ~400 líneas de cambio revisables.
- **Rama por cambio**: nunca directo a `main`/`master`. Nombra la rama con el
  tipo y una descripción corta, p. ej. `feat/ingesta-incremental` o
  `fix/token-ttl`.
- **CI en verde** antes de solicitar revisión (build, lint y pruebas).
- **Autorreview** antes de pedir revisión: relee tu propio diff.
- **Vincula el ticket** con `Closes`/`Refs` para trazabilidad.
- **Sin secretos** en el diff (revisa `.env`, credenciales, tokens).
- Marca el PR como **Draft** mientras esté en progreso.
- Responde a los comentarios de revisión con un nuevo commit; evita
  `--force`/`--amend` sobre commits ya publicados salvo acuerdo del equipo.
