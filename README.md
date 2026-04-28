# Ops Center · Clipp
## Sistema integral de gestión operativa de despliegues

> **Versión:** 5.50 + Cadena de Valor 1.0
> **Stack:** HTML/CSS/JS vanilla + Supabase + Chart.js
> **Arquitectura:** SPA single-file · sin build · sin dependencias externas instalables

Sistema interno de Clipp para gestionar el ciclo completo de implementación de servicios de taxi: desde captación comercial, contratos y cobranzas, hasta el despliegue técnico paso a paso con cronograma Gantt y motor de reglas.

---

## 📦 Contenido del paquete

| Archivo | Para qué sirve |
|---|---|
| `index.html` | Aplicación completa. Reemplaza al `index.html` actual. |
| `supabase_cadena_valor.sql` | Schema de la base de datos del módulo nuevo. Se ejecuta UNA vez en Supabase. |
| `diagnose_catalogs.sql` | Script de diagnóstico/reparación si los catálogos se ven vacíos. |
| `SETUP.md` | Guía de activación paso a paso (10 minutos). |
| `README.md` | Este archivo. Documentación completa del sistema. |

---

## 🆕 ¿Qué hay de nuevo?

### 1. Módulo Cadena de Valor (centro maestro de despliegues)
Sección completamente nueva en el menú lateral. Gestiona el ciclo de implementación de un cliente con un motor de reglas, plantillas Tipo 1–8 y un Gantt visual.

### 2. Catálogos: render completo
Antes algunas secciones (Aplicativos, Tipos de Cliente, Modalidades, Categorías, Tipos de Cobro, Estados de Riesgo) solo se cargaban al hacer click. Ahora **todas se renderean al entrar** a Catálogos.

### 3. Botones de diagnóstico en Catálogos
Dos botones nuevos en la cabecera:
- **🔍 Diagnóstico** — analiza qué hay en Supabase y en memoria, te dice si algo está mal.
- **🔄 Recargar** — vuelve a leer los catálogos desde Supabase y refresca la pantalla.

### 4. 15 tablas nuevas en Supabase
Para gestionar componentes del ecosistema, plantillas, checklists, reglas, gates, proyectos, fases, tareas, feriados y auditoría centralizada.

### 5. Hook automático al login
Después de autenticarse contra Supabase, además de cargar `ops_data` (legacy), también se cargan automáticamente los datos del módulo Cadena de Valor.

### 6. Caché offline
El módulo Cadena de Valor guarda copia local en `localStorage` (clave `cv_cache_v1`). Si Supabase no responde, sigue funcionando con la última versión cacheada.

---

## 🗺️ Mapa del sistema

### Vistas principales (sidebar izquierdo)

#### Principal
- **📊 Dashboard Ejecutivo** — Resumen operativo del mes con KPIs
- **📅 Línea de Tiempo** — Historial y proyección por categoría

#### Operaciones
- **🚀 Despliegues** — Seguimiento operativo de implementaciones en curso
- **✅ Clientes Operativos** — Clientes con despliegue finalizado
- **⛔ Clientes Inactivos** — Clientes deshabilitados o perdidos
- **⚡ Capacidad Operativa** — Carga del equipo en tiempo real
- **🔗 Cadena de Valor** ✨ NUEVO — Centro maestro de configuración de despliegue

#### CRM
- **👥 Clientes** — Gestión de cartera de clientes
- **📋 Contratos** — Condiciones de cobro por cliente
- **💰 Cobranzas** — Facturación y cobros mensuales

#### Configuración
- **🗂 Catálogos** — Valores maestros del sistema (mejorado)
- **📖 Manual de Uso** — Guía de referencia interna
- **👤 Usuarios y Roles** — Accesos y permisos por módulo

---

## 🔗 Módulo Cadena de Valor (detalle)

Este módulo gestiona el ciclo completo de implementación de un cliente nuevo. Está diseñado en torno a tres conceptos:

1. **Componentes** — los módulos del ecosistema Clipp (Admin Web, App Conductor, Bot WhatsApp, etc.)
2. **Plantillas** — combinaciones predefinidas de componentes (Tipo 1, Tipo 2, ..., Tipo 8)
3. **Proyectos de despliegue** — instancias concretas de implementación para un cliente real

### Tabs internos

| Tab | Contenido |
|---|---|
| 📊 **Resumen** | KPIs globales, alertas, fases activas, salud de despliegues, gráfico doughnut. |
| 🎯 **Despliegues por Cliente** | Lista filtrable de proyectos. Botón **+ Nuevo Despliegue** abre wizard. |
| 🧩 **Componentes** | Catálogo maestro de módulos del ecosistema. CRUD completo. |
| 📋 **Plantillas** | Plantillas Tipo 1–8 con combinaciones predefinidas. Editables. |
| ✅ **Checklists** | Items de checklist por fase y módulo. Generan tareas del Gantt. |
| ⚙️ **Reglas** | Motor de validaciones (missing/has/both → block/warn/info). |
| 🚪 **Gates** | Hitos de aprobación entre fases. |
| 📈 **Gantt** | Cronograma visual por proyecto. Respeta feriados y findes. |
| 🔧 **Configuración** | Feriados, auditoría, sincronización, datos demo. |

### Componentes disponibles (datos demo iniciales)

| Código | Nombre | Categoría | Duración |
|---|---|---|---|
| `admin_web` | Administrador Web | Core (obligatorio) | 5 días |
| `app_conductor` | App Conductor | Módulo | 8 días |
| `app_cliente` | App Cliente | Módulo | 7 días |
| `bot_normal` | Bot WhatsApp Normal | Módulo | 4 días |
| `bot_coexistencia` | Bot WhatsApp Coexistencia | Módulo | 5 días |
| `callcenter` | Callcenter | Módulo | 6 días |
| `counter` | Counter (POS) | Módulo | 5 días |

### Plantillas disponibles

| Tipo | Combinación |
|---|---|
| Tipo 1 | Admin Web + App Conductor + Bot Normal |
| Tipo 2 | Admin Web + App Conductor + Bot Coexistencia |
| Tipo 3 | Admin Web + App Conductor + Bot + Callcenter |
| Tipo 4 | Admin Web + App Conductor + Counter |
| Tipo 5 | Admin Web + App Conductor + Bot + Counter + Callcenter |
| Tipo 6 | Admin Web + App Cliente + Bot |
| Tipo 7 | Admin Web + App Cliente + App Conductor |
| Tipo 8 | Admin Web + Bot + Callcenter + Counter |

### Las 9 fases canónicas de un despliegue

1. **Prospecto** 🔍 — captación inicial
2. **Contrato** 📋 — firma y aprobación
3. **Alta técnica** 🛠️ — datos base del cliente
4. **Configuración Base** ⚙️ — ciudad, geocercas, tarifarios
5. **Configuración de Módulos** 🧩 — comandos por módulo
6. **Capacitación** 🎓 — entrenamiento del equipo
7. **Piloto** 🧪 — pruebas en ambiente real
8. **Producción** 🚀 — paso definitivo a operación
9. **Soporte continuo** 🤝 — acompañamiento post-go-live

Cada fase tiene su **gate** (hito de aprobación). Sin aprobar el gate, la siguiente fase no se activa.

### Reglas predefinidas (motor de validaciones)

1. **Admin Web Obligatorio** — todo cliente debe tener Admin Web (bloqueante)
2. **Bot único** — Bot Normal y Bot Coexistencia no pueden coexistir (bloqueante)
3. **Counter requiere Agencia** — recordatorio (advertencia)
4. **Callcenter requiere Operadores** — recordatorio (advertencia)
5. **App Conductor requiere Vehículos** — recordatorio (info)

Tipos de condiciones:
- `missing` — dispara si NO está un componente
- `has` — dispara si SÍ está un componente
- `both` — dispara si ambos están

Tipos de acciones:
- `block` — bloquea el avance (rojo)
- `warn` — advertencia (amarillo)
- `info` — informativo (azul)

---

## 🪄 Wizard "Nuevo Despliegue" (5 pasos)

Tab **🎯 Despliegues por Cliente** → botón **+ Nuevo Despliegue**

| Paso | Qué pedís |
|---|---|
| **1. Cliente** | Nombre, país, ciudad, contacto, notas |
| **2. Plantilla** | Elegir entre Tipo 1–8 |
| **3. Módulos** | Activar/desactivar módulos. Admin Web es obligatorio. Reglas en vivo. |
| **4. Cronograma** | Fecha de inicio + responsable. Preview de fechas calculadas por fase. |
| **5. Confirmar** | Resumen + crear |

Al confirmar, el sistema genera automáticamente:

- Cliente en `cv_clients`
- Proyecto en `deployment_projects`
- 9 fases en `deployment_phases`
- Tareas (instancias de checklist filtrado por módulos elegidos) en `deployment_tasks`

Las fechas se calculan respetando **feriados** y **fines de semana** (algoritmo de "business days").

---

## 📈 Gantt visual

Tab **📈 Gantt** → seleccionar proyecto.

### Características
- Header con meses y días
- Barras coloreadas por fase
- Fines de semana en fondo gris claro
- Feriados resaltados en amarillo
- Línea roja vertical en "hoy"
- Tareas vencidas en rojo
- Tareas completadas en verde
- Tareas críticas marcadas con ●
- Click en barra abre detalle del proyecto

### Acciones disponibles
- **🔄 Regenerar** — recalcula fechas a partir de los checklists actuales
- **📤 Exportar HTML** — abre vista imprimible
- **📊 Exportar CSV** — descarga tareas (con BOM para Excel)
- **🖨️ Imprimir** — imprime el Gantt en pantalla

---

## 🗄️ Catálogos (mejorados)

Vista de **Catálogos** del menú Configuración. Ahora carga **todas las secciones automáticamente** al abrir la vista.

### Secciones disponibles

| Sección | Cuántos elementos en tu Supabase |
|---|---|
| ⚙️ Módulos | 18 |
| 📚 Categorías de Módulos | 8 |
| 📱 Aplicativos | 26 |
| 🆔 Tipos de Cliente | 2 |
| 🚀 Modalidades de Despliegue | 6 |
| 👤 Launchers | 4 |
| 🎯 Complejidad | 3 |
| 📍 Fases de Implementación | 9 |
| ⭐ Tipos de Cobro | 8 |
| ⚠️ Estados de Riesgo | 3 |

### Botones nuevos en el header

- **🔍 Diagnóstico** — analiza el estado de los catálogos. Muestra:
  - Si la sesión de Supabase está activa
  - Cuántas filas tiene `ops_data`
  - Si la fila `cats` existe y qué llaves tiene
  - Cuántos elementos hay en cada categoría
  - Recomendaciones si algo falla

- **🔄 Recargar** — fuerza la lectura desde Supabase y re-renderiza todo.

---

## 🗃️ Estructura de la base de datos

### Tablas legacy (existentes, intactas)

| Tabla | Filas esperadas |
|---|---|
| `ops_data` | 6 — keys: cats, cls, deps, roaming_state, roles, usuarios |

La tabla `ops_data` usa modelo key/value JSONB. Cada llave corresponde a una colección entera serializada:
- `cats` → todos los catálogos en un solo objeto
- `cls` → clientes
- `deps` → despliegues
- `roles` → roles disponibles
- `usuarios` → usuarios del sistema
- `roaming_state` → estado de sincronización

### Tablas nuevas del módulo Cadena de Valor

| Tabla | Propósito |
|---|---|
| `components_catalog` | Catálogo maestro de componentes del ecosistema |
| `deployment_templates` | Plantillas Tipo 1–8 |
| `checklist_templates` | Items de checklist por fase + módulo |
| `deployment_rules` | Reglas del motor de validación |
| `deployment_gates` | Hitos de aprobación por fase |
| `cv_clients` | Clientes (paralelo al `cls` legacy de `ops_data`) |
| `client_modules` | Módulos activos por cliente |
| `deployment_projects` | Proyectos de despliegue activos |
| `deployment_phases` | 9 fases por proyecto |
| `deployment_tasks` | Tareas del Gantt (instancias de checklist) |
| `deployment_task_logs` | Log de cambios sobre tareas |
| `gantt_projects` / `gantt_tasks` | Snapshots de Gantt (reservadas) |
| `holidays` | Feriados que se descuentan del cronograma |
| `activity_logs` | Auditoría central |
| `notifications` | Notificaciones (reservada) |

### Auditoría

Cada acción importante (crear proyecto, aprobar gate, completar tarea, editar componente, etc.) se registra en `activity_logs` con:
- `user_id` y `user_email`
- `module = 'cadena_valor'`
- `entity` (tipo de objeto modificado)
- `entity_id` (cuál)
- `action` — create, update, delete, approve, regenerate, complete, reopen
- `changes` — jsonb con el delta del cambio

---

## 🎨 UI/UX

- **100% reutilización** de variables CSS existentes (`--bg`, `--blue`, `--text`, `--muted`, etc.)
- **Dark mode automático** — sin código duplicado, las variables ya soportan ambos temas
- **Prefijos consistentes** — clases con `cv-` (Cadena de Valor) y `g-` (Gantt) para evitar colisiones
- **Iconos emoji nativos** — sin dependencias de librerías de iconos
- **Responsive** — KPIs en grid, panels apilables, sidebar colapsable
- **Sin frameworks** — JS vanilla, sin React/Vue/Angular

---

## 🔐 Seguridad

### RLS (Row Level Security) en todas las tablas nuevas

Cada tabla nueva tiene 2 políticas:
- `<tabla>_read_auth` — lectura solo para usuarios autenticados
- `<tabla>_write_auth` — escritura solo para usuarios autenticados

Modelo simple por defecto. Si necesitás restricciones más finas (por organización, por rol), se ajusta en `pg_policies`.

### Auditoría

- Cada operación CRUD pasa por Supabase y se registra
- `created_by` y `updated_by` se completan automáticamente con el usuario actual
- Modo offline solo permite **lectura desde caché** — escrituras requieren conexión

### Validación de inputs

- Sanitización con `esc(s)` en todos los renders
- Validación de campos obligatorios en wizard
- Confirmación previa para acciones destructivas (eliminar proyecto, aprobar gate)

---

## ⚙️ Configuración técnica

### Conexión a Supabase

Variables definidas en el `index.html`:

```javascript
const SB_URL = 'https://owfetilyxgvkrtdopskz.supabase.co';
const SB_KEY = '<anon key>';
```

Sesión persistente en `localStorage` con clave `clipp_ops_session`.

### Caché local

| Clave | Contenido |
|---|---|
| `od27` | despliegues |
| `oc27` | clientes legacy |
| `ct27` | catálogos |
| `ct_v1` | contratos |
| `cobros_v1` | cobros |
| `roles_v1` | roles |
| `usuarios_v1` | usuarios |
| `chk_v1` | checklists legacy |
| `trash_v1` | papelera |
| `cv_cache_v1` | cache completo de Cadena de Valor |
| `clipp_authed` | flag de sesión activa |

### Realtime

Suscripción activa a `postgres_changes` sobre `ops_data` para sincronización entre pestañas/usuarios. Cada cambio en otra sesión se refleja en menos de 2 segundos.

---

## 📚 Funcionalidades del sistema completo

### Despliegues (legacy)
- Tabla con paginación de 50 filas, filtros por estado/launcher/aplicativo/módulo
- Edición inline de fase, fechas y notas
- Cálculo automático de días transcurridos
- Sincronización bidireccional con Capacidad Operativa

### Clientes Operativos (legacy)
- Listado de clientes con despliegue finalizado
- Edición de modalidad, contrato, soporte
- Vinculación con cobros mensuales

### Capacidad Operativa
- KPIs de carga del equipo
- Gráfico mensual de horas de soporte vs despliegue
- Distribución por aplicativo y modalidad

### Contratos & Cobranzas
- Condiciones de cobro por cliente
- Generación automática de cobros mensuales
- Estados: pendiente, cobrado, atrasado
- Alertas de cobros vencidos en dashboard

### Usuarios y Roles
- Gestión de usuarios autorizados
- Asignación de roles (Administrador, Gerencia, Operaciones, Comercial, Soporte, Solo lectura)
- Permisos por módulo

### Catálogos (mejorado)
- 10 secciones de configuración maestra
- Edición inline con modal
- Sincronización automática con Supabase
- Botón de diagnóstico nuevo
- Render no-lazy (todo visible al entrar)

---

## 🚀 Activación e instalación

Ver `SETUP.md` para la guía paso a paso (10 minutos). Resumen:

1. **Backup** de `ops_data` (1 query SQL)
2. **Ejecutar `supabase_cadena_valor.sql`** en SQL Editor
3. **Verificar** que las 15 tablas se crearon
4. **Verificar** que las políticas RLS están activas
5. **Subir** el `index.html` nuevo
6. **Probar** Catálogos + Cadena de Valor + crear primer despliegue

---

## 🛠️ Troubleshooting

### "No veo el módulo Cadena de Valor en el menú"
1. Hard reload del navegador: **Ctrl+Shift+R** (Windows/Linux) o **Cmd+Shift+R** (Mac)
2. Verificá que el `index.html` actualizado pesa ~640 KB / ~11.500 líneas
3. Abrí la consola (F12) y buscá errores en rojo

### "Catálogos vacíos"
1. Click en **🔍 Diagnóstico** dentro de Catálogos
2. Compartí el output con soporte
3. Si dice "Faltan llaves" → ejecutar bloques 4 o 5 de `diagnose_catalogs.sql`

### "Permission denied" en cualquier tabla
- RLS demasiado restrictiva. Ver `SETUP.md` sección Troubleshooting.

### "Crea el despliegue pero no aparece"
- Verificar que las políticas RLS de `deployment_projects` están activas
- Revisar la consola por errores

### "Quiero limpiar todo y empezar de cero"
- Ver `SETUP.md` → bloque "Limpiar todo"

### "El Gantt no muestra nada"
- Asegurate de tener un proyecto seleccionado en el dropdown
- Si el proyecto no tiene tareas, click en **🔄 Regenerar**

### "Las dependencias de un componente no se respetan"
- Reglas del motor en tab **⚙️ Reglas**. Activá la regla apropiada o creá una nueva.

---

## 📊 Versionado

| Componente | Versión | Fecha |
|---|---|---|
| Ops Center base | 5.50 | 16 Abr 2026 |
| Cadena de Valor | 1.0 | 28 Abr 2026 |
| SQL schema | 1.0 | 28 Abr 2026 |

### Compatibilidad

- Requiere Supabase con la tabla `ops_data` ya configurada
- Requiere autenticación activa (cualquier usuario authenticated funciona)
- Compatible con datos legacy (no toca `ops_data`)
- Cache key: `cv_cache_v1` (independiente del cache legacy)

---

## 🎯 Roadmap sugerido

### Corto plazo
- Personalizar los 7 componentes demo según tu realidad
- Personalizar las 8 plantillas Tipo 1–8 con tus combinaciones reales
- Revisar y ajustar los ~50 items de checklist demo
- Cargar feriados de los países donde operás (ya hay 11 de Ecuador 2026)

### Mediano plazo
- Conectar `cv_clients` con `cls` legacy (vincular por `legacy_id`)
- Notificaciones push cuando un gate está pendiente
- Reportes mensuales exportables a PDF
- Vista calendario alternativa al Gantt

### Largo plazo
- App móvil para operadores en campo
- Integración con Slack/Teams para alertas
- Dashboard de NPS por cliente

---

## 💡 Tips de uso

1. **Para crear un cliente nuevo en Cadena de Valor**: usar el wizard. NO crear manualmente filas en las tablas, te perdés la generación automática de fases y tareas.

2. **Para personalizar checklists**: editar en tab Checklists antes de crear el primer cliente. Los cambios solo afectan despliegues nuevos (los existentes no se regeneran salvo botón "Regenerar Gantt").

3. **Para feriados**: cargarlos antes del año fiscal. El Gantt los respeta automáticamente.

4. **Para auditoría**: hay log de todo en `activity_logs`. Útil para reportes "quién hizo qué".

5. **Modo offline**: los datos se mantienen en caché. Pero las escrituras requieren conexión a Supabase.

---

## 📞 Soporte

Si encontrás algo extraño:

1. Abrí la consola del navegador (F12)
2. Mirá la pestaña Console por errores en rojo
3. Mirá la pestaña Network por requests fallidos a Supabase
4. Click en **🔍 Diagnóstico** dentro de Catálogos
5. Capturá pantalla del error y del diagnóstico
6. Reportá con esos datos

---

## 🏗️ Arquitectura del archivo

```
index.html (~640 KB · 11.430 líneas)
├── <head>
│   ├── <meta>, <title>, fuentes
│   ├── <style> 1: CSS principal (líneas 9-851)
│   ├── <style> 2: CSS overrides v5.x (líneas 852-1235)
│   └── <style> 3: CSS Cadena de Valor (líneas 1238-1429)
├── <body>
│   ├── Sidebar (líneas 1432-1490)
│   ├── Top bar (líneas 1492-1505)
│   ├── Content (líneas 1507-2588)
│   │   ├── view-dashboard
│   │   ├── view-timeline
│   │   ├── view-despliegues
│   │   ├── view-operativos
│   │   ├── view-inactivos
│   │   ├── view-clientes
│   │   ├── view-capacidad
│   │   ├── view-contratos
│   │   ├── view-cobranzas
│   │   ├── view-estrategia
│   │   ├── view-manual
│   │   ├── view-catalogos
│   │   ├── view-usuarios
│   │   └── view-cadena ✨ NUEVO
│   ├── Modales (líneas 2596-7100)
│   │   ├── Modales legacy (cliente, despliegue, contrato, etc.)
│   │   └── Modales Cadena de Valor (componente, plantilla, wizard, proyecto, checklist, regla, gate)
│   └── <script>s
│       ├── Lógica principal (líneas 3000-9000)
│       ├── Supabase client setup
│       ├── Render functions de cada vista
│       ├── ✨ Cadena de Valor JS Parte 1 (estado + carga)
│       ├── ✨ Cadena de Valor JS Parte 2 (CRUDs catálogos)
│       └── ✨ Cadena de Valor JS Parte 3 (proyectos + wizard + Gantt)
```

---

¡Listo para usar! 🚀
