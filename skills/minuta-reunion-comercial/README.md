# Skill: Minuta / Resumen Ejecutivo de Reunión Comercial

Transforma las **notas de una reunión** (generadas por IA de transcripción o
tomadas a mano) en una **minuta** estructurada, y la exporta en **Markdown** y
**PDF** (con logo y tipografía corporativa).

## Cómo se usa

Aporta las notas (pegadas en el chat o como archivo) y pídele al agente:

- "Genera la minuta de esta reunión con ACME a partir de estas notas: ..."
- "Resume estas notas de la reunión y sácame los compromisos"
- "Haz el acta del kickoff con el cliente X" (adjuntando la transcripción)

El agente activará la skill y seguirá el procedimiento del `SKILL.md`.

## Entrada requerida

Las **notas de la reunión**. Pueden ser:

- Texto pegado en el chat.
- Un archivo adjunto o ruta (`.md`, `.txt`, transcripción de IA, export de una
  herramienta de reuniones).
- Notas manuales, aunque estén desordenadas.

Si no hay notas, el agente las pide: no inventa una minuta de ejemplo.

## Qué hace, paso a paso

1. **Lee las notas** e identifica cliente, asunto, fecha, asistentes, temas,
   decisiones y acciones.
2. **Sintetiza la minuta** en `comercial/minutas/` con la plantilla
   `assets/template.md`: resumen ejecutivo, acuerdos, compromisos (responsable +
   fecha), temas abiertos y riesgos comerciales.
3. **Genera el PDF** con portada, logo y estilos.
4. **Verifica**: cada compromiso con responsable y fecha (o marcado como
   pendiente); reporta lo que falta por confirmar.

## Contenido de la minuta

Resumen ejecutivo · Asistentes · Temas tratados · Acuerdos y decisiones ·
Compromisos / próximos pasos (con responsable, fecha y estado) · Temas abiertos ·
Riesgos y observaciones · Próxima reunión.

## Privacidad

- Incluye solo lo relevante para negocio; omite PII innecesaria.
- **Nunca** transcribe credenciales o secretos que aparezcan en las notas: los
  omite y lo advierte (ver steering `datos-privacidad-comercial`).
- Clasificación por defecto "Uso interno"; se sube a "Confidencial" si aplica.

## Steerings relacionados

- `datos-privacidad-comercial`, `tono-estilo-comercial`
- `glosario-nomenclatura-comercial`, `perfil-empresa-servicios`

## Personalización

| Qué | Cómo |
|-----|------|
| Logo | Reemplaza `assets/logo.svg` (SVG/PNG) |
| Colores / tipografía | Variables CSS al inicio de `assets/pdf-style.css` |
| Ruta de salida | Por defecto `comercial/minutas/`; pide otra al agente |

## Requisitos

- Ninguna dependencia externa: el agente sintetiza y genera el PDF. Si el entorno
  impide el PDF, se entrega el Markdown (alternativa: `pandoc`).

## Estructura de la skill

```
minuta-reunion-comercial/
├── SKILL.md                 # Instrucciones que sigue el agente
├── README.md                # Este archivo
└── assets/
    ├── template.md          # Plantilla de la minuta
    ├── pdf-style.css        # Estilos del PDF
    └── logo.svg             # Logo por defecto (reemplazable)
```
