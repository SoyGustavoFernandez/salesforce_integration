# Integración ERP MONARK → Salesforce: alcance y objetivos

| | |
|---|---|
| **Estado** | Borrador v0.1, pendiente de aprobación |
| **Autor** | Gustavo Fernández Saavedra |
| **Fecha** | 2026-11-03 |

> Convención de este documento: los puntos marcados **[Por validar]** son supuestos o datos aún no confirmados.

## 1. Contexto y objetivo

MONARK usa un ERP propio (.NET sobre Oracle) y se incorporará Salesforce como CRM. El objetivo de este proyecto es **sincronizar automáticamente los datos del ERP hacia Salesforce**, para que el área comercial trabaje con información actualizada sin cargas manuales.

## 2. Alcance

### Dentro del alcance (Fase 1)
- Sincronización de **clientes** (personas jurídicas y personas naturales) del ERP hacia Salesforce.
- **Carga inicial** de los clientes existentes, como proceso separado y reanudable.
- **Sincronización incremental programada** cada 30 minutos.
- Reflejo en Salesforce de las **bajas o desactivaciones** de clientes en el ERP.
- Registro de cada ejecución y de cada error (auditoría).

### Fuera del alcance (por ahora)
- Sincronización en tiempo real o basada en eventos.
- Sincronización de Salesforce hacia el ERP (el flujo es **solo de ida**).
- Ventas, documentos y otras entidades: se tratarán en fases posteriores, una vez cerrada la de clientes.
- Configuración y administración de la org de Salesforce (a cargo del administrador funcional).

## 3. Fases

| Fase | Contenido | Estado |
|---|---|---|
| 1 | Clientes: carga inicial e incremental | Definida |
| 2 | Ventas, documentos y otras entidades | **[Por validar]** orden y alcance |

## 4. Condiciones de partida

### Confirmadas por el equipo
- ERP propio en .NET con base de datos Oracle.
- Despliegue **on premise**, en un servidor **Windows** de MONARK.
- Frecuencia de sincronización: **cada 30 minutos**, programada.
- Volumen de cambios estimado en **máximo ~20 clientes por día**, incluso en campaña alta. Es una estimación verbal: **[por validar]** con datos de Oracle.
- Acceso completo a la base de datos Oracle.
- No existen estándares, documentación ni nomenclaturas previas: se definen en este proyecto.

### Aún no disponibles
- No hay **org de Salesforce ni sandbox** de MONARK. **[Por validar]** con el administrador funcional.
- No hay definición de **mapeo de campos** a Salesforce.

## 5. Datos de partida (clientes)

Fuente principal: tabla `GXNUC.NMEC` (clave primaria `NMECCOD`), con apoyo de `NMPN` (datos de persona natural), `NMPL` (direcciones) y `NMZG` (ubigeo).

Hechos observados:
- Las estadísticas de la tabla indican unos **219 mil** clientes; el equipo habló de cerca de **300 mil**. **[Por validar]** con un conteo real.
- Existen consultas preparadas para personas jurídicas (`NMECTIP = 'J'`) y personas naturales (distinto de `'J'`), ambas con `NMECEST = 0` (activos).
- Los datos incluyen valores de relleno (RUC `0`, nombres como `--NO VALIDO--`) y RUC repetidos entre clientes, por lo que **el RUC no sirve como identificador único**.
- Las fechas de modificación (`NMECFMOD`) están vacías o con valor centinela en gran parte de los registros, y la dirección y los datos de persona residen en otras tablas. Por eso se propone detectar cambios por **hash** (ver documento de arquitectura).

## 6. Criterios de éxito (propuestos)

- Todos los clientes activos del ERP existen en Salesforce tras la carga inicial.
- Un cliente nuevo o modificado en el ERP aparece en Salesforce en la siguiente ejecución (máximo ~30 minutos).
- Un cliente desactivado en el ERP queda reflejado en Salesforce.
- Reprocesar una ejecución no crea duplicados.
- Cada ejecución deja un registro con cantidades procesadas y errores.

## 7. Riesgos

| Riesgo | Impacto | Mitigación propuesta |
|---|---|---|
| Calidad de datos del ERP (RUC repetidos, valores de relleno, campos vacíos) | Rechazos o duplicados en Salesforce | Reglas de limpieza definidas en el mapeo; informe de rechazos |
| Sin sandbox de Salesforce | Pruebas con riesgo sobre datos reales | Solicitar sandbox; mientras tanto, org Developer Edition con datos ficticios |
| Decisión de Person Accounts (irreversible) | Rehacer el modelo si se elige mal | Decisión funcional previa, probada en sandbox |
| Límites de API de Salesforce | Fallos por exceso de llamadas | Volumen bajo previsto; reintentos con espera y registro de errores |
| Datos personales (fecha de nacimiento, teléfono, correo) enviados a un servicio en la nube | Cumplimiento normativo | **[Por validar]** política de MONARK y Ley 29733 |

## 8. Preguntas abiertas

| # | Pregunta | Responsable |
|---|---|---|
| 1 | ¿Existe sandbox de Salesforce? ¿Qué edición y licencias tiene MONARK? | Administradora funcional |
| 2 | ¿Person Accounts o Account + Contact para personas naturales? | Administradora funcional |
| 3 | ¿Quién crea el campo External ID, la aplicación de integración y el usuario de integración? | Administradora funcional |
| 4 | ¿Cuántos clientes hay realmente y cuántos cambian por día? | Desarrollo (consulta a Oracle) |
| 5 | Se adopta .NET 10. ¿El servidor de producción puede instalarlo y qué versión de .NET usa el ERP? | Desarrollo / Infraestructura |
| 6 | ¿Hay política interna sobre datos personales en la nube? | MONARK |
| 7 | ¿Las fechas de modificación son fiables en producción? | Desarrollo / equipo del ERP |
| 8 | ¿Cómo debe reflejarse una baja en Salesforce: eliminar el registro o marcarlo como inactivo (campo a crear)? | Administradora funcional |
