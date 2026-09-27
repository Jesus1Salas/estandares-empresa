# Skill: Matriz de Trazabilidad de Requisitos (RTM)

Construye una **matriz de trazabilidad** que conecta requisitos ↔ casos de prueba
↔ resultados, calcula la cobertura y detecta huecos. Exporta en **Markdown** y
**PDF** (con logo y tipografía corporativa).

## Cómo se usa

Pídele al agente algo como:

- "Genera la matriz de trazabilidad del proyecto X"
- "RTM entre estos requisitos y estos casos de prueba"
- "Dame la cobertura de requisitos y qué falta por cubrir"

El agente activará la skill y seguirá el procedimiento del `SKILL.md`.

## Entrada requerida

- **Requisitos / criterios de aceptación** (con IDs), y
- **Casos de prueba** (con IDs y el requisito que cubren), y opcionalmente
- **Resultados de ejecución** y **defectos** asociados.

Pueden provenir de las skills `generador-casos-prueba` y
`reporte-ejecucion-pruebas`, o aportarse directamente. Lo que no se pueda trazar
se reporta como hallazgo, no se inventa.

## Qué hace, paso a paso

1. **Reúne** requisitos y casos, normaliza los IDs para cruzarlos.
2. **Construye la matriz** en `qa/trazabilidad/` con la plantilla
   `assets/template.md`: por requisito, sus casos, tipo, último resultado y
   defectos.
3. **Calcula la cobertura** y separa requisitos SIN COBERTURA y casos huérfanos.
4. **Genera el PDF** con portada, logo y estilos.
5. **Verifica** coherencia del % de cobertura y reporta los huecos como acciones.

## Contenido de la RTM

Resumen de cobertura (totales y %) · Matriz requisito → caso → resultado →
defecto → cobertura · Requisitos sin cobertura (hallazgos) · Casos huérfanos ·
Observaciones.

## Para qué sirve

- Demostrar que **cada requisito** tiene pruebas (cobertura de requisitos, objetivo
  100% según `estrategia-pruebas`).
- Detectar **huecos** (requisitos sin caso) y **casos huérfanos** (sin requisito).
- Dar trazabilidad de auditoría: requisito → caso → resultado → defecto.

## Steerings relacionados

- `estrategia-pruebas`, `convenciones-casos-prueba`
- `definicion-hecho-aceptacion`, `gestion-defectos`

## Personalización

| Qué | Cómo |
|-----|------|
| Logo | Reemplaza `assets/logo.svg` (SVG/PNG) |
| Colores / tipografía | Variables CSS al inicio de `assets/pdf-style.css` |
| Ruta de salida | Por defecto `qa/trazabilidad/`; pide otra al agente |

## Requisitos

- Ninguna dependencia externa: el agente mapea y genera el PDF. Si el entorno
  impide el PDF, se entrega el Markdown (alternativa: `pandoc`).

## Estructura de la skill

```
matriz-trazabilidad/
├── SKILL.md                 # Instrucciones que sigue el agente
├── README.md                # Este archivo
└── assets/
    ├── template.md          # Plantilla de la matriz (RTM)
    ├── pdf-style.css        # Estilos del PDF (cobertura, resultados)
    └── logo.svg             # Logo por defecto (reemplazable)
```
