# Skill: Extraer Architecture Decision Record (ADR)

**Descripción de Activación:** Ejecuta este flujo cuando el usuario indique: **"extrae un ADR de [tema/sesión]"**, **"crea un ADR sobre [decisión]"**, o cuando el usuario solicite documentar una decisión de arquitectura importante.

**Objetivo:** Capturar decisiones técnicas importantes como documentos independientes y reutilizables, evitando que queden sepultadas en las bitácoras de sesión.

---

## Variables del Skill

- **Directorio ADRs:** `rsa/RSA-Metodologias/decisiones/`
- **Índice maestro:** `rsa/RSA-Metodologias/indice/indice_tematico.md`

---

## Pasos de Ejecución

### 1. Identificación de la Decisión
- Si el usuario especificó una sesión de origen, léela para extraer el contexto.
- Identifica:
  - **Problema/contexto:** ¿Qué situación motivó la decisión?
  - **Opciones evaluadas:** ¿Qué alternativas se consideraron?
  - **Decisión tomada:** ¿Qué se eligió y por qué?
  - **Consecuencias:** ¿Qué implica esta decisión a futuro?

### 2. Asignación de Número Secuencial
- Lista los archivos existentes en `rsa/RSA-Metodologias/decisiones/`.
- El siguiente número es el mayor existente + 1 (formato `NNN` con ceros a la izquierda, ej: `001`, `002`).
- Si no hay ADRs previos, empieza en `001`.

### 3. Generación del Título
- Crea un título descriptivo en español (snake_case): `protocolo_mqtt_acelerografo`, `kiosk_grafana_wayland`.
- El nombre del archivo será: `NNN_titulo.md`.

### 4. Escritura del ADR

Usa la siguiente estructura:

```markdown
---
id: ADR-NNN
titulo: [Título descriptivo]
estado: Aceptado
fecha: YYYY-MM-DD
temas: [tema1, tema2]
entorno: [proyecto/sistema afectado]
---

# ADR-NNN: [Título]

## Estado

**Aceptado** | Fecha: YYYY-MM-DD

## Contexto

[Descripción del problema o situación que motivó esta decisión. Incluir restricciones técnicas, de negocio o de equipo relevantes.]

## Opciones Evaluadas

### Opción A: [Nombre]
- **Ventajas:** ...
- **Desventajas:** ...

### Opción B: [Nombre]
- **Ventajas:** ...
- **Desventajas:** ...

## Decisión

Se eligió la **Opción [X]** porque [justificación técnica concreta].

## Consecuencias

- [Consecuencia positiva 1]
- [Consecuencia negativa o deuda técnica 1]
- [Trabajo futuro derivado]

## Referencias

- Sesión de origen: [ruta relativa a la sesión donde se tomó la decisión, si aplica]
- Contexto técnico relacionado: [ruta al contexto técnico, si aplica]
```

### 5. Actualización del Índice
- Abre `rsa/RSA-Metodologias/indice/indice_tematico.md`.
- Añade la entrada bajo `## Decisiones de Arquitectura`:
  ```markdown
  - ADR-NNN: decisiones/NNN_titulo.md (tema1, tema2) — [descripción de una línea]
  ```

### 6. Confirmación al Usuario
- Reporta la ruta del ADR generado y su número asignado.
- Confirma la actualización del índice.
