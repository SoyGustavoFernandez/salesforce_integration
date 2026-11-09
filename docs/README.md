# Integración ERP MONARK → Salesforce

| | |
|---|---|
| **Estado** | En definición (documentación previa al desarrollo) |
| **Responsable** | Gustavo Fernández Saavedra |
| **Última actualización** | 2026-11-04 |

## ¿De qué trata?

Sincronización automática y de un solo sentido de los clientes del ERP (Oracle) hacia Salesforce, ejecutada cada 30 minutos. La Fase 1 cubre clientes. Ventas, documentos y otras entidades se tratarán después.

## Documentos

| Documento | Estado |
|---|---|
| [Alcance y objetivos](00-alcance-y-objetivos.md) | Borrador v0.1 |
| [Arquitectura de alto nivel](01-arquitectura.md) | Borrador v0.1 |
| [Estructura de proyectos](02-estructura-de-proyectos.md) | Borrador v0.1 |
| [Convenciones: nomenclatura, ramas y commits](03-convenciones.md) | Borrador v0.1 |
| Mapeo de clientes | Próximamente |

## Decisiones pendientes

- Sandbox de Salesforce y edición de la licencia.
- Person Accounts o Account + Contact para personas naturales.
- Cómo reflejar las bajas de clientes en Salesforce.
- Nivel de API a exponer. La versión de .NET definida es .NET 10, pendiente de confirmar con infraestructura.

## Glosario breve

| Término | Significado |
|---|---|
| **ERP** | Sistema interno de MONARK, fuente de los datos |
| **CRM / Salesforce** | Sistema comercial de destino |
| **Account** | Registro de cliente en Salesforce |
| **External ID** | Campo que guarda el código del cliente del ERP para evitar duplicados |
| **Upsert** | Crear el registro si no existe, o actualizarlo si ya existe |
| **ADR** | Documento corto que registra una decisión de arquitectura |
