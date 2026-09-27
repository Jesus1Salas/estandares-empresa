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
├── mcp/               Configuraciones MCP de uso general (JSON, se fusionan)
├── catalog.json       Índice: qué hay, versión, categoría, destino, descripción
├── CHANGELOG.md       Historial de cambios de los estándares
└── README.md          Este archivo
```

> Según la guía (Opción A), este repo puede crecer con `plantillas/`.
> Hoy contiene `steering/`, `skills/`, `agents/`, `hooks/` y `mcp/`.

## Contenido por departamento

**39 artefactos** en total: 24 steering, 7 skills, 3 agentes, 3 hooks y 2 MCP.

| Departamento | Total | Steering | Skills | Agentes | Hooks | MCP |
|--------------|:-----:|:--------:|:------:|:-------:|:-----:|:---:|
| Comercial | 11 | 7 | 3 | 1 | — | — |
| QA | 12 | 8 | 3 | 1 | — | — |
| Desarrollo | 3 | 2 | — | 1 | — | — |
| Global / Técnico | 10 | 4 | 1 | — | 3 | 2 |
| Infraestructura | 2 | 2 | — | — | — | — |
| Gestión de proyectos | 1 | 1 | — | — | — | — |
| **Total** | **39** | **24** | **7** | **3** | **3** | **2** |

### Comercial (11)

- **Steering (7):** perfil de empresa y servicios · tono y estilo comercial ·
  estructura de propuestas · política de precios y cotización · legal y
  cumplimiento · datos y privacidad · glosario y nomenclatura.
- **Skills (3):** propuesta comercial · cotización · minuta de reunión.
- **Agente (1):** agente comercial (orquesta las skills comerciales).

### QA (12)

- **Steering (8):** estrategia de pruebas · definición de hecho y aceptación ·
  convenciones de casos de prueba · gestión de defectos · automatización de
  pruebas · datos y entornos de prueba · criterios de entrada/salida · QA no
  funcional.
- **Skills (3):** generador de casos de prueba · reporte de ejecución · matriz de
  trazabilidad.
- **Agente (1):** agente QA (orquesta las skills de QA).

### Desarrollo (3)

- **Steering (2):** ingeniería de datos / ETL · calidad de datos.
- **Agente (1):** revisor de datos (audita el código de ingeniería de datos).

### Global / Técnico (10)

- **Steering (4):** patrones de programación · commits y pull requests ·
  arquitectura AWS · seguridad y DevSecOps.
- **Skill (1):** manual técnico-operativo.
- **Hooks (3):** cargar contexto (SessionStart) · generar contexto de sesión
  (Stop) · log de trabajo (PostToolUse).
- **MCP (2):** GitHub (desarrollo) · DrawIO (diagramas). De uso general, se
  fusionan en `.kiro/settings/mcp.json` y se bajan bajo demanda.

### Infraestructura (2)

- **Steering (2):** CloudFormation · diagramas de arquitectura.

### Gestión de proyectos (1)

- **Steering (1):** gestión de proyectos (Scrum).

El detalle completo (id, versión, rutas, obligatoriedad) vive en
[`catalog.json`](./catalog.json).

## El catálogo (`catalog.json`)

Es el índice legible del repo; el consultor lo lee **primero** para saber qué
ofrecer o bajar. Cada artefacto se identifica por `id` (`<tipo>.<ámbito>.<nombre>`)
y describe:

| Campo | Significado |
|-------|-------------|
| `id` | Identificador estable, clave del artefacto |
| `tipo` | `steering` \| `skill` \| `agent` \| `hook` \| `mcp` (a futuro `plantilla`, `doc`) |
| `ambito` | `global` \| `infra` \| `desarrollo` \| `qa` \| `comercial` \| `pmo` |
| `obligatoriedad` | `obligatorio` \| `recomendado` \| `opcional` |
| `version` | Versión semántica propia del artefacto |
| `ruta` | Dónde está en este repo |
| `destino` | Dónde aterriza en el proyecto consumidor (`.kiro/...`) |
| `materializable` | `true` / `false` si se copia al proyecto; `"merge"` para `mcp` (se fusiona en `.kiro/settings/mcp.json`, no se sobrescribe) |
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
