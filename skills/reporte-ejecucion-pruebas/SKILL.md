---
name: reporte-ejecucion-pruebas
description: >-
  Genera un reporte de ejecución de pruebas (QA sign-off) de una ronda o ciclo:
  casos ejecutados, aprobados/fallidos/bloqueados, defectos por severidad,
  evaluación de criterios de salida y recomendación de liberar o no. Produce
  salida en Markdown y en PDF con logo y tipografía corporativa. Úsala cuando el
  usuario pida "genera el reporte de ejecución", "resumen de la ronda de pruebas",
  "QA sign-off" o similar.
version: 1.0.0
---

# Skill: Reporte de Ejecución de Pruebas (QA Sign-off)

Esta skill consolida los **resultados de una ronda de pruebas** en un reporte con
formato corporativo y una recomendación de liberación. Se genera en dos formatos:

- **Markdown**: `qa/reportes/reporte-ejecucion-<proyecto>-<ciclo>-<fecha>.md`
- **PDF**: mismo nombre con extensión `.pdf` (portada, logo y tipografía).

El principio rector es **reportar resultados reales, no inventarlos**. Los números
deben ser coherentes (ejecutados = pasados + fallidos + bloqueados) y el veredicto
se deriva objetivamente de los criterios de salida.

## Cuándo se activa

Cuando el usuario pida un reporte/resumen de ejecución de pruebas, un informe de
ciclo o un QA sign-off para decidir si se libera.

## Entrada requerida

Los **resultados de la ronda**: casos planificados/ejecutados, pass/fail/bloqueados
(por módulo si es posible), defectos abiertos por severidad, entorno y build. El
usuario puede aportarlos pegados, como archivo/export de la herramienta de gestión
de pruebas, o pidiendo que se derive de una matriz/casos ya generados. No inventes
resultados: si faltan, pídelos o márcalos `> ⚠️ PENDIENTE:`.

## Steerings que debe respetar

- `criterios-entrada-salida` — evaluación de criterios de salida y veredicto.
- `gestion-defectos` — clasificación de defectos por severidad/prioridad.
- `datos-entornos-prueba` — registrar entorno y build; enmascarar PII/secretos.
- `estrategia-pruebas` — contexto de niveles y cobertura.

## Artefactos incluidos

- `assets/template.md` — plantilla del reporte (resumen, resultados, defectos,
  criterios de salida, veredicto).
- `assets/pdf-style.css` — estilos corporativos (estados pass/fail, severidades,
  veredicto).
- `assets/logo.svg` — logo por defecto (reemplazable).

Esta skill **no incluye scripts**: la consolidación y el PDF los produce el agente.

## Procedimiento (seguir en orden)

### Paso 0 — Reunir resultados
Obtén los resultados de la ronda. Verifica coherencia numérica y calcula
% ejecución y pass rate. Si algo falta, pídelo o márcalo pendiente.

### Paso 1 — Preparar carpeta de salida
Crea `qa/reportes/` si no existe.

### Paso 2 — Redactar el reporte
Copia `assets/template.md` al archivo de salida y complétalo:

1. Nombra el archivo `reporte-ejecucion-<proyecto>-<ciclo>-<AAAA-MM-DD>.md`.
2. Rellena front-matter (proyecto, ciclo, entorno, build, fecha, autor).
3. Completa **resultados** (totales y por módulo) con números coherentes.
4. Resume **defectos** por severidad y lista los críticos/altos abiertos, con IDs
   según `gestion-defectos`.
5. Evalúa la tabla de **criterios de salida** contra `criterios-entrada-salida`,
   marcando cada uno como cumple/no cumple con el valor real.
6. Redacta la **recomendación**: "Se recomienda liberar" solo si se cumplen los
   criterios; si no, "No se recomienda liberar" indicando los incumplidos y el
   riesgo. Si el negocio libera con criterios incumplidos, registra la excepción y
   quién la aprueba.

### Paso 3 — Generar el PDF (lo hace el agente)
1. HTML con **portada** (logo data URI, título "Reporte de Ejecución de Pruebas",
   proyecto, ciclo, entorno, build, fecha, clasificación) y **cuerpo** (Markdown a
   HTML).
2. Aplica `assets/pdf-style.css` (usa las clases de severidad y de veredicto).
3. Exporta a PDF A4 con `create_artifact` (`kind: pdf`); pie con numeración.
4. Guarda el PDF junto al Markdown.

Si el entorno impide el PDF, entrega el Markdown e infórmalo (alternativa:
`pandoc <archivo>.md -o <archivo>.pdf`).

### Paso 4 — Verificar
- Confirma que existen el `.md` y (si fue posible) el `.pdf`.
- No deben quedar marcadores `{{...}}` sin sustituir.
- Verifica coherencia de los números y que el veredicto concuerde con los
  criterios de salida. Reporta los `⚠️ PENDIENTE`.

## Personalización

- **Logo:** reemplaza `assets/logo.svg`.
- **Colores/tipografía:** variables CSS al inicio de `assets/pdf-style.css`.
- **Ruta de salida:** por defecto `qa/reportes/`.
