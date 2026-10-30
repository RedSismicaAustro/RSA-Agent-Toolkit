# Instalación del RSA-Agent-Toolkit

Esta guía explica cómo integrar el toolkit en tu workspace para que el agente IA tenga las reglas y skills disponibles.

---

## Requisitos Previos

- Workspace raíz del IDE: `git/` (directorio padre de todos los repos)
- Repos clonados bajo `git/`:
  - `git/rsa/RSA-Agent-Toolkit/` (este repo)
  - `git/rsa/RSA-Metodologias/`
  - `git/rsa/RSA-PPP-Toolkit/`
  - `git/institucional/RSA-Bitacora-LLM-{TuNombre}/`
  - `git/rsa-proyectos/` (carpeta para sistemas y proyectos institucionales)
  - `git/rsa-ppp/` (carpeta para repositorios de pasantías)

---

## Instalación Inicial

Copia manualmente a `git/` raíz (solo la primera vez):

**Windows 11 (PowerShell):**
```powershell
# Desde git/
Copy-Item -Recurse -Force rsa\RSA-Agent-Toolkit\.agents .agents
Copy-Item -Force rsa\RSA-Agent-Toolkit\AGENTS.md AGENTS.md
```

**Linux / WSL (Bash):**
```bash
# Desde git/
cp -r rsa/RSA-Agent-Toolkit/.agents .agents
cp rsa/RSA-Agent-Toolkit/AGENTS.md AGENTS.md
```

---

## Actualización (con el agente)

Después de hacer `git pull` en `RSA-Agent-Toolkit`, simplemente di al agente:

> **"sincroniza el toolkit"**

El agente copiará automáticamente todos los archivos y paquetes de skills actualizados. Funciona igual en W11 y WSL.

---

## Estructura Resultante en `git/`

```text
git/
├── .agents/
│   ├── rules/
│   │   ├── commits.md
│   │   ├── restriccion_sshfs.md
│   │   └── idioma.md
│   └── skills/
│       ├── volcado_bitacora/SKILL.md
│       ├── consulta_historica/SKILL.md
│       ├── generar_contexto/SKILL.md
│       ├── extraer_adr/SKILL.md
│       ├── sincronizar_toolkit/SKILL.md
│       ├── crear_transicion_tecnica/SKILL.md
│       ├── planning_guide/SKILL.md
│       ├── execution_guide/SKILL.md
│       ├── diagnostico_tecnico/SKILL.md
│       └── despliegue_release/SKILL.md
└── AGENTS.md
```

---

## Prompts de Inicio Recomendados

Guarda estos prompts en tu `git/README.md` como referencia rápida:

**Inicio de sesión:**
```
Para esta sesión, nuestro directorio de trabajo exclusivo será [DIRECTORIO].
No modifiques ni busques archivos fuera de esta ruta a menos que te lo pida.
```

**Volcado de bitácora:**
```
ejecuta el volcado de bitácora
```

**Sincronizar toolkit:**
```
sincroniza el toolkit
```
