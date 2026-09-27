---
inclusion: always
---

# QA No Funcional (Performance, Seguridad, Accesibilidad)

Prácticas y umbrales para las pruebas no funcionales. Complementa
`estrategia-pruebas`. Contexto: **AWS + Python**.

## Performance / Rendimiento

**Los umbrales se definen por proyecto** según los requisitos no funcionales
acordados con el cliente; no hay número fijo por defecto. Al iniciar un proyecto,
acuerda y documenta las métricas objetivo antes de probar.

| Métrica | Objetivo |
|---------|----------|
| Tiempo de respuesta (p95) | Definir por proyecto |
| Throughput | Definir por proyecto |
| Tasa de error bajo carga | Definir por proyecto |
| Concurrencia soportada | Definir por proyecto |

- Herramienta por defecto: **Locust** (Python); alternativa `k6`.
- Define escenarios: carga esperada, pico, estrés y resistencia (soak).
- Mide en un entorno representativo (`qa`); documenta datos, carga y herramienta.
- Reporta percentiles (p95/p99), no solo promedios.
- Consideraciones AWS: cold starts de Lambda, límites de concurrencia, capacidad
  provisionada/on-demand de DynamoDB, y throttling de servicios; tenlos en cuenta
  al interpretar resultados.

## Seguridad (nivel QA)

Checklist básico alineado a OWASP (no sustituye una auditoría de seguridad
dedicada):

- Validación de entradas; sin inyección (SQL en RDS/Redshift/Athena, comandos, XSS).
- Autenticación y autorización correctas (control de acceso por rol).
- Gestión segura de secretos y sesiones; sin credenciales en cliente/logs.
- Manejo de errores sin filtrar información sensible.
- Dependencias sin vulnerabilidades conocidas (análisis SCA, p. ej. `pip-audit`).
- Transporte cifrado (TLS) y cabeceras de seguridad.

Específico de AWS:

- **IAM de mínimo privilegio:** roles de Lambda/Glue/CodeBuild sin `*:*`.
- **Cifrado en reposo:** S3, DynamoDB, RDS/Aurora, Redshift con KMS/SSE.
- **Secretos:** en SSM Parameter Store o Secrets Manager, nunca embebidos ni en
  variables de entorno de Lambda en texto plano.
- **Superficie pública:** API Gateway con autorización; buckets S3 sin acceso
  público salvo justificación explícita.

> Hallazgos de seguridad relevantes se tratan con confidencialidad y se escalan
> según la política; no se publican en reportes de circulación amplia.

## Accesibilidad

- Nivel objetivo: **WCAG 2.1 AA** cuando la solución tenga interfaz de usuario.
- Muchos componentes AWS/Python son backend/data (Lambda, Glue, APIs) sin UI: en
  esos casos, la accesibilidad se marca **N/A** de forma explícita en el alcance.
- Verifica: contraste, navegación por teclado, foco visible, textos alternativos,
  etiquetas de formularios, estructura semántica y roles ARIA correctos.
- Combina herramientas automáticas (axe, Lighthouse) con **pruebas manuales**.

> La validación completa de accesibilidad requiere pruebas manuales con tecnología
> asistiva (lectores de pantalla) y revisión experta; las herramientas automáticas
> detectan solo una parte de los problemas. Decláralo así en los reportes.

## Reglas para el agente

- Al planificar QA no funcional, parte de estos umbrales y ajústalos a los
  requisitos del proyecto; decláralos explícitamente.
- Reporta performance con percentiles y condiciones de la prueba.
- En accesibilidad, aclara siempre que la cobertura automática es parcial y que se
  requiere validación manual/experta para afirmar cumplimiento WCAG.
- Trata los hallazgos de seguridad con confidencialidad.
