# Skill: Manual Técnico-Operativo

Genera un **Manual Técnico-Operativo** completo (10 secciones) para un proyecto de
software, **autocompletando** el contenido desde el análisis del repositorio, y
lo exporta en **Markdown** y **PDF** (con logo y tipografía corporativa).

## Cómo se usa

Pídele al agente algo como:

- "Genera el manual técnico-operativo de este proyecto"
- "Documenta el proyecto y créame el PDF del manual"

El agente activará la skill y seguirá el procedimiento del `SKILL.md`.

## Qué hace, paso a paso

1. **Investiga el repo** (lo hace el propio agente): stack, dependencias,
   scripts, variables de entorno, servicios, Docker/CI/IaC, estructura,
   arquitectura, git y referencias a tickets. En repos grandes o desconocidos,
   el agente delega la investigación en el sub-agente `context-gatherer`.

2. **Rellena la plantilla** `assets/template.md` con datos reales y la escribe en
   `docs/manual-tecnico-operativo.md`. No deja marcadores; lo no inferible queda
   como `> ⚠️ PENDIENTE: ...`.

3. **Genera el PDF** (lo hace el propio agente): arma una portada con el logo,
   convierte el Markdown a HTML aplicando `assets/pdf-style.css`, renderiza los
   diagramas Mermaid y exporta `docs/manual-tecnico-operativo.pdf`. No requiere
   scripts ni herramientas externas instaladas.

## Las 10 secciones

1. Introducción y descripción general
2. Objetivo
3. Alcance
4. Necesidad de negocio
5. Prerrequisitos y dependencias
6. Arquitectura de la solución (con diagrama Mermaid)
7. Guía de despliegue
8. Operación y mantenimiento
9. Troubleshooting / errores comunes
10. Rollback, seguridad y referencias (incluye glosario)

## Personalización

| Qué | Cómo |
|-----|------|
| Logo | Reemplaza `assets/logo.svg` (SVG/PNG); el agente lo embebe en la portada |
| Colores / tipografía | Edita las variables CSS al inicio de `assets/pdf-style.css` |
| Ruta de salida | Por defecto `docs/manual-tecnico-operativo.pdf`; pide otra al agente |

## Requisitos

- Ninguna dependencia externa: el agente investiga el repo, redacta el Markdown y
  genera el PDF con sus propias herramientas.
- Si el entorno impide producir el PDF, el manual **en Markdown** se genera
  igualmente; como alternativa manual para el PDF puede usarse `pandoc`.

## Estructura de la skill

```
manual-tecnico-operativo/
├── SKILL.md                 # Instrucciones que sigue el agente
├── README.md                # Este archivo
└── assets/
    ├── template.md          # Plantilla de las 10 secciones
    ├── pdf-style.css        # Estilos del PDF (portada, tipografía, colores)
    └── logo.svg             # Logo por defecto (reemplazable)
```
