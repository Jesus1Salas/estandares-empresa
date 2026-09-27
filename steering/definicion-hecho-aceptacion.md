---
inclusion: always
---

# Definición de Hecho (DoD) y Criterios de Aceptación

Qué significa que una historia o feature esté "terminada" desde la perspectiva de
calidad, y cómo deben escribirse los criterios de aceptación. Da un estándar común
para cerrar trabajo con confianza.

> ⚠️ COMPLETAR: ajusta la DoD a la realidad del equipo/cliente.

## Definición de Hecho (Definition of Done)

Una historia/feature está "Hecha" cuando cumple TODO lo siguiente:

- [ ] Código revisado (code review aprobado) y fusionado.
- [ ] Pruebas unitarias y de integración pasando; cobertura según
      `estrategia-pruebas`.
- [ ] Todos los criterios de aceptación verificados con casos de prueba.
- [ ] Pruebas de regresión relevantes ejecutadas sin fallos nuevos.
- [ ] Sin defectos abiertos de severidad **Crítica** o **Alta** asociados.
- [ ] Documentación actualizada (funcional/técnica según aplique).
- [ ] Trazabilidad requisito → caso → resultado registrada.
- [ ] Desplegable en el entorno objetivo sin pasos manuales no documentados.

> ⚠️ COMPLETAR: añade/quita ítems según la política real (p. ej. accesibilidad,
> performance, feature flags, aprobación de negocio).

## Criterios de aceptación: cómo se escriben

- **Verificables:** cada criterio debe poder confirmarse como cumplido o no.
- **Orientados al comportamiento**, no a la implementación.
- Formato recomendado (Gherkin):

```gherkin
Given <contexto/precondición>
When <acción>
Then <resultado observable esperado>
```

- O en lista de verificación cuando el flujo sea simple:
  - "El sistema rechaza montos negativos y muestra el mensaje X."

## Relación con las pruebas

- Cada criterio de aceptación genera al menos un caso de prueba (positivo) y sus
  negativos/borde relevantes (ver `convenciones-casos-prueba`).
- Una feature no se marca "Hecha" si algún criterio no está cubierto por un caso
  ejecutado con resultado satisfactorio.

## Reglas para el agente

- Al evaluar si algo está "terminado", contrasta contra esta DoD y reporta cada
  ítem no cumplido.
- Al recibir una historia sin criterios de aceptación claros, ayúdales a
  redactarlos en formato verificable antes de diseñar casos.
- No consideres una feature lista si hay criterios sin cubrir o bugs críticos/altos
  abiertos.
