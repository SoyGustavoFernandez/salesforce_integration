# Integración ERP MONARK → Salesforce: estructura de proyectos

| | |
|---|---|
| **Estado** | Borrador v0.1, pendiente de aprobación |
| **Autor** | Gustavo Fernández Saavedra |
| **Fecha** | 2026-11-09 |
| **Relacionado** | [01-arquitectura.md](01-arquitectura.md) |

> Los nombres de proyectos y carpetas de este documento son **provisionales** hasta que se aprueben las convenciones. Se eligieron nombres neutros para poder renombrarlos fácilmente. Lo marcado **[Por validar]** está pendiente de decisión.

## 1. Árbol de carpetas

```
/
├─ .claude/
│   ├─ skills/                 (reservada, vacía)
│   └─ rules/                  (reservada, vacía)
├─ openspec/
│   ├─ specs/                  (reservada, vacía)
│   └─ changes/                (reservada, vacía)
├─ docs/
│   ├─ adr/                    (decisiones de arquitectura, vacía)
│   ├─ plantillas/
│   └─ *.md
├─ src/
│   ├─ Integration.Domain/
│   ├─ Integration.Application/
│   ├─ Integration.Infrastructure/
│   └─ Integration.Worker/
├─ tests/
│   └─ Integration.UnitTests/
├─ SalesforceIntegration.slnx
├─ Directory.Build.props
├─ global.json
├─ .editorconfig
└─ .gitignore
```

## 2. Proyectos y responsabilidades

| Proyecto | Tipo | Responsabilidad prevista |
|---|---|---|
| `Integration.Domain` | Biblioteca de clases | Núcleo del modelo, sin dependencias externas. Contenido concreto **[Por validar]**. |
| `Integration.Application` | Biblioteca de clases | Casos de uso y contratos (interfaces) que la infraestructura implementará. Contenido concreto **[Por validar]**. |
| `Integration.Infrastructure` | Biblioteca de clases | Implementaciones de los contratos y acceso a sistemas externos. Contenido concreto **[Por validar]**. |
| `Integration.Worker` | Worker Service | Proceso anfitrión que ejecutará la sincronización programada. Forma de hospedaje (por ejemplo, como servicio de Windows) **[Por validar]**. |
| `Integration.UnitTests` | xUnit | Pruebas unitarias de Domain y Application. |

Por ahora todos los proyectos están vacíos (cascarón): no contienen lógica de negocio.

## 3. Dirección de las dependencias

```
Domain  ←  Application  ←  Infrastructure
                        ←  Worker  (también referencia a Infrastructure)

UnitTests  →  Domain, Application
```

- `Domain` no referencia a ningún otro proyecto.
- `Application` referencia solo a `Domain`.
- `Infrastructure` referencia a `Application`.
- `Worker` referencia a `Application` e `Infrastructure`.
- `UnitTests` referencia a `Domain` y `Application`.

## 4. Configuración común

- `Directory.Build.props`: `net10.0`, `Nullable` y `ImplicitUsings` habilitados, `LangVersion` por defecto.
- `global.json`: fija el SDK instalado con `rollForward` en `latestFeature`.
- `.editorconfig`: convenciones básicas de C# (4 espacios, UTF-8). Reglas adicionales **[Por validar]**.
- Solución en formato `.slnx`, el predeterminado del SDK.

## 5. Carpetas reservadas para herramientas de IA

| Carpeta | Uso previsto | Estado |
|---|---|---|
| `openspec/specs/` | Especificaciones vigentes | Vacía (solo `.gitkeep`) |
| `openspec/changes/` | Cambios propuestos | Vacía (solo `.gitkeep`) |
| `.claude/skills/` | Skills del proyecto | Vacía (solo `.gitkeep`) |
| `.claude/rules/` | Reglas del proyecto | Vacía (solo `.gitkeep`) |

Si se adopta OpenSpec y cómo se usará **[Por validar]**. No se ha ejecutado ninguna herramienta de inicialización.

## 6. Pendientes

- Aprobar nombres de proyectos, espacios de nombres y convenciones de código **[Por validar]**.
- Decidir el hospedaje del Worker **[Por validar]**.
- Decidir si se expondrá alguna API **[Por validar]**.
- Definir proyectos de pruebas adicionales (integración) **[Por validar]**.
