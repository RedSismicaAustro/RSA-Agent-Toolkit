# Guía para Agentes de IA — [NOMBRE DEL PROYECTO]

Este archivo configura el comportamiento del agente IA para el proyecto **[NOMBRE DEL PROYECTO]** de la Red Sísmica del Austro (RSA).

---

## 🚀 Inicio de Conversación

Al inicio de cada sesión, el usuario indicará el directorio de trabajo:
> *"Para esta sesión, nuestro directorio de trabajo exclusivo será [DIRECTORIO]."*

No modifiques ni busques archivos fuera de esa ruta a menos que el usuario te lo pida.

---

## ⚙️ Skills Disponibles

Usa los skills definidos en `.agents/skills/` del workspace raíz:

- **`volcado_bitacora`**: "ejecuta el volcado de bitácora"
- **`consulta_historica`**: actívado por preguntas sobre el pasado del proyecto
- **`generar_contexto`**: "genera el contexto de [archivo]"
- **`extraer_adr`**: "extrae un ADR sobre [decisión]"

---

## 🛡️ Reglas Críticas

1. **SSHFS**: Prohibido ejecutar comandos en `montajes/**`. Delega al usuario.
2. **Commits**: No ejecutes commits. Muestra el texto en formato `tipo: descripción` (minúsculas).
3. **Idioma**: Toda la documentación en español.

---

## 📂 Estructura del Proyecto

```text
[NOMBRE DEL PROYECTO]/
├── docs/
│   └── context/   ← Contextos técnicos generados por el agente
├── ...
└── README.md
```
