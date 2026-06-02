---
trigger: always
---

# Regla: Formato de Mensajes de Commit

Esta regla aplica en todas las interacciones dentro de este workspace.

## Restricción de Ejecución

Cada vez que se solicite generar un commit, **no debes tratar de ejecutarlo directamente en la terminal**. Limítate a mostrar el texto del commit.

## Formato Obligatorio

El texto del commit generado debe seguir ESTRICTAMENTE estas reglas:

1. **Idioma:** Todo el texto debe estar en español.
2. **Estructura:** Usa la convención `tipo: descripción corta` (minúsculas).
3. **Tipos válidos:** `feat`, `fix`, `docs`, `style`, `refactor`, `chore`, `test`, `perf`, `build`, `ci`, `security`, `hotfix`, `wip`.
4. **Detalles:** A continuación del guión, lista los cambios específicos con viñetas (`-`).
5. **Sub-viñetas:** Si un cambio involucra múltiples elementos dentro del mismo archivo o componente, usa viñetas anidadas con dos espacios de sangría (`  -`).

## Ejemplo de Formato

```
feat: mejora de métricas de salud y refactorización de heartbeat
- Se implementaron métricas detalladas de hardware en mqtt_coordinator.py:
  - Porcentaje de espacio en disco (reemplaza GB).
  - RAM disponible en MB.
  - Load Average de 15 minutos.
  - Temperatura del CPU usando 'vcgencmd'.
- Se eliminó la lógica de publicación de heartbeat del script coordinador.
```
