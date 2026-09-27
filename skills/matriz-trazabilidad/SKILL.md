---
name: matriz-trazabilidad
description: >-
  Genera una matriz de trazabilidad de requisitos (RTM) que mapea cada requisito o
  criterio de aceptación a sus casos de prueba y al resultado de ejecución,
  calculando la cobertura y detectando requisitos sin cobertura y casos huérfanos.
  Produce salida en Markdown y en PDF con logo y tipografía corporativa. Úsala
  cuando el usuario pida "genera la matriz de trazabilidad", "RTM", "cobertura de
  requisitos" o similar.
version: 1.0.0
---

# Skill: Matriz de Trazabilidad de Requisitos (RTM)

Esta skill construye una **matriz de trazabilidad** que conecta requisitos ↔
casos de prueba ↔ resultados, y evalúa la cobertura. Se genera en dos formatos:

- **Markdown**: `qa/trazabilidad/matriz-trazabilidad-<proyecto>-<fecha>.md`
- **PDF**: mismo nombre con extensión `.pdf` (portada, logo y tipografía).

El principio rector es **trazar lo que existe, no inventar cobertura**. Un
requisito sin caso de prueba es un **hallazgo** (SIN COBERTURA), no algo a
completar con un caso ficticio.

## Cuándo se activa

Cuando el usuario pida una matriz de trazabilidad (RTM), un análisis de cobertura
de requisitos o vincular requisitos con casos de prueba y resultados.

## Entrada requerida

- **Requisitos / criterios de aceptación** (con sus IDs), y
- **Casos de prueba** (con sus IDs y a qué requisito cubren), y opcionalmente
- **Resultados de ejecución** y **defectos** asociados.

Pueden venir de archivos ya generados por las skills `generador-casos-prueba` y
`reporte-ejecucion-pruebas`, o aportados por el usuario. Si falta el mapeo
requisito↔caso, dedúcelo de las referencias de los casos; lo que no se pueda
trazar se marca como hallazgo, no se inventa.

## Steerings que debe respetar

- `estrategia-pruebas` — cobertura de requisitos al 100% como objetivo.
- `convenciones-casos-prueba` — IDs de casos (`TC-<MODULO>-<NNN>`) y su vínculo al
  criterio.
- `definicion-hecho-aceptacion` — los criterios de aceptación como unidad a trazar.
- `gestion-defectos` — IDs de defectos asociados a casos fallidos.

## Artefactos incluidos

- `assets/template.md` — plantilla de la RTM (resumen de cobertura + matriz +
  hallazgos).
- `assets/pdf-style.css` — estilos corporativos (cobertura cubierto/sin cobertura).
- `assets/logo.svg` — logo por defecto (reemplazable).

Esta skill **no incluye scripts**: el mapeo y el PDF los produce el agente.

## Procedimiento (seguir en orden)

### Paso 0 — Reunir requisitos y casos
Obtén la lista de requisitos/criterios (con IDs) y los casos de prueba (con IDs y
el requisito que cubren). Si hay resultados de ejecución, inclúyelos. Normaliza
los IDs para que el cruce sea consistente.

### Paso 1 — Preparar carpeta de salida
Crea `qa/trazabilidad/` si no existe.

### Paso 2 — Construir la matriz
Copia `assets/template.md` al archivo de salida y complétalo:

1. Nombra el archivo `matriz-trazabilidad-<proyecto>-<AAAA-MM-DD>.md`.
2. Por cada **requisito/criterio**, lista los **casos** que lo cubren, su tipo, el
   **último resultado** (Pass/Fail/Bloqueado/No ejecutado/N/A) y los **defectos**
   asociados si falló.
3. Marca la **cobertura** de cada requisito: "Cubierto" o "SIN COBERTURA".
4. Calcula el **resumen de cobertura**: total de requisitos, cubiertos, sin
   cobertura, % de cobertura, total de casos y casos huérfanos.
5. Lista aparte los **requisitos sin cobertura** (hallazgos) y los **casos
   huérfanos** (sin requisito). Si no hay, indica "Ninguno".
6. No inventes cobertura ni resultados; lo que no se pueda trazar se reporta.

### Paso 3 — Generar el PDF (lo hace el agente)
1. HTML con **portada** (logo data URI, título "Matriz de Trazabilidad", proyecto,
   referencia, fecha, clasificación) y **cuerpo** (Markdown a HTML).
2. Aplica `assets/pdf-style.css` (usa las clases de cobertura y resultado).
3. Como la matriz puede ser ancha, considera orientación horizontal si hay muchas
   columnas; exporta a PDF con `create_artifact` (`kind: pdf`); pie con numeración.
4. Guarda el PDF junto al Markdown.

Si el entorno impide el PDF, entrega el Markdown e infórmalo (alternativa:
`pandoc <archivo>.md -o <archivo>.pdf`).

### Paso 4 — Verificar
- Confirma que existen el `.md` y (si fue posible) el `.pdf`.
- No deben quedar marcadores `{{...}}` sin sustituir.
- Verifica coherencia: el % de cobertura concuerda con los conteos; reporta los
  requisitos SIN COBERTURA y los casos huérfanos como acciones a resolver.

## Personalización

- **Logo:** reemplaza `assets/logo.svg`.
- **Colores/tipografía:** variables CSS al inicio de `assets/pdf-style.css`.
- **Ruta de salida:** por defecto `qa/trazabilidad/`.
