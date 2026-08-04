# Guía para Agentes de IA

Este archivo configura el comportamiento del agente IA para proyectos de la **Red Sísmica del Austro (RSA)**.

Como agente de IA, lee este archivo para entender cómo interactuar con los repositorios, cómo catalogar la información y qué reglas de seguridad debes seguir estrictamente.

---

## 🚀 Inicio de Conversación

Al inicio de cada sesión, el usuario indicará el directorio de trabajo con una instrucción como:
> *"Para esta sesión, nuestro directorio de trabajo exclusivo será [DIRECTORIO]."*

Respeta esa restricción: no modifiques ni busques archivos fuera de esa ruta a menos que el usuario te lo pida explícitamente.

---

## 📂 Estructura del Exocortex RSA

El conocimiento está distribuido en tres repositorios:

| Repositorio | Cuenta | Contenido |
|-------------|--------|-----------|
| `RSA-Agent-Toolkit` | rsa/ | Skills, reglas y plantillas del agente |
| `RSA-Metodologias` | rsa/ | Índice federado, ADRs y guías institucionales |
| `RSA-Bitacora-LLM-{Nombre}` | institucional/ | Bitácoras personales de sesiones |

### Rutas clave en el workspace `git/`

- **Índice maestro**: `rsa/RSA-Metodologias/indice/indice_tematico.md`
- **Catálogo de contribuidores**: `rsa/RSA-Metodologias/indice/catalogo_contribuidores.md`
- **Bitácora de Milton**: `institucional/RSA-Bitacora-LLM-Milton/sesiones/`
- **ADRs**: `rsa/RSA-Metodologias/decisiones/`

---

## ⚙️ Habilidades y Flujos de Trabajo (Skills)

Los skills están en `.agents/skills/`. Se activan con comandos específicos del usuario:

| Skill | Activación | Descripción |
|-------|-----------|-------------|
| `volcado_bitacora` | "ejecuta el volcado de bitácora" o "actualiza la bitácora" | Guarda el registro de la sesión actual en la bitácora personal |
| `consulta_historica` | Preguntas sobre el pasado de proyectos | Enruta la consulta a través del índice federado |
| `generar_contexto` | "genera el contexto de [archivo]" | Genera documentación de contexto técnico en el proyecto |
| `extraer_adr` | "extrae un ADR sobre [decisión]" | Documenta una decisión de arquitectura |
| `sincronizar_toolkit` | "sincroniza el toolkit" | Copia `.agents/` y `AGENTS.md` a `git/` raíz |
| `crear_transicion_tecnica` | "crea un archivo de transición técnica" o "genera la transición técnica" | Genera el documento de transición semántica en docs/progress/ |
| `planning_guide` | "planifica [tarea]" o "crea un blueprint para [objetivo]" | Explora el codebase y genera un blueprint estructurado en docs/blueprints/ |
| `execution_guide` | "ejecuta el blueprint [nombre]" o "implementa el plan" | Ejecuta un blueprint al pie de la letra sin improvisar ni desviarse del plan |

---

## 🛡️ Reglas de Comportamiento Críticas

Las reglas detalladas están en `.agents/rules/`. Resumen:

1. **Restricción SSHFS** (`restriccion_sshfs.md`): No ejecutes comandos autónomos en rutas bajo `montajes/**`. Delega al usuario.
2. **Formato de Commits** (`commits.md`): No ejecutes commits. Muestra el texto en formato `tipo: descripción` (minúsculas).
3. **Idioma** (`idioma.md`): Todas las interacciones y documentación en español.

---

## 🔧 Sincronización del Toolkit

Este archivo (`AGENTS.md`) y el directorio `.agents/` en `git/` son copias de `rsa/RSA-Agent-Toolkit/`. Para actualizarlos tras un pull del toolkit, di: **"sincroniza el toolkit"**.
