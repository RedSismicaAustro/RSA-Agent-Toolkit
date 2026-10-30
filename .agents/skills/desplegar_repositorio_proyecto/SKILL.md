---
name: desplegar_repositorio_proyecto
description: "Guía interactiva para desplegar, inicializar y estandarizar repositorios de producción e instrumentación bajo RSA-Proyectos (desde cero o graduando desde un PPP/legacy)."
---

# Skill: Desplegar Repositorio de Proyecto (RSA-Proyectos)

Esta habilidad automatiza y estandariza la creación e inicialización de un nuevo repositorio de sistemas operativos, instrumentación o software institucional bajo la organización [`RSA-Proyectos`](https://github.com/RSA-Proyectos).

Soporta dos escenarios operacionales:
1. **Escenario 1 (Greenfield / Desde Cero):** Creación y andamiaje de un nuevo sistema institucional a partir de una formulación técnica.
2. **Escenario 2 (Brownfield / Graduación de PPP o Migración Legacy):** Transición de una prueba de concepto previa (ej. repositorios de pasantías en `rsa-ppp/` o proyectos antiguos desorganizados `RSA-Intern-*`), realizando triaje de código limpio, descartando basura técnica, preservando la trazabilidad del autor original y generando contextos iniciales.

---

## ⚙️ Configuración y Variables de Entorno

Antes de operar, define o verifica la ruta raíz de trabajo local para proyectos de producción:

```text
RUTA_BASE_PROYECTOS = "C:\Users\miltonrsa\Documents\git\rsa-proyectos"
ORGANIZACION_GITHUB = "RSA-Proyectos"
HOST_SSH_RSA = "git@github.com-rsa"
FORMATO_NOMBRE = "RSA-Palabra-Palabra"  # Kebab-Case con iniciales en mayúscula
```

> 💡 **Ruta de Plantillas:** Las plantillas institucionales de andamiaje se ubican en la subcarpeta local de esta skill:  
> `rsa/RSA-Agent-Toolkit/.agents/skills/desplegar_repositorio_proyecto/templates/`

---

## 🎯 Disparadores (Triggers)

La habilidad se activa con expresiones como:
- *"despliega el repositorio para el proyecto [Nombre]"*
- *"inicializa un nuevo proyecto institucional [Nombre]"*
- *"crea el repositorio de producción para [tema]"*
- *"gradúa el repositorio [repo-ppp] a RSA-Proyectos"*
- *"migra el repositorio [repo-legacy] a RSA-Proyectos"*

---

## 📋 Flujo de Ejecución Paso a Paso

El flujo consta de **5 Fases Interactivas** con checkpoints:

```text
+-----------------------+     +-----------------------+     +-----------------------+
|        FASE 1         |     |        FASE 2         |     |        FASE 3         |
|  Diagnóstico Escenario| --> |  Creación en GitHub   | --> | Configuración About   |
|   y Ficha Técnica     |     |   y Clonación Local   |     |    y Topics en Web    |
+-----------------------+     +-----------------------+     +-----------------------+
                                                                        |
                                                                        v
+-----------------------+                                   +-----------------------+
|        FASE 5         |                                   |        FASE 4         |
|  Comandos de Commit   | <-------------------------------- | Generación Andamiaje  |
|  y Push a origin/main |                                   |  Local y Triaje/Docs  |
+-----------------------+                                   +-----------------------+
```

---

### 🔹 Fase 1: Diagnóstico de Escenario y Formulación Técnica

El agente consulta al usuario para identificar el escenario correspondiente:

#### A. Si es Escenario 1 (Greenfield / Desde Cero):
1. **Entrevista / Formulación Breve:**
   - Propósito central del sistema o instrumento.
   - Stack de tecnologías (ej. Python, C/C++, MicroPython, KiCad).
   - Plataforma o hardware objetivo (ej. Raspberry Pi 4, dsPIC33, ESP32, Servidor Ubuntu).
   - Estructura preferida de código fuente (`firmware/`, `scripts/` o `src/`).
2. **Definición del Nombre Canónico:**
   - Aplicar estrictamente el formato `RSA-Palabra-Palabra` (ej. `RSA-Multiplexor-Galgas`, `RSA-Sensor-Ultrasonico`, `RSA-Daq-Presas`).

#### B. Si es Escenario 2 (Brownfield / Graduación de PPP o Migración Legacy):
1. **Identificación del Repositorio Origen:**
   - Ruta local del repo base (ej. `rsa-ppp/ppp-2026-11-shm-nodos-sensores` o `rsa/RSA-Intern-SHM`).
2. **Auditoría y Triaje Técnico del Código:**
   - El agente inspecciona el repositorio origen y presenta una tabla de triaje:
     - **Activos a Migrar (Código limpio y documentación de valor):** Firmwares funcionales, drivers desacoplados, scripts de análisis, esquemas y diagramas.
     - **Deuda Técnica a Descartar (Basura / Temporales):** Archivos intermedios (`*.dci`, `*.lst`, `*.hex` viejos, `__pycache__`), carpetas de pruebas fallidas, scripts descartados.
3. **Mapeo de Destino:**
   - Definir cómo se reubicarán los módulos limpios en la estructura canónica institucional (`configuration/`, `src/` o `firmware/`, `data/`).
4. **Registro de Trazabilidad:**
   - Extraer datos del proyecto previo: código institucional (ej. `RSA-PPP-2026-11`), autor original y URL del repositorio precedente para los créditos en `README.md` y `CHANGELOG.md`.

---

### 🔹 Fase 2: Creación del Repositorio en GitHub y Clonación

1. **Instrucciones para GitHub Web:**
   Presentar una ficha clara para la creación manual en la organización:
   - **Enlace directo:** `https://github.com/organizations/RSA-Proyectos/repositories/new`
   - **Repository name:** `[Nombre-Canónico]` (ej. `RSA-Multiplexor-Galgas`)
   - **Description:** `[Descripción institucional en 1 línea]`
   - **Visibilidad:** Público (o Interno según determine el usuario).
   - ⚠️ **Importante:** **NO marcar** "Add a README file", **NO marcar** ".gitignore", **NO marcar** "Choose a license". El repositorio debe nacer 100% vacío.

2. **Comando de Clonación PowerShell:**
   Entregar el comando exacto usando el alias SSH institucional y la ruta dedicada:
   ```powershell
   git clone git@github.com-rsa:RSA-Proyectos/[Nombre-Canónico].git C:\Users\miltonrsa\Documents\git\rsa-proyectos\[Nombre-Canónico]
   ```

3. 🛑 **Checkpoint 1 (Esperar confirmación):**
   El agente se detiene y espera a que el usuario confirme la clonación local:
   > *"Por favor crea el repositorio en GitHub y ejecuta la clonación. Cuando esté listo, avísame para continuar con la configuración del About y la generación de archivos."*

---

### 🔹 Fase 3: Configuración del About y Topics en GitHub

Una vez confirmado el clon local, el agente entrega los metadatos para la sección **About** (engranaje ⚙️ en GitHub Web):

1. **Description:** Síntesis institucional formal.
2. **Website:** `https://redsismicaaustro.github.io/RSA-Metodologias`
3. **Topics:** Lista canónica obligatoria bajo el vocabulario de `docs/taxonomia_topics.md`:
   - Obligatorios: `rsa-proyectos`, `ucuenca`, `red-sismica-austro`.
   - Dominio: (ej. `shm`, `seismic-monitoring`, `dam-monitoring`, `daq`, `iot`).
   - Hardware: (ej. `dspic33`, `esp32`, `raspberry-pi`, `ni-usb-6210`).
   - Protocolo: (ej. `rs485`, `daisy-chain`, `spi`, `mqtt`, `uart`).
   - Software: (ej. `python`, `c`, `micropython`, `kicad`, `grafana`).

---

### 🔹 Fase 4: Generación Autónoma del Andamiaje Local

En `C:\Users\miltonrsa\Documents\git\rsa-proyectos\[Nombre-Canónico]`, el agente realiza las siguientes acciones:

1. **Creación del Árbol de Carpetas Canónico con `.gitkeep`:**
   ```text
   [Nombre-Canónico]/
   ├── configuration/
   ├── docs/
   │   ├── adr/
   │   ├── analysis/
   │   ├── blueprints/
   │   ├── context/
   │   └── progress/
   ├── [firmware/ | scripts/ | src/]/
   │   ├── common/
   │   ├── drivers/
   │   └── tests/
   └── data/
       ├── evidence/
       └── raw_samples/
   ```

2. **Renderizado de Archivos Base desde `templates/`:**
   - **`AGENTS.md`:** A partir de `templates/AGENTS.md.template`. Incorpora la **Jerarquía de la Verdad**, las reglas operativas y el mapa semántico.
   - **`README.md`:** A partir de `templates/README.md.template`. Inyecta ficha técnica, diagrama Mermaid de arquitectura, estructura canónica y, si aplica, el bloque de **Trazabilidad de Proyecto Precedente**.
   - **`CHANGELOG.md`:** A partir de `templates/CHANGELOG.md.template`. Inicializa la versión `0.1.0` (o `1.0.0` si es un sistema ya consolidado).
   - **`.gitignore`:** A partir de `templates/gitignore.template`.

3. **Acciones Adicionales en Escenario 2 (Migración / Graduación):**
   - Copiar selectivamente los archivos del triaje desde el repositorio fuente a sus carpetas de destino limpio.
   - Generar el primer archivo de contexto técnico en `docs/context/` utilizando la skill `generar_contexto` sobre el módulo principal migrado.
   - Enlazar explícitamente en el `README.md` el crédito al autor original y el repositorio histórico previo.

---

### 🔹 Fase 5: Verificación y Primer Commit

1. El agente ejecuta `git status` en el nuevo repositorio para verificar que no existan archivos residuales ni temporales no deseados.
2. Proporciona al usuario los comandos exactos de Git para el despliegue del andamiaje (respetando la regla de commits en minúsculas `commits.md`):

```powershell
cd C:\Users\miltonrsa\Documents\git\rsa-proyectos\[Nombre-Canónico]
git add .
git commit -m "feat: inicializar andamiaje institucional de produccion y documentacion base"
git push origin main
```

3. Confirmar que el repositorio en `RSA-Proyectos` se encuentra **100% operativo, documentado y listo para desarrollo continuo**.
