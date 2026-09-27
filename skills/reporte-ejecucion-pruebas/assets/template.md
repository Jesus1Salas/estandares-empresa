<!--
PLANTILLA: Reporte de Ejecución de Pruebas / QA Sign-off
Instrucciones para el agente:
- Rellena con los RESULTADOS reales de la ronda (aportados por el usuario o de un
  informe de ejecución). No inventes resultados ni bugs.
- Aplica `criterios-entrada-salida` para el veredicto y `gestion-defectos` para
  clasificar por severidad.
- Registra siempre entorno y build/versión (ver `datos-entornos-prueba`).
- Los números deben ser coherentes (ejecutados = pasados + fallidos + bloqueados).
- Elimina estos comentarios HTML en la salida final.
-->

---
titulo: "Reporte de Ejecución de Pruebas"
proyecto: "{{PROYECTO}}"
ciclo: "{{CICLO}}"
entorno: "{{ENTORNO}}"
build: "{{BUILD}}"
fecha: "{{FECHA}}"
autor: "{{AUTOR}}"
clasificacion: "Uso interno"
---

# Reporte de Ejecución de Pruebas — {{PROYECTO}}

**Ciclo/Ronda:** {{CICLO}} · **Entorno:** {{ENTORNO}} · **Build/Versión:** {{BUILD}}
**Fecha:** {{FECHA}} · **Autor:** {{AUTOR}}

## 1. Resumen ejecutivo

{{RESUMEN_EJECUTIVO}}
<!-- 3-6 líneas: qué se probó, cómo salió y la recomendación. Se lee solo. -->

## 2. Alcance de la ronda

{{ALCANCE}}
<!-- Funcionalidades/módulos cubiertos y lo que quedó fuera. -->

## 3. Resultados de ejecución

| Métrica | Valor |
|---------|------:|
| Casos planificados | {{PLANIFICADOS}} |
| Casos ejecutados | {{EJECUTADOS}} |
| Aprobados (pass) | {{PASS}} |
| Fallidos (fail) | {{FAIL}} |
| Bloqueados | {{BLOCKED}} |
| No ejecutados | {{NO_EJEC}} |
| **% Ejecución** | **{{PCT_EJEC}}** |
| **% Aprobación (pass rate)** | **{{PASS_RATE}}** |

## 4. Detalle por módulo / suite

| Módulo / Suite | Ejec. | Pass | Fail | Bloq. | Pass rate |
|----------------|------:|-----:|-----:|------:|----------:|
{{TABLA_POR_MODULO}}

## 5. Defectos

**Resumen por severidad:**

| Severidad | Abiertos | Cerrados en la ronda |
|-----------|---------:|---------------------:|
| Crítica | {{CRIT_ABIERTOS}} | {{CRIT_CERRADOS}} |
| Alta | {{ALTA_ABIERTOS}} | {{ALTA_CERRADOS}} |
| Media | {{MEDIA_ABIERTOS}} | {{MEDIA_CERRADOS}} |
| Baja | {{BAJA_ABIERTOS}} | {{BAJA_CERRADOS}} |

**Defectos destacados (abiertos):**

| ID | Título | Severidad | Prioridad | Estado |
|----|--------|-----------|-----------|--------|
{{TABLA_DEFECTOS}}
<!-- Prioriza críticos/altos. IDs según `gestion-defectos` (BUG-<PROYECTO>-<NNN>). -->

## 6. Evaluación de criterios de salida

<!-- Evalúa contra `criterios-entrada-salida`; marca cada uno con su estado real. -->

| Criterio de salida | Objetivo | Real | ¿Cumple? |
|--------------------|----------|------|----------|
| % casos ejecutados | {{OBJ_EJEC}} | {{PCT_EJEC}} | {{CUMPLE_EJEC}} |
| Pass rate | {{OBJ_PASS}} | {{PASS_RATE}} | {{CUMPLE_PASS}} |
| Bugs Críticos/Altos abiertos | 0 | {{CRIT_ALTA_TOTAL}} | {{CUMPLE_BUGS}} |
| Cobertura de requisitos | 100% | {{COBERTURA}} | {{CUMPLE_COB}} |

## 7. Riesgos y observaciones

{{RIESGOS}}
<!-- Riesgos residuales, deuda de pruebas, áreas no cubiertas. -->

## 8. Recomendación (QA sign-off)

{{VEREDICTO}}
<!-- "Se recomienda liberar" solo si se cumplen los criterios de salida. Si no,
"No se recomienda liberar" con los criterios incumplidos y el riesgo. Si el
negocio decide liberar igual, deja constancia de la excepción y quién la aprueba. -->
