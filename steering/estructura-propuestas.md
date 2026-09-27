---
inclusion: fileMatch
fileMatchPattern: '*propuesta*'
---

# Estructura Estándar de Propuestas Comerciales

Define las secciones obligatorias y el orden de toda propuesta comercial o SOW
(Statement of Work) de la consultora. Se aplica automáticamente al trabajar sobre
archivos cuyo nombre contiene `propuesta` y lo usa la skill
`propuesta-comercial`.

## Secciones obligatorias (en este orden)

1. **Portada** — logo, título "Propuesta comercial", nombre del cliente, nombre
   del proyecto, fecha, número/identificador de propuesta y validez de la oferta.
2. **Resumen ejecutivo** — 1-3 párrafos: el problema del cliente, la solución
   propuesta y el valor esperado. Debe entenderse sin leer el resto.
3. **Entendimiento del problema / contexto** — demuestra que comprendimos la
   necesidad del cliente. Se redacta desde su perspectiva.
4. **Objetivos y resultados esperados** — qué se logra, en términos medibles
   cuando sea posible.
5. **Alcance** — dos subsecciones claras:
   - **Incluye:** qué se entrega.
   - **No incluye (fuera de alcance):** qué queda explícitamente fuera. Esta
     subsección es obligatoria; protege a ambas partes.
6. **Enfoque y metodología** — cómo se ejecutará (fases, metodología, prácticas).
7. **Entregables** — lista concreta de artefactos con su descripción.
8. **Plan de trabajo y cronograma** — fases, hitos y duración estimada.
9. **Equipo propuesto** — roles y perfiles (seniority), sin exponer costos
   internos por persona.
10. **Supuestos y dependencias** — condiciones que asumimos y qué necesitamos del
    cliente (accesos, disponibilidad, insumos).
11. **Inversión / propuesta económica** — precio según `politica-precios-cotizacion`.
    Puede referenciar una cotización detallada anexa.
12. **Condiciones comerciales** — validez de la oferta, forma de pago, moneda,
    impuestos, y remisión a términos legales.
13. **Próximos pasos** — cómo aceptar y arrancar; llamado a la acción claro.
14. **Anexos** (opcional) — cotización detallada, casos de éxito, CVs resumidos.

## Reglas de contenido

- El **resumen ejecutivo** siempre habla primero del cliente, no de la consultora.
- El **alcance** debe incluir SIEMPRE la subsección "No incluye". Si el agente no
  tiene datos para poblarla, la deja con `> ⚠️ PENDIENTE:` en vez de omitirla.
- Los **entregables** se enuncian como sustantivos concretos ("Documento de
  arquitectura", "Pipeline de ingesta desplegado"), no como actividades vagas.
- Los **supuestos** convierten riesgos en condiciones explícitas ("Se asume que
  el cliente provee accesos a la cuenta AWS en la semana 1").
- No se exponen tarifas internas, márgenes ni costos por recurso: solo el precio
  al cliente (ver `politica-precios-cotizacion`).
- Toda propuesta indica su **validez** (p. ej. 30 días) y un **identificador**.

## Estilo

- Sigue el steering `tono-estilo-comercial`.
- Datos de empresa y servicios provienen de `perfil-empresa-servicios`; no se
  inventan capacidades.

## Reglas para el agente

- Al generar o editar una propuesta, respeta este orden de secciones.
- No elimines secciones obligatorias; si falta información, márcala como
  `> ⚠️ PENDIENTE: <qué falta>`.
- Personaliza cada sección al cliente concreto; nunca dejes texto de plantilla
  genérico ("[nombre del cliente]") en la salida final.
