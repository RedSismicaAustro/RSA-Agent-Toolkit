---
trigger: glob
globs: `montajes/**`
---

# Regla: Restricción de Comandos en Rutas SSHFS

**Contexto del Workspace:**
El entorno de trabajo puede contener el subdirectorio `montajes/` con sistemas de archivos de equipos remotos (hardware limitado como Raspberry Pi) montados localmente mediante `sshfs`.

**Limitación de Entorno:**
Al operar sobre el directorio `montajes/`, el Agente NO tiene una sesión SSH activa ni acceso de ejecución sobre el hardware remoto subyacente. Intentar ejecutar comandos directamente en la terminal local producirá errores o afectará a la máquina anfitriona incorrecta.

**Reglas de Comportamiento (Cumplimiento Obligatorio):**

1. **PROHIBICIÓN DE EJECUCIÓN AUTÓNOMA:** Si el contexto de tu tarea involucra la ruta `montajes/`, tienes estrictamente prohibido ejecutar comandos de forma autónoma (ej. ejecutar scripts, compilar código, reiniciar servicios o revisar logs).
2. **DELEGACIÓN OBLIGATORIA:** Cada vez que determines que es necesario ejecutar un comando en el equipo remoto, DEBES detenerte y solicitar al usuario que lo ejecute manualmente.
3. **FORMATO DE ENTREGA:** Proporciona el comando exacto en un bloque de código Bash.

**Ejemplo de respuesta esperada:**

"Para verificar el estado del servicio, ejecuta este comando manualmente en la terminal del equipo remoto:"
```bash
sudo systemctl status rsa-acelerografo.service
```
"Esperaré la salida de este comando para continuar con el análisis."
