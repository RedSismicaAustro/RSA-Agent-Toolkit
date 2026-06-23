# Skill: Crear Archivo de Transición Técnica

**Descripción de Activación:** Ejecuta este flujo ÚNICAMENTE cuando el usuario indique: **"crea un archivo de transición técnica"**, **"genera la transición técnica de esta sesión"** o comandos con intenciones equivalentes.

**Objetivo:** Sistematizar los avances, el estado técnico de los entornos de desarrollo, las decisiones de código y los siguientes pasos en un documento Markdown estandarizado dentro del repositorio activo, de modo que cualquier agente de IA subsecuente pueda reanudar el trabajo de forma inmediata y sin pérdida de contexto semántico.

---

## Pasos de Ejecución

### 1. Identificación del Repositorio y Rutas
- Identifica la raíz del proyecto activo.
- La ruta de destino del archivo generado SIEMPRE será: `<raiz_del_proyecto>/docs/progress/`.
- El archivo se nombrará siguiendo la estructura: `YYYY-MM-DD_contexto-agente.md` utilizando la fecha local del sistema.
  - Ejemplo: `2026-06-22_contexto-agente.md`

### 2. Extracción y Estructuración de Información
Analiza todo el historial de la conversación actual para recopilar:
- **Hitos**: Los logros técnicos completados en la sesión (migraciones, nuevas características, refactorizaciones, etc.).
- **Estado de Entornos**: Detalle de los entornos virtuales creados o modificados, especificando nombres, versiones de Python relevantes y estrategias aplicadas (por ejemplo, instalaciones mixtas Conda/Pip).
- **Decisiones Técnicas**: Resoluciones de diseño de software, eliminaciones de librerías redundantes, y porqués de cambios estructurales.
- **Estructura**: Un árbol representativo del repositorio o los directorios modificados durante la sesión.
- **Pendientes**: Un listado claro e incremental de tareas a realizar para guiar al siguiente agente de desarrollo.

### 3. Escritura del Documento de Transición
Crea el archivo utilizando exactamente la siguiente estructura estándar:

```markdown
# Resumen de Sesión: [Título descriptivo del objetivo principal]

**Fecha**: YYYY-MM-DD  
**Repositorio**: `[nombre_del_repositorio]`  
**Agente de IA**: [Nombre del agente de IA actual]  
**Usuario**: [Nombre del usuario]  

---

## 🎯 Objetivo de la Sesión
[Breve párrafo de 2-3 líneas describiendo la meta de la sesión de hoy].

---

## 📂 Estructura del Repositorio Implementada
[Si aplica, mostrar un diagrama de árbol de la estructura de archivos y directorios creados o modificados].

```text
[Árbol de directorios]

---

## ⚙️ Configuración del Entorno Virtual (`[nombre_del_entorno]`)
[Detallar las configuraciones de los entornos de desarrollo creados, dependencias científicas pesadas, canales y si se requirieron soluciones mixtas de empaquetado].

---

## 🛠️ Modificaciones de Código y Refactorización
[Detallar los cambios puntuales en los archivos fuente, lógica de programación depurada, simplificaciones de código y eliminación de dependencias obsoletas].

---

## 📋 Pasos Sugeridos para el Siguiente Agente
[Detallar de forma secuencial y numerada las tareas que debe tomar el siguiente agente para continuar de inmediato].

```

### 4. Confirmación al Usuario
Al finalizar la escritura del archivo:
- Reporta la ruta absoluta del archivo Markdown generado.
- Presenta un resumen muy breve de los puntos clave documentados.
- Sugiere un formato de commit para el repositorio de trabajo según las normas institucionales (minúsculas y prefijado por el tipo).
