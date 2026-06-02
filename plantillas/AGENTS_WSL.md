# Guía para Agentes de IA — Workspace WSL

Este archivo configura el comportamiento del agente IA para sesiones en entorno WSL/Linux de la Red Sísmica del Austro (RSA).

---

## 🚀 Inicio de Conversación

Al inicio de cada sesión, el usuario indicará el directorio de trabajo:
> *"Para esta sesión, nuestro directorio de trabajo exclusivo será [DIRECTORIO]."*

No modifiques ni busques archivos fuera de esa ruta a menos que el usuario te lo pida.

**Nota de entorno**: En WSL, las rutas del workspace tienen la forma `/home/{usuario}/git/`. El workspace raíz es equivalente a `C:\Users\{usuario}\Documents\git\` en Windows 11.

---

## ⚙️ Skills Disponibles

Los skills están en `.agents/skills/` del workspace raíz (`git/`):

- **`volcado_bitacora`**: "ejecuta el volcado de bitácora"
- **`consulta_historica`**: actívado por preguntas sobre el pasado del proyecto
- **`generar_contexto`**: "genera el contexto de [archivo]"
- **`extraer_adr`**: "extrae un ADR sobre [decisión]"
- **`sincronizar_toolkit`**: "sincroniza el toolkit"

---

## 🛡️ Reglas Críticas

1. **SSHFS**: Prohibido ejecutar comandos en `montajes/**`. Delega al usuario.
2. **Commits**: No ejecutes commits. Muestra el texto en formato `tipo: descripción` (minúsculas).
3. **Idioma**: Toda la documentación en español.

---

## 🔧 Notas WSL Específicas

- Los repos de GitHub están disponibles bajo `/home/{usuario}/git/` (mismo workspace que W11).
- Los montajes SSHFS de equipos remotos están bajo `montajes/` dentro del workspace.
- Para ejecutar comandos en hardware remoto (Raspberry Pi), el agente debe delegar al usuario y proporcionar el comando exacto.
