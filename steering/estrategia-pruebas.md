---
inclusion: always
---

# Estrategia y Estándares de Pruebas

Política base de calidad de la consultora: qué se prueba, en qué niveles y con qué
criterios mínimos. Aplica a todo proyecto salvo que exista una estrategia
específica acordada con el cliente.

## Contexto tecnológico

- **Nube:** AWS. **Lenguaje principal:** Python.
- **Tipos de aplicación predominantes:**
  - **Serverless:** Lambda + API Gateway + DynamoDB.
  - **Data / ETL:** Glue, EMR (PySpark), Step Functions, S3, Redshift, Athena,
    Kinesis.
- **Servicios AWS frecuentes:** Lambda, API Gateway, DynamoDB, S3, RDS/Aurora,
  SQS/SNS, Step Functions, Glue, Kinesis, Redshift, Athena.
- **Stack de pruebas (Python):** `pytest` (unitarias e integración),
  `pytest-cov` (cobertura), `moto`/`pytest-mock` para simular servicios AWS,
  `boto3` para integración real, `Faker` + factories para datos sintéticos.
  Ver `automatizacion-pruebas` para el detalle.

## Principios

- **Shift-left:** las pruebas empiezan lo antes posible (desde el refinamiento de
  requisitos), no al final del desarrollo.
- **La calidad es del equipo:** QA facilita y verifica, pero la calidad no es
  responsabilidad exclusiva del rol QA.
- **Automatiza lo repetitivo, explora lo nuevo:** regresión y flujos estables se
  automatizan; lo nuevo o incierto se prueba de forma exploratoria.
- **Basado en riesgo:** prioriza el esfuerzo de prueba según impacto y
  probabilidad de fallo, no cobertura uniforme.

## Niveles de prueba

| Nivel | Responsable típico | Cuándo | Obligatorio |
|-------|--------------------|--------|-------------|
| Unitaria | Desarrollo | Cada cambio | Sí |
| Integración | Desarrollo / QA | Al integrar componentes | Sí |
| Sistema / E2E | QA | Por feature / release | Sí |
| Regresión | QA (automatizada) | Antes de cada release | Sí |
| Aceptación (UAT) | Cliente / negocio | Antes de producción | Según proyecto |
| Performance | QA / especialista | Releases con impacto de carga | Según proyecto |
| Seguridad | QA / especialista | Releases con superficie sensible | Según proyecto |
| Accesibilidad | QA / especialista | Interfaces de cara al usuario | Según proyecto (solo si hay UI) |

## Pirámide de testing (proporción objetivo)

- Base amplia de **unitarias** (rápidas, baratas): lógica de negocio de las Lambda
  y transformaciones PySpark, con dependencias AWS simuladas (`moto`/mocks).
- Capa media de **integración/API**: contratos de API Gateway, integraciones entre
  Lambda, DynamoDB, S3, colas y pasos de Step Functions/Glue.
- Punta reducida de **E2E** (flujos críticos completos y, si hay UI, E2E de UI).

Evita el "anti-patrón cono de helado" (muchas E2E, pocas unitarias). En procesos
Data/ETL, prioriza pruebas de transformación y de calidad de datos sobre E2E de
pipeline completo.

## Cobertura mínima

- **Cobertura de código (unitaria):** **≥ 80%** en lógica de negocio, medida con
  `pytest-cov`; el número no sustituye a la calidad de las aserciones. No cuentes
  como cubierto el código de infraestructura (plantillas) ni los adaptadores
  triviales de boto3.
- **Cobertura de requisitos:** el 100% de los criterios de aceptación debe tener
  al menos un caso de prueba (ver skill `matriz-trazabilidad`).

## Reglas para el agente

- Al diseñar pruebas, cubre los niveles obligatorios aplicables y prioriza por
  riesgo.
- Respeta la pirámide: no propongas E2E para lo que puede cubrir una prueba de
  integración o unitaria.
- Si un proyecto no define umbrales, aplica los de este documento y decláralo.
- Nunca declares "probado" sin trazar los criterios de aceptación a casos concretos.
