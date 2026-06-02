# RSA-Agent-Toolkit

Paquete mínimo y reutilizable de configuración para agentes IA en proyectos de la **Red Sísmica del Austro (RSA)**.

**Parte del exocortex RSA** — [Ver arquitectura completa](https://github.com/RedSismicaAustro/RSA-Metodologias)

---

## 📦 Contenido

```text
RSA-Agent-Toolkit/
├── .agents/
│   ├── rules/
│   │   ├── commits.md           # Regla: formato de commits (minúsculas)
│   │   ├── restriccion_sshfs.md # Regla: no ejecutar en montajes SSHFS
│   │   └── idioma.md            # Regla: idioma español
│   └── skills/
│       ├── volcado_bitacora.md  # Guardar sesión en bitácora personal
│       ├── consulta_historica.md# Consultar conocimiento pasado
│       ├── generar_contexto.md  # Generar documentación de contexto técnico
│       ├── extraer_adr.md       # Extraer Architecture Decision Records
│       └── sincronizar_toolkit.md # Copiar .agents/ y AGENTS.md a git/
├── plantillas/
│   ├── AGENTS.md                # Plantilla genérica para proyectos RSA
│   ├── AGENTS_WSL.md            # Variante para WSL
│   ├── contexto_script.md       # Plantilla para contextos técnicos
│   ├── sesion.md                # Plantilla para bitácoras de sesión
│   └── adr.md                   # Plantilla para ADRs
├── AGENTS.md                    # Guía del agente (se copia a git/ raíz)
├── INSTALL.md                   # Instrucciones de instalación
└── README.md                    # Este archivo
```

---

## 🚀 Instalación

### Opción A: Clonar y sincronizar con el agente (recomendado)

1. Clona este repo dentro de tu workspace bajo `git/rsa/`:
   ```bash
   cd ~/git/rsa
   git clone git@github.com-rsa:RedSismicaAustro/RSA-Agent-Toolkit.git
   ```

2. En tu próxima sesión con el agente IA, di:
   > *"sincroniza el toolkit"*

   El agente copiará automáticamente `.agents/` y `AGENTS.md` a `git/`.

### Opción B: Copia manual (inicial)

```bash
# Desde git/
cp -r rsa/RSA-Agent-Toolkit/.agents/ .agents/
cp rsa/RSA-Agent-Toolkit/AGENTS.md AGENTS.md
```

---

## 🔄 Actualización

Cuando hagas cambios al toolkit:

```bash
cd rsa/RSA-Agent-Toolkit
git add .
git commit -m "feat: descripción del cambio"
git push
```

Luego, en el IDE con el agente activo, di: **"sincroniza el toolkit"**.

---

## 🏗️ Arquitectura del Exocortex RSA

```
git/                              ← Workspace raíz del IDE
├── .agents/                      ← Copiado desde RSA-Agent-Toolkit/
├── AGENTS.md                     ← Copiado desde RSA-Agent-Toolkit/
├── rsa/
│   ├── RSA-Agent-Toolkit/        ← Este repo (fuente canónica)
│   ├── RSA-Metodologias/         ← Índice federado, ADRs, guías
│   └── RSA-Otros-Proyectos/
└── institucional/
    └── RSA-Bitacora-LLM-{Nombre}/  ← Bitácoras personales
```

---

## 📋 Repositorios del Exocortex

| Repo | Cuenta | Descripción |
|------|--------|-------------|
| `RSA-Agent-Toolkit` | RedSismicaAustro | Este repo — skills y reglas |
| `RSA-Metodologias` | RedSismicaAustro | Índice federado, ADRs, guías institucionales |
| `RSA-Bitacora-LLM-Milton` | Institucional | Bitácora personal de sesiones de Milton |
