# Skill: Planificación de Arquitectura (Agent Planner)

**Descripción de Activación:** Ejecuta este flujo cuando el usuario indique: **"planifica [tarea/feature]"**, **"diseña la arquitectura de [componente]"**, **"crea un blueprint para [objetivo]"**, o cualquier solicitud que requiera diseño técnico previo a la implementación.

**Objetivo:** Explorar el codebase real del proyecto, razonar críticamente sobre las restricciones del entorno, y producir un **plan de implementación por fases** que sea correcto, completo y verificable. El plan debe ser lo suficientemente detallado para que un agente ejecutor lo implemente, pero lo suficientemente flexible para adaptarse a la realidad del sistema.

---

## Filosofía de Planificación

> **El valor de un plan no está en su formato, sino en su profundidad de análisis.** Un plan con formato perfecto pero basado en suposiciones falsas es peor que inútil: es peligroso.

### Prioridades (en orden)
1. **Corrección técnica** — ¿El plan es factible con la infraestructura y código existentes?
2. **Completitud** — ¿Cubre todos los flujos, edge cases y dependencias entre componentes?
3. **Verificabilidad** — ¿Cada fase tiene un checkpoint concreto que demuestre que funciona?
4. **Claridad** — ¿Un agente o desarrollador puede implementarlo sin ambigüedades?

---

## Pasos de Ejecución

### 1. Comprensión del Requerimiento
- Asegúrate de entender completamente lo que el usuario necesita.
- Si el requerimiento es ambiguo, **pregunta antes de planificar**. No asumas.
- Identifica: ¿Es una feature nueva? ¿Una refactorización? ¿Una corrección de bug? ¿Un cambio de arquitectura?

### 2. Exploración Profunda del Codebase

Antes de diseñar, **investiga el estado real del proyecto**. Este es el paso más importante y donde más tiempo debes invertir.

1. **Lee la estructura de directorios** del proyecto para entender la organización.
2. **Busca archivos de contexto existentes:**
   - `SYSTEM_STATE.md`, `README.md`, `docs/context/*.md` — para entender el estado actual.
   - `docs/blueprints/*.md` — para verificar si ya existe un blueprint relacionado.
   - ADRs en `decisiones/` o `adr/` — para respetar decisiones previas.
3. **Lee los archivos de código relevantes** al requerimiento:
   - Archivos que se van a modificar (lee el código fuente completo, no solo la estructura).
   - Archivos que dependen de los que se van a modificar.
   - Archivos de configuración (dependencias, variables de entorno, docker-compose, etc.).
4. **Identifica restricciones técnicas:**
   - Lenguaje y versión del runtime.
   - Dependencias existentes (no introduzcas nuevas sin justificación).
   - Patrones de código ya establecidos en el proyecto (respétalos).
   - Limitaciones de infraestructura (RAM, disco, red, firewalls, acceso remoto).

> **REGLA CRÍTICA:** No planifiques sin haber leído al menos la estructura del proyecto y los archivos directamente afectados. Un plan basado en suposiciones produce blueprints defectuosos.

### 3. Análisis Crítico (Antes de Escribir)

Después de explorar, y **antes de escribir una sola línea del plan**, responde internamente estas preguntas:

- **¿Qué dependencias nuevas se necesitan?** ¿Están declaradas en los archivos de requisitos actuales?
- **¿Qué archivos de configuración se deben modificar?** (docker-compose, .env, config.json, etc.)
- **¿Qué variables de entorno necesitan los servicios para comunicarse entre sí?**
- **¿El plan introduce datos, esquemas o formatos nuevos?** Si es así, ¿están definidos explícitamente con tipos, nombres y ejemplos concretos? Un plan que dice "define el esquema" sin definirlo es inútil.
- **¿El plan asume la existencia de algo que NO existe todavía?** (ej. un bucket de base de datos, un tópico MQTT, un token de acceso).
- **¿El plan propone sustituir algo existente?** Si es así, ¿qué funcionalidad depende de lo que se va a sustituir? ¿Se rompe algo?
- **¿Los checkpoints de verificación son realmente ejecutables?** ¿Incluyen los comandos exactos o son descripciones vagas?

> **REGLA:** Si descubres que el plan tiene un vacío o una suposición no validada durante este análisis, resuélvelo antes de escribir. No lo dejes como "TODO" ni como "pendiente de definir".

### 4. Diseño de la Solución
- Evalúa las opciones técnicas viables.
- Elige la que mejor se alinee con la arquitectura existente del proyecto.
- Si la decisión es significativa, documéntala con las alternativas evaluadas y la justificación.

### 5. Escritura del Plan de Implementación

Crea el archivo en `<raíz_del_proyecto>/docs/blueprints/` con el nombre `YYYY-MM-DD_<titulo-descriptivo>.md`.

**Estructura recomendada** (adaptar según la complejidad del proyecto):

```markdown
# Plan de Implementación: [Título descriptivo]

**Fecha**: YYYY-MM-DD
**Proyecto**: [nombre_del_repositorio]
**Objetivo**: [1-2 párrafos describiendo QUÉ se va a lograr y POR QUÉ]

---

## Prerequisitos
[Condiciones que deben cumplirse antes de iniciar. Incluir configuraciones
de infraestructura, herramientas requeridas, y verificaciones del entorno.]

## Fase N: [Título de la fase]

**Objetivo**: [Qué se logra al completar esta fase]

### [Esquemas / Estructuras de datos / Payloads definidos]
[Si la fase introduce datos nuevos, definirlos aquí con tablas, ejemplos JSON,
o diagramas. NUNCA dejar un esquema sin definir.]

### Acciones
1. [Acción concreta con ruta de archivo y descripción de cambio]
2. [Siguiente acción...]

### Comprobación (Checkpoint)
[Pasos verificables para confirmar que la fase funciona.
Incluir comandos exactos cuando sea posible.]

---

[Repetir para cada fase]

## Decisiones Pendientes
[Si hay decisiones que requieren input del usuario, listarlas aquí
con las opciones evaluadas.]

## Diagrama de Arquitectura
[Si aplica, incluir un diagrama ASCII o Mermaid de la visión general.]
```

### 6. Auto-Revisión del Plan

Antes de presentar el plan al usuario, revísalo contra esta checklist:

- [ ] ¿Leí el código fuente de todos los archivos que el plan propone modificar?
- [ ] ¿Los esquemas de datos están definidos explícitamente (tipos, nombres, ejemplos)?
- [ ] ¿Los archivos de configuración afectados están identificados con las variables necesarias?
- [ ] ¿Las dependencias nuevas están listadas con versiones?
- [ ] ¿Los checkpoints incluyen comandos o pasos concretos (no descripciones vagas)?
- [ ] ¿El plan contradice alguna decisión previa documentada en ADRs o contextos?
- [ ] ¿El plan es factible con la infraestructura existente (RAM, disco, red, permisos)?

Si algún punto falla, corrige el plan antes de entregarlo.

### 7. Confirmación al Usuario
- Reporta la ruta del blueprint generado.
- Resume los puntos clave del diseño en 3-5 líneas.
- Indica si hay decisiones que requieran validación del usuario antes de proceder.

---

## Reglas de Calidad

1. **Sustancia sobre forma.** El formato del plan se adapta a la complejidad del problema. No fuerces secciones innecesarias. Un plan de 3 fases no necesita la misma estructura que uno de 10.
2. **Las rutas deben ser exactas y relativas** a la raíz del proyecto.
3. **No dejes decisiones para el ejecutor.** Frases como "elige la mejor opción" o "implementa según convenga" están **prohibidas**. Toda decisión de diseño se toma en la planificación.
4. **Los datos concretos valen más que las descripciones.** En lugar de "Define el esquema de la base de datos", incluye la tabla con campos, tipos y descripciones. En lugar de "Crea un payload JSON", incluye el JSON de ejemplo completo.
5. **Ordena las fases por dependencia.** Si la Fase 3 depende de la Fase 1, la Fase 1 va primero.
6. **Los comandos de terminal deben ser copy-paste.** Incluye flags, rutas y argumentos completos.
7. **Si el plan propone sustituir un componente existente, verifica qué depende de él.** Lee el código que lo usa antes de proponer eliminarlo.
