---
inclusion: fileMatch
fileMatchPattern: ['**/tests/**', '**/e2e/**', '*.spec.*', '*.test.*', '**/*_test.*', '**/test_*.*']
---

# Automatización de Pruebas

Convenciones para el código de automatización de pruebas. Se aplica al trabajar
sobre archivos de test y guía la skill de andamiaje si se usa.

Contexto: **AWS + Python**. Los frameworks por defecto son Python-native.

## Frameworks aprobados

Usa solo frameworks aprobados por la consultora para mantener consistencia:

| Capa | Framework por defecto | Alternativas aprobadas |
|------|------------------------|------------------------|
| Unitaria / Integración (Python) | `pytest` | `unittest` |
| Cobertura | `pytest-cov` (coverage.py) | — |
| Mock de servicios AWS | `moto` | `pytest-mock`, `botocore.stub.Stubber` |
| Mock / fixtures generales | `pytest-mock`, `pytest` fixtures | `unittest.mock` |
| Pruebas de API | `pytest` + `requests` | `httpx` |
| E2E web (si hay UI) | `Playwright` (Python) | `Selenium` |
| Carga / Performance | `Locust` (Python) | `k6` |
| Datos sintéticos | `Faker` + factories | `factory_boy` |
| Calidad de datos (ETL) | `pytest` + aserciones sobre DataFrames | `Great Expectations` |

No introduzcas un framework nuevo sin justificarlo y acordarlo con el equipo.

### Pruebas específicas de AWS

- **Lambda:** separa la lógica de negocio del handler para poder probarla sin AWS;
  simula servicios con `moto`. Prueba el handler con eventos de ejemplo (API GW,
  S3, SQS, EventBridge).
- **DynamoDB / S3 / SQS / SNS:** usa `moto` para unitarias; integración real contra
  recursos efímeros de `dev`/`qa`.
- **Step Functions / Glue:** prueba cada tarea/paso de forma aislada; valida la
  máquina de estados o el job con datos de muestra pequeños.
- **PySpark (Glue/EMR):** prueba las transformaciones con una `SparkSession` local
  y DataFrames de muestra; verifica esquema y reglas de negocio.
- **Redshift / Athena:** valida consultas contra un set de datos de prueba
  reducido; parametriza la conexión por entorno.

## Estructura y organización

- Los tests viven en `tests/` (por defecto), organizados por tipo:
  `tests/unit/`, `tests/integration/`, `tests/e2e/`.
- Nombra archivos `test_<modulo>.py` y funciones `test_<comportamiento>`.
- Separa: **casos** (tests), **fixtures/helpers** (`conftest.py`), **datos**
  (factories/fixtures) y **configuración** por entorno.
- Para E2E de UI, aplica **Page Object Model**.

## Patrones

- **Page Object Model (POM)** para E2E de UI: la lógica de localización y acciones
  vive en objetos de página, no en los specs.
- **Arrange–Act–Assert** (o Given–When–Then) como estructura de cada test.
- **Datos de prueba** desacoplados del test: fixtures/factories, no hardcodeados
  dispersos. Sin datos productivos ni PII real (ver `datos-entornos-prueba`).

## Estabilidad (evitar flakiness)

- Prohibido usar esperas fijas (`sleep`/`wait(ms)`); usa esperas explícitas por
  condición/elemento.
- Cada test es **independiente e idempotente**: no depende del orden ni del estado
  dejado por otro.
- Limpia el estado creado (teardown). Aísla datos por ejecución cuando sea posible.
- Un test que falla de forma intermitente se corrige o se marca/quarantine; no se
  ignora silenciosamente.

## Aserciones y nombres

- Aserciones específicas y con mensaje claro; evita `assert true` genéricos.
- Nombre del test = comportamiento esperado
  (`deberia_rechazar_login_con_password_invalido`).

## Qué automatizar vs. dejar manual

- **Automatiza:** regresión, flujos estables y repetitivos, validaciones de API,
  transformaciones de datos, cálculos, smoke tests.
- **Manual/exploratorio:** UX, exploración de features nuevas, casos de una sola vez.

## Integración en CI/CD

Las pruebas corren automáticamente en el pipeline. Plataformas usadas:

- **GitHub Actions:** ejecuta `pytest` con `pytest-cov` en cada push/PR; falla el
  build si la cobertura baja del umbral (ver `estrategia-pruebas`, ≥ 80%).
- **AWS CodePipeline / CodeBuild:** etapa de pruebas en `buildspec.yml` que corre
  `pytest`; publica resultados y cobertura como artefactos.

Pautas:

- Unitarias y de integración con `moto` corren en cada PR (rápidas, sin AWS real).
- Las de integración contra AWS real corren en `dev`/`qa` con credenciales por
  rol IAM (nunca claves embebidas); usa OIDC en GitHub Actions o el rol de
  CodeBuild.
- Publica el reporte de cobertura (`coverage xml`/`html`) como artefacto del build.
- Un fallo de prueba o de umbral de cobertura **rompe** el pipeline.

## Reglas para el agente

- Genera código de automatización con los frameworks aprobados (pytest + moto) y
  estos patrones.
- Separa la lógica de negocio del handler de Lambda para poder probarla sin AWS.
- Nada de esperas fijas ni tests dependientes entre sí.
- Desacopla datos y aplica POM en E2E de UI.
- No incluyas secretos ni credenciales AWS en el código de test; usa variables de
  entorno, roles IAM o un gestor de secretos.
