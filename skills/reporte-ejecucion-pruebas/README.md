# Skill: Reporte de Ejecución de Pruebas (QA Sign-off)

Consolida los **resultados de una ronda de pruebas** en un reporte con
recomendación de liberación, y lo exporta en **Markdown** y **PDF** (con logo y
tipografía corporativa).

## Cómo se usa

Pídele al agente algo como:

- "Genera el reporte de ejecución del ciclo 2 del proyecto X"
- "Resume la ronda de pruebas: 120 casos, 108 pass, 8 fail, 4 bloqueados"
- "Haz el QA sign-off con estos resultados y dime si se puede liberar"

El agente activará la skill y seguirá el procedimiento del `SKILL.md`.

## Entrada requerida

Los **resultados de la ronda**: casos planificados/ejecutados, pass/fail/bloqueados
(por módulo si es posible), defectos abiertos por severidad, entorno y build.
Pueden venir pegados, como export de la herramienta de gestión de pruebas, o
derivarse de casos/matriz ya generados. Si faltan, el agente los pide.

## Qué hace, paso a paso

1. **Reúne resultados** y verifica coherencia (ejecutados = pass + fail + bloqueados);
   calcula % ejecución y pass rate.
2. **Redacta el reporte** en `qa/reportes/` con la plantilla `assets/template.md`.
3. **Evalúa los criterios de salida** y emite recomendación de liberar o no.
4. **Genera el PDF** con portada, logo y estilos.
5. **Verifica** coherencia numérica y que el veredicto concuerde con los criterios.

## Contenido del reporte

Resumen ejecutivo · Alcance · Resultados de ejecución (totales y por módulo) ·
Defectos por severidad · Evaluación de criterios de salida · Riesgos ·
Recomendación (QA sign-off).

## Cómo se decide el veredicto

Se deriva de `criterios-entrada-salida`: "Se recomienda liberar" solo si se
cumplen todos los criterios de salida (ejecución, pass rate, cero bugs
críticos/altos, cobertura). Si no, "No se recomienda liberar" con los criterios
incumplidos y el riesgo. Las excepciones aprobadas por negocio quedan registradas.

## Steerings relacionados

- `criterios-entrada-salida`, `gestion-defectos`
- `datos-entornos-prueba`, `estrategia-pruebas`

## Personalización

| Qué | Cómo |
|-----|------|
| Logo | Reemplaza `assets/logo.svg` (SVG/PNG) |
| Colores / tipografía | Variables CSS al inicio de `assets/pdf-style.css` |
| Ruta de salida | Por defecto `qa/reportes/`; pide otra al agente |

## Requisitos

- Ninguna dependencia externa: el agente consolida y genera el PDF. Si el entorno
  impide el PDF, se entrega el Markdown (alternativa: `pandoc`).

## Estructura de la skill

```
reporte-ejecucion-pruebas/
├── SKILL.md                 # Instrucciones que sigue el agente
├── README.md                # Este archivo
└── assets/
    ├── template.md          # Plantilla del reporte
    ├── pdf-style.css        # Estilos del PDF (estados, severidad, veredicto)
    └── logo.svg             # Logo por defecto (reemplazable)
```
