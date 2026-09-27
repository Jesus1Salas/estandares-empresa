---
name: propuesta-comercial
description: >-
  Genera una propuesta comercial o SOW completo para un cliente a partir de un
  brief o requerimiento, siguiendo la estructura y el tono comercial de la
  consultora. Produce salida en Markdown y en PDF con portada, logo y tipografía
  corporativa. Úsala cuando el usuario pida "crear una propuesta", "arma una
  propuesta comercial", "genera un SOW", "propuesta para <cliente>" o similar.
version: 1.0.0
---

# Skill: Propuesta Comercial / SOW

Esta skill produce una **propuesta comercial** (o SOW) con formato corporativo
consistente, a partir de un brief del cliente. Se genera en dos formatos:

- **Markdown**: `comercial/propuestas/propuesta-<cliente>-<proyecto>-<fecha>.md`
- **PDF**: mismo nombre con extensión `.pdf` (portada, logo y tipografía).

El principio rector es **personalizar y autocompletar**: adapta cada sección al
cliente concreto usando el brief y los steerings comerciales. No dejes texto de
plantilla genérico. Cuando un dato no exista tras revisar el brief, márcalo como
`> ⚠️ PENDIENTE: <qué falta>` en vez de inventarlo.

## Cuándo se activa

Cuando el usuario pida crear, redactar o actualizar una propuesta comercial o un
SOW para un cliente/oportunidad.

## Steerings que debe respetar

Esta skill se apoya en los steerings del área comercial (se aplican solos, pero
tenlos presentes):

- `estructura-propuestas` — orden y secciones obligatorias (incluye "No incluye").
- `tono-estilo-comercial` — voz de marca, tratamiento, formato.
- `perfil-empresa-servicios` — servicios y capacidades reales (no inventar).
- `politica-precios-cotizacion` — cómo expresar la inversión; sin costos internos.
- `legal-cumplimiento-comercial` — cláusulas estándar y qué requiere revisión legal.
- `datos-privacidad-comercial` — manejo de PII y confidencialidad.
- `glosario-nomenclatura-comercial` — nombres de servicio y nombre del archivo.

## Artefactos incluidos

- `assets/template.md` — plantilla de la propuesta con todas las secciones.
- `assets/pdf-style.css` — estilos corporativos del PDF.
- `assets/logo.svg` — logo por defecto (reemplazable por el del cliente/consultora).

Esta skill **no incluye scripts**: la redacción y el PDF los produce el agente.

## Procedimiento (seguir en orden)

### Paso 0 — Reunir el brief
Necesitas, como mínimo: cliente, proyecto/oportunidad, problema o necesidad,
objetivos, alcance deseado, restricciones (plazo, presupuesto) y modelo de precio
preferido. Si el usuario no lo dio, pídelo o infiérelo del contexto disponible.
No inventes datos del cliente.

### Paso 1 — Preparar carpeta de salida
Crea `comercial/propuestas/` si no existe.

### Paso 2 — Redactar la propuesta
Copia `assets/template.md` al archivo de salida y complétalo:

1. Nombra el archivo según `glosario-nomenclatura-comercial`:
   `propuesta-<cliente>-<proyecto>-<AAAA-MM-DD>.md` (cliente/proyecto en
   minúsculas, sin acentos ni espacios).
2. Rellena el front-matter (cliente, proyecto, id de propuesta, fecha, validez,
   autor, clasificación "Confidencial").
3. Completa TODAS las secciones en el orden de `estructura-propuestas`. El
   **resumen ejecutivo** habla primero del cliente. El **alcance** SIEMPRE
   incluye "No incluye".
4. En **Inversión**, aplica `politica-precios-cotizacion`: precio al cliente,
   moneda, impuestos y validez. No expongas tarifas internas ni márgenes. Si hay
   una cotización detallada, remite a ella como anexo.
5. En **Condiciones comerciales**, inserta las cláusulas estándar de
   `legal-cumplimiento-comercial` y marca `> ⚠️ REQUIERE REVISIÓN LEGAL` lo que
   corresponda.
6. Usa la voz de `tono-estilo-comercial`: honesta, sin superlativos vacíos ni
   garantías de resultado. Cierra con próximos pasos accionables.
7. Sustituye TODOS los marcadores `{{...}}`. Lo no inferible → `> ⚠️ PENDIENTE:`.

### Paso 3 — Generar el PDF (lo hace el agente)
Genera el PDF a partir del Markdown, sin scripts externos:

1. Construye un HTML con **portada** (logo embebido como data URI, título
   "Propuesta Comercial", cliente, proyecto, N.º de propuesta, fecha, validez,
   autor y clasificación desde el front-matter) y **cuerpo** (Markdown a HTML).
2. Aplica `assets/pdf-style.css`.
3. Renderiza a PDF A4 con `create_artifact` (`kind: pdf`) o el mecanismo de
   exportación disponible; pie con numeración de páginas.
4. Guarda el PDF junto al Markdown, con el mismo nombre base.

Si el entorno impide producir el PDF, entrega igual el Markdown e infórmalo,
ofreciendo como alternativa `pandoc <archivo>.md -o <archivo>.pdf`.

### Paso 4 — Verificar
- Confirma que existen el `.md` y (si fue posible) el `.pdf`.
- No deben quedar marcadores `{{...}}` sin sustituir.
- Lista al usuario las secciones `⚠️ PENDIENTE` y las marcadas para revisión legal.
- Recuerda que precios y textos legales definitivos requieren validación interna.

## Personalización

- **Logo:** reemplaza `assets/logo.svg` (SVG/PNG); el agente lo embebe en portada.
- **Colores/tipografía:** edita las variables CSS al inicio de `assets/pdf-style.css`.
- **Ruta de salida:** por defecto `comercial/propuestas/`; indícale otra al agente.
