<!--
PLANTILLA: Matriz de Trazabilidad de Requisitos (RTM)
Instrucciones para el agente:
- Mapea cada requisito/criterio de aceptación a sus casos de prueba y al resultado.
- La fuente son los requisitos y los casos existentes; no inventes coberturas ni
  resultados. Si un requisito no tiene caso, márcalo SIN COBERTURA (es un hallazgo).
- Usa IDs consistentes: requisitos (REQ-/CA-) y casos (TC-<MODULO>-<NNN>).
- Elimina estos comentarios HTML en la salida final.
-->

---
titulo: "Matriz de Trazabilidad"
proyecto: "{{PROYECTO}}"
referencia: "{{REFERENCIA}}"
fecha: "{{FECHA}}"
autor: "{{AUTOR}}"
clasificacion: "Uso interno"
---

# Matriz de Trazabilidad de Requisitos (RTM) — {{PROYECTO}}

**Proyecto:** {{PROYECTO}} · **Referencia:** {{REFERENCIA}}
**Fecha:** {{FECHA}} · **Autor:** {{AUTOR}}

## Resumen de cobertura

| Métrica | Valor |
|---------|------:|
| Requisitos / criterios totales | {{TOTAL_REQ}} |
| Requisitos con al menos un caso | {{REQ_CUBIERTOS}} |
| Requisitos SIN cobertura | {{REQ_SIN_COBERTURA}} |
| **% Cobertura de requisitos** | **{{PCT_COBERTURA}}** |
| Casos de prueba totales | {{TOTAL_CASOS}} |
| Casos huérfanos (sin requisito) | {{CASOS_HUERFANOS}} |

## Matriz requisito → caso → resultado

| Req. ID | Requisito / Criterio | Prioridad | Caso(s) de prueba | Tipo | Último resultado | Defecto(s) | Cobertura |
|---------|----------------------|-----------|-------------------|------|------------------|-----------|-----------|
{{TABLA_RTM}}
<!-- Una fila por requisito (o por par requisito-caso). "Cobertura": Cubierto /
SIN COBERTURA. "Último resultado": Pass / Fail / Bloqueado / No ejecutado / N/A.
Defecto(s): IDs BUG-... asociados si el caso falló. -->

## Requisitos sin cobertura (hallazgos)

{{LISTA_SIN_COBERTURA}}
<!-- Lista de requisitos sin ningún caso de prueba. Es un hallazgo a resolver
antes de declarar cobertura completa. Si no hay, indica "Ninguno". -->

## Casos huérfanos (sin requisito asociado)

{{LISTA_HUERFANOS}}
<!-- Casos que no trazan a ningún requisito: revisar si sobran o si falta el
requisito. Si no hay, indica "Ninguno". -->

## Observaciones

{{OBSERVACIONES}}
<!-- Notas sobre trazabilidad bidireccional, requisitos volátiles, dependencias. -->
