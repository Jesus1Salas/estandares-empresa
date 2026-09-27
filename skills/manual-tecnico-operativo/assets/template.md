<!--
PLANTILLA: Manual Técnico-Operativo
Instrucciones para el agente:
- Sustituye todos los marcadores {{...}} por contenido REAL investigado del repo.
- No dejes marcadores sin resolver. Si un dato no existe, usa: > ⚠️ PENDIENTE: <motivo>.
- Los comentarios HTML (como este) NO deben aparecer en la salida final: elimínalos.
- Escribe en el idioma del proyecto (por defecto español).
-->

---
titulo: "Manual Técnico - Operativo"
proyecto: "{{NOMBRE_PROYECTO}}"
version: "{{VERSION}}"
fecha: "{{FECHA}}"
autor: "{{AUTOR}}"
clasificacion: "Uso interno"
---

# Manual Técnico - Operativo

**Proyecto:** {{NOMBRE_PROYECTO}}
**Versión:** {{VERSION}} · **Fecha:** {{FECHA}} · **Autor:** {{AUTOR}}

## Control de versiones

| Versión | Fecha | Autor | Revisor | Estado | Descripción del cambio |
|---------|-------|-------|---------|--------|------------------------|
| {{VERSION}} | {{FECHA}} | {{AUTOR}} | {{REVISOR}} | Borrador | Versión inicial del manual |

## Tabla de contenido

1. Introducción y descripción general del proyecto
2. Objetivo
3. Alcance
4. Necesidad de negocio
5. Prerrequisitos y dependencias
6. Arquitectura de la solución
7. Guía de despliegue
8. Operación y mantenimiento
9. Troubleshooting / errores comunes
10. Rollback, seguridad y referencias

---

## 1. Introducción y descripción general del proyecto

{{DESCRIPCION_GENERAL}}
<!-- Deriva del README y del propósito del código. 1-3 párrafos: qué es, en qué
contexto se usa, qué tipo de sistema es (API, batch, web, servicio, etc.). -->

## 2. Objetivo

{{OBJETIVO}}
<!-- Qué problema resuelve el sistema y para quién. Un párrafo claro y medible. -->

## 3. Alcance

**Incluye:**
{{ALCANCE_INCLUYE}}

**No incluye (fuera de alcance):**
{{ALCANCE_EXCLUYE}}

## 4. Necesidad de negocio

{{NECESIDAD_NEGOCIO}}
<!-- Justificación: por qué existe, qué valor aporta, qué pasa si no existiera. -->

## 5. Prerrequisitos y dependencias

**Entorno de ejecución:**

| Componente | Versión requerida | Notas |
|------------|-------------------|-------|
| {{RUNTIME}} | {{RUNTIME_VERSION}} | |

**Dependencias principales:**

{{DEPENDENCIAS_PRINCIPALES}}
<!-- Lista derivada del manifiesto (package.json, pom.xml, requirements.txt...). -->

**Servicios externos / integraciones:**

{{SERVICIOS_EXTERNOS}}
<!-- Bases de datos, colas, APIs de terceros, cache, almacenamiento. -->

**Accesos y credenciales necesarias:**

{{ACCESOS_REQUERIDOS}}
<!-- Qué cuentas, roles, tokens o VPN hacen falta. Referencia por nombre, nunca valores. -->

## 6. Arquitectura de la solución

{{DESCRIPCION_ARQUITECTURA}}

**Diagrama de componentes:**

```mermaid
{{DIAGRAMA_MERMAID}}
```
<!-- Genera un diagrama Mermaid a partir de los componentes reales detectados.
Ejemplo mínimo si es un servicio con BD:
flowchart LR
  Cliente --> API[API {{NOMBRE_PROYECTO}}]
  API --> DB[(Base de datos)]
-->

**Flujo de datos / interacción de componentes:**

{{FLUJO_DATOS}}

## 7. Guía de despliegue

**Entornos:**

| Entorno | Propósito | URL / Host |
|---------|-----------|------------|
| Desarrollo | | {{URL_DEV}} |
| QA / Pruebas | | {{URL_QA}} |
| Producción | | {{URL_PROD}} |

**Variables de entorno:**

| Variable | Descripción | Obligatoria |
|----------|-------------|-------------|
{{TABLA_VARIABLES_ENTORNO}}
<!-- Derivadas de .env.example / config. Nunca incluyas valores secretos reales. -->

**Pasos de despliegue:**

{{PASOS_DESPLIEGUE}}
<!-- Comandos REALES derivados de scripts del manifiesto y del CI. Numerados. -->

## 8. Operación y mantenimiento

**Arranque y parada del servicio:**

{{ARRANQUE_PARADA}}

**Logs:**

{{LOGS}}
<!-- Dónde se escriben, cómo consultarlos, niveles. -->

**Monitoreo y salud:**

{{MONITOREO}}
<!-- Endpoints de health, métricas, dashboards si existen. -->

**Copias de seguridad (backups):**

{{BACKUPS}}

**Tareas programadas / mantenimiento periódico:**

{{TAREAS_PROGRAMADAS}}

## 9. Troubleshooting / errores comunes

| Síntoma | Causa probable | Solución |
|---------|----------------|----------|
{{TABLA_TROUBLESHOOTING}}
<!-- Al menos 3-5 filas plausibles según el stack y lo documentado. -->

## 10. Rollback, seguridad y referencias

### 10.1 Rollback y plan de contingencia

{{ROLLBACK}}
<!-- Cómo revertir un despliegue; versión anterior estable; responsables. -->

### 10.2 Seguridad

{{SEGURIDAD}}
<!-- Manejo de secretos, autenticación/autorización, roles, datos sensibles,
gestión de dependencias vulnerables. -->

### 10.3 Glosario

{{GLOSARIO}}
<!-- Términos y acrónimos del dominio. -->

### 10.4 Referencias

{{REFERENCIAS}}
<!-- Enlaces a repositorio, tickets Jira, otras páginas, documentación relacionada. -->
