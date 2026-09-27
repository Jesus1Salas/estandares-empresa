# Skill: Cotización Comercial

Genera una **cotización** para un cliente aplicando la política de precios de la
consultora, y la exporta en **Markdown** y **PDF** (con logo y tipografía
corporativa). No expone tarifas internas ni márgenes: al cliente se le muestra el
precio.

## Cómo se usa

Pídele al agente algo como:

- "Arma una cotización para ACME por 2 ingenieros senior durante 3 meses"
- "Cotiza este proyecto a precio fijo con 10% de descuento"
- "Genera la cotización del proyecto Data Lake para el cliente X"

El agente activará la skill y seguirá el procedimiento del `SKILL.md`.

## Qué hace, paso a paso

1. **Reúne datos**: cliente, proyecto, modelo de precio, esfuerzo/perfiles,
   moneda, impuestos y descuentos. Si falta algo, lo pide.
2. **Calcula el precio** con `politica-precios-cotizacion`: tarifas internas
   (solo para el cálculo), margen mínimo, descuentos, impuestos y redondeo.
3. **Redacta la cotización** en `comercial/cotizaciones/` con la plantilla
   `assets/template.md`: detalle de conceptos y resumen económico.
4. **Genera el PDF** con portada, logo y estilos.
5. **Verifica**: sin tarifas internas ni márgenes en la salida; reporta total y
   cualquier descuento que requiera aprobación.

## Contenido de la cotización

Modelo de precio · Detalle de conceptos · Resumen económico (subtotal, descuento,
impuestos, total) · Condiciones (validez, forma y plazo de pago) · Supuestos ·
Notas.

## Reglas de precios clave

- Al cliente se le muestra el **precio final**, nunca el costo interno ni el margen.
- El **descuento** siempre es una línea explícita.
- Si un descuento baja del **margen mínimo**, la skill avisa y no lo aplica por
  defecto.
- Moneda, impuestos y validez son obligatorios.

## Steerings relacionados

- `politica-precios-cotizacion` (se activa con archivos `*cotiz*`)
- `tono-estilo-comercial`, `perfil-empresa-servicios`
- `legal-cumplimiento-comercial`, `datos-privacidad-comercial`
- `glosario-nomenclatura-comercial`

## Personalización

| Qué | Cómo |
|-----|------|
| Logo | Reemplaza `assets/logo.svg` (SVG/PNG) |
| Colores / tipografía | Variables CSS al inicio de `assets/pdf-style.css` |
| Ruta de salida | Por defecto `comercial/cotizaciones/`; pide otra al agente |

## Requisitos

- Ninguna dependencia externa: el agente calcula, redacta y genera el PDF. Si el
  entorno impide el PDF, se entrega el Markdown (alternativa: `pandoc`).

## Estructura de la skill

```
cotizacion-comercial/
├── SKILL.md                 # Instrucciones que sigue el agente
├── README.md                # Este archivo
└── assets/
    ├── template.md          # Plantilla de la cotización
    ├── pdf-style.css        # Estilos del PDF (fila total, alineación numérica)
    └── logo.svg             # Logo por defecto (reemplazable)
```
