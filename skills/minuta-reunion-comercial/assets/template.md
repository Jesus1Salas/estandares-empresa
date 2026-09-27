<!--
PLANTILLA: Minuta / Resumen Ejecutivo de Reunión Comercial
Instrucciones para el agente:
- Se rellena a partir de las NOTAS de la reunión que aporta el usuario (generadas
  por IA o manuales). No inventes acuerdos, cifras ni compromisos que no estén en
  las notas; si algo es ambiguo, márcalo como > ⚠️ PENDIENTE: <qué aclarar>.
- Aplica `datos-privacidad-comercial`: incluye solo lo relevante para negocio;
  omite PII innecesaria y NUNCA transcribas credenciales o secretos.
- Aplica `tono-estilo-comercial`: claro, conciso, orientado a acción.
- Cada compromiso tiene responsable y fecha objetivo cuando las notas lo permitan.
- Elimina estos comentarios HTML en la salida final.
-->

---
titulo: "Minuta de Reunión"
cliente: "{{CLIENTE}}"
asunto: "{{ASUNTO}}"
fecha_reunion: "{{FECHA_REUNION}}"
fecha_emision: "{{FECHA_EMISION}}"
redactor: "{{REDACTOR}}"
clasificacion: "Uso interno"
---

# Minuta de Reunión — {{ASUNTO}}

**Cliente:** {{CLIENTE}}
**Fecha de la reunión:** {{FECHA_REUNION}} · **Emitida:** {{FECHA_EMISION}}
**Redactó:** {{REDACTOR}}

## Resumen ejecutivo

{{RESUMEN_EJECUTIVO}}
<!-- 3-6 líneas: de qué trató la reunión, a qué se llegó y qué sigue. Debe poder
leerse solo, sin el resto de la minuta. -->

## Asistentes

| Nombre | Organización | Rol |
|--------|--------------|-----|
{{TABLA_ASISTENTES}}
<!-- Solo los necesarios. Si las notas no listan asistentes, deja > ⚠️ PENDIENTE. -->

## Temas tratados

{{TEMAS_TRATADOS}}
<!-- Puntos discutidos, agrupados por tema. Sintetiza; no transcribas literal. -->

## Acuerdos y decisiones

{{ACUERDOS}}
<!-- Decisiones tomadas de forma explícita. Una idea por viñeta. -->

## Compromisos / próximos pasos

| # | Compromiso | Responsable | Fecha objetivo | Estado |
|---|-----------|-------------|----------------|--------|
{{TABLA_COMPROMISOS}}
<!-- Acción concreta + responsable + fecha. Estado inicial: Pendiente. -->

## Temas abiertos / a decidir

{{TEMAS_ABIERTOS}}
<!-- Dudas o decisiones que quedaron sin cerrar. -->

## Riesgos y observaciones

{{RIESGOS}}
<!-- Señales comerciales relevantes: objeciones, competidores mencionados,
sensibilidad al precio, urgencias, cambios de alcance. Omite si no aplica. -->

## Próxima reunión

{{PROXIMA_REUNION}}
<!-- Fecha/objetivo si se acordó; si no, > ⚠️ PENDIENTE: por confirmar. -->
