# Skill: Diagnóstico Técnico

**Descripción de Activación:** Ejecuta este flujo cuando el usuario indique: **"genera un diagnóstico técnico de [problema/componente/sistema]"**, **"diagnostica [situación]"**, o cuando se necesite documentar formalmente errores, anomalías o situaciones técnicas para resolverlas posteriormente.

**Objetivo:** Analizar un problema técnico observado (error, anomalía, degradación, comportamiento inesperado), documentar el estado actual, la evidencia, los hallazgos y las opciones de solución en un documento estructurado que sirva como insumo para la toma de decisiones, la generación de blueprints (vía `planning_guide`) o la extracción de ADRs (vía `extraer_adr`).

---

## Alcance y Límites del Skill

Este skill **genera el diagnóstico**, no la solución. Su salida es un documento que:
- Identifica y documenta el **estado actual** del sistema afectado.
- Presenta la **evidencia** recopilada (logs, configuraciones, comportamientos observados).
- Analiza la **causa raíz** de cada hallazgo.
- Evalúa **opciones y decisiones** cuando corresponda.
- Cataloga las **mejoras pendientes** como backlog accionable.

Si del diagnóstico surgen:
- **Decisiones de arquitectura importantes** → Sugerir al usuario: `"extrae un ADR de [decisión]"`.
- **Un plan de implementación** → Sugerir al usuario: `"planifica [solución]"` (skill `planning_guide`).

---

## Pasos de Ejecución

### 1. Identificación del Problema y Alcance

- Confirma con el usuario **qué problema o componente** se va a diagnosticar.
- Identifica la **raíz del proyecto** al que pertenece el componente afectado.
- Determina el **alcance** del diagnóstico:
  - ¿Es un error puntual en un componente, servicio o instancia específica?
  - ¿Es un problema sistémico que afecta múltiples componentes o el sistema en general?
  - ¿Es un análisis investigativo (entender un comportamiento, no necesariamente un fallo)?

### 2. Recopilación de Evidencia

- Solicita al usuario los **archivos de logs, configuración o recursos** necesarios para el análisis. El usuario proporcionará acceso a estos archivos indicando la carpeta donde los ha colocado.
- Lee y analiza:
  - **Logs de aplicación**: Errores, advertencias, secuencias temporales.
  - **Configuración**: Variables de entorno, archivos de configuración, docker-compose, etc.
  - **Código fuente**: Archivos directamente relacionados con el componente afectado.
  - **Documentación existente**: Contextos técnicos (`docs/context/`), ADRs, blueprints.
- Construye una **cronología del evento** basada en timestamps de los logs.

### 3. Análisis y Diagnóstico

- Para cada síntoma o anomalía observada, identifica:
  - **Descripción técnica** del fallo o comportamiento.
  - **Causa raíz**: ¿Por qué sucedió?
  - **Condiciones de activación**: ¿Por qué se manifestó en este contexto particular?
  - **Impacto**: ¿Qué componentes dependientes se vieron afectados?

### 4. Determinación de Secciones a Incluir

El agente decide qué secciones incluir según la naturaleza del problema. **No se etiqueta un "modo"**; simplemente se incluyen las secciones que aporten valor:

| Sección | Incluir cuando... |
|---|---|
| Encabezado y Metadatos | **Siempre** |
| Resumen Ejecutivo | **Siempre** |
| Estado Actual | **Siempre** — tabla de componentes afectados con su estado operativo |
| Evidencia y Análisis | **Siempre** — logs, extractos, cronología |
| Hallazgos y Causa Raíz | **Siempre** |
| Evaluación de Riesgo | El problema tiene impacto potencial en múltiples componentes o el sistema en general |
| Opciones y Decisiones | Existen decisiones de diseño abiertas con múltiples alternativas viables |
| Mitigaciones Aplicadas | **Siempre** — puede indicar "Ninguna aplicada aún" si corresponde |
| Backlog de Mejoras | **Siempre** |
| Dependencias y Prerrequisitos | La solución requiere cambios de infraestructura, herramientas o configuraciones previas |
| Plan de Validación | Se proponen cambios verificables con checkpoints concretos |

### 5. Escritura del Documento de Diagnóstico

#### Ruta de salida

```
<raíz_del_proyecto>/docs/analysis/YYYY-MM-DD_diagnostico_<tema-descriptivo>.md
```

Ejemplo: `docs/analysis/2026-09-01_diagnostico_parada_adquisicion_cha01.md`

#### Plantilla del documento

```markdown
---
proyecto: [nombre_del_proyecto]
tipo: diagnostico_tecnico
resolucion: pendiente | en_proceso | resuelto
temas: [tema1, tema2]
fecha: YYYY-MM-DD
---

# Diagnóstico Técnico: [Título descriptivo del problema]

**Fecha**: YYYY-MM-DD  
**Proyecto / Repositorio**: `[nombre]`  
**Componente(s) afectado(s)**: [lista de componentes, servicios o módulos]  
**Estado**: Diagnosticado | Mitigado localmente | En investigación | Pendiente de despliegue  
**Severidad**: Crítica | Alta | Media | Baja  

---

## 1. Resumen Ejecutivo
[1-3 párrafos: qué ocurrió, cómo se detectó, conclusión general del diagnóstico.]

---

## 2. Estado Actual
[Tabla de componentes relevantes con su estado operativo actual.]

| Componente | Estado | Observaciones |
|---|---|---|
| [Componente 1] | ✅ Operativo / ⚠️ Degradado / ❌ Inoperativo | [Detalle] |

---

## 3. Evidencia y Análisis
* **Ruta de los recursos analizados**: [rutas de logs, configuraciones, etc.]
* **Extractos relevantes**: [Fragmentos clave con bloques de código]
* **Cronología del evento**: [Secuencia temporal numerada]

---

## 4. Hallazgos y Causa Raíz

### Hallazgo 1: [Título descriptivo]
* **Descripción técnica**: [Qué ocurrió]
* **Causa**: [Por qué ocurrió]
* **Condiciones de activación**: [Por qué en este contexto particular]

### Hallazgo N: [...]

---

## 5. Evaluación de Riesgo
<!-- Incluir solo si el problema tiene impacto potencial en múltiples componentes o el sistema en general -->

| # | Escenario | Probabilidad | Impacto | Mitigación Requerida |
|---|---|---|---|---|
| R1 | [Escenario de riesgo] | Baja/Media/Alta | Bajo/Medio/Alto/Crítico | [Acción requerida] |

---

## 6. Opciones y Decisiones
<!-- Incluir solo si hay decisiones de diseño abiertas -->

### [Decisión 1]: [Pregunta que debe resolverse]

| Opción | Descripción | Ventaja | Desventaja |
|---|---|---|---|
| A | [Descripción] | [Pro] | [Contra] |
| B | [Descripción] | [Pro] | [Contra] |

> **Recomendación**: [Opción recomendada y justificación]

---

## 7. Mitigaciones Aplicadas
[Acciones puntuales tomadas para estabilizar el sistema. Si no se aplicaron aún, indicar "Ninguna aplicada aún — pendiente de implementación".]

---

## 8. Backlog de Mejoras
[Catálogo numerado de mejoras identificadas para evitar recurrencia o aumentar resiliencia.]

1. **[Mejora 1]**: [Descripción de la corrección o refactorización sugerida.]
2. **[Mejora 2]**: [Nuevas alertas, validaciones o automatizaciones.]
3. **Criterios de aplicación**: [Bajo qué condiciones implementar estas mejoras.]

---

## 9. Dependencias y Prerrequisitos
<!-- Incluir solo si la solución requiere cambios de infraestructura -->

| Prerrequisito | Estado | Acción Requerida |
|---|---|---|
| [Prerrequisito 1] | ✅ Disponible / ⬜ Por configurar | [Acción] |

---

## 10. Plan de Validación
<!-- Incluir solo si se proponen cambios verificables -->

| # | Checkpoint | Criterio de Éxito |
|---|---|---|
| CP-1 | [Qué verificar] | [Cómo confirmar que funciona] |
```

### 6. Actualización del Índice Temático

- Abre `rsa/RSA-Metodologias/indice/indice_tematico.md`.
- Bajo la sección `## Diagnósticos Técnicos` (créala si no existe), añade la entrada:

  ```markdown
  - **[entorno/proyecto]**:
    - `YYYY-MM-DD` — [Resumen de una línea del problema].
      Estado: **pendiente**.
      → [ruta relativa al archivo en el proyecto](enlace al repo de GitHub)
  ```

- **No dupliques** entradas existentes; actualízalas si ya existen.

#### Ciclo de vida en el índice

Cuando el diagnóstico cambie de estado, el usuario actualizará la entrada del índice:

- **Pendiente**: Entrada con enlace al archivo del proyecto.
  ```markdown
  - `2026-09-01` — Parada de adquisición CHA01: Fallo en cascada por named pipe tras reinicio.
    Estado: **pendiente**.
    → [diagnostico_parada_adquisicion_cha01.md](enlace)
  ```

- **En proceso**: Se actualiza el estado.
  ```markdown
    Estado: **en proceso**.
  ```

- **Resuelto**: Se actualiza el estado, se añade fecha de cierre, se referencian artefactos derivados, y se elimina el enlace al archivo (ya que este será borrado del proyecto).
  ```markdown
  - `2026-09-01` — Parada de adquisición CHA01: Fallo en cascada por named pipe tras reinicio.
    Estado: **resuelto** (2026-09-05). Produjo: ADR-018, blueprint fase-6.
  ```

### 7. Sugerencias de Seguimiento

Al finalizar, presenta al usuario las acciones de seguimiento disponibles según el contenido del diagnóstico:

- Si se identificaron **decisiones de arquitectura importantes**:
  > "¿Deseas extraer un ADR? Di: `extrae un ADR de [decisión]`"

- Si el diagnóstico requiere un **plan de implementación**:
  > "¿Deseas generar un blueprint? Di: `planifica [solución]`"

- Si no se requieren acciones adicionales, simplemente confirma la ruta del archivo generado y el resumen.

### 8. Confirmación al Usuario

- Reporta la ruta del archivo de diagnóstico generado.
- Resume los hallazgos principales en 3-5 líneas.
- Confirma la actualización del índice temático.
- Presenta las sugerencias de seguimiento (paso 7).
- Sugiere un formato de commit:
  ```
  docs: diagnóstico técnico de [tema descriptivo]
  ```
