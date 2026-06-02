# Skill: Volcado de Bitácora RSA

**Descripción de Activación:** Ejecuta este flujo ÚNICAMENTE cuando el usuario indique: **"ejecuta el volcado de bitácora"** o **"actualiza la bitácora"**.

**RESTRICCIÓN CRÍTICA:** Tienes estrictamente prohibido usar el sistema nativo de "Knowledge Items", "Agent Memory" o cualquier base de datos interna del IDE. Esta habilidad consiste **exclusivamente** en la escritura de archivos de texto plano (`.md`) en el sistema de archivos local.

---

## Variables de Entorno del Skill

Antes de ejecutar, identifica estas rutas relativas al workspace raíz (`git/`):

- **Repo de bitácora personal:** `institucional/RSA-Bitacora-LLM-Milton/`
- **Índice maestro:** `rsa/RSA-Metodologias/indice/indice_tematico.md`

---

## Pasos de Ejecución

### 1. Determinación del Origen Temporal
- Identifica la fecha exacta en la que se inició el primer mensaje de esta conversación.
- Usa esa fecha (YYYY-MM-DD) para calcular la ruta de destino: `institucional/RSA-Bitacora-LLM-Milton/sesiones/YYYY/MM/`.
- Genera el nombre del archivo basado en el tema y la fecha (ej: `2026-04-20_migracion_kicad.md`).

### 2. Extracción Automática de Metadatos
- **Temas:** Extrae entre 1 y 4 palabras clave (ej. `mqtt`, `grafana`, `telemetria`).
- **Entorno:** Identifica el proyecto o hardware en que se enfocó el trabajo (ej. `acelerografo`, `tig`, `edge-device`).
- **Formato YAML frontmatter:**
  ```yaml
  ---
  fecha: YYYY-MM-DD
  temas: [tema1, tema2]
  entorno: [entorno1]
  autor: Milton
  ---
  ```

### 3. Reconstrucción Cronológica
- Analiza el historial completo de la sesión actual.
- Agrupa los avances, decisiones de arquitectura y fragmentos de código por día calendario.

### 4. Escritura o Actualización del Archivo de Bitácora

**Si el archivo NO existe:**
1. Crea el archivo con el bloque YAML en la línea 1.
2. A continuación, escribe el registro cronológico del día:

```markdown
# Actividad del [FECHA]

**Hitos de la jornada:**
[Resumen técnico detallado de 2-3 párrafos]

**Decisiones y Cambios:**
- [Cambio 1]
- [Cambio 2]

**Scripts/Comandos relevantes:**
```bash
[Código clave]
```
---
```

**Si el archivo YA existe:**
1. Lee el bloque YAML existente.
2. Combina `temas` y `entorno` sin duplicar. Actualiza el YAML.
3. Añade el registro del nuevo día al final del archivo respetando la estructura anterior.

### 5. Actualización del Índice Temático Federado
- Abre `rsa/RSA-Metodologias/indice/indice_tematico.md`.
- Mantén ESTRICTAMENTE este formato:

```markdown
## Sesiones por Entorno
- **acelerografo**:
  - @Milton: 2026/01/archivo.md, 2026/02/archivo.md

## Sesiones por Tema
- **mqtt**:
  - @Milton: 2026/02/archivo.md
```

- **Regla de inserción:**
  - Si `@Milton:` ya existe bajo la categoría, añade la nueva ruta separada por coma.
  - Si la categoría no existe, créala.
  - Si `@Milton:` no existe bajo una categoría existente, agrégalo en nueva línea.
  - **NUNCA** modifiques la sección `## Contextos Técnicos`.
  - **NUNCA** modifiques la sección `## Decisiones de Arquitectura`.

### 6. Confirmación Final
Imprime en el chat un resumen con:
- Ruta de la bitácora actualizada.
- Temas y entorno indexados.
- Confirmación de actualización del índice.
