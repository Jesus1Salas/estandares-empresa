# Skill: Generador de Casos de Prueba

Diseña **casos de prueba** (positivos, negativos y de borde) a partir de criterios
de aceptación, una historia de usuario o un requisito, y los exporta en
**Markdown** y **PDF** (con logo y tipografía corporativa).

## Cómo se usa

Pídele al agente algo como:

- "Genera casos de prueba para esta historia: como usuario quiero..."
- "Diseña los test cases del módulo de login con estos criterios de aceptación"
- "Casos de prueba para el endpoint de pagos, incluye negativos y borde"

El agente activará la skill y seguirá el procedimiento del `SKILL.md`.

## Entrada requerida

Al menos uno de: criterios de aceptación, historia de usuario, requisito, o una
referencia a dónde están (archivo/ticket). Si los criterios no son claros, el
agente ayuda a redactarlos en formato verificable antes de generar casos.

## Qué hace, paso a paso

1. **Analiza la entrada**: módulo, reglas, entradas, salidas y condiciones de borde.
2. **Diseña los casos** en `qa/casos-prueba/` con la plantilla `assets/template.md`:
   por cada criterio, al menos un positivo, un negativo y los borde relevantes.
3. **Genera el PDF** con portada, logo y estilos.
4. **Verifica** cobertura: cada criterio con sus casos; reporta lo pendiente.

## Contenido del documento

Alcance · Criterios cubiertos · Resumen (totales por tipo) · Tabla de casos
(ID, título, criterio, precondiciones, datos, pasos, resultado esperado,
prioridad, tipo, automatizable) · Anexo Gherkin opcional.

## Convenciones aplicadas

- IDs `TC-<MODULO>-<NNN>` (negativos con sufijo `-N`).
- Resultado esperado verificable y sin ambigüedad.
- Datos de prueba sintéticos; nunca PII real ni secretos.
- Cada caso enlaza su criterio/requisito (alimenta la matriz de trazabilidad).

## Steerings relacionados

- `convenciones-casos-prueba` (se activa con archivos `*casos-prueba*`)
- `estrategia-pruebas`, `definicion-hecho-aceptacion`, `datos-entornos-prueba`

## Personalización

| Qué | Cómo |
|-----|------|
| Logo | Reemplaza `assets/logo.svg` (SVG/PNG) |
| Colores / tipografía | Variables CSS al inicio de `assets/pdf-style.css` |
| Ruta de salida | Por defecto `qa/casos-prueba/`; pide otra al agente |
| Gherkin | Se incluye u omite según el proyecto use BDD |

## Requisitos

- Ninguna dependencia externa: el agente diseña y genera el PDF. Si el entorno
  impide el PDF, se entrega el Markdown (alternativa: `pandoc`).

## Estructura de la skill

```
generador-casos-prueba/
├── SKILL.md                 # Instrucciones que sigue el agente
├── README.md                # Este archivo
└── assets/
    ├── template.md          # Plantilla de casos de prueba
    ├── pdf-style.css        # Estilos del PDF (estados pass/fail/blocked)
    └── logo.svg             # Logo por defecto (reemplazable)
```
