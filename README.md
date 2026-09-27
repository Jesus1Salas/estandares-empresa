# estandares-empresa

Repositorio central de **estándares de la consultora**: la **fuente de verdad** de
convenciones (steering), procedimientos (skills) y configuraciones que los
proyectos consultan y materializan mediante el consultor de estándares.

Contexto tecnológico transversal: **AWS + Python**, entornos **dev / qa / prod**,
región **us-east-1**, infraestructura como código en **CloudFormation** y
metodología **Scrum**.

> Este repo es de **solo lectura** desde los proyectos consumidores: se lee y se
> copia, nunca se escribe de vuelta. Todo cambio entra por **Pull Request** con
> revisión del equipo de plataforma/arquitectura.

---

## Estructura

```
estandares-empresa/
├── steering/          Convenciones y contexto persistente (Markdown)
├── skills/            Procedimientos repetibles (carpeta por skill)
├── agents/            Agentes dedicados reutilizables (Markdown)
├── hooks/             Automatizaciones por evento (JSON)
├── catalog.json       Índice: qué hay, versión, categoría, destino, descripción
├── CHANGELOG.md       Historial de cambios de los estándares
└── README.md          Este archivo
```

> Según la guía (Opción A), este repo puede crecer con `mcp/` y `plantillas/`.
> Hoy contiene `steering/`, `skills/`, `agents/` y `hooks/`.

## Contenido actual

- **24 steering** — global/técnico (AWS, seguridad, patrones, commits,
  CloudFormation, diagramas), datos (ingeniería y calidad), gestión de proyectos,
  comercial y QA.
- **7 skills** — manual técnico, comercial (propuesta, cotización, minuta) y QA
  (casos de prueba, reporte de ejecución, matriz de trazabilidad).
- **3 agentes** — comercial (orquesta skills comerciales), QA (orquesta skills de
  QA), revisor de datos (audita código de ingeniería de datos).
- **3 hooks** — cargar contexto (SessionStart), generar contexto de sesión (Stop)
  y log de trabajo (PostToolUse).

El detalle vive en [`catalog.json`](./catalog.json).

## El catálogo (`catalog.json`)

Es el índice legible del repo; el consultor lo lee **primero** para saber qué
ofrecer o bajar. Cada artefacto se identifica por `id` (`<tipo>.<ámbito>.<nombre>`)
y describe:

| Campo | Significado |
|-------|-------------|
| `id` | Identificador estable, clave del artefacto |
| `tipo` | `steering` \| `skill` \| `agent` \| `hook` (a futuro `mcp`, `plantilla`, `doc`) |
| `ambito` | `global` \| `infra` \| `datos` \| `qa` \| `comercial` \| `pmo` |
| `obligatoriedad` | `obligatorio` \| `recomendado` \| `opcional` |
| `version` | Versión semántica propia del artefacto |
| `ruta` | Dónde está en este repo |
| `destino` | Dónde aterriza en el proyecto consumidor (`.kiro/...`) |
| `materializable` | Si se puede copiar al proyecto |
| `descripcion` / `palabras_clave` | Ayudan a mapear una pregunta con el artefacto |

**Regla de oro:** ningún artefacto existe si no está en el catálogo. El catálogo se
actualiza en el mismo PR que agrega o cambia un artefacto.

## Cómo contribuir un estándar

1. Crea una rama: `feat/<ambito>-<nombre>` o `fix/<ambito>-<nombre>`.
2. Añade o edita el artefacto en `steering/` o `skills/`.
3. **Actualiza `catalog.json`**: agrega/edita su entrada y sube la `version` del
   artefacto (semántica: MAJOR incompatible, MINOR añade, PATCH corrige).
4. Añade una entrada en `CHANGELOG.md`.
5. Abre un **Pull Request** siguiendo `steering/commits-and-pull-requests.md`
   (Conventional Commits, PR pequeño y enfocado, descripción con resumen/cambios/
   cómo se probó).
6. Espera revisión del equipo de plataforma/arquitectura.

### Versionado

- **Por artefacto:** el campo `version` en `catalog.json`.
- **Del catálogo:** el campo `version` de nivel superior sube cuando cambia el
  conjunto (nuevos artefactos o cambios relevantes).

## Convenciones de los artefactos

- **Steering:** Markdown con front-matter `inclusion` (`always` o `fileMatch` con
  su `fileMatchPattern`). Nombre de archivo en `kebab-case`.
- **Skills:** carpeta `skills/<id>/` con `SKILL.md` (front-matter `name` /
  `description` / `version`), `README.md` y `assets/`.
- Los marcadores `⚠️ COMPLETAR` señalan datos que cada proyecto/consultora ajusta
  (identidad, tarifas, roles); no se rellenan con datos inventados.

## Uso desde un proyecto

Los proyectos no clonan este repo dentro de su código: instalan el **consultor de
estándares** (Power + Skill) que se conecta aquí vía MCP de solo lectura, responde
consultas citando la fuente y **materializa** artefactos a `.kiro/` cuando se
solicita. Ver la guía `opcion-a-consultor-estandares.md`.
