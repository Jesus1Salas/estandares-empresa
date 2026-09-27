---
name: cotizacion-comercial
description: >-
  Genera una cotización comercial para un cliente aplicando la política de
  precios de la consultora (modelos de precio, tarifas, descuentos, impuestos y
  validez), sin exponer costos internos ni márgenes. Produce salida en Markdown y
  en PDF con portada, logo y tipografía corporativa. Úsala cuando el usuario pida
  "arma una cotización", "cotiza este proyecto", "genera la cotización para
  <cliente>" o similar.
version: 1.0.0
---

# Skill: Cotización Comercial

Esta skill produce una **cotización** con formato corporativo, aplicando la
política de precios de la consultora. Se genera en dos formatos:

- **Markdown**: `comercial/cotizaciones/cotizacion-<cliente>-<proyecto>-<fecha>.md`
- **PDF**: mismo nombre con extensión `.pdf` (portada, logo y tipografía).

El principio rector es **calcular con las reglas reales, sin inventar cifras**.
Cuando falte una tarifa o dato, márcalo `> ⚠️ PENDIENTE:` en vez de cerrar un
total ficticio. Al cliente se le muestra el **precio**, nunca el costo interno ni
el margen.

## Cuándo se activa

Cuando el usuario pida crear, calcular o actualizar una cotización o presupuesto
para un cliente/oportunidad.

## Steerings que debe respetar

- `politica-precios-cotizacion` — modelos de precio, tarifas, margen mínimo,
  descuentos, impuestos, validez y confidencialidad de cifras internas.
- `tono-estilo-comercial` — formato de números y moneda, voz de marca.
- `perfil-empresa-servicios` — servicios reales que se pueden cotizar.
- `legal-cumplimiento-comercial` — condiciones de pago y qué requiere revisión.
- `datos-privacidad-comercial` — manejo de datos del cliente.
- `glosario-nomenclatura-comercial` — nombre del archivo y términos (T&M, CR...).

## Artefactos incluidos

- `assets/template.md` — plantilla de la cotización (conceptos + resumen económico).
- `assets/pdf-style.css` — estilos corporativos del PDF (con fila de total y
  alineación numérica).
- `assets/logo.svg` — logo por defecto (reemplazable).

Esta skill **no incluye scripts**: el cálculo y el PDF los produce el agente.

## Procedimiento (seguir en orden)

### Paso 0 — Reunir datos de entrada
Necesitas: cliente, proyecto, modelo de precio, alcance/estimación de esfuerzo
(horas o perfiles por fase), moneda, impuestos aplicables y cualquier descuento
solicitado. Si falta algo, pídelo. No inventes tarifas ni volúmenes.

### Paso 1 — Preparar carpeta de salida
Crea `comercial/cotizaciones/` si no existe.

### Paso 2 — Calcular el precio
Aplica `politica-precios-cotizacion`:

1. Selecciona el **modelo de precio** (fijo, T&M, retainer, staffing).
2. Calcula el precio a partir del esfuerzo/perfiles y las tarifas internas. Las
   tarifas y márgenes son de uso interno: se usan para calcular, **no se muestran**
   al cliente.
3. Verifica que el **margen** resultante no baje del mínimo. Si un descuento
   solicitado lo vulnera, no lo apliques por defecto: avísalo al usuario y pide
   confirmación/aprobación.
4. Aplica **descuento** (como línea explícita), **impuestos** y **redondeo** según
   la política.
5. Si falta una tarifa o dato, usa `> ⚠️ PENDIENTE: definir tarifa de <rol>` y no
   cierres un total inventado.

### Paso 3 — Redactar la cotización
Copia `assets/template.md` al archivo de salida y complétalo:

1. Nombra el archivo según `glosario-nomenclatura-comercial`:
   `cotizacion-<cliente>-<proyecto>-<AAAA-MM-DD>.md`. Nota: el nombre contiene
   `cotiz`, lo que activa el steering de precios automáticamente.
2. Rellena el front-matter y el detalle de conceptos (precio al cliente).
3. Completa el **resumen económico**: subtotal, descuento, base imponible,
   impuestos y **total**, con moneda explícita y formato de miles.
4. Incluye SIEMPRE validez, forma y plazo de pago, y supuestos.

### Paso 4 — Generar el PDF (lo hace el agente)
1. HTML con **portada** (logo data URI, título "Cotización", cliente, proyecto,
   N.º, fecha, validez, moneda, clasificación) y **cuerpo** (Markdown a HTML).
2. Aplica `assets/pdf-style.css`; marca la fila de total con `total-row` y alinea
   a la derecha las celdas numéricas (`num`).
3. Exporta a PDF A4 con `create_artifact` (`kind: pdf`); pie con numeración.
4. Guarda el PDF junto al Markdown.

Si el entorno impide el PDF, entrega el Markdown e infórmalo (alternativa:
`pandoc <archivo>.md -o <archivo>.pdf`).

### Paso 5 — Verificar
- Confirma que existen el `.md` y (si fue posible) el `.pdf`.
- No deben quedar marcadores `{{...}}` sin sustituir.
- Confirma que la salida NO contiene tarifas internas ni márgenes.
- Reporta al usuario: total, margen resultante (en un aparte interno, no en el
  documento del cliente), y cualquier `⚠️ PENDIENTE` o descuento que requiera
  aprobación.

## Personalización

- **Logo:** reemplaza `assets/logo.svg`.
- **Colores/tipografía:** variables CSS al inicio de `assets/pdf-style.css`.
- **Ruta de salida:** por defecto `comercial/cotizaciones/`.
