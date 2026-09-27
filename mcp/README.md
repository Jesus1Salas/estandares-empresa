# Configuraciones MCP de referencia

Configuraciones MCP de **uso general del desarrollador** que los proyectos pueden
adoptar bajo demanda. Son las conexiones que el equipo usa para sus desarrollos.

| Archivo | Servidor | Uso |
|---------|----------|-----|
| `github.json` | `github` | GitHub MCP oficial para desarrollo (crear repos, PRs, issues). Requiere `GITHUB_PERSONAL_ACCESS_TOKEN` por variable de entorno. |
| `drawio.json` | `drawio` | DrawIO MCP para generar/editar diagramas. |

## Cómo se materializan (merge, no copia)

Estos artefactos son de tipo `mcp` y **se fusionan** en el
`.kiro/settings/mcp.json` del proyecto; **no se copian** sobrescribiendo el
archivo. Al adoptarlos, se añade su bloque `mcpServers.<nombre>` sin borrar otros
servidores ya configurados. Se bajan **bajo demanda** (uno u otro, o ambos).

## Separación importante

- Estas conexiones (`github`, `drawio`) son para el **trabajo del desarrollador** y
  son independientes del MCP `estandares`.
- El **agente consultor de estándares** usa su propia conexión `estandares` (solo
  lectura, apunta al repo `estandares-empresa`), que se instala con el Power
  `power-consultor-estandares` y **no se mezcla** con el `github` de desarrollo.
- Nunca se coloca el token de GitHub en texto plano: va por variable de entorno
  (`${GITHUB_PERSONAL_ACCESS_TOKEN}`), según `steering/seguridad-devsecops.md`.
