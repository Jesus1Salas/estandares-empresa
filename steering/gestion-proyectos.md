---
inclusion: always
---

# Gestión de Proyectos / PMO

Convenciones para la gestión y el seguimiento de proyectos de la consultora:
estados, reportes de avance, riesgos, cambios de alcance y entregas. Da un marco
común para comunicar el estado de forma consistente.

## Metodología y cadencia

- **Metodología:** **Scrum**.
- **Cadencia por defecto:** sprints de `<2 semanas>` (ajustable por proyecto);
  reporte de avance al cierre de cada sprint.
- **Ceremonias Scrum:** planificación de sprint (sprint planning), daily standup,
  revisión de sprint (sprint review) y retrospectiva.
- **Roles Scrum:** Product Owner, Scrum Master y equipo de desarrollo.
- **Artefactos:** product backlog, sprint backlog e incremento.

## Estados de proyecto

| Estado | Significado |
|--------|-------------|
| **Iniciación** | Kickoff, alcance y equipo definidos |
| **En ejecución** | Desarrollo/entrega en curso |
| **En riesgo** | Desviación de alcance, plazo o presupuesto que requiere atención |
| **Bloqueado** | Detenido por una dependencia o decisión pendiente |
| **En pausa** | Suspendido temporalmente por acuerdo |
| **Cerrado** | Entregado y aceptado formalmente |

## Salud del proyecto (semáforo)

| Color | Criterio |
|-------|----------|
| 🟢 Verde | En alcance, plazo y presupuesto |
| 🟡 Amarillo | Desviación menor con plan de mitigación |
| 🔴 Rojo | Desviación grave; requiere escalamiento y decisión |

## Reporte de avance (estructura)

Todo reporte de estado incluye:

- **Resumen ejecutivo** y semáforo de salud (verde/amarillo/rojo).
- **Avance vs. plan:** hitos completados, en curso y próximos.
- **Logros del periodo** y **plan para el siguiente**.
- **Riesgos e issues** abiertos, con responsable y acción.
- **Decisiones/dependencias pendientes** del cliente.
- **Alcance y cambios:** change requests en curso.

## Gestión de riesgos

- Cada riesgo se registra con: descripción, **probabilidad**, **impacto**,
  **respuesta** (mitigar/transferir/aceptar/evitar) y **responsable**.
- Se priorizan por probabilidad × impacto; los altos se escalan.
- Un riesgo materializado se convierte en **issue** con plan de acción.

## Gestión de cambios (Change Requests)

- Todo cambio de alcance se gestiona por **CR formal**: descripción, impacto en
  **alcance, plazo y costo**, y aprobación del cliente antes de ejecutarlo.
- No se ejecuta trabajo fuera de alcance sin CR aprobado.
- Los CR se referencian en el reporte de avance y en la trazabilidad del proyecto.

## Hitos del ciclo de vida

- **Kickoff:** objetivos, alcance, equipo, plan, riesgos iniciales, canales de
  comunicación y criterios de aceptación.
- **Seguimiento:** reportes de avance según la cadencia.
- **Cierre:** entrega final, aceptación formal, lecciones aprendidas y handover
  (documentación técnica y operativa; ver skill de manual técnico si aplica).

## Nomenclatura y ubicación

> ⚠️ COMPLETAR con la estructura real del repositorio/gestor documental.

- Documentos de gestión en `proyectos/<cliente>-<proyecto>/`.
- Reportes: `reporte-avance-<proyecto>-<AAAA-MM-DD>.md`.
- Actas/minutas de reuniones: usan la skill `minuta-reunion-comercial` cuando sean
  comerciales, o el mismo formato para reuniones de proyecto.

## Reglas para el agente

- Al reportar estado, usa la estructura anterior y un semáforo de salud honesto.
- No marques trabajo fuera de alcance como "hecho" sin un CR aprobado; señálalo.
- Registra riesgos con probabilidad, impacto y responsable; escala los altos.
- Sé transparente con desviaciones de plazo/alcance/costo; no las ocultes.
- No comprometas plazos ni recursos del cliente sin confirmación (coordina con
  `legal-cumplimiento-comercial` para compromisos contractuales).
