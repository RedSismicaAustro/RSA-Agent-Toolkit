# Skill: Ejecución de Plan de Implementación (Agent Executor)

**Descripción de Activación:** Ejecuta este flujo cuando el usuario indique: **"ejecuta el blueprint [nombre]"**, **"implementa el plan en docs/blueprints/"**, **"aplica el blueprint"**, o cualquier solicitud que requiera implementar un plan previamente generado por un agente planificador.

**Objetivo:** Leer un plan de implementación generado por un agente planificador e implementar cada fase verificando contra el estado real del código. El ejecutor respeta el plan como guía principal, pero tiene la responsabilidad de detectar y reportar problemas antes de que causen daño.

---

## Filosofía de Ejecución

> **El plan es la guía, pero el código real es la fuente de verdad.** Si el plan dice "modifica la función X en la línea 50" y la función X está en la línea 80, adapta la ejecución al estado real del archivo. Si el plan dice "elimina el componente Y" pero descubres que otros módulos dependen de Y y el plan no lo contempla, detente y reporta.

### Rol del Ejecutor
- **Sí:** Implementar lo que el plan describe, adaptar líneas/rutas a la realidad del código, reportar discrepancias.
- **No:** Tomar decisiones de arquitectura o diseño que el plan no contempla, agregar funcionalidad no solicitada, ignorar errores para avanzar.

---

## Reglas de Ejecución

1. **El plan es la guía principal.** No tomes decisiones de diseño propias. No "mejores" el plan con funcionalidad extra.
2. **Ejecuta las fases en el orden especificado.** No reordenes, no omitas fases.
3. **Valida antes de actuar.** Antes de modificar un archivo, léelo para confirmar que el estado actual es el que el plan asume. Si difiere, reporta la discrepancia.
4. **Si encuentras una discrepancia menor** (cambio de número de línea, variable renombrada, indentación diferente), adáptate al estado real del archivo y continúa. Documenta la adaptación.
5. **Si encuentras una discrepancia mayor** (un archivo que no existe, una dependencia faltante, un componente que el plan asume pero que funciona diferente a lo descrito), **DETENTE** y reporta al usuario. No intentes resolver conflictos de diseño por tu cuenta.
6. **Si un paso del plan es ambiguo** (no tiene código concreto, usa frases vagas como "implementa según convenga"), **DETENTE** y pide clarificación. No improvises.
7. **Preserva comentarios y docstrings existentes** en archivos que se modifican, a menos que el plan indique lo contrario.
8. **No elimines código existente** que no esté explícitamente marcado para eliminación en el plan.

---

## Pasos de Ejecución

### 1. Localización del Plan
- Si el usuario especifica un archivo concreto, léelo directamente.
- Si el usuario dice "ejecuta el último blueprint" o similar, lista los archivos en `docs/blueprints/` y selecciona el más reciente por fecha en el nombre del archivo.
- Si no hay blueprints o el directorio no existe, informa al usuario y detente.

### 2. Lectura y Comprensión del Plan
- Lee el plan completo **antes de ejecutar cualquier paso**.
- Identifica:
  - **Objetivo general:** ¿Qué se busca lograr?
  - **Número de fases y sus dependencias.**
  - **Archivos que se crearán o modificarán.**
  - **Prerequisitos** que deben verificarse antes de empezar.
- Reporta al usuario un resumen breve: *"Plan: [título], [N] fases. Procedo con la ejecución."*

### 3. Verificación de Prerequisitos
- Si el plan tiene una sección de prerequisitos o pre-condiciones, verifica **cada una**.
- Si algún prerequisito no se puede verificar porque requiere acceso a un servidor remoto (restricción SSHFS), reporta al usuario los comandos que debe ejecutar manualmente y espera confirmación.
- No procedas con la implementación hasta que los prerequisitos estén verificados.

### 4. Ejecución Fase por Fase

Para cada fase del plan:

1. **Anuncia** la fase que vas a ejecutar.
2. **Lee el estado actual** de todos los archivos que la fase afecta. Esto es obligatorio para detectar discrepancias temprano.
3. **Implementa cada acción** de la fase:
   - Si el plan incluye código o configuración exacta, úsala como referencia principal.
   - Si el plan describe una modificación pero el archivo real difiere en detalles menores (líneas, indentación), adapta el cambio al estado real del archivo.
   - Si el plan propone crear un archivo nuevo, verifica que la ruta y las dependencias (imports, variables de entorno) son coherentes con el proyecto.
4. **Ejecuta el checkpoint de comprobación** de la fase antes de avanzar a la siguiente.
   - Si el checkpoint requiere ejecutar comandos en un servidor remoto (restricción SSHFS), proporciona los comandos al usuario y espera el resultado.
   - Si el checkpoint falla, reporta el error con contexto y espera instrucciones.
5. **Avanza** a la siguiente fase solo cuando el checkpoint anterior pasa.

### 5. Reporte Final

Al completar todas las fases (o al detenerte por un error), presenta un resumen:

```markdown
## Reporte de Ejecución

**Plan:** [título]
**Fases ejecutadas:** [N/total]
**Archivos creados:** [lista]
**Archivos modificados:** [lista]
**Adaptaciones menores:** [lista de discrepancias menores resueltas, o "Ninguna"]
**Checkpoints pasados:** [N/total]
**Estado:** Completado / Completado con adaptaciones / Detenido en Fase N
```

---

## Manejo de Situaciones Especiales

### El plan tiene un error técnico evidente
Si detectas un error claro en el plan (una ruta imposible, sintaxis inválida en el código, un comando que no existe, un componente que se elimina pero tiene dependientes), **reporta el error específico y propón la corrección**, pero espera confirmación del usuario antes de aplicarla. No apliques cambios que contradigan el diseño del plan sin aprobación.

### El plan está desactualizado
Si el código ha cambiado desde que se generó el plan (funciones renombradas, archivos movidos, estructura diferente), trata las discrepancias como oportunidades para adaptar:
- **Cambios menores** (líneas, nombres): Adapta y documenta.
- **Cambios mayores** (archivos eliminados, arquitectura diferente): Detente y reporta.

### El usuario pide cambios durante la ejecución
Si el usuario solicita una desviación del plan:
1. Ejecuta el cambio solicitado (el usuario tiene prioridad sobre el plan).
2. Documenta la desviación en el reporte final.
3. Continúa con la siguiente fase del plan.

### El plan tiene fases que se ejecutan en servidores remotos
Si el directorio de trabajo está bajo la restricción SSHFS (`montajes/**`), recuerda que no puedes ejecutar comandos de terminal de forma autónoma. Proporciona los comandos al usuario en bloques de código Bash y espera la salida antes de continuar.
