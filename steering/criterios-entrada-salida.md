---
inclusion: always
---

# Criterios de Entrada y Salida (Test Gates)

Condiciones para empezar a probar (entry) y para dar por terminada una fase de
prueba o autorizar una liberación (exit). Dan objetividad a la decisión de avanzar.
Aplican a los entornos **dev / qa / prod**; la ronda formal de QA se ejecuta en
`qa`.

## Criterios de entrada (para iniciar pruebas)

Antes de comenzar una ronda de pruebas debe cumplirse:

- [ ] Build/artefacto desplegado y estable en el entorno de prueba (`qa`).
- [ ] Infra provisionada (plantillas aplicadas) y ambiente/datos listos
      (ver `datos-entornos-prueba`).
- [ ] Requisitos/criterios de aceptación disponibles y entendidos.
- [ ] Casos de prueba diseñados y revisados para el alcance de la ronda.
- [ ] Smoke test inicial superado (endpoints/funciones núcleo responden; los jobs
      de datos arrancan).

Si no se cumplen, la ronda no inicia; se documenta el bloqueo.

## Criterios de salida (para cerrar la fase / recomendar liberación)

Para dar la fase por terminada y recomendar avanzar:

- [ ] **≥ 95%** de los casos planificados **ejecutados**.
- [ ] **≥ 90%** de los casos ejecutados **aprobados** (pass rate).
- [ ] **Cero** defectos abiertos de severidad **Crítica** o **Alta**.
- [ ] Defectos Medios/Bajos restantes documentados y aceptados por el responsable.
- [ ] Cobertura de requisitos completa (matriz de trazabilidad sin huecos).
- [ ] Cobertura de código ≥ 80% en la lógica de negocio (ver `estrategia-pruebas`).
- [ ] Resultados y evidencias registrados en el reporte de ejecución.

> ⚠️ COMPLETAR: define quién aprueba la salida (rol) y las excepciones permitidas.

## Suspensión / reanudación

- **Suspender** una ronda si un defecto bloqueante impide seguir probando o el
  ambiente se cae.
- **Reanudar** cuando el bloqueo se resuelve y se repite el smoke test.

## Reglas para el agente

- Al preparar o cerrar una ronda, evalúa estos criterios y repórtalos como lista
  de verificación con su estado real.
- No recomiendes liberar si hay criterios de salida incumplidos; si el usuario
  decide liberar igual, deja constancia de los criterios no cumplidos y del riesgo.
- Sé explícito con los números (pass rate, casos ejecutados, bugs por severidad).
