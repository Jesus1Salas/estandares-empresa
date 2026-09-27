---
inclusion: always
---

# Datos y Privacidad (Clientes y Prospectos)

Cómo tratar datos personales y confidenciales de clientes y prospectos en
documentos comerciales, correos, minutas y en el CRM. Complementa
`legal-cumplimiento-comercial`.

## Principios

- **Minimización:** usa solo los datos personales estrictamente necesarios para
  el entregable. No incluyas PII que no aporte al documento.
- **Finalidad:** los datos de un prospecto se usan para la gestión comercial
  acordada, no para otros fines.
- **Confidencialidad:** la información del cliente (volúmenes, cifras, arquitectura,
  nombres de proyecto) es confidencial salvo autorización de mención.

## Qué se considera dato sensible / confidencial

- **PII:** nombre completo, correo, teléfono, cargo asociado a una persona
  identificable, identificadores fiscales.
- **Confidencial de negocio:** cifras financieras del cliente, contratos, precios
  pactados, arquitecturas internas, credenciales, datos productivos.

## Reglas de manejo

- **En plantillas y ejemplos:** usa marcadores o datos ficticios (`Cliente Demo`,
  `contacto@ejemplo.com`), nunca PII real.
- **En entregables reales:** usa los datos reales que aporte el usuario, pero solo
  los necesarios. Evita pegar listas completas de contactos si basta con uno.
- **Anonimización:** para casos de éxito o material de venta sin permiso de
  mención, sustituye el nombre del cliente por su sector ("un banco tier-1").
- **Credenciales y secretos:** nunca se incluyen en propuestas, cotizaciones,
  minutas ni correos. Si aparecen en notas de origen, el agente los omite del
  entregable y lo advierte.
- **Compartir con terceros:** no se comparten datos del cliente con terceros
  (otros clientes, proveedores) sin autorización.
- **Almacenamiento:** los documentos comerciales se guardan en `<repositorio/CRM
  autorizado>`; no se dejan en ubicaciones no controladas.

## Al procesar notas de reunión (minutas)

- Las notas de una reunión pueden contener PII y datos confidenciales. Al generar
  la minuta, incluye solo lo relevante para acuerdos y próximos pasos.
- Omite datos personales sensibles que no sean necesarios (p. ej. comentarios
  personales, datos de salud, opiniones ajenas al negocio).
- Si las notas incluyen algo que parezca un secreto (token, contraseña, clave de
  acceso), no lo transcribas: refiérete a él por su nombre y avisa.

## Reglas para el agente

- Aplica minimización: incluye solo la PII necesaria.
- Nunca transcribas credenciales o secretos en un entregable.
- Anonimiza clientes cuando no haya permiso de mención (según
  `perfil-empresa-servicios`).
- Si dudas sobre si un dato puede incluirse o compartirse, trátalo como
  confidencial y consúltalo con el usuario.
