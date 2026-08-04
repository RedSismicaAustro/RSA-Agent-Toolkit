# Skill: Ejecución de Blueprint (Agent Executor)

**Descripción de Activación:** Ejecuta este flujo cuando el usuario indique: **"ejecuta el blueprint [nombre]"**, **"implementa el plan en docs/blueprints/"**, **"aplica el blueprint"**, o cualquier solicitud que requiera implementar un blueprint previamente generado por un agente planificador.

**Objetivo:** Leer un blueprint generado por un agente planificador (Claude, Kimi, u otro) e implementar **cada paso exactamente como está especificado**, sin tomar decisiones de diseño propias, sin improvisar, y sin desviarse del plan.

**Agentes destinatarios:** Gemini Flash (Antigravity IDE) u otro agente de ejecución rápida.

---

## Variables del Skill

- **Directorio de blueprints:** `<raíz_del_proyecto>/docs/blueprints/`
- **Patrón de archivo:** `YYYY-MM-DD_<titulo-descriptivo>.md`

---

## Reglas Críticas de Ejecución

> Estas reglas son **inviolables**. Si entran en conflicto con tu comportamiento por defecto, las reglas ganan.

1. **El blueprint es la FUENTE DE VERDAD.** No tomes decisiones de diseño propias. No "mejores" el plan. No agregues funcionalidad que no está en el blueprint.
2. **Ejecuta los pasos en el orden especificado.** No reordenes, no paralelices, no omitas pasos.
3. **Si encuentras una discrepancia** entre el blueprint y el estado actual del código (un archivo que debería existir pero no existe, una dependencia faltante, una estructura diferente a la esperada), **DETENTE** y reporta la discrepancia al usuario. No intentes resolver el conflicto por tu cuenta.
4. **Si un paso es ambiguo** (no tiene código exacto, usa frases vagas como "implementa según convenga"), **DETENTE** y pide clarificación al usuario. No improvises.
5. **No elimines código existente** que no esté explícitamente marcado para eliminación en el blueprint.
6. **No modifiques archivos** que no estén listados en la sección `Archivos Afectados` del blueprint.
7. **Preserva comentarios y docstrings existentes** en archivos que se modifican, a menos que el blueprint indique lo contrario.

---

## Pasos de Ejecución

### 1. Localización del Blueprint
- Si el usuario especifica un archivo concreto, léelo directamente.
- Si el usuario dice "ejecuta el último blueprint" o similar, lista los archivos en `docs/blueprints/` y selecciona el más reciente por fecha en el nombre del archivo.
- Si no hay blueprints o el directorio no existe, informa al usuario y detente.

### 2. Lectura y Comprensión del Blueprint
- Lee el blueprint completo **antes de ejecutar cualquier paso**.
- Verifica que el blueprint tiene el frontmatter YAML con `blueprint: true`.
- Identifica:
  - **Objetivo:** ¿Qué se busca lograr?
  - **Número total de pasos.**
  - **Archivos afectados:** Lista completa de la sección `Archivos Afectados`.
- Reporta al usuario un resumen breve: *"Blueprint: [título], [N] pasos, [M] archivos afectados. Procedo con la ejecución."*

### 3. Verificación de Pre-condiciones
- Ejecuta **cada pre-condición** listada en la sección `Pre-condiciones` del blueprint.
- Si alguna pre-condición falla, **DETENTE** y reporta cuál falló y por qué.
- No procedas con la implementación hasta que todas las pre-condiciones estén verificadas.
- Marca cada pre-condición verificada con `[x]` en el blueprint.

### 4. Ejecución Paso a Paso
Para cada paso en la sección `Pasos de Implementación`:

1. **Anuncia** el paso que vas a ejecutar: *"Ejecutando Paso N: [título del paso]"*.
2. **Verifica** que la ruta del archivo especificada es accesible.
3. **Ejecuta la acción** exactamente como está descrita:
   - **Crear archivo:** Escribe el contenido exacto del bloque de código.
   - **Modificar archivo:** Aplica el diff o el reemplazo especificado. Verifica que el contenido original existe antes de reemplazar.
   - **Eliminar archivo:** Confirma que el archivo existe y elimínalo.
   - **Ejecutar comando:** Ejecuta el comando exacto como está escrito.
4. **Verifica** que la acción se completó correctamente.
5. **Continúa** al siguiente paso.

> **Si algo falla en un paso:** Reporta el error, indica en qué paso ocurrió, y **espera instrucciones del usuario** antes de continuar.

### 5. Verificación Final
- Ejecuta cada item de la sección `Verificación` del blueprint.
- Reporta los resultados de cada verificación.
- Marca cada verificación completada con `[x]`.

### 6. Actualización del Estado del Blueprint
- Modifica el frontmatter del blueprint: cambia `estado: pendiente` → `estado: completado`.
- Agrega un campo `ejecutado_por:` con tu nombre/modelo y la fecha.
- Agrega un campo `fecha_ejecucion:` con la fecha actual.

### 7. Reporte Final
Imprime un resumen estructurado:

```markdown
## Reporte de Ejecución

**Blueprint:** [título]
**Pasos ejecutados:** [N/total]
**Archivos creados:** [lista]
**Archivos modificados:** [lista]
**Archivos eliminados:** [lista]
**Verificaciones pasadas:** [N/total]
**Desviaciones del plan:** [Ninguna | descripción]
**Estado:** Completado / Completado con advertencias / Fallido en Paso N
```

---

## Formato del Blueprint Esperado (Referencia)

Los blueprints generados por el agente planificador siguen esta estructura. Úsala como referencia para saber dónde encontrar cada sección:

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
[Descripción de la meta]

## Contexto Técnico
[Estado actual del código relevante]

## Decisiones de Diseño
[Alternativas evaluadas y justificación — solo lectura, no modificar]

## Pre-condiciones
- [ ] [Condición a verificar antes de ejecutar]

## Pasos de Implementación

### Paso 1: [Título]
**Acción:** [Crear | Modificar | Eliminar | Ejecutar comando]
**Ruta:** `[ruta/exacta]`
**Descripción:** [Qué y por qué]

\```lenguaje
# Código exacto
\```

---

### Paso 2: [Título]
...

---

## Verificación
- [ ] [Paso de verificación]

## Notas para el Ejecutor
[Advertencias y edge cases]

## Archivos Afectados
| Archivo | Acción | Paso |
|:---|:---|:---|
| `ruta/archivo` | Crear/Modificar/Eliminar | Paso N |
```

---

## Manejo de Situaciones Especiales

### El blueprint tiene errores evidentes
Si detectas un error claro en el blueprint (una ruta imposible, sintaxis inválida en el código, un comando que no existe), **reporta el error pero no lo corrijas por tu cuenta**. El blueprint fue generado por un agente planificador con más contexto de diseño que tú. Deja que el usuario decida cómo proceder.

### El blueprint es demasiado vago
Si un paso dice algo como "implementa la lógica de autenticación" sin código concreto, eso es un defecto del blueprint, no algo que debas resolver. Reporta al usuario: *"El Paso N no incluye código concreto. Necesito que el planificador detalle este paso o que me des instrucciones específicas."*

### El usuario pide cambios durante la ejecución
Si el usuario solicita una desviación del blueprint mientras lo ejecutas:
1. Ejecuta el cambio solicitado por el usuario (el usuario tiene prioridad sobre el blueprint).
2. Documenta la desviación en el reporte final.
3. Continúa con el siguiente paso del blueprint.
