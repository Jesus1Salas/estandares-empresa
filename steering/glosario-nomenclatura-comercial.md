---
inclusion: always
---

# Glosario y Nomenclatura Comercial

Nombres oficiales, siglas y convenciones de nombres de archivos del área
comercial. Garantiza que todos los entregables usen la misma terminología y que
los documentos sean fáciles de localizar.

> ⚠️ COMPLETAR: ajusta nombres de servicios y siglas a la realidad de la
> consultora.

## Nombres oficiales de servicios

Usa SIEMPRE el nombre exacto de la línea de servicio tal como figura en
`perfil-empresa-servicios`. No uses variantes ni traducciones libres.

| Nombre oficial | Variantes que NO se deben usar |
|----------------|--------------------------------|
| `<Ingeniería de Datos>` | `<"data eng", "el área de datos">` |
| `<Analítica y BI>` | `<"reporting", "los dashboards">` |
| `<Nube / DevSecOps>` | `<"lo de la nube", "cloud stuff">` |

## Siglas y términos comunes

| Sigla / término | Significado |
|-----------------|-------------|
| **SOW** | Statement of Work (documento de alcance de trabajo) |
| **RFP** | Request for Proposal (solicitud de propuesta) |
| **RFI** | Request for Information (solicitud de información) |
| **NDA** | Non-Disclosure Agreement (acuerdo de confidencialidad) |
| **MSA** | Master Service Agreement (contrato marco) |
| **T&M** | Time & Materials (tiempo y materiales) |
| **PoC** | Proof of Concept (prueba de concepto) |
| **KPI** | Key Performance Indicator (indicador clave) |
| **CR** | Change Request (solicitud de cambio de alcance) |
| `<...>` | `<...>` |

## Convención de nombres de archivos

Formato general en `kebab-case`, con fecha en formato `AAAA-MM-DD`:

```
<tipo>-<cliente>-<proyecto>-<AAAA-MM-DD>.<ext>
```

| Tipo de documento | Prefijo | Ejemplo |
|-------------------|---------|---------|
| Propuesta comercial | `propuesta-` | `propuesta-acme-datalake-2026-09-26.md` |
| Cotización | `cotizacion-` | `cotizacion-acme-datalake-2026-09-26.md` |
| Minuta de reunión | `minuta-` | `minuta-acme-kickoff-2026-09-26.md` |
| SOW / contrato | `sow-` | `sow-acme-datalake-2026-09-26.md` |
| Correo (borrador) | `correo-` | `correo-acme-seguimiento-2026-09-26.md` |

Reglas:

- Cliente y proyecto en minúsculas, sin espacios ni acentos (`acme`, no `Acme S.A.`).
- La fecha es la de emisión del documento.
- Los prefijos coinciden con los `fileMatchPattern` de los steerings, para que
  las reglas correctas se activen solas.

## Ubicación de documentos comerciales

> ⚠️ COMPLETAR con la estructura real del repositorio/CRM.

```
comercial/
├── propuestas/
├── cotizaciones/
├── minutas/
└── contratos/
```

## Reglas para el agente

- Usa los nombres oficiales de servicio; corrige variantes informales en la salida.
- Nombra los archivos generados siguiendo la convención (prefijo + cliente +
  proyecto + fecha) para que se activen los steerings por `fileMatch`.
- Expande las siglas la primera vez que aparezcan en un documento de cara al
  cliente que no sea técnico.
