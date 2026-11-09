# Ficha de integración: [NOMBRE DE LA INTEGRACIÓN]

> **Cómo usar esta plantilla:** copia este archivo, reemplaza los textos entre `[corchetes]` y borra las notas en cursiva. Si un apartado no aplica, escribe "No aplica" y no lo borres.
>
> **Regla de seguridad:** en este documento **nunca** se escriben contraseñas, secretos, tokens ni claves. Solo se indica **dónde** se guardan y **quién** los administra.

## 0. Control del documento

| | |
|---|---|
| **Versión** | [0.1] |
| **Estado** | [Borrador / En revisión / Aprobado] |
| **Fecha** | [AAAA-MM-DD] |
| **Autor** | [Nombre] |
| **Revisores / aprobadores** | [Nombres y áreas] |

## 1. Identificación

| | |
|---|---|
| **Nombre** | [Nombre de la integración] |
| **Código interno** | [Si MONARK usa codificación] |
| **Descripción breve** | [Qué hace, en dos o tres líneas] |
| **Objetivo de negocio** | [Qué problema resuelve] |
| **Dirección** | [Sistema A → Sistema B / bidireccional] |
| **Modalidad** | [Programada / tiempo real / bajo demanda] |
| **Criticidad** | [Alta / Media / Baja y por qué] |
| **Responsable de negocio** | [Nombre y área] |
| **Responsable técnico** | [Nombre y área] |

## 2. Sistemas involucrados

| Rol | Sistema | Versión / edición | Tecnología | Propietario | Observaciones |
|---|---|---|---|---|---|
| Origen | [ ] | [ ] | [ ] | [ ] | [ ] |
| Destino | [ ] | [ ] | [ ] | [ ] | [ ] |
| Intermedio (si existe) | [ ] | [ ] | [ ] | [ ] | [ ] |

## 3. Funcionalidades

*Una fila por funcionalidad o flujo.*

| ID | Funcionalidad | Descripción | Entidad | Dirección | Frecuencia | Prioridad | Fase | Estado |
|---|---|---|---|---|---|---|---|---|
| F-01 | [ ] | [ ] | [ ] | [ ] | [ ] | [ ] | [ ] | [ ] |

## 4. Datos y mapeo

| | |
|---|---|
| **Entidades que se sincronizan** | [ ] |
| **Identificador único entre sistemas** | [Campo del origen y campo del destino] |
| **Documento de mapeo detallado** | [Enlace] |
| **Reglas de limpieza y transformación** | [ ] |
| **Tratamiento de bajas / eliminaciones** | [ ] |
| **Volumen inicial (carga inicial)** | [ ] |
| **Volumen de cambios por día** | [ ] |

## 5. Programación

| | |
|---|---|
| **Frecuencia** | [Cada X minutos / diaria / ...] |
| **Ventana horaria permitida** | [ ] |
| **Duración esperada de una ejecución** | [ ] |
| **Qué ocurre si una ejecución se solapa con otra** | [ ] |

## 6. Usuarios, roles y permisos

*Incluye personas y cuentas técnicas, en todos los sistemas.*

| Cuenta / usuario | Sistema | Tipo (persona / servicio) | Rol o perfil | Permisos concedidos | Propietario | Fecha de alta | Vencimiento o rotación |
|---|---|---|---|---|---|---|---|
| [ ] | [ ] | [ ] | [ ] | [ ] | [ ] | [ ] | [ ] |

**Principio:** cada cuenta técnica tiene los permisos mínimos necesarios.

## 7. Credenciales y secretos

*Solo referencias, nunca valores.*

| Secreto | Sistema | Dónde se guarda | Responsable | Frecuencia de rotación | Procedimiento si se compromete |
|---|---|---|---|---|---|
| [Ej.: clave de la aplicación de integración] | [ ] | [ ] | [ ] | [ ] | [ ] |

## 8. Conectividad y seguridad

| | |
|---|---|
| **Direcciones / endpoints** | [URLs o hosts, sin credenciales] |
| **Puertos y sentido de la conexión** | [Entrada / salida] |
| **Proxy, firewall o listas permitidas** | [ ] |
| **Cifrado en tránsito** | [ ] |
| **Autenticación entre sistemas** | [Método] |
| **Datos personales o sensibles que se transfieren** | [Cuáles] |
| **Normativa o política aplicable** | [ ] |

## 9. Entornos

| Entorno | Sistema origen | Sistema destino | Servidor de la integración | Estado |
|---|---|---|---|---|
| Desarrollo | [ ] | [ ] | [ ] | [ ] |
| Pruebas / QA | [ ] | [ ] | [ ] | [ ] |
| Producción | [ ] | [ ] | [ ] | [ ] |

## 10. Infraestructura y despliegue

| | |
|---|---|
| **Servidor** | [Nombre, sistema operativo y versión] |
| **Runtime / dependencias** | [Versión de .NET, cliente de base de datos, etc.] |
| **Forma de ejecución** | [Servicio de Windows, tarea programada, ...] |
| **Ruta de instalación** | [ ] |
| **Quién despliega y cómo** | [ ] |
| **Procedimiento de reversión (rollback)** | [ ] |

## 11. Manejo de errores

| Tipo de error | Ejemplo | Acción automática | Quién se entera | Acción manual |
|---|---|---|---|---|
| [Conexión] | [ ] | [Reintentar N veces] | [ ] | [ ] |
| [Dato inválido] | [ ] | [ ] | [ ] | [ ] |
| [Límite del sistema destino] | [ ] | [ ] | [ ] | [ ] |

## 12. Monitoreo y registros

| | |
|---|---|
| **Dónde se guardan los logs** | [ ] |
| **Retención** | [ ] |
| **Qué se mide** | [Registros procesados, errores, duración] |
| **Alertas** | [Condición, canal y destinatario] |

## 13. Pruebas

| ID | Caso de prueba | Resultado esperado | Entorno | Resultado | Fecha | Evidencia |
|---|---|---|---|---|---|---|
| P-01 | [ ] | [ ] | [ ] | [ ] | [ ] | [ ] |

## 14. Operación y soporte

| | |
|---|---|
| **Contactos (primer y segundo nivel)** | [ ] |
| **Horario de soporte** | [ ] |
| **Procedimiento para reprocesar datos** | [ ] |
| **Procedimiento para pausar o detener la integración** | [ ] |
| **Documentación de apoyo (runbook)** | [Enlace] |

## 15. Dependencias y riesgos

| Tipo | Descripción | Impacto | Mitigación | Responsable | Estado |
|---|---|---|---|---|---|
| [Dependencia / Riesgo] | [ ] | [ ] | [ ] | [ ] | [ ] |

## 16. Historial de cambios

| Versión | Fecha | Autor | Cambio |
|---|---|---|---|
| 0.1 | [ ] | [ ] | Creación del documento |

## 17. Aprobaciones

| Nombre | Cargo / área | Decisión | Fecha |
|---|---|---|---|
| [ ] | [ ] | [Aprobado / Observado] | [ ] |
