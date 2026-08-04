# Skill: Planificación de Arquitectura (Agent Planner)

**Descripción de Activación:** Ejecuta este flujo cuando el usuario indique: **"planifica [tarea/feature]"**, **"diseña la arquitectura de [componente]"**, **"crea un blueprint para [objetivo]"**, o cualquier solicitud que requiera diseño técnico previo a la implementación.

**Objetivo:** Explorar el codebase del proyecto, diseñar una solución técnica y producir un **blueprint** estructurado y sin ambigüedades que un agente ejecutor (típicamente Gemini Flash en Antigravity) pueda implementar al pie de la letra sin tomar decisiones de diseño propias.

**Agentes destinatarios:** Claude (Open Code), Kimi, o cualquier agente con capacidad de razonamiento profundo y acceso al sistema de archivos.

---

## Variables del Skill

- **Directorio de salida de blueprints:** `<raíz_del_proyecto>/docs/blueprints/`
- **Nombre del archivo:** `YYYY-MM-DD_<titulo-descriptivo>.md` (fecha local del sistema, título en kebab-case)
- **Estado del proyecto (si existe):** `<raíz_del_proyecto>/docs/context/` o cualquier `SYSTEM_STATE.md` disponible

---

## Pasos de Ejecución

### 1. Comprensión del Requerimiento
- Asegúrate de entender completamente lo que el usuario necesita.
- Si el requerimiento es ambiguo, **pregunta antes de planificar**. No asumas.
- Identifica: ¿Es una feature nueva? ¿Una refactorización? ¿Una corrección de bug? ¿Un cambio de arquitectura?

### 2. Exploración del Codebase
Antes de diseñar, **investiga el estado actual del proyecto**:

1. **Lee la estructura de directorios** del proyecto para entender la organización.
2. **Busca archivos de contexto existentes:**
   - `SYSTEM_STATE.md`, `README.md`, `docs/context/*.md` — para entender el estado actual.
   - `docs/blueprints/*.md` — para verificar si ya existe un blueprint relacionado.
   - ADRs en `decisiones/` o `adr/` — para respetar decisiones previas.
3. **Lee los archivos de código relevantes** al requerimiento:
   - Archivos que se van a modificar.
   - Archivos que dependen de los que se van a modificar.
   - Archivos de configuración (dependencias, variables de entorno, etc.).
4. **Identifica restricciones técnicas:**
   - Lenguaje y versión del runtime.
   - Dependencias existentes (no introduzcas nuevas sin justificación).
   - Patrones de código ya establecidos en el proyecto (respétalos).

> **REGLA:** No planifiques sin haber leído al menos la estructura del proyecto y los archivos directamente afectados. Un plan basado en suposiciones produce blueprints defectuosos.

### 3. Diseño de la Solución
- Evalúa las opciones técnicas viables.
- Elige la que mejor se alinee con la arquitectura existente del proyecto.
- Si la decisión es significativa (afecta a múltiples componentes o es difícil de revertir), documéntala en la sección `Decisiones de Diseño` del blueprint.

### 4. Escritura del Blueprint
Crea el archivo en `<raíz_del_proyecto>/docs/blueprints/` usando **exactamente** el formato especificado a continuación. No omitas secciones. Si una sección no aplica, escribe "N/A" en lugar de eliminarla.

### 5. Confirmación al Usuario
- Reporta la ruta del blueprint generado.
- Resume los puntos clave del diseño en 3-5 líneas.
- Indica si hay decisiones que requieran validación del usuario antes de proceder a la ejecución.

---

## Formato del Blueprint (Especificación Obligatoria)

Todo blueprint generado por esta skill **DEBE** seguir exactamente esta estructura:

```markdown
---
blueprint: true
titulo: "[Título descriptivo del objetivo]"
proyecto: "[nombre_del_repositorio]"
fecha: YYYY-MM-DD
planificado_por: "[Nombre del agente/modelo que generó este blueprint]"
estado: pendiente
---

# Blueprint: [Título descriptivo]

## Objetivo
[1-2 párrafos claros describiendo QUÉ se va a lograr y POR QUÉ. El agente ejecutor
debe entender la meta sin contexto adicional.]

## Contexto Técnico
[Resumen del estado actual del código relevante a este cambio. Incluir:
- Archivos existentes que se modificarán y su rol actual.
- Dependencias relevantes.
- Restricciones técnicas descubiertas durante la exploración.]

## Decisiones de Diseño
[Solo si aplica. Documentar las alternativas evaluadas y la justificación de la
elección. Si no hay decisiones significativas, escribir "N/A".]

## Pre-condiciones
[Lista de verificación que el agente ejecutor DEBE confirmar antes de empezar.
Usar checkboxes.]

- [ ] Verificar que [dependencia/herramienta] está disponible
- [ ] Verificar que [archivo/directorio] existe en [ruta exacta]
- [ ] Verificar que [servicio] está en estado [esperado]

## Pasos de Implementación

### Paso 1: [Título descriptivo de la acción]
**Acción:** [Crear archivo | Modificar archivo | Eliminar archivo | Ejecutar comando]
**Ruta:** `[ruta/exacta/al/archivo]`
**Descripción:** [Explicación breve de qué hace este paso y por qué]

[Incluir el contenido exacto o el diff a aplicar en un bloque de código con
el lenguaje correcto:]

\```python
# Código exacto a escribir o cambio a realizar
\```

[Si es una modificación parcial, usar formato diff:]

\```diff
- línea_original_a_reemplazar
+ línea_nueva_que_la_sustituye
\```

---

### Paso 2: [Título descriptivo de la acción]
...

[Repetir para cada paso. Cada paso debe ser atómico: una sola acción clara.]

---

## Verificación
[Lista de pasos que el agente ejecutor debe realizar para confirmar que la
implementación fue exitosa.]

- [ ] Ejecutar `[comando_de_test]` → resultado esperado: [descripción]
- [ ] Verificar que [archivo] contiene [contenido esperado]
- [ ] Confirmar que [funcionalidad] opera correctamente

## Notas para el Ejecutor
[Advertencias, edge cases, o información que el agente ejecutor necesita saber
para evitar errores. Si no hay notas especiales, escribir "N/A".]

## Archivos Afectados
[Lista completa de todos los archivos que este blueprint crea, modifica o elimina.
Esto permite al ejecutor verificar el alcance antes de empezar.]

| Archivo | Acción | Paso |
|:---|:---|:---|
| `ruta/al/archivo1.py` | Crear | Paso 1 |
| `ruta/al/archivo2.md` | Modificar | Paso 3 |
| `ruta/al/archivo3.cfg` | Eliminar | Paso 5 |
```

---

## Reglas de Calidad del Blueprint

1. **Cada paso debe ser autocontenido.** El ejecutor no debe necesitar inferir qué código escribir. Si el paso dice "Crear archivo", el blueprint DEBE incluir el contenido completo del archivo.
2. **Las rutas deben ser exactas y relativas** a la raíz del proyecto. Nunca uses rutas ambiguas como "el archivo de configuración".
3. **Los diffs deben ser aplicables.** Si usas formato diff, incluye suficiente contexto para identificar la ubicación exacta del cambio.
4. **No dejes decisiones para el ejecutor.** Frases como "elige la mejor opción" o "implementa según convenga" están **prohibidas**. Toda decisión de diseño se toma en la planificación.
5. **Ordena los pasos por dependencia.** Si el Paso 3 depende del Paso 1, el Paso 1 va primero. Nunca asumas ejecución paralela.
6. **Los comandos de terminal deben ser copy-paste.** Incluye flags, rutas y argumentos completos. No uses variables de entorno que el ejecutor no pueda resolver.
