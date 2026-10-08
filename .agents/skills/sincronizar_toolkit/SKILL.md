---
name: sincronizar_toolkit
description: "Sincroniza .agents/ y AGENTS.md desde RSA-Agent-Toolkit hacia la raiz git/ del workspace."
---
# Skill: Sincronizar Toolkit al Workspace Raíz

**Descripción de Activación:** Ejecuta este flujo cuando el usuario indique: **"sincroniza el toolkit"**, **"actualiza las reglas del agente"** o **"actualiza el AGENTS.md"**.

**Objetivo:** Mantener actualizados los archivos `.agents/` y `AGENTS.md` en el directorio raíz del workspace (`git/`) sin necesidad de scripts ni symlinks, copiando desde `RSA-Agent-Toolkit/` como fuente canónica.

**Compatibilidad:** Este skill funciona en Windows 11 y WSL/Linux ya que usa rutas relativas y las operaciones de escritura de archivos del agente.

---

## Variables del Skill

- **Fuente:** `rsa/RSA-Agent-Toolkit/`
- **Destino raíz:** `git/` (el workspace raíz del IDE)

---

## Pasos de Ejecución

### 1. Verificación de Fuente
- Confirma que `rsa/RSA-Agent-Toolkit/.agents/` existe y tiene contenido.
- Confirma que `rsa/RSA-Agent-Toolkit/AGENTS.md` existe.
- Si alguno no existe, detente y avisa al usuario.

### 2. Copia de Reglas
- Lee cada archivo en `rsa/RSA-Agent-Toolkit/.agents/rules/`.
- Sobrescribe el archivo correspondiente en `git/.agents/rules/`.
- Archivos a copiar: `commits.md`, `restriccion_sshfs.md`, `idioma.md`.

### 3. Copia de Skills
- Copia recursivamente cada subdirectorio de skill en `rsa/RSA-Agent-Toolkit/.agents/skills/` hacia `git/.agents/skills/`.
- Cada paquete de skill contiene su archivo `SKILL.md` (con frontmatter YAML) y cualquier recurso adicional (`scripts/`, `templates/`, etc.).
- Skills a copiar: `volcado_bitacora/`, `consulta_historica/`, `generar_contexto/`, `extraer_adr/`, `sincronizar_toolkit/`, `crear_transicion_tecnica/`, `planning_guide/`, `execution_guide/`, `diagnostico_tecnico/`, `despliegue_release/`.

### 4. Copia de AGENTS.md
- Lee `rsa/RSA-Agent-Toolkit/AGENTS.md`.
- Sobrescribe `git/AGENTS.md` con ese contenido.

### 5. Confirmación Final
Imprime en el chat:
- Lista de archivos actualizados en `git/.agents/rules/`.
- Lista de archivos actualizados en `git/.agents/skills/`.
- Confirmación de actualización de `git/AGENTS.md`.
- Mensaje: "El toolkit está sincronizado. Los cambios tendrán efecto en la próxima conversación."
