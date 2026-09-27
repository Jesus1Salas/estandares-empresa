<!--
PLANTILLA: Casos de Prueba
Instrucciones para el agente:
- Deriva los casos de los criterios de aceptación / historia / requisito de entrada.
- Por cada criterio: al menos un caso positivo, uno negativo y los borde relevantes.
- Aplica `convenciones-casos-prueba` (anatomía, IDs TC-<MODULO>-<NNN>, verificables).
- No inventes reglas de negocio no indicadas; si algo es ambiguo, > ⚠️ PENDIENTE: ...
- No uses datos productivos ni PII real (ver `datos-entornos-prueba`).
- Elimina estos comentarios HTML en la salida final.
-->

---
titulo: "Casos de Prueba"
proyecto: "{{PROYECTO}}"
modulo: "{{MODULO}}"
referencia: "{{REFERENCIA_REQUISITO}}"
fecha: "{{FECHA}}"
autor: "{{AUTOR}}"
clasificacion: "Uso interno"
---

# Casos de Prueba — {{MODULO}}

**Proyecto:** {{PROYECTO}} · **Módulo:** {{MODULO}}
**Referencia (historia/requisito):** {{REFERENCIA_REQUISITO}}
**Fecha:** {{FECHA}} · **Autor:** {{AUTOR}}

## Alcance

{{ALCANCE}}
<!-- Qué funcionalidad cubren estos casos y qué queda fuera. -->

## Criterios de aceptación cubiertos

{{LISTA_CRITERIOS}}
<!-- Lista de los criterios de aceptación que originan los casos, con su ID. -->

## Resumen

| Total casos | Positivos | Negativos | Borde | Automatizables |
|------------:|----------:|----------:|------:|---------------:|
| {{TOTAL}} | {{N_POS}} | {{N_NEG}} | {{N_BORDE}} | {{N_AUTO}} |

## Casos de prueba

| ID | Título | Criterio | Precondiciones | Datos | Pasos | Resultado esperado | Prioridad | Tipo | Automatizable |
|----|--------|----------|----------------|-------|-------|--------------------|-----------|------|---------------|
{{TABLA_CASOS}}
<!-- Un caso por fila. IDs TC-<MODULO>-<NNN> (negativos con sufijo -N). Pasos
numerados dentro de la celda usando <br>. Resultado esperado verificable. -->

## Anexo: casos en formato Gherkin (opcional)

<!-- Incluir solo si el proyecto usa BDD. Un Scenario por caso relevante. -->

```gherkin
{{GHERKIN}}
```
