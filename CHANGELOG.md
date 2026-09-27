# Changelog — Estándares de la empresa

Historial de cambios del repositorio de estándares (`estandares-empresa`), la
fuente de verdad de convenciones, skills y configuraciones de la consultora.

El formato sigue [Keep a Changelog](https://keepachangelog.com/es/1.1.0/) y los
artefactos usan **versionado semántico** individual en `catalog.json`:

- **MAJOR**: cambio incompatible que obliga a migrar.
- **MINOR**: añade contenido de forma retrocompatible.
- **PATCH**: correcciones de redacción o ajustes menores.

> Cada PR que agrega o modifica un artefacto debe subir su `version` en
> `catalog.json` y añadir una entrada aquí.

---

## [1.2.0] — 2026-09-26

Añade configuraciones MCP de uso general del desarrollador (39 artefactos:
24 steering + 7 skills + 3 agentes + 3 hooks + 2 MCP).

### Añadido

**Configuraciones MCP** (tipo `mcp`, se **fusionan** en `.kiro/settings/mcp.json`)
- `mcp.global.github` (1.0.0) — GitHub MCP para desarrollo (repos, PRs, issues); token por variable de entorno `GITHUB_PERSONAL_ACCESS_TOKEN`.
- `mcp.global.drawio` (1.0.0) — DrawIO MCP para generar/editar diagramas.

### Notas

- Estos MCP son de **uso general del desarrollador** y se materializan **bajo
  demanda** (uno u otro, o ambos). Se **fusionan**, no sobrescriben, el
  `.kiro/settings/mcp.json`.
- Son **independientes** del MCP `estandares` (solo lectura) que usa el agente
  consultor para apuntar al repo de estándares; ese se instala con el Power
  `power-consultor-estandares` y no se mezcla con el `github` de desarrollo.
- El token de GitHub nunca va en texto plano: variable de entorno.

---

## [1.1.0] — 2026-09-26

Añade agentes y hooks al catálogo (37 artefactos: 24 steering + 7 skills +
3 agentes + 3 hooks).

### Añadido

**Agentes**
- `agent.comercial.agente-comercial` (1.0.0) — Orquesta las skills comerciales (propuesta, cotización, minuta).
- `agent.qa.agente-qa` (1.0.0) — Orquesta las skills de QA y mantiene la trazabilidad requisito/caso/defecto.
- `agent.datos.revisor-datos` (1.0.0) — Auditor de solo lectura del código de ingeniería de datos (estructura, patrones, calidad, seguridad); emite informe con veredicto.

**Hooks**
- `hook.global.cargar-contexto` (1.0.0) — SessionStart: vuelca el contexto de sesiones previas al iniciar.
- `hook.global.generar-contexto-sesion` (1.0.0) — Stop: resume la sesión y la acumula en el histórico de contexto.
- `hook.global.log-trabajo` (1.0.0) — PostToolUse: registra cada escritura en un log de trabajo con timestamp.

### Notas

- Los agentes dependen de que las skills que orquestan estén materializadas en el
  proyecto consumidor (`resources` con `skill://.kiro/skills/...`).
- Los hooks de contexto/log producen bitácora **local** (`.kiro/contexto/`,
  `.kiro/CHANGELOG-trabajo.md`), que no se versiona en el repo consumidor.

---

## [1.0.0] — 2026-09-26

Versión inicial del catálogo con 31 artefactos (24 steering + 7 skills).

### Añadido

**Steering — Global / Técnico (AWS + Python)**
- `steering.global.programming-patterns` (1.0.0) — Convenciones de codificación Python/PySpark.
- `steering.global.commits-and-pull-requests` (1.0.0) — Conventional Commits y PRs.
- `steering.global.arquitectura-aws` (1.0.0) — Well-Architected, región us-east-1, dev/qa/prod, tags.
- `steering.global.seguridad-devsecops` (1.0.0) — Secretos, IAM, cifrado KMS, SAST/SCA, vulnerabilidades (7/30/90).
- `steering.infra.cloudformation` (1.0.0) — Plantillas CloudFormation en YAML.
- `steering.infra.architecture-diagrams` (1.0.0) — Diagramas de arquitectura AWS.

**Steering — Datos**
- `steering.datos.ingenieria-datos` (1.0.0) — ETL/PySpark, capas raw/curated/data-product, Glue Catalog.
- `steering.datos.calidad-datos` (1.0.0) — Dimensiones de calidad, validaciones y cuarentena.

**Steering — Gestión de proyectos**
- `steering.pmo.gestion-proyectos` (1.0.0) — Scrum, estados, reportes, riesgos y change requests.

**Steering — Comercial**
- `steering.comercial.perfil-empresa-servicios` (1.0.0) — Perfil y catálogo de servicios.
- `steering.comercial.tono-estilo-comercial` (1.0.0) — Voz de marca comercial.
- `steering.comercial.estructura-propuestas` (1.0.0) — Estructura de propuestas/SOW.
- `steering.comercial.politica-precios-cotizacion` (1.0.0) — Política de precios y cotización.
- `steering.comercial.legal-cumplimiento-comercial` (1.0.0) — Legal y cumplimiento comercial.
- `steering.comercial.datos-privacidad-comercial` (1.0.0) — Privacidad de clientes y prospectos.
- `steering.comercial.glosario-nomenclatura-comercial` (1.0.0) — Glosario y nombres de archivo.

**Steering — QA**
- `steering.qa.estrategia-pruebas` (1.0.0) — Estrategia, pirámide y cobertura (pytest/moto).
- `steering.qa.definicion-hecho-aceptacion` (1.0.0) — DoD y criterios de aceptación.
- `steering.qa.convenciones-casos-prueba` (1.0.0) — Anatomía y nomenclatura de casos.
- `steering.qa.gestion-defectos` (1.0.0) — Reporte de bugs, severidad y SLA.
- `steering.qa.automatizacion-pruebas` (1.0.0) — Frameworks AWS+Python y CI.
- `steering.qa.datos-entornos-prueba` (1.0.0) — Datos sintéticos y entornos dev/qa/prod.
- `steering.qa.criterios-entrada-salida` (1.0.0) — Test gates (95% ejecución / 90% pass).
- `steering.qa.qa-no-funcional` (1.0.0) — Performance, seguridad y accesibilidad.

**Skills**
- `skill.global.manual-tecnico-operativo` (1.0.0) — Manual Técnico-Operativo (MD + PDF).
- `skill.comercial.propuesta-comercial` (1.0.0) — Propuesta/SOW (MD + PDF).
- `skill.comercial.cotizacion-comercial` (1.0.0) — Cotización (MD + PDF).
- `skill.comercial.minuta-reunion-comercial` (1.0.0) — Minuta de reunión (MD + PDF).
- `skill.qa.generador-casos-prueba` (1.0.0) — Casos de prueba (MD + PDF).
- `skill.qa.reporte-ejecucion-pruebas` (1.0.0) — Reporte de ejecución / QA sign-off (MD + PDF).
- `skill.qa.matriz-trazabilidad` (1.0.0) — Matriz de trazabilidad RTM (MD + PDF).

### Notas

- Contexto tecnológico transversal: **AWS + Python**, entornos **dev/qa/prod**,
  región **us-east-1**, IaC en **CloudFormation**, metodología **Scrum**.
- Varios artefactos conservan marcadores `⚠️ COMPLETAR` para datos propios de la
  consultora (identidad, tarifas, rol aprobador de QA, ruta documental de proyectos).

[1.2.0]: #120--2026-09-26
[1.1.0]: #110--2026-09-26
[1.0.0]: #100--2026-09-26
