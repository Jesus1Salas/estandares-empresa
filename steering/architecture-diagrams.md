# Architecture Diagrams

Convenciones para crear y validar diagramas de arquitectura en este proyecto.
Aplica siempre que generes, edites o revises un diagrama de arquitectura.
Este proyecto usa exclusivamente AWS como proveedor de nube.

## Principios

- El diagrama debe contar una historia: de dónde vienen los datos, cómo fluyen y
  dónde terminan. Prioriza la legibilidad del flujo sobre la cantidad de detalle.
- Usa iconos oficiales y correctos del proveedor (AWS) para cada servicio; no
  sustituyas un servicio por un icono genérico o de otro servicio.
- Evita caracteres especiales problemáticos en etiquetas, nombres de nodos e
  identificadores (ver criterio 3).
- Documenta los parámetros relevantes (región, entorno, tipo de instancia,
  tamaños, protocolos/puertos) directamente en el diagrama o en una leyenda.
- Identifica explícitamente fuentes (orígenes) y destinos (sumideros) de datos.
- Guarda el archivo en la ubicación correcta del repositorio.

## Ubicación y formato de archivo

- Guarda los diagramas en `docs/diagrams/`.
- Nombra el archivo en `kebab-case` describiendo el contenido, p. ej.
  `ingesta-datos-ventas.drawio` o `arquitectura-plataforma-analitica.svg`.
- Formato preferido editable: draw.io (`.drawio`) o Mermaid embebido en Markdown.
  Exporta además a `.svg` o `.png` cuando se necesite para documentación.

## Iconos AWS

- Usa el conjunto oficial de iconos de arquitectura de AWS y el servicio exacto
  (p. ej. S3, Lambda, Glue, Kinesis, Redshift, RDS, DynamoDB, Step Functions).
- Mantén coherencia visual: un mismo servicio se representa siempre con el mismo
  icono a lo largo del diagrama.
- Agrupa recursos dentro de sus fronteras lógicas: Cuenta / Región / VPC /
  Subred / AZ, usando contenedores anidados en vez de superponer cajas.

## Caracteres especiales problemáticos

Para evitar errores de parseo/renderizado (especialmente en Mermaid y draw.io):

- En **identificadores de nodo** usa solo `[A-Za-z0-9_]`. No uses espacios,
  guiones, ni palabras reservadas (`end`, `class`, `subgraph`).
- En **etiquetas** entrecomilla el texto cuando contenga `:`, `()`, `/`, `&`,
  acentos o caracteres no ASCII. Escapa `<`, `>`, `&` como `&lt;`, `&gt;`,
  `&amp;` en XML de draw.io.
- Evita emojis y símbolos decorativos dentro de nodos y aristas.

## Parámetros a documentar

Cuando apliquen, anota en el diagrama o en una leyenda:

- Región y entorno (dev / qa / prod).
- Protocolos y puertos de las conexiones relevantes.
- Formato y volumen de datos (p. ej. Parquet, batch diario, streaming).
- Tipos/tamaños de recursos clave (instancias, memoria, particiones).

## Criterios de validación

Antes de dar por terminado un diagrama de arquitectura, verifica que cumple
TODOS los siguientes criterios:

- [ ] Diagrama muestra flujo claro de datos
- [ ] Iconos AWS correctos
- [ ] Sin caracteres especiales problemáticos
- [ ] Parámetros documentados
- [ ] Fuentes y destinos identificados
- [ ] Archivo guardado en ubicación correcta

Si algún criterio no se cumple, corrige el diagrama antes de entregarlo. Reporta
al usuario cualquier criterio que no pueda cumplirse y por qué.
