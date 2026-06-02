# Skill: Consulta Histórica de Bitácoras

**Descripción de Activación:** Ejecuta este flujo cuando el usuario realice preguntas sobre el estado pasado de un proyecto, resúmenes de períodos anteriores, o busque información sobre temas específicos (ej. "¿qué hicimos la semana pasada?", "busca información sobre mqtt").

**Objetivo:** Minimizar el consumo de tokens cargando únicamente los archivos estrictamente necesarios.

---

## Variables del Skill

- **Índice maestro:** `rsa/RSA-Metodologias/indice/indice_tematico.md`
- **Catálogo de contribuidores:** `rsa/RSA-Metodologias/indice/catalogo_contribuidores.md`
- **Prefijo repos de bitácora:** (definido en el catálogo)

## Resolución de Rutas de Sesiones

El índice usa el formato `@Usuario: YYYY/MM/archivo.md`. Para resolver esta ruta:
1. Lee el `catalogo_contribuidores.md` para obtener el repositorio y cuenta de ese usuario.
2. Construye la ruta completa: `{cuenta}/{repo}/sesiones/{ruta_relativa}`.
   - Ejemplo: `@Milton: 2026/05/archivo.md` → `institucional/RSA-Bitacora-LLM-Milton/sesiones/2026/05/archivo.md`

---

## Pasos de Ejecución

### 1. Consulta del Índice (Fase de Enrutamiento)
- Tienes prohibido leer archivos dentro de los repos de bitácora directamente.
- Tu primera acción OBLIGATORIA es leer `rsa/RSA-Metodologias/indice/indice_tematico.md`.

### 2. Selección de Archivos
- Cruza la consulta con las tres secciones del índice:
  - `## Contextos Técnicos`: para preguntas sobre cómo funciona un script.
  - `## Sesiones por Entorno` / `## Sesiones por Tema`: para preguntas históricas.
  - `## Decisiones de Arquitectura`: para preguntas sobre el porqué de una decisión técnica.
- Si la consulta es sobre funcionalidad de código, prioriza el **Contexto Técnico** y luego las sesiones más recientes.
- Si la consulta es sobre historial de cambios o fechas, prioriza las **Sesiones**.

### 3. Carga de Archivos (Fase de Lectura)
- Resuelve las rutas usando el catálogo de contribuidores.
- Para consultas técnicas de código: lee primero el Contexto Técnico, luego las sesiones (máximo 3-5 archivos).
- Para consultas históricas: lee las sesiones identificadas (máximo 3-5 archivos).
- Para ADRs: lee el archivo correspondiente en `rsa/RSA-Metodologias/decisiones/`.

### 4. Síntesis y Respuesta
- Integra la información del contexto técnico + decisiones + sesiones.
- Responde de forma directa y técnica en español.
- Incluye al final las referencias explícitas a los archivos leídos.
  Ejemplo: *Fuentes consultadas: rsa/RSA-Metodologias/indice/indice_tematico.md, institucional/RSA-Bitacora-LLM-Milton/sesiones/2026/05/...*
