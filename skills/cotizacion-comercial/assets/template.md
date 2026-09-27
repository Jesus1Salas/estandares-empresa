<!--
PLANTILLA: Cotización Comercial
Instrucciones para el agente:
- Sustituye todos los marcadores {{...}} por contenido REAL.
- Aplica `politica-precios-cotizacion`: moneda explícita, impuestos, validez,
  condiciones de pago. NUNCA muestres tarifas internas ni márgenes.
- Verifica que el margen resultante no baje del mínimo definido; si un descuento
  lo vulnera, no lo apliques y avísalo.
- Si falta una tarifa, usa > ⚠️ PENDIENTE: definir tarifa de <rol>. No inventes un total.
- Alinea a la derecha las celdas numéricas usando la clase `num` en HTML si generas tabla HTML.
- Elimina estos comentarios HTML en la salida final.
-->

---
titulo: "Cotización"
cliente: "{{CLIENTE}}"
proyecto: "{{PROYECTO}}"
cotizacion_id: "{{COTIZACION_ID}}"
fecha: "{{FECHA}}"
validez: "{{VALIDEZ}}"
moneda: "{{MONEDA}}"
autor: "{{AUTOR}}"
clasificacion: "Confidencial"
---

# Cotización

**Cliente:** {{CLIENTE}}
**Proyecto:** {{PROYECTO}}
**Cotización N.º:** {{COTIZACION_ID}} · **Fecha:** {{FECHA}} · **Validez:** {{VALIDEZ}}
**Moneda:** {{MONEDA}}

## Modelo de precio

{{MODELO_PRECIO}}
<!-- Precio fijo / T&M / Retainer / Staffing, según `politica-precios-cotizacion`. -->

## Detalle de conceptos

| Concepto | Cant. / Unidad | Precio unitario | Subtotal |
|----------|---------------:|----------------:|---------:|
{{TABLA_CONCEPTOS}}
<!-- Conceptos de cara al cliente (fases, entregables, perfiles). Precio al
cliente, NO costo interno. Cantidades y unidades claras (horas, meses, ítems). -->

## Resumen económico

| | Importe ({{MONEDA}}) |
|---|---:|
| Subtotal | {{SUBTOTAL}} |
| Descuento {{DESCUENTO_PCT}} | {{DESCUENTO_IMPORTE}} |
| Base imponible | {{BASE_IMPONIBLE}} |
| Impuestos {{IMPUESTO_PCT}} | {{IMPUESTO_IMPORTE}} |
| **Total** | **{{TOTAL}}** |
<!-- Muestra el descuento como línea explícita, nunca oculto en la tarifa.
Marca la fila Total con la clase `total-row` si generas HTML. -->

## Condiciones

- **Validez de la oferta:** {{VALIDEZ}}
- **Forma de pago:** {{FORMA_PAGO}}
- **Plazo de pago:** {{PLAZO_PAGO}}
- **Moneda:** {{MONEDA}}
- **Impuestos:** {{IMPUESTOS_NOTA}}

## Supuestos

{{SUPUESTOS}}
<!-- Qué se asume para que el precio sea válido (alcance, disponibilidad, insumos).
Todo cambio de alcance se gestiona por control de cambios (ver glosario: CR). -->

## Notas

{{NOTAS}}
<!-- Aclaraciones adicionales. Remite a la propuesta o SOW asociado si existe. -->
