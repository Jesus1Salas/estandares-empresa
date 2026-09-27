---
name: minuta-reunion-comercial
description: >-
  Genera una minuta o resumen ejecutivo de una reunión comercial a partir de las
  notas de la reunión (generadas por IA o tomadas manualmente), destacando
  acuerdos, compromisos con responsable y fecha, y próximos pasos. Produce salida
  en Markdown y en PDF con logo y tipografía corporativa. Úsala cuando el usuario
  pida "genera la minuta de esta reunión", "resume estas notas de la reunión",
  "haz el acta de la reunión con el cliente" o similar.
version: 1.0.0
---

# Skill: Minuta / Resumen Ejecutivo de Reunión Comercial

Esta skill transforma las **notas de una reunión** (generadas por IA de
transcripción o tomadas a mano) en una **minuta** estructurada con formato
corporativo. Se genera en dos formatos:

- **Markdown**: `comercial/minutas/minuta-<cliente>-<asunto>-<fecha>.md`
- **PDF**: mismo nombre con extensión `.pdf` (portada, logo y tipografía).

El principio rector es **sintetizar sin inventar**: la minuta se basa únicamente
en las notas de entrada. No agregues acuerdos, cifras ni compromisos que no estén
en las notas; lo ambiguo se marca `> ⚠️ PENDIENTE: <qué aclarar>`.

## Cuándo se activa

Cuando el usuario pida generar una minuta, acta o resumen ejecutivo de una
reunión comercial a partir de notas o de una transcripción.

## Entrada requerida

Las **notas de la reunión**, que el usuario aporta de alguna de estas formas:

- Pegadas directamente en el chat.
- Como archivo adjunto o ruta a un archivo (`.md`, `.txt`, transcripción de IA,
  export de herramienta de reuniones).
- Como notas manuales, aunque estén desordenadas o en viñetas sueltas.

Si el usuario no proporcionó notas, pídelas antes de continuar. No generes una
minuta "de ejemplo" con datos inventados.

## Steerings que debe respetar

- `datos-privacidad-comercial` — incluir solo lo relevante; omitir PII innecesaria
  y NUNCA transcribir credenciales o secretos.
- `tono-estilo-comercial` — redacción clara, concisa y orientada a la acción.
- `glosario-nomenclatura-comercial` — nombre del archivo y términos.
- `perfil-empresa-servicios` — nombres correctos de servicios si se mencionan.

## Artefactos incluidos

- `assets/template.md` — plantilla de la minuta (resumen, acuerdos, compromisos...).
- `assets/pdf-style.css` — estilos corporativos del PDF.
- `assets/logo.svg` — logo por defecto (reemplazable).

Esta skill **no incluye scripts**: la síntesis y el PDF los produce el agente.

## Procedimiento (seguir en orden)

### Paso 0 — Obtener y leer las notas
Recibe las notas del usuario (texto pegado o archivo). Si es un archivo, léelo.
Identifica: cliente, asunto/objetivo de la reunión, fecha, asistentes, temas,
decisiones y acciones. Si falta cliente/asunto/fecha, pídelos o infiérelos del
propio contenido; no los inventes.

### Paso 1 — Preparar carpeta de salida
Crea `comercial/minutas/` si no existe.

### Paso 2 — Sintetizar la minuta
Copia `assets/template.md` al archivo de salida y complétalo:

1. Nombra el archivo según `glosario-nomenclatura-comercial`:
   `minuta-<cliente>-<asunto>-<AAAA-MM-DD>.md` (cliente/asunto en minúsculas, sin
   acentos ni espacios; usa la fecha de la reunión).
2. Redacta un **resumen ejecutivo** de 3-6 líneas que se entienda solo.
3. Extrae **acuerdos/decisiones** explícitos y sepáralos de los **temas abiertos**.
4. Convierte las acciones en **compromisos** con responsable y fecha objetivo
   cuando las notas lo permitan; estado inicial "Pendiente". Si falta responsable
   o fecha, márcalo `> ⚠️ PENDIENTE:`.
5. Incluye **riesgos/observaciones comerciales** solo si aparecen (objeciones,
   competidores, sensibilidad al precio, urgencias, cambios de alcance).
6. Aplica privacidad: omite PII y comentarios personales irrelevantes; si detectas
   un secreto/credencial en las notas, no lo transcribas y avísalo.
7. No dejes marcadores `{{...}}` sin sustituir.

### Paso 3 — Generar el PDF (lo hace el agente)
1. HTML con **portada** (logo data URI, título "Minuta de Reunión", cliente,
   asunto, fecha de la reunión, redactor, clasificación) y **cuerpo** (Markdown a
   HTML).
2. Aplica `assets/pdf-style.css`.
3. Exporta a PDF A4 con `create_artifact` (`kind: pdf`); pie con numeración.
4. Guarda el PDF junto al Markdown.

Si el entorno impide el PDF, entrega el Markdown e infórmalo (alternativa:
`pandoc <archivo>.md -o <archivo>.pdf`).

### Paso 4 — Verificar
- Confirma que existen el `.md` y (si fue posible) el `.pdf`.
- No deben quedar marcadores `{{...}}` sin sustituir.
- Todo compromiso debería tener responsable y fecha, o un `⚠️ PENDIENTE`.
- Reporta al usuario los `⚠️ PENDIENTE` (p. ej. responsables/fechas por confirmar)
  y sugiérele validar la minuta antes de enviarla al cliente.

## Personalización

- **Logo:** reemplaza `assets/logo.svg`.
- **Colores/tipografía:** variables CSS al inicio de `assets/pdf-style.css`.
- **Ruta de salida:** por defecto `comercial/minutas/`.
- **Clasificación:** por defecto "Uso interno"; súbela a "Confidencial" si la
  reunión trató información sensible del cliente.
