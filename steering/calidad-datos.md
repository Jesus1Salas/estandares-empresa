---
inclusion: always
---

# Calidad de Datos

Reglas para asegurar la calidad de los datos que fluyen por los pipelines.
Complementa `ingenieria-datos`. Contexto: **AWS + PySpark**.

## Dimensiones de calidad

Todo dataset promovido a `curated`/`data-product` se valida en estas dimensiones:

| Dimensión | Qué verifica |
|-----------|--------------|
| **Completitud** | Campos obligatorios no nulos; no faltan registros esperados |
| **Unicidad** | Sin duplicados según la clave de negocio |
| **Validez** | Tipos, formatos, rangos y dominios correctos |
| **Consistencia** | Coherencia entre campos y entre tablas relacionadas |
| **Exactitud** | Los valores reflejan la realidad esperada del origen |
| **Puntualidad** | Los datos llegan y se procesan dentro del SLA acordado |

## Validaciones mínimas antes de escribir

Antes de promover a `curated` o `data-product`, valida:

- **Esquema:** columnas y tipos esperados (falla si el esquema no coincide).
- **Claves:** no nulas y únicas donde corresponda.
- **Conteos:** número de registros dentro de un rango razonable (detecta cargas
  vacías o anómalas).
- **Reglas de negocio:** rangos y dominios (p. ej. montos ≥ 0, estados en un
  conjunto permitido).

## Manejo de datos que fallan

- **Cuarentena:** los registros inválidos se apartan en una zona/tabla de rechazos
  con el motivo del fallo, en vez de descartarse en silencio o romper el job.
- **Umbral de tolerancia:** define qué % de rechazos es aceptable; superarlo
  detiene la promoción y alerta.
- **Trazabilidad:** registra métricas de calidad por ejecución (procesados,
  válidos, rechazados) para auditoría y tendencia.

## Herramientas

- Validaciones con `pytest` + aserciones sobre DataFrames PySpark, o
  **Great Expectations** para suites de expectativas declarativas.
- Las métricas de calidad se registran en logs estructurados y/o una tabla de
  control (ver `observabilidad` si existe).

## Reglas para el agente

- No promuevas datos a `curated`/`data-product` sin validar esquema, claves y
  reglas de negocio.
- Aparta los registros inválidos en cuarentena con su motivo; no los descartes en
  silencio.
- Reporta métricas de calidad por ejecución (procesados/válidos/rechazados).
- Si el % de rechazos supera el umbral, detén la promoción y notifícalo.
