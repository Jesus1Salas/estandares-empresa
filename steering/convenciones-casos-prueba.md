---
inclusion: fileMatch
fileMatchPattern: '*casos-prueba*'
---

# Convenciones de Casos de Prueba

Formato y nomenclatura estándar de los casos de prueba. Se aplica automáticamente
al trabajar sobre archivos cuyo nombre contiene `casos-prueba` y lo usa la skill
`generador-casos-prueba`.

## Anatomía de un caso de prueba

Todo caso incluye, como mínimo:

| Campo | Descripción |
|-------|-------------|
| **ID** | Identificador único (ver nomenclatura abajo) |
| **Título** | Qué verifica, en una línea, orientado al comportamiento |
| **Requisito / criterio** | Referencia al requisito o criterio de aceptación que cubre |
| **Precondiciones** | Estado necesario antes de ejecutar |
| **Datos de prueba** | Entradas concretas (o referencia al set de datos) |
| **Pasos** | Acciones numeradas, una por línea |
| **Resultado esperado** | Salida/estado observable y verificable |
| **Prioridad** | Alta / Media / Baja (según riesgo e impacto) |
| **Tipo** | Positivo / Negativo / Borde |
| **Automatizable** | Sí / No / Ya automatizado |

## Nomenclatura de IDs

Formato: `TC-<MODULO>-<NNN>`

- `TC` = Test Case. Para casos negativos usa `TC-<MODULO>-<NNN>-N`.
- `MODULO` en mayúsculas y corto (`LOGIN`, `PAGOS`, `API`).
- `NNN` correlativo de tres dígitos.
- Ejemplos: `TC-LOGIN-001`, `TC-PAGOS-014-N`.

## Cobertura mínima por funcionalidad

Para cada criterio de aceptación, genera al menos:

- Un caso **positivo** (camino feliz).
- Un caso **negativo** (entrada inválida, error esperado).
- Los casos **borde** relevantes (límites, vacíos, máximos, concurrencia).

## Estilo BDD (Gherkin) opcional

Cuando el proyecto use BDD, redacta en Gherkin:

```gherkin
Feature: Inicio de sesión
  Scenario: Credenciales válidas
    Given un usuario registrado con correo "demo@ejemplo.com"
    When inicia sesión con la contraseña correcta
    Then accede a su panel principal
```

- Un `Scenario` por caso; usa `Scenario Outline` con `Examples` para datos múltiples.
- Mantén los pasos declarativos (qué), no imperativos de UI (cómo), salvo E2E.

## Reglas de redacción

- El resultado esperado debe ser **verificable** y sin ambigüedad ("se muestra el
  mensaje 'Saldo insuficiente'", no "funciona bien").
- Un caso prueba **una** cosa; si necesitas varias verificaciones no relacionadas,
  divídelo.
- Los pasos son reproducibles por alguien que no participó en el diseño.
- No incluyas datos productivos reales ni PII (ver `datos-entornos-prueba`).

## Reglas para el agente

- Aplica esta anatomía y nomenclatura a todos los casos que generes.
- Cubre positivo + negativo + borde por cada criterio de aceptación.
- Enlaza cada caso a su requisito/criterio para alimentar la matriz de trazabilidad.
- Si falta información para un campo, márcalo `> ⚠️ PENDIENTE:` en vez de inventarlo.
