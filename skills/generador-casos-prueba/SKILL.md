---
name: generador-casos-prueba
description: >-
  Genera casos de prueba (positivos, negativos y de borde) a partir de criterios
  de aceptación, una historia de usuario o un requisito, aplicando las
  convenciones de QA de la consultora. Produce salida en Markdown y en PDF con
  logo y tipografía corporativa, en tabla y opcionalmente en Gherkin. Úsala cuando
  el usuario pida "genera casos de prueba", "diseña los test cases", "casos para
  esta historia" o similar.
version: 1.0.0
---

# Skill: Generador de Casos de Prueba

Esta skill diseña **casos de prueba** a partir de criterios de aceptación, una
historia de usuario o un requisito. Se genera en dos formatos:

- **Markdown**: `qa/casos-prueba/casos-prueba-<proyecto>-<modulo>-<fecha>.md`
- **PDF**: mismo nombre con extensión `.pdf` (portada, logo y tipografía).

El principio rector es **derivar, no inventar**: los casos salen de los criterios
de entrada. No agregues reglas de negocio no indicadas; lo ambiguo se marca
`> ⚠️ PENDIENTE: <qué aclarar>`.

## Cuándo se activa

Cuando el usuario pida diseñar/generar casos de prueba o test cases a partir de
requisitos, historias o criterios de aceptación.

## Entrada requerida

Al menos uno de: criterios de aceptación, historia de usuario, requisito
funcional, o una referencia a dónde están (archivo, ticket). Si no hay criterios
claros, ayuda a redactarlos en formato verificable antes de generar casos
(ver `definicion-hecho-aceptacion`).

## Steerings que debe respetar

- `convenciones-casos-prueba` — anatomía del caso, nomenclatura de IDs, cobertura
  positivo/negativo/borde, estilo Gherkin. (Se activa con el nombre del archivo.)
- `estrategia-pruebas` — nivel de prueba apropiado y enfoque basado en riesgo.
- `definicion-hecho-aceptacion` — criterios verificables como origen de los casos.
- `datos-entornos-prueba` — datos sintéticos, sin PII real ni secretos.

## Artefactos incluidos

- `assets/template.md` — plantilla de casos (resumen + tabla + anexo Gherkin).
- `assets/pdf-style.css` — estilos corporativos del PDF (estados pass/fail, etc.).
- `assets/logo.svg` — logo por defecto (reemplazable).

Esta skill **no incluye scripts**: el diseño y el PDF los produce el agente.

## Procedimiento (seguir en orden)

### Paso 0 — Analizar la entrada
Lee los criterios/historia/requisito. Identifica módulo, reglas de negocio,
entradas, salidas esperadas y condiciones de borde. Si algo no está definido,
márcalo como pendiente en vez de suponerlo.

### Paso 1 — Preparar carpeta de salida
Crea `qa/casos-prueba/` si no existe.

### Paso 2 — Diseñar los casos
Copia `assets/template.md` al archivo de salida y complétalo:

1. Nombra el archivo `casos-prueba-<proyecto>-<modulo>-<AAAA-MM-DD>.md`
   (minúsculas, sin acentos). El nombre contiene `casos-prueba`, lo que activa el
   steering de convenciones.
2. Por **cada criterio de aceptación**, genera como mínimo:
   - un caso **positivo** (camino feliz),
   - un caso **negativo** (entrada inválida / error esperado),
   - los casos **borde** relevantes (límites, vacíos, máximos, concurrencia).
3. Aplica la anatomía de `convenciones-casos-prueba`: ID `TC-<MODULO>-<NNN>`
   (negativos con sufijo `-N`), título orientado al comportamiento, pasos
   numerados, resultado esperado **verificable**, prioridad y tipo.
4. Enlaza cada caso a su criterio/requisito (columna "Criterio") para que la
   matriz de trazabilidad pueda construirse.
5. Rellena el **resumen** (totales por tipo y automatizables).
6. Si el proyecto usa BDD, completa el anexo **Gherkin**; si no, omítelo.
7. Usa datos de prueba sintéticos; nunca PII real ni secretos.

### Paso 3 — Generar el PDF (lo hace el agente)
1. HTML con **portada** (logo data URI, título "Casos de Prueba", proyecto,
   módulo, referencia, fecha, autor, clasificación) y **cuerpo** (Markdown a HTML).
2. Aplica `assets/pdf-style.css`.
3. Exporta a PDF A4 con `create_artifact` (`kind: pdf`); pie con numeración.
4. Guarda el PDF junto al Markdown.

Si el entorno impide el PDF, entrega el Markdown e infórmalo (alternativa:
`pandoc <archivo>.md -o <archivo>.pdf`).

### Paso 4 — Verificar
- Confirma que existen el `.md` y (si fue posible) el `.pdf`.
- No deben quedar marcadores `{{...}}` sin sustituir.
- Verifica que cada criterio de aceptación tenga al menos un caso positivo y sus
  negativos/borde; reporta los `⚠️ PENDIENTE`.

## Personalización

- **Logo:** reemplaza `assets/logo.svg`.
- **Colores/tipografía:** variables CSS al inicio de `assets/pdf-style.css`.
- **Ruta de salida:** por defecto `qa/casos-prueba/`.
- **Gherkin:** actívalo/omítelo según use BDD el proyecto.
