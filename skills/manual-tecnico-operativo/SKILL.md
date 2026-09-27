---
name: manual-tecnico-operativo
description: >-
  Genera un Manual Técnico-Operativo completo (10 secciones) para un proyecto de
  software, autocompletando el contenido a partir del análisis del repositorio.
  Produce salida en Markdown y en PDF con logos y tipografía corporativa.
  Úsala cuando el usuario pida "generar el manual técnico", "documentar el
  proyecto", "crear el manual técnico-operativo" o similar.
version: 1.0.0
---

# Skill: Manual Técnico-Operativo

Esta skill produce un **Manual Técnico-Operativo** de un proyecto de software con
un formato corporativo consistente. El manual se genera en dos formatos:

- **Markdown**: `docs/manual-tecnico-operativo.md`
- **PDF**: `docs/manual-tecnico-operativo.pdf` (con portada, logo y tipografía)

El principio rector es **autocompletar**: NO dejes placeholders vacíos. Cada
sección debe rellenarse investigando el repositorio real. Solo cuando un dato no
pueda inferirse tras investigar, marca `> ⚠️ PENDIENTE: <qué falta y por qué>`
para que una persona lo complete, en vez de inventar.

---

## Cuándo se activa

Actívala cuando el usuario solicite generar/actualizar el manual técnico u
operativo del proyecto, documentar el proyecto de forma formal, o preparar
documentación de entrega/handover.

---

## Artefactos incluidos en la skill

- `assets/template.md` — plantilla Markdown con las 10 secciones y su guía interna.
- `assets/pdf-style.css` — estilos corporativos para el PDF (tipografía, portada, colores).
- `assets/logo.svg` — logo por defecto (reemplazable por el del cliente).

Esta skill **no incluye scripts**. Tanto la investigación del repositorio como la
generación del PDF las realiza el propio agente con sus herramientas:

- **Investigación**: usa las herramientas de lectura/búsqueda de código y, cuando
  el repo sea grande o desconocido, delega en el sub-agente `context-gatherer`.
- **PDF**: el agente genera el PDF directamente (ver Paso 3), sin depender de
  Node ni de herramientas externas instaladas.

---

## Procedimiento de generación (seguir en orden)

### Paso 0 — Preparar carpeta de salida
Crea `docs/` en la raíz del repo si no existe.

### Paso 1 — Investigar el repositorio (autocompletado)
Antes de escribir nada, investiga el repositorio para obtener contenido real.
Hazlo tú mismo con las herramientas de lectura/búsqueda de código. Si el repo es
grande, desconocido o multi-módulo, **delega esta investigación en el sub-agente
`context-gatherer`** con un prompt que pida explícitamente lo que necesitas para
el manual (stack, arquitectura, despliegue, operación, seguridad). Trata su
resultado como tus propias lecturas; no repitas las búsquedas que ya hizo.

No te limites a hacer coincidencias de patrones: **razona** sobre el código para
inferir arquitectura, propósito y flujo de datos, no solo listar ficheros.

Recopila y deduce, como mínimo:

- **Identidad del proyecto**: nombre, versión y descripción (del manifiesto y/o
  del README).
- **Ficheros de manifiesto** por lenguaje para el stack y dependencias:
  `package.json`, `pom.xml`, `build.gradle`, `requirements.txt`,
  `pyproject.toml`, `go.mod`, `Cargo.toml`, `*.csproj`, `composer.json`.
- **README** y cualquier `docs/` existente: reutiliza descripción, objetivo,
  instrucciones de arranque.
- **Arquitectura**: carpetas `src/`, servicios, `docker-compose.yml`,
  `Dockerfile`, IaC (`terraform/`, `k8s/`, `helm/`), diagramas existentes.
- **Configuración/entorno**: `.env.example`, `config/`, `appsettings*.json`,
  `application*.yml`.
- **CI/CD**: `.github/workflows/`, `.gitlab-ci.yml`, `azure-pipelines.yml`,
  `Jenkinsfile`.
- **Seguridad**: cómo se manejan secretos, autenticación, dependencias de
  seguridad, `SECURITY.md`.
- **Git**: `git log --oneline -20`, tags/versión, autor(es) para control de
  versiones y créditos.
- **Tickets**: si hay referencias a Jira (p. ej. `ORV-123`) en commits o
  README, enlázalas en la sección de referencias.

### Paso 2 — Rellenar la plantilla
Copia `assets/template.md` a `docs/manual-tecnico-operativo.md` y completa las
**10 secciones** con lo investigado. Reglas:

1. Escribe en el idioma del proyecto (por defecto español).
2. Sustituye TODOS los marcadores `{{...}}` por contenido real.
3. Rellena la tabla de **Control de versiones** con la versión detectada (del
   manifiesto o tag git), fecha actual, y autor (del git config o commits).
4. Para la **arquitectura**, si no hay diagrama, genera uno en Mermaid a partir
   de los componentes detectados (servicios, base de datos, colas, front, APIs).
5. Para **despliegue** y **operación**, deriva los comandos reales de los
   scripts del manifiesto (`npm run ...`, `mvn ...`, `make ...`) y de los
   ficheros de CI. No inventes comandos que no existan.
6. Para **troubleshooting**, incluye errores plausibles según el stack y
   cualquier problema documentado en el README/issues.
7. Cuando un dato no exista, usa `> ⚠️ PENDIENTE: ...`. No inventes.

### Paso 3 — Generar el PDF (lo hace el agente)
El agente genera `docs/manual-tecnico-operativo.pdf` a partir del Markdown, sin
scripts ni dependencias externas. Procedimiento:

1. Construye un documento HTML con este contenido, en este orden:
   - Una **portada**: logo embebido, título "Manual Técnico - Operativo", nombre
     del proyecto, versión, fecha, autor y clasificación (toma estos datos del
     front-matter del Markdown).
   - El **cuerpo**: el Markdown convertido a HTML.
2. Aplica los estilos de `assets/pdf-style.css` (tipografía, colores, portada,
   tablas, código, avisos PENDIENTE).
3. Embebe el logo `assets/logo.svg` (o el que indique el usuario) como data URI,
   para que el PDF sea autocontenido.
4. Renderiza los diagramas **Mermaid** a SVG antes de exportar; si algún
   diagrama no puede renderizarse, déjalo como bloque de código (no bloquees la
   generación del PDF por eso).
5. Produce el PDF con la herramienta de artefactos disponible en la sesión
   (`create_artifact` con `kind: pdf`) o el mecanismo de exportación a PDF que
   tengas a mano, tamaño A4, con numeración de páginas en el pie.
6. Guarda el resultado en `docs/manual-tecnico-operativo.pdf`.

Si por alguna limitación del entorno no puedes producir el PDF, entrega igual el
Markdown completo e infórmalo al usuario, ofreciendo como alternativa manual
`pandoc docs/manual-tecnico-operativo.md -o docs/manual-tecnico-operativo.pdf`.

### Paso 4 — Verificar
- Confirma que existen ambos ficheros en `docs/`.
- Revisa que no queden marcadores `{{...}}` sin sustituir en el Markdown.
- Lista al usuario las secciones marcadas como `⚠️ PENDIENTE` para que las complete.

---

## Las 10 secciones del manual

1. **Introducción y descripción general** — qué es el proyecto, contexto.
2. **Objetivo** — qué problema resuelve y para quién.
3. **Alcance** — qué cubre y qué queda fuera.
4. **Necesidad de negocio** — justificación y valor.
5. **Prerrequisitos y dependencias** — runtimes, librerías, servicios, accesos.
6. **Arquitectura de la solución** — componentes, diagrama, flujo de datos.
7. **Guía de despliegue** — entornos, variables, comandos paso a paso.
8. **Operación y mantenimiento** — arranque/parada, logs, monitoreo, backups, cron.
9. **Troubleshooting / errores comunes** — tabla síntoma → causa → solución.
10. **Rollback y contingencia**, **Seguridad**, y **Glosario/Referencias** — cierre operativo.

> Nota: se listan como bloques 1–10 conservando la estructura del manual original
> (Introducción/Objetivo/Alcance/Necesidad → Implementación) y ampliándola con las
> secciones operativas y de seguridad solicitadas.

---

## Personalización

- **Logo del cliente**: reemplaza `assets/logo.svg` (admite SVG/PNG). El agente
  lo embebe en la portada del PDF.
- **Colores/tipografía**: edita `assets/pdf-style.css` (variables CSS al inicio).
- **Ruta de salida**: por defecto `docs/manual-tecnico-operativo.pdf`; indícale
  al agente otra ruta si la necesitas.
