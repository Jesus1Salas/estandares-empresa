---
inclusion: fileMatch
fileMatchPattern: '*cotiz*'
---

# Política de Precios y Cotización

Reglas para armar cotizaciones y la sección económica de las propuestas. Se
aplica automáticamente al trabajar sobre archivos cuyo nombre contiene `cotiz` y
la usa la skill `cotizacion-comercial`.

> ⚠️ COMPLETAR: reemplaza los marcadores `<...>` y las tarifas de ejemplo con los
> valores reales de la consultora. Estas cifras son internas: ver "Confidencialidad".

## Modelos de precio soportados

| Modelo | Cuándo usarlo | Base de cálculo |
|--------|---------------|-----------------|
| **Precio fijo (fixed price)** | Alcance bien definido y estable | Esfuerzo estimado × tarifa + margen |
| **Tiempo y materiales (T&M)** | Alcance variable o exploratorio | Horas reales × tarifa por rol |
| **Retainer / bolsa de horas** | Soporte continuo o capacidad reservada | Horas mensuales comprometidas |
| **Staffing / por perfil** | Cesión de perfiles al equipo del cliente | Tarifa mensual por perfil |

## Tarifas por rol y seniority (INTERNO)

> ⚠️ COMPLETAR con las tarifas reales. Son de uso interno para calcular el precio;
> NO se muestran desglosadas por rol al cliente salvo decisión comercial expresa.

| Rol | Junior | Semi-senior | Senior | Lead / Arquitecto |
|-----|--------|-------------|--------|-------------------|
| Ingeniero de datos | `<USD/h>` | `<USD/h>` | `<USD/h>` | `<USD/h>` |
| Desarrollador | `<USD/h>` | `<USD/h>` | `<USD/h>` | `<USD/h>` |
| Consultor / PM | `<USD/h>` | `<USD/h>` | `<USD/h>` | `<USD/h>` |

## Reglas de cálculo

- **Moneda por defecto:** `<USD>`. Indica siempre la moneda de forma explícita.
- **Margen mínimo:** `<XX%>` sobre costo. Ninguna cotización baja de este margen
  sin aprobación de dirección.
- **Impuestos:** los precios se indican `<sin IVA / + IVA XX%>`; se detalla el
  impuesto aplicable como línea separada.
- **Redondeo:** redondea el total a `<la unidad / decena>` más cercana.
- **Descuentos permitidos:** hasta `<X%>` a criterio comercial; entre `<X% y Y%>`
  requiere aprobación de `<rol aprobador>`; por encima de `<Y%>` requiere
  dirección. Todo descuento se refleja como línea explícita, no oculto en la
  tarifa.
- **Contingencia / buffer:** añade `<X%>` de contingencia en proyectos a precio
  fijo con incertidumbre de alcance.

## Estructura de la cotización (cara al cliente)

- Encabezado: cliente, proyecto, número de cotización, fecha, validez.
- Tabla de conceptos: descripción, cantidad/unidad, precio unitario, subtotal.
- Subtotal, descuento (si aplica), impuestos, **total**.
- Condiciones: validez de la oferta, forma y plazo de pago, moneda, supuestos.
- Lo que el cliente ve es el **precio**, no el costo interno ni el margen.

## Validez y condiciones de pago

- **Validez de la oferta:** `<30 días>` desde la fecha de emisión.
- **Forma de pago:** `<p. ej. 40% al inicio, 30% a mitad, 30% contra entrega>`.
- **Plazo de pago de facturas:** `<30 días>`.
- **Penalización por mora / reajuste:** `<según contrato>`.

## Confidencialidad (crítico)

- Las **tarifas internas, costos por recurso y márgenes NUNCA** aparecen en un
  documento que va al cliente. Al cliente se le muestra el precio final (y, si se
  decide, un desglose por concepto o fase, nunca por costo interno).
- El agente no debe filtrar estas cifras internas en propuestas, cotizaciones,
  correos ni anexos dirigidos al cliente.

## Reglas para el agente

- Calcula precios aplicando estas reglas; no inventes tarifas: si faltan, usa
  `> ⚠️ PENDIENTE: definir tarifa de <rol>` y no cierres un total ficticio.
- Verifica que el margen resultante no baje del mínimo; si un descuento lo
  vulnera, avísalo explícitamente y no lo apliques por defecto.
- Incluye SIEMPRE moneda, impuestos, validez y condiciones de pago.
- Nunca expongas tarifas internas ni márgenes en la salida al cliente.
