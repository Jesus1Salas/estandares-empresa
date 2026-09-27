---
inclusion: always
---

# Seguridad y DevSecOps

Prácticas de seguridad aplicadas a lo largo del ciclo de desarrollo. Contexto:
**AWS + Python**. Complementa `qa-no-funcional` (checklist de seguridad de QA) y
`datos-entornos-prueba`.

## Principios

- **Seguridad desde el diseño (shift-left):** se considera desde el
  refinamiento, no al final.
- **Mínimo privilegio:** cada identidad y componente tiene solo los permisos que
  necesita.
- **Defensa en profundidad:** múltiples capas de control (red, identidad, datos,
  aplicación).
- **Cero secretos en el código:** ningún secreto se versiona ni se hardcodea.

## Gestión de secretos

- Secretos en **AWS Secrets Manager** o **SSM Parameter Store (SecureString)**;
  nunca en el código, variables de entorno en texto plano, logs ni repos.
- En CloudFormation usa referencias dinámicas
  (`{{resolve:ssm-secure:...}}` / `{{resolve:secretsmanager:...}}`).
- Rota credenciales periódicamente; revoca las que se expongan.
- El agente **nunca** imprime ni transcribe secretos; si detecta uno en el código
  o en un diff, lo señala y detiene la acción.

## IAM y control de acceso

- Roles de **mínimo privilegio** para Lambda, Glue, EMR, CodeBuild, Step
  Functions; nada de `Action: "*"` / `Resource: "*"` sin justificación explícita.
- Un rol por función/servicio; evita roles compartidos amplios.
- Acceso humano y de CI vía roles asumibles (OIDC en GitHub Actions), no usuarios
  con claves de larga duración.
- Autenticación/autorización en APIs (API Gateway con authorizer, Cognito o
  equivalente); valida permisos por recurso y acción.

## Cifrado

- **En reposo:** S3, DynamoDB, RDS/Aurora, Redshift, EBS y colas cifrados con KMS
  (o SSE-S3 donde aplique). Buckets S3 sin acceso público salvo justificación.
- **En tránsito:** TLS en todos los endpoints; rechaza tráfico no cifrado.
- Gestiona las claves KMS con política de acceso restringida.

## Seguridad en el código y dependencias

- **SAST:** análisis estático del código Python (p. ej. `bandit`) en el pipeline.
- **SCA:** análisis de dependencias (p. ej. `pip-audit` / `safety`) para detectar
  vulnerabilidades conocidas; fija versiones (pinning) en `requirements.txt`.
- **IaC scanning:** revisa las plantillas CloudFormation (p. ej. `cfn-lint` +
  `cfn_nag`/checkov) buscando malas configuraciones de seguridad.
- **Validación de entradas:** evita inyección (SQL en RDS/Redshift/Athena,
  comandos), y sanea datos externos. Trata todo input externo como no confiable.
- **Dependencias confiables:** usa paquetes mantenidos; desconfía de nombres
  sospechosos (typosquatting).

## Gestión de vulnerabilidades

| Severidad | Plazo de remediación objetivo |
|-----------|-------------------------------|
| Crítica | 7 días |
| Alta | 30 días |
| Media | 90 días |
| Baja | Según backlog |

- Los hallazgos se registran, priorizan por severidad y se trazan hasta su cierre.
- Un hallazgo **Crítico/Alto** bloquea la promoción a producción hasta mitigarse o
  aceptarse formalmente el riesgo.

## Datos personales y privacidad

- Minimiza el uso de PII; anonimiza/enmascara en entornos que no sean producción.
- El tratamiento de datos personales se rige por la normativa aplicable
  (GDPR / ley local) y por `datos-entornos-prueba`.

## En el pipeline (DevSecOps)

- SAST, SCA e IaC scanning corren en CI (GitHub Actions / CodeBuild).
- Un hallazgo de severidad Crítica/Alta **rompe** el build.
- Publica los reportes de seguridad como artefactos del pipeline.

## Reglas para el agente

- Nunca escribas secretos en código, plantillas, logs o diffs; si detectas uno,
  detente y avisa.
- Aplica mínimo privilegio en todo rol/política IAM que generes.
- Habilita cifrado por defecto (KMS) en recursos con estado.
- Fija versiones de dependencias y prefiere paquetes mantenidos.
- Trata operaciones de seguridad de alto impacto (permisos, exposición pública,
  acceso a datos) como cambios que requieren confirmación explícita.
