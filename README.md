# CliniBook

## 📋 Descripción

Aplicación móvil desarrollada para el registro y control de chequeos médicos de pacientes, orientada a profesionales de la salud y centros clínicos que necesitan digitalizar el seguimiento de signos vitales y métricas corporales. La plataforma permite capturar datos como peso, altura, presión arterial, edad y comentarios del paciente, calculando automáticamente el Índice de Masa Corporal (IMC) y clasificando el resultado según los estándares de la OMS. Cada registro se almacena localmente con fecha y hora, posibilitando la consulta histórica ordenada por fecha descendente. Como resultado, se elimina el registro en papel y se agiliza la consulta del historial clínico básico desde un único dispositivo móvil.

## ✨ Funcionalidades

- **Registro de control médico** — Formulario con validaciones en tiempo real y cálculo automático del IMC
- **Listado cronológico** — Visualización de todos los registros ordenados por fecha de forma descendente
- **Detalle con clasificación de IMC** — Vista individual con categorización (bajo peso, normal, sobrepeso, obesidad)
- **Eliminación con confirmación** — Borrado de registros mediante diálogo de confirmación con alerta de persistencia
- **Almacenamiento local** — Base de datos SQLite embebida sin necesidad de conexión a internet

## 🛠 Stack Tecnológico

| Capa | Tecnología |
|---|---|
| Lenguaje | Kotlin |
| UI | Activities + XML (Material Design) |
| Base de Datos | SQLite (SQLiteOpenHelper) |
| Async | Coroutines (Dispatchers.IO + Main) |
| Patrón | DAO + Adapter + ViewHolder |

## 📁 Estructura del Proyecto

```
app/src/main/java/com/example/meditrackappv100/
├── activitys/
│   ├── ListControlMedico.kt      # Pantalla principal con lista y acceso a registro
│   ├── RegisControlMedico.kt     # Formulario de registro con validación y cálculo de IMC
│   └── DetalleControl.kt         # Detalle del control, clasificación de IMC y eliminación
├── adapter/
│   └── ControlMedicoAdapter.kt   # Adaptador RecyclerView con ViewHolder
├── data/
│   ├── AppDatabaseHelper.kt      # Helper de SQLite (creación y migración)
│   └── ControlMedicoDAO.kt       # CRUD (insertar, listar, obtener por ID, eliminar)
└── entity/
    └── ControlMedico.kt          # Data class con campos del control médico
```

## 🏗 Arquitectura

```
[Activity] → [ControlMedicoDAO] → [AppDatabaseHelper] → [SQLite]
     ↕
[Adapter / ViewHolder]
```

**Patrones implementados:**
- **DAO (Data Access Object)** — `ControlMedicoDAO` encapsula todas las operaciones de base de datos
- **Adapter + ViewHolder** — `ControlMedicoAdapter` con patrón ViewHolder para RecyclerView eficiente
- **Data Class** — `ControlMedico` como modelo inmutable para transferencia de datos
- **Coroutines** — Operaciones de BD ejecutadas en `Dispatchers.IO` y resultados en `Dispatchers.Main`

**Flujo de operaciones:** Cada solicitud de lectura/escritura abre y cierra la conexión a SQLite mediante `CoroutineScope`, ejecutando las consultas en segundo plano para no bloquear la UI. Los inserts utilizan `ContentValues` y las lecturas `rawQuery` con `Cursor`.

## 🚀 Requisitos Previos

- Android Studio (Hedgehog o superior)
- SDK Android 21+ (mínimo)
- Kotlin plugin
- Gradle 8.x
