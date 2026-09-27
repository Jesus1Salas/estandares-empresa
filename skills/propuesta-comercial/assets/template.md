<!--
PLANTILLA: Propuesta Comercial / SOW
Instrucciones para el agente:
- Sustituye todos los marcadores {{...}} por contenido REAL del brief y de los steerings.
- No dejes marcadores sin resolver. Si un dato no existe, usa: > ⚠️ PENDIENTE: <motivo>.
- Respeta el orden de secciones del steering `estructura-propuestas`.
- Aplica la voz del steering `tono-estilo-comercial`. No expongas costos internos ni márgenes.
- Elimina estos comentarios HTML en la salida final.
-->

---
titulo: "Propuesta Comercial"
cliente: "{{CLIENTE}}"
proyecto: "{{PROYECTO}}"
propuesta_id: "{{PROPUESTA_ID}}"
fecha: "{{FECHA}}"
validez: "{{VALIDEZ}}"
autor: "{{AUTOR}}"
clasificacion: "Confidencial"
---

# Propuesta Comercial

**Cliente:** {{CLIENTE}}
**Proyecto:** {{PROYECTO}}
**Propuesta N.º:** {{PROPUESTA_ID}} · **Fecha:** {{FECHA}} · **Validez:** {{VALIDEZ}}

## 1. Resumen ejecutivo

{{RESUMEN_EJECUTIVO}}
<!-- 1-3 párrafos centrados en el cliente: su problema, la solución propuesta y el
valor esperado. Debe entenderse sin leer el resto del documento. -->

## 2. Entendimiento del problema

{{ENTENDIMIENTO_PROBLEMA}}
<!-- Demuestra que comprendimos la necesidad, desde la perspectiva del cliente. -->

## 3. Objetivos y resultados esperados

{{OBJETIVOS}}
<!-- Qué se logra, medible cuando sea posible. -->

## 4. Alcance

**Incluye:**

{{ALCANCE_INCLUYE}}

**No incluye (fuera de alcance):**

{{ALCANCE_EXCLUYE}}
<!-- Subsección obligatoria. Si falta info, deja > ⚠️ PENDIENTE: ... -->

## 5. Enfoque y metodología

{{ENFOQUE_METODOLOGIA}}
<!-- Fases, metodología y prácticas. Deriva de las líneas de servicio reales. -->

## 6. Entregables

| # | Entregable | Descripción | Fase |
|---|-----------|-------------|------|
{{TABLA_ENTREGABLES}}
<!-- Entregables como sustantivos concretos, no actividades vagas. -->

## 7. Plan de trabajo y cronograma

| Fase | Actividades principales | Duración estimada | Hito |
|------|-------------------------|-------------------|------|
{{TABLA_CRONOGRAMA}}

## 8. Equipo propuesto

| Rol | Perfil / Seniority | Dedicación |
|-----|--------------------|------------|
{{TABLA_EQUIPO}}
<!-- Roles y seniority; NUNCA costos internos por persona. -->

## 9. Supuestos y dependencias

{{SUPUESTOS}}
<!-- Condiciones asumidas y qué necesitamos del cliente (accesos, insumos, disponibilidad). -->

## 10. Inversión

{{INVERSION}}
<!-- Precio al cliente según `politica-precios-cotizacion`. Puede remitir a la
cotización detallada anexa. Moneda explícita, impuestos y condiciones. Sin
tarifas internas ni márgenes. -->

## 11. Condiciones comerciales

- **Validez de la oferta:** {{VALIDEZ}}
- **Forma de pago:** {{FORMA_PAGO}}
- **Moneda:** {{MONEDA}}
- **Impuestos:** {{IMPUESTOS}}
- **Términos legales:** {{TERMINOS_LEGALES}}
<!-- Inserta cláusulas estándar según `legal-cumplimiento-comercial`. Marca lo que
requiera revisión legal con > ⚠️ REQUIERE REVISIÓN LEGAL. -->

## 12. Próximos pasos

{{PROXIMOS_PASOS}}
<!-- Cómo aceptar y arrancar. Cierra con un llamado a la acción claro. -->

## 13. Anexos

{{ANEXOS}}
<!-- Opcional: cotización detallada, casos de éxito (solo con permiso de mención),
CVs resumidos. Omite la sección si no aplica. -->
