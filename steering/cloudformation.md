# CloudFormation

Convenciones para generar y definir plantillas de AWS CloudFormation en este
proyecto. Aplica siempre que crees o modifiques una plantilla CloudFormation.

- Formato obligatorio: **YAML** (no JSON).
- Versión de plantilla: `AWSTemplateFormatVersion: "2010-09-09"`.
- Una plantilla por stack lógico; evita plantillas monolíticas gigantes.

## 1. Estructura general

Las secciones se declaran siempre en este orden. Omite las que no apliquen, pero
no cambies el orden.

```yaml
AWSTemplateFormatVersion: "2010-09-09"
Description: "Breve descripción del propósito del stack."

Metadata: {}      # Agrupación/orden de parámetros (interfaz de consola)
Parameters: {}    # Entradas configurables
Mappings: {}      # Tablas estáticas (p. ej. por ambiente o región)
Conditions: {}    # Lógica condicional (p. ej. IsProd)
Resources: {}     # Recursos AWS (única sección obligatoria)
Outputs: {}       # Valores exportados
```

## 2. Convención de nombres (PascalCase)

- **Logical IDs** de recursos, parámetros, condiciones, mappings y outputs en
  **PascalCase**: `DataBucket`, `IngestLambdaRole`, `Environment`, `IsProd`.
- El Logical ID describe el recurso, no repite el tipo innecesariamente:
  usa `DataBucket`, no `DataBucketS3BucketResource`.
- **Nombres físicos** de recursos en `kebab-case` e incluyen proyecto y
  ambiente para trazabilidad y unicidad:
  `!Sub "${ProjectName}-${Environment}-data"`.
- **Tags** obligatorias en recursos que las soporten: `Project`, `Environment`,
  `Owner`, `ManagedBy: cloudformation`.

```yaml
Resources:
  DataBucket:
    Type: AWS::S3::Bucket
    Properties:
      BucketName: !Sub "${ProjectName}-${Environment}-data"
      Tags:
        - Key: Project
          Value: !Ref ProjectName
        - Key: Environment
          Value: !Ref Environment
        - Key: ManagedBy
          Value: cloudformation
```

## 3. Parámetros

Agrupa y ordena los parámetros por categoría usando
`AWS::CloudFormation::Interface` en `Metadata`. Categorías sugeridas:

- **Core parameters**: identidad y ambiente (`ProjectName`, `Environment`).
- **AWS resources**: nombres/ARNs de recursos existentes o referencias
  (VPC, subredes, KMS key, buckets externos).
- **App configuration**: parámetros de la aplicación (memoria, timeouts, rutas).

Reglas para todo parámetro:

- Declara `Type`, `Description` y, cuando aplique, `AllowedValues`,
  `AllowedPattern`, `Default` y `NoEcho: true` (para secretos).
- No pongas valores sensibles como `Default`. Los secretos vienen de SSM
  Parameter Store o Secrets Manager, no como parámetros en texto plano.

```yaml
Metadata:
  AWS::CloudFormation::Interface:
    ParameterGroups:
      - Label: { default: "Core parameters" }
        Parameters: [ProjectName, Environment]
      - Label: { default: "AWS resources" }
        Parameters: [VpcId, PrivateSubnetIds, KmsKeyArn]
      - Label: { default: "App configuration" }
        Parameters: [LambdaMemory, LambdaTimeout]

Parameters:
  ProjectName:
    Type: String
    Description: "Identificador del proyecto usado en nombres y tags."
    AllowedPattern: "^[a-z][a-z0-9-]{2,30}$"

  Environment:
    Type: String
    Description: "Ambiente de despliegue."
    AllowedValues: [dev, qa, prod]
    Default: dev

  VpcId:
    Type: AWS::EC2::VPC::Id
    Description: "VPC donde se despliegan los recursos de red."

  LambdaMemory:
    Type: Number
    Description: "Memoria (MB) asignada a la Lambda."
    Default: 512
```

## 4. Elementos modificables

Todo lo que pueda variar entre despliegues o ambientes debe ser **parametrizable**,
no estar codificado en duro dentro de `Resources`:

- Nombres, tamaños, capacidades, timeouts, memoria, número de particiones.
- Rutas de buckets, prefijos, nombres de tablas.
- Flags de comportamiento (habilitar/deshabilitar features vía `Conditions`).

Usa `Mappings` para valores fijos que dependen del ambiente o la región, y
`Conditions` para incluir/excluir recursos.

```yaml
Mappings:
  EnvSettings:
    dev:  { InstanceType: t3.small,  MinSize: 1, MaxSize: 2 }
    qa:   { InstanceType: t3.medium, MinSize: 1, MaxSize: 3 }
    prod: { InstanceType: m5.large,  MinSize: 2, MaxSize: 6 }

Conditions:
  IsProd: !Equals [!Ref Environment, prod]
```

## 5. Salidas (Outputs)

- Exporta solo lo que otros stacks o procesos necesiten consumir.
- Cada `Output` lleva `Description` y, si es para referencia cruzada, `Export`
  con nombre único que incluya proyecto y ambiente.

```yaml
Outputs:
  DataBucketName:
    Description: "Nombre del bucket de datos."
    Value: !Ref DataBucket
    Export:
      Name: !Sub "${ProjectName}-${Environment}-DataBucketName"
```

## 6. Convenciones por ambiente

- Ambientes soportados: `dev`, `qa`, `prod`, controlados por el parámetro
  `Environment`.
- Las diferencias por ambiente se resuelven con `Mappings` y `Conditions`, no
  con plantillas separadas y duplicadas.
- El ambiente forma parte de nombres físicos, tags y nombres de export.
- Recursos con datos productivos: `DeletionPolicy: Retain` cuando `IsProd`
  (p. ej. buckets, tablas), y protecciones más estrictas en `prod`.

## 7. Buenas prácticas

- **Validar** antes de desplegar: `aws cloudformation validate-template` y, si
  está disponible, `cfn-lint`.
- **Mínimo privilegio** en roles/políticas IAM; nada de `Action: "*"` /
  `Resource: "*"` salvo justificación explícita.
- **Cifrado por defecto**: S3 (SSE-KMS/SSE-S3), RDS, EBS, colas y logs cifrados.
- **Sin secretos en la plantilla**: usa SSM Parameter Store / Secrets Manager y
  referencias dinámicas (`{{resolve:ssm-secure:...}}`).
- **Funciones intrínsecas** en lugar de valores fijos: `!Ref`, `!Sub`, `!GetAtt`,
  `!ImportValue`, `!FindInMap`.
- **`Description`** claro en la plantilla, en cada parámetro y en cada output.
- Habilita `DeletionPolicy` / `UpdateReplacePolicy` en recursos con estado.
- Mantén las plantillas idempotentes y re-desplegables sin efectos colaterales.

## Ubicación de archivos

- Guarda las plantillas en `infra/cloudformation/`.
- Nómbralas en `kebab-case` describiendo el stack, p. ej.
  `data-ingest-stack.yaml` o `network-stack.yaml`.
