---
inclusion: fileMatch
fileMatchPattern: '*bug*'
---

# Gestión y Reporte de Defectos (Bugs)

Cómo se reportan, clasifican y gestionan los defectos. Se aplica automáticamente
al trabajar sobre archivos cuyo nombre contiene `bug` y guía cualquier reporte de
defecto que genere el agente.

## Plantilla de reporte de bug

Todo bug incluye:

| Campo | Descripción |
|-------|-------------|
| **ID** | `BUG-<PROYECTO>-<NNN>` |
| **Título** | Síntoma concreto, en una línea |
| **Severidad** | Crítica / Alta / Media / Baja (ver matriz) |
| **Prioridad** | P1 / P2 / P3 / P4 |
| **Entorno** | Ambiente, versión/build, navegador/OS, datos |
| **Precondiciones** | Estado previo necesario |
| **Pasos para reproducir** | Numerados y reproducibles |
| **Resultado actual** | Lo que ocurre (el fallo) |
| **Resultado esperado** | Lo que debería ocurrir |
| **Evidencia** | Captura, log, video (referencia; sin secretos) |
| **Caso de prueba** | ID del caso que lo detectó, si aplica |

## Matriz de severidad

| Severidad | Definición |
|-----------|------------|
| **Crítica** | Caída del sistema, pérdida de datos o bloqueo total sin workaround |
| **Alta** | Función principal inoperante; hay workaround costoso |
| **Media** | Función secundaria afectada; workaround simple |
| **Baja** | Cosmético o menor; sin impacto funcional |

**Severidad** = impacto técnico; **prioridad** = urgencia de negocio para
resolverlo. No siempre coinciden (un cosmético en la home puede ser P2).

## Flujo de estados

```
Nuevo → Asignado → En progreso → Resuelto → En verificación → Cerrado
                                     │
                                     └── Reabierto (si la verificación falla)
```

Estados adicionales: `Rechazado` (no es defecto), `Duplicado`, `Diferido`.

## SLA por severidad

| Severidad | Primera respuesta | Resolución objetivo |
|-----------|-------------------|---------------------|
| Crítica | 2 h | 24 h |
| Alta | 1 día | 3 días |
| Media | 3 días | Sprint actual |
| Baja | Backlog | Según capacidad |

## Dónde se registran

Los defectos se gestionan como **archivos en el repositorio** (Markdown/CSV), no en
una herramienta externa:

- Ubicación por defecto: `qa/bugs/` (Markdown por bug o un `bugs.csv` consolidado).
- Nombre de archivo por bug: `bug-<proyecto>-<NNN>.md` (contiene `bug`, lo que
  activa este steering).
- Los IDs `BUG-<PROYECTO>-<NNN>` son los que enlazan los defectos con los casos de
  prueba en la matriz de trazabilidad y en el reporte de ejecución.

## Reglas de calidad del reporte

- Un bug = un defecto. No agrupes varios problemas en un ticket.
- Los pasos deben permitir reproducir sin conocimiento previo.
- Evidencia sin datos sensibles: enmascara PII y **nunca** incluyas credenciales
  ni tokens en capturas o logs (ver `datos-entornos-prueba`).
- Distingue defecto de mejora: si es un cambio de alcance, es un Change Request.

## Reglas para el agente

- Al redactar un bug, completa la plantilla y clasifica severidad y prioridad.
- Si faltan datos para reproducir, pídelos o márcalos `> ⚠️ PENDIENTE:`.
- Enmascara información sensible en la evidencia y avisa si detectas secretos.
