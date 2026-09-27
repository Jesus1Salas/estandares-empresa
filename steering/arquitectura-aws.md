---
inclusion: always
---

# Arquitectura y Estándares AWS

Principios y convenciones de arquitectura para soluciones en AWS. Contexto:
**AWS + Python** (serverless y data/ETL). Se apoya en el AWS Well-Architected
Framework y complementa `cloudformation`, `seguridad-devsecops` e
`ingenieria-datos`.

- **Región principal:** `us-east-1`. Documenta explícitamente cualquier recurso o
  despliegue multi-región.

## Pilares Well-Architected

Toda decisión de arquitectura se evalúa contra los seis pilares:

| Pilar | Qué buscamos |
|-------|--------------|
| **Excelencia operativa** | Automatización (IaC), observabilidad, runbooks, despliegues repetibles |
| **Seguridad** | Mínimo privilegio, cifrado, secretos gestionados (ver `seguridad-devsecops`) |
| **Fiabilidad** | Tolerancia a fallos, reintentos, recuperación, multi-AZ donde aplique |
| **Eficiencia del rendimiento** | Servicio correcto para cada carga; dimensionamiento adecuado |
| **Optimización de costos** | Pago por uso, apagar lo ocioso, tamaños correctos, tags de costo |
| **Sostenibilidad** | Uso eficiente de recursos; evitar sobreaprovisionamiento |

## Infraestructura como código

- **Toda** la infraestructura se define en **CloudFormation (YAML)** según el
  steering `cloudformation`. Nada de cambios manuales en consola en `qa`/`prod`.
- Parametriza por entorno (`dev`/`qa`/`prod`) con `Mappings`/`Conditions`; una
  plantilla re-desplegable, no plantillas duplicadas por entorno.
- Recursos con estado (S3, DynamoDB, RDS, Redshift) con `DeletionPolicy: Retain`
  en `prod`.

## Estrategia multi-cuenta / multi-entorno

- Entornos **dev / qa / prod**, preferiblemente en cuentas AWS separadas
  (aislamiento de blast radius y de datos).
- Región principal: **us-east-1**. Documenta si hay multi-región.
- Nombres de recursos incluyen entorno para trazabilidad y unicidad.

## Convención de nombres y etiquetado

- **Nombres físicos:** `kebab-case` con proyecto y entorno,
  `!Sub "${ProjectName}-${Environment}-<recurso>"`.
- **Logical IDs (CloudFormation):** `PascalCase` (ver `cloudformation`).
- **Tags obligatorias** en todo recurso que las soporte:

| Tag | Ejemplo |
|-----|---------|
| `Project` | `ventas` |
| `Environment` | `qa` |
| `Owner` | equipo/responsable |
| `CostCenter` | `<centro de costo>` |
| `ManagedBy` | `cloudformation` |

Las tags de `Project`/`CostCenter`/`Environment` habilitan la asignación y el
seguimiento de costos.

## Elección de servicios (patrones por defecto)

| Necesidad | Servicio por defecto | Notas |
|-----------|----------------------|-------|
| Cómputo event-driven / API | Lambda (Python) | Separa lógica del handler |
| API HTTP | API Gateway | Con authorizer; versionado |
| Orquestación de flujos | Step Functions | Reintentos y manejo de error explícitos |
| ETL / transformación | Glue (PySpark) / EMR | Ver `ingenieria-datos` |
| Almacenamiento de objetos | S3 | Cifrado, sin acceso público |
| NoSQL clave-valor | DynamoDB | Diseño por patrón de acceso |
| Relacional | RDS / Aurora | Multi-AZ en prod |
| Data warehouse / analítica | Redshift / Athena | Athena sobre S3 + Glue Catalog |
| Mensajería / desacople | SQS / SNS / EventBridge | Colas con DLQ |

> No introduzcas un servicio fuera de esta lista sin justificarlo (costo,
> operación, seguridad) y acordarlo.

## Fiabilidad y resiliencia

- Diseña para el fallo: reintentos con backoff, colas de mensajes muertos (DLQ),
  timeouts explícitos e idempotencia.
- Multi-AZ para servicios con estado en `prod`; define RTO/RPO cuando aplique.
- Desacopla componentes con colas/eventos para absorber picos y fallos parciales.

## Costos

- Prefiere modelos serverless/pago por uso cuando el patrón de carga lo favorezca.
- Dimensiona (memoria de Lambda, DPUs de Glue, tipo de nodo Redshift) según
  medición, no por defecto.
- Etiqueta para asignación de costos; revisa y apaga recursos ociosos en `dev`/`qa`.

## Reglas para el agente

- Evalúa cada diseño contra los seis pilares; señala tensiones (p. ej. costo vs.
  rendimiento) explícitamente.
- Toda infraestructura va en CloudFormation parametrizado por entorno.
- Aplica la convención de nombres y las tags obligatorias.
- Usa los servicios por defecto de la tabla; justifica cualquier alternativa.
- Los diagramas de arquitectura siguen `architecture-diagrams` (iconos AWS,
  fuentes y destinos, parámetros documentados).
