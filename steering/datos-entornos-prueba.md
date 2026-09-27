---
inclusion: always
---

# Datos de Prueba y Entornos

Reglas para el uso de datos de prueba y la gestión de entornos. Protege datos
sensibles y asegura que las pruebas sean reproducibles.

## Datos de prueba

- **Nunca uses datos productivos reales** con PII sin enmascarar en entornos de
  prueba.
- **Anonimiza / enmascara** cualquier dato derivado de producción (nombres,
  correos, documentos de identidad, tarjetas, teléfonos).
- Prefiere **datos sintéticos** generados (factories, generadores de datos falsos)
  sobre copias de producción.
- Datos de prueba versionados y descriptivos: `usuario_valido`, `pago_rechazado`,
  no valores mágicos sin contexto.
- **Sin secretos en los datos ni en el repo:** credenciales, tokens y claves van
  en variables de entorno o gestor de secretos, nunca hardcodeados ni en capturas.

## Entornos

Tres entornos, típicamente cuentas o stacks AWS separados:

| Entorno | Propósito | Datos | Notas |
|---------|-----------|-------|-------|
| **dev** | Pruebas de desarrollo, unitarias e integración temprana | Sintéticos | Inestable por diseño; recursos AWS efímeros |
| **qa** | Ejecución de casos, regresión e integración con AWS real | Sintéticos / anonimizados | Base para los reportes de ejecución |
| **prod** | Producción | Reales | No se prueban aquí salvo smoke acordado y sin efectos colaterales |

- Los recursos AWS se identifican por entorno (sufijo/tag `dev`/`qa`/`prod`); nunca
  cruces datos o recursos entre entornos.
- El acceso a AWS en pruebas es por **rol IAM** (OIDC en GitHub Actions o rol de
  CodeBuild), nunca con claves de acceso embebidas.
- Cada reporte de prueba indica el **entorno y build/versión** en que se ejecutó.
- No pruebes en `prod` salvo smoke tests acordados y sin efectos colaterales.

## Datos de prueba en AWS

- **DynamoDB / RDS / Redshift:** carga sets de datos de prueba reducidos y
  descriptivos; limpia (teardown) lo que crees en la ronda.
- **S3:** usa prefijos o buckets por entorno; no reutilices rutas de `prod`.
- **Glue / Athena / Redshift (ETL):** trabaja sobre un subconjunto pequeño y
  representativo; valida esquema y reglas de negocio, no volúmenes productivos.
- **Datos derivados de producción:** solo si están **anonimizados/enmascarados**;
  preferir datos sintéticos (`Faker`) sobre copias de prod.

## Cumplimiento

- El tratamiento de datos personales en pruebas se rige por la normativa aplicable
  (p. ej. GDPR / ley local). Ante datos sensibles, minimiza y anonimiza.

## Reglas para el agente

- Al generar datos de prueba, usa datos sintéticos/ficticios; nunca inventes PII
  real ni copies datos productivos.
- Enmascara PII en ejemplos, evidencias y reportes.
- Registra siempre entorno y versión/build al documentar una ejecución.
- Si detectas un secreto en datos, tests o evidencias, no lo reproduzcas y avisa.
