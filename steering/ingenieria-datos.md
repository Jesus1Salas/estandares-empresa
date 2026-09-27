---
inclusion: always
---

# Ingeniería de Datos / ETL

Convenciones para el diseño y la construcción de pipelines de datos. Contexto:
**AWS + Python (PySpark)**. Aplica a todo proceso de ingesta, transformación y
carga.

## Arquitectura de datos por capas

Modelo de capas (medallion), cada una en su prefijo/bucket:

| Capa | Equivalente medallion | Propósito | Formato | Notas |
|------|-----------------------|-----------|---------|-------|
| **raw** | bronze | Datos crudos tal como llegan del origen | Original / Parquet | Inmutable; no se transforma, solo se ingesta |
| **curated** | silver | Datos limpios, tipados, deduplicados | **Parquet** | Reglas de calidad aplicadas |
| **data-product** | gold | Datos modelados para negocio/analítica | **Parquet** | Agregados, data marts, listos para BI |

- Cada capa es reconstruible desde la anterior; los procesos son **idempotentes**.
- No se escribe directo a `data-product` saltándose `curated`.

## Formatos y almacenamiento

- **Formato por defecto:** Parquet (columnar, comprimido con Snappy).
- **Compresión:** Snappy por defecto; evalúa ZSTD para almacenamiento frío.
- **Particionado:** por fecha/campo de alta cardinalidad de consulta
  (`anio=YYYY/mes=MM/dia=DD/`). Evita el exceso de particiones pequeñas
  (problema de "small files").
- **Catálogo:** AWS Glue Data Catalog como catálogo central; tablas registradas
  para consulta con Athena/Redshift Spectrum.

## Convención de nombres

- **Buckets/prefijos:** `<proyecto>-<capa>-<entorno>`, con capa en
  `{raw, curated, data-product}` (p. ej. `ventas-curated-qa`,
  `ventas-data-product-prod`).
- **Bases y tablas Glue:** `snake_case`, prefijo por dominio
  (`ventas_curated.pedidos_diarios`).
- **Jobs (Glue/EMR) y máquinas de estado (Step Functions):**
  `<dominio>-<proceso>-<entorno>` en `kebab-case`.
- **Scripts PySpark:** siguen `programming-patterns` (tipado, docstrings,
  estructura de script, imports y config Spark).

## Diseño de pipelines

- **Idempotencia:** reejecutar un proceso sobre la misma entrada produce el mismo
  resultado; usa sobrescritura por partición (`overwrite` dinámico), no `append`
  ciego.
- **Reprocesos:** el pipeline permite reprocesar una partición/fecha concreta sin
  duplicar datos.
- **Marca de agua (watermark):** para cargas incrementales, persiste el último
  punto procesado (p. ej. en SSM Parameter Store o tabla de control).
- **Orquestación:** Step Functions para orquestar; cada paso hace una cosa y es
  reintentable. Define reintentos y manejo de fallos explícitos.
- **Esquema:** valida el esquema antes de escribir en `curated`/`data-product`
  (ver `calidad-datos`). Gestiona la evolución de esquema de forma controlada.
- **Volumetría:** dimensiona particiones y recursos (DPUs/executors) según el
  volumen; documenta el tipo de carga (batch diario, micro-batch, streaming).

## Rendimiento (PySpark / Glue)

- Evita `collect()` sobre datasets grandes; trabaja de forma distribuida.
- Controla el shuffle (`spark.sql.shuffle.partitions`) según volumen.
- Usa broadcast joins para tablas pequeñas; filtra y proyecta temprano
  (predicate/column pruning).
- Compacta archivos pequeños en `curated`/`data-product`.

## Reglas para el agente

- Respeta el flujo raw → curated → data-product; no te saltes capas.
- Escribe en Parquet particionado y registra en el Glue Data Catalog.
- Diseña procesos idempotentes y reprocesables por partición.
- Aplica `calidad-datos` antes de promover datos a `curated`/`data-product`.
- El código PySpark sigue `programming-patterns`.
- Nunca uses datos productivos con PII sin anonimizar (ver `datos-entornos-prueba`).
