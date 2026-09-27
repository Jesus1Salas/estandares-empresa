# Skill: Propuesta Comercial / SOW

Genera una **propuesta comercial** (o SOW) completa a partir de un brief del
cliente, siguiendo la estructura y el tono comercial de la consultora, y la
exporta en **Markdown** y **PDF** (con logo y tipografía corporativa).

## Cómo se usa

Pídele al agente algo como:

- "Crea una propuesta comercial para ACME sobre el proyecto Data Lake"
- "Arma un SOW para el cliente X con este brief: ..."
- "Genera la propuesta a partir de esta reunión / requerimiento"

El agente activará la skill y seguirá el procedimiento del `SKILL.md`.

## Qué hace, paso a paso

1. **Reúne el brief**: cliente, proyecto, problema, objetivos, alcance,
   restricciones y modelo de precio. Si falta algo clave, lo pide.
2. **Redacta la propuesta** en `comercial/propuestas/` con la plantilla
   `assets/template.md`, respetando el orden de secciones del steering
   `estructura-propuestas` y la voz de `tono-estilo-comercial`.
3. **Genera el PDF** con portada, logo y estilos (`assets/pdf-style.css`).
4. **Verifica**: sin marcadores pendientes, lista lo `⚠️ PENDIENTE` y lo que
   requiere revisión legal.

## Secciones de la propuesta

Resumen ejecutivo · Entendimiento del problema · Objetivos · Alcance
(incluye / no incluye) · Enfoque y metodología · Entregables · Cronograma ·
Equipo · Supuestos y dependencias · Inversión · Condiciones comerciales ·
Próximos pasos · Anexos.

## Steerings relacionados

Se aplican automáticamente y guían el contenido:

- `estructura-propuestas` (se activa con archivos `*propuesta*`)
- `tono-estilo-comercial`, `perfil-empresa-servicios`
- `politica-precios-cotizacion`, `legal-cumplimiento-comercial`
- `datos-privacidad-comercial`, `glosario-nomenclatura-comercial`

## Personalización

| Qué | Cómo |
|-----|------|
| Logo | Reemplaza `assets/logo.svg` (SVG/PNG) |
| Colores / tipografía | Variables CSS al inicio de `assets/pdf-style.css` |
| Ruta de salida | Por defecto `comercial/propuestas/`; pide otra al agente |

## Requisitos

- Ninguna dependencia externa: el agente redacta el Markdown y genera el PDF con
  sus propias herramientas. Si el entorno impide el PDF, se entrega el Markdown y
  puede usarse `pandoc` como alternativa.

## Estructura de la skill

```
propuesta-comercial/
├── SKILL.md                 # Instrucciones que sigue el agente
├── README.md                # Este archivo
└── assets/
    ├── template.md          # Plantilla de la propuesta
    ├── pdf-style.css        # Estilos del PDF
    └── logo.svg             # Logo por defecto (reemplazable)
```
