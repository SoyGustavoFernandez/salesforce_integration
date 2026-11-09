# Integración ERP MONARK → Salesforce: convenciones

| | |
|---|---|
| **Estado** | Borrador v0.1, pendiente de aprobación |
| **Autor** | Gustavo Fernández Saavedra |
| **Fecha** | 2026-11-09 |
| **Relacionado** | [02-estructura-de-proyectos.md](02-estructura-de-proyectos.md) |

> Este documento es una **propuesta**. MONARK no tiene estándares previos, así que se adoptan convenciones ampliamente usadas y se mantienen simples. Lo marcado **[Por validar]** está pendiente de decisión.

## 1. Principios

- **Consistencia antes que preferencia personal:** una convención sencilla y aplicada siempre vale más que una sofisticada y ocasional.
- **Seguir los estándares de la plataforma:** en .NET, las guías de Microsoft; en Git, Conventional Commits.
- **Nombres que se lean sin conocer el ERP:** el ERP usa códigos crípticos (por ejemplo `NMEC`, `NMECCOD`). El código de esta solución usa nombres claros y deja la traducción en un glosario (ver sección 6).

## 2. Idioma

| Elemento | Idioma | Ejemplo |
|---|---|---|
| Código (clases, métodos, variables, proyectos) | Inglés | `CustomerSynchronizer` |
| Documentación (`docs/`) | Español | `01-arquitectura.md` |
| Mensajes de commit | Inglés, una sola línea | `feat(worker): add timer` |
| Registros de log | Inglés | `Synchronization finished` |
| Etiquetas visibles en Salesforce | Español | "Cod Cliente" |

## 3. Nomenclatura en C#

| Elemento | Convención | Ejemplo |
|---|---|---|
| Clases, estructuras, enumeraciones, métodos, propiedades | `PascalCase` | `CustomerMapper`, `Execute()` |
| Interfaces | `PascalCase` con prefijo `I` | `ICustomerRepository` |
| Parámetros y variables locales | `camelCase` | `customerCode` |
| Campos privados | `_camelCase` | `_httpClient` |
| Constantes | `PascalCase` | `DefaultBatchSize` |
| Métodos asíncronos | Sufijo `Async` | `SendCustomersAsync` |
| Archivos | Un tipo público por archivo, con el nombre del tipo | `CustomerMapper.cs` |
| Espacios de nombres | Igual que la ruta de carpetas del proyecto | `Integration.Application.Customers` |

- Se usa **`Nullable` habilitado**: no se silencian las advertencias de nulabilidad sin justificarlo.
- Los nombres de los proyectos siguen el patrón `Integration.<Capa>` (ver documento 02) **[Por validar]**.

## 4. Pruebas

| Elemento | Convención | Ejemplo |
|---|---|---|
| Proyecto | `<Proyecto probado>.UnitTests` o `.IntegrationTests` | `Integration.UnitTests` |
| Clase de prueba | `<TipoProbado>Tests` | `CustomerMapperTests` |
| Método de prueba | `Metodo_Escenario_ResultadoEsperado` | `Map_WithoutLastName_ReturnsError` |

## 5. Objetos propios en Oracle y Salesforce

### Oracle (tablas de control propias, no del ERP) **[Por validar]**

| Elemento | Convención | Ejemplo |
|---|---|---|
| Tablas | Mayúsculas, `SNAKE_CASE`, prefijo `SYNC_` | `SYNC_CLIENTE` |
| Columnas | Mayúsculas, `SNAKE_CASE` | `COD_CLIENTE`, `FECHA_ULT_ENVIO` |
| Claves primarias | `PK_<TABLA>` | `PK_SYNC_CLIENTE` |
| Índices | `IX_<TABLA>_<COLUMNA>` | `IX_SYNC_CLIENTE_ESTADO` |

Estos nombres no copian el estilo del ERP (`NMEC`, `NMECCOD`): son legibles y separados por prefijo para distinguir lo nuevo de lo existente.

### Salesforce (campos personalizados)

| Elemento | Convención | Ejemplo |
|---|---|---|
| Etiqueta (*Label*) | Español, legible | `Cod Cliente` |
| Nombre de API (*Field Name*) | Palabras en `Pascal_Case` separadas por guion bajo, en español, sin acentos; Salesforce añade `__c` | `Cod_Cliente__c` |
| Descripción | Obligatoria: qué contiene y de qué campo del ERP proviene | "Código del cliente en el ERP de MONARK (NMECCOD)" |

## 6. Glosario de términos del dominio

Une el nombre en el código con el del ERP y el de Salesforce, para que cualquiera pueda traducir.

| Concepto | En el código | En el ERP (Oracle) | En Salesforce |
|---|---|---|---|
| Cliente | `Customer` | `NMEC` | `Account` |
| Código del cliente | `CustomerCode` | `NMECCOD` | `Cod_Cliente__c` |
| Persona jurídica | `LegalEntity` **[Por validar]** | `NMECTIP = 'J'` | Account de empresa |
| Persona natural | `NaturalPerson` **[Por validar]** | `NMECTIP <> 'J'`, datos en `NMPN` | Person Account o Account + Contact **[Por validar]** |

El glosario crece con cada entidad nueva (ventas, documentos, etc.).

## 7. Git: ramas

Flujo simple, basado en `main` y ramas cortas.

| Rama | Uso | Duración |
|---|---|---|
| `main` | Rama principal; debe compilar siempre | Permanente |
| `feature/<descripcion>` | Funcionalidad nueva | Hasta fusionar |
| `fix/<descripcion>` | Corrección de un error | Hasta fusionar |
| `docs/<descripcion>` | Cambios solo de documentación | Hasta fusionar |
| `chore/<descripcion>` | Mantenimiento: dependencias, configuración | Hasta fusionar |
| `hotfix/<descripcion>` | Corrección urgente en producción **[Por validar]** | Hasta fusionar |

- La descripción va en **minúsculas, con guiones, en español o inglés** y breve: `feature/carga-inicial-clientes`.
- Si se adopta un sistema de tickets, se antepone el código: `feature/ABC-123-carga-inicial` **[Por validar]**.
- No se trabaja directamente sobre `main` salvo en la configuración inicial del repositorio.
- Las ramas se borran después de fusionarse.

## 8. Git: mensajes de commit

Se usa [Conventional Commits](https://www.conventionalcommits.org/), en inglés y en **una sola línea** (sin cuerpo).

```
<tipo>(<alcance opcional>): <descripción en imperativo, sin punto final>
```

| Tipo | Cuándo |
|---|---|
| `feat` | Funcionalidad nueva |
| `fix` | Corrección de error |
| `docs` | Solo documentación |
| `refactor` | Cambio interno que no altera el comportamiento |
| `test` | Pruebas |
| `build` | Proyecto, dependencias, SDK |
| `ci` | Integración y despliegue continuos |
| `chore` | Mantenimiento general |
| `perf` | Mejora de rendimiento |

**Alcances sugeridos:** `domain`, `application`, `infrastructure`, `worker`, `tests`, `docs`.

**Reglas:**
- Una sola línea de **72 caracteres o menos**, en imperativo ("add", no "added"), sin cuerpo.
- **Un cambio lógico por commit**.
- Si rompe compatibilidad, se indica con `!` tras el tipo (por ejemplo `feat(worker)!: ...`).
- Sin líneas `Co-Authored-By`: el commit lleva solo al autor que lo hace.
- Nunca se incluyen contraseñas, secretos ni datos reales de clientes.

**Ejemplos:**

```
feat(worker): add 30-minute synchronization timer
fix(infrastructure): trim padded CHAR fields before hashing
docs: add naming, branching and commit conventions
build: pin .NET 10 SDK
```

## 9. Pull requests y fusión **[Por validar]**

- Aunque el proyecto lo desarrolla una sola persona, se recomienda usar *pull requests* para dejar trazabilidad de cada cambio.
- Fusión con **squash**, de modo que cada PR deje un único commit en `main` con un mensaje en formato Conventional Commits.
- Antes de fusionar: la solución compila, las pruebas pasan y la documentación afectada está actualizada.
- Protección de la rama `main` en el repositorio remoto **[Por validar]**.

## 10. Versionado y etiquetas **[Por validar]**

- Versionado semántico (`MAYOR.MENOR.PARCHE`).
- Cada versión desplegada se marca con una etiqueta: `v0.1.0`.
- Mientras no haya una primera versión en producción, se usa `0.x`.

## 11. Documentación

| Elemento | Convención | Ejemplo |
|---|---|---|
| Archivos numerados | `NN-nombre-en-kebab.md` | `03-convenciones.md` |
| Decisiones de arquitectura (ADR) | `docs/adr/NNNN-titulo-en-kebab.md` | `0001-deteccion-de-cambios-por-hash.md` |
| Plantillas | `docs/plantillas/` | `ficha-de-integracion.md` |
| Estado en la cabecera | Borrador, En revisión, Aprobado | "Borrador v0.1" |
| Pendientes | Se marcan con **[Por validar]** | |

- Cada documento lleva su tabla de cabecera (estado, autor, fecha).
- Lo no confirmado se marca como **[Por validar]**; no se presenta como hecho.
- La documentación se escribe y se aprueba **antes** de implementar la parte que describe.

## 12. Pendientes

- Aprobar o ajustar estas convenciones **[Por validar]**.
- Confirmar si MONARK tiene algún estándar propio que prevalezca **[Por validar]**.
- Decidir si se usará un sistema de tickets y cómo se codifican **[Por validar]**.
- Definir las reglas adicionales de `.editorconfig` **[Por validar]**.
