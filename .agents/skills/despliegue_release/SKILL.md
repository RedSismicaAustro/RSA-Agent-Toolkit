---
name: despliegue_release
description: "Guia interactiva para versionado SemVer, changelog y comandos de despliegue a DEV y produccion."
---
# Skill: Asistente Interactivo de Release y Despliegue RSA

**Descripción de Activación:** Ejecuta este flujo cuando el usuario indique: **"prepara el release"**, **"orquesta el despliegue"**, **"despliega a producción"**, **"inicia el protocolo de despliegue"**, **"asiste en el release de la versión"** o frases con intención equivalente.

**Objetivo:** Guiar de manera interactiva, asistida y libre de errores humanos todo el ciclo de vida de un release de software/firmware en la Red Sísmica del Austro: desde la determinación semántica de versión (SemVer) y redacción del registro de cambios (`CHANGELOG.md`), hasta la sincronización selectiva hacia `main` (aislando la documentación de desarrollo) y la entrega de comandos textuales listos para copiar y pegar en los equipos remotos vía SSH, cumpliendo estrictamente con la restricción SSHFS y utilizando `logs.tmp` como canal de comunicación.

---

## 🛡️ Reglas Críticas de Cumplimiento Obligatorio

1. **Prohibición Estricta de Ejecución Autónoma en Remoto (`restriccion_sshfs.md`)**:
   - Tienes terminantemente prohibido intentar ejecutar comandos directamente en la terminal sobre rutas montadas (`montajes/**`) o sobre hardware remoto.
   - Todos los comandos dirigidos a equipos de desarrollo o estaciones en campo deben proporcionarse en bloques de código Bash explícitos para que el usuario los copie y pegue manualmente.
2. **Canal de Entrada Exclusivo vía `logs.tmp`**:
   - Para conocer el estado del repositorio, los commits pendientes, los tags existentes o los resultados de pruebas post-despliegue, **solicita al usuario que pegue la salida en `logs.tmp`** y lee dicho archivo.
3. **Formato Institucional de Commits (`commits.md`)**:
   - Todo commit sugerido debe seguir estrictamente la estructura `tipo: descripción corta` en minúsculas, viñetas (`-`) y detalles en español con tipos permitidos (`chore`, `fix`, `feat`, `docs`).
4. **Aislamiento de Documentación en Producción (`docs/`)**:
   - La rama de producción `main` y los tags oficiales **nunca** deben incluir el directorio de desarrollo `docs/` para proteger la memoria flash limitada de las estaciones de campo.

---

## 📋 Pasos de Ejecución

### Paso 1: Solicitud e Ingesta de Información vía `logs.tmp`

1. Si `logs.tmp` no contiene la información requerida, solicita al usuario que ejecute los siguientes comandos en el equipo de desarrollo (`DEV00`) y pegue la salida completa en `logs.tmp`:
   ```bash
   # En el equipo de desarrollo (DEV00):
   git describe --tags --abbrev=0
   git log --oneline $(git describe --tags --abbrev=0)..develop
   git status -s
   ```
2. Lee `logs.tmp` e identifica:
   - **Último tag oficial**: Versión previa (ej. `v4.5.1`).
   - **Commits acumulados en `develop`**: Lista de cambios desde el último tag.
   - **Estado del árbol**: Confirmar que no haya conflictos o cambios sin commitear.

---

### Paso 2: Determinación SemVer y Redacción del Changelog

1. Analiza los cambios identificados contra las reglas de [Semantic Versioning (SemVer 2.0.0)](https://semver.org/lang/es/) y los criterios de la RSA ([protocolo-despliegue-release.md](file:///home/rsa/git/rsa/RSA-Metodologias/manuales/git/protocolo-despliegue-release.md)):
   - **`PATCH` (`vX.Y.Z+1`)**: Correcciones de bugs, mejoras de robustez, blindaje de excepciones, ajustes de configuración sin romper compatibilidad.
   - **`MINOR` (`vX.Y+1.0`)**: Nuevos módulos, daemons en Supervisor, servicios systemd o features compatibles hacia atrás.
   - **`MAJOR` (`vX+1.0.0`)**: Modificación del protocolo binario SPI dsPIC-RPi o reestructuración profunda del pipeline.
2. Determina y justifica la nueva versión (ej. `v4.5.2`).
3. Redacta el bloque exacto para `CHANGELOG.md` siguiendo el estándar [Keep a Changelog](https://keepachangelog.com/es-ES/1.0.0/) con fecha del día (`YYYY-MM-DD`):

```markdown
## [vX.Y.Z] — YYYY-MM-DD

### Añadido
- [Funcionalidades o scripts nuevos]

### Modificado
- [Cambios de comportamiento o refactorizaciones]

### Corregido
- [Bugs erradicados o blindaje de fallos]
```

4. Indica al usuario que inserte dicho bloque en `CHANGELOG.md` de la rama `develop`.

---

### Paso 3: Entrega de Comandos para el Equipo de Desarrollo (`DEV00`)

Proporciona al usuario el bloque exacto de comandos Git para que lo copie y pegue en la terminal de `DEV00`:

```bash
# 1. Asegurar rama develop y registrar el changelog
git checkout develop
git pull origin develop
git add CHANGELOG.md
git commit -m "chore: preparar release vX.Y.Z
- Se actualizó CHANGELOG.md con los cambios de la versión vX.Y.Z.
- [Resumen de los cambios principales en viñetas]"
git push origin develop

# 2. Sincronización selectiva hacia main (excluyendo deliberadamente docs/)
git checkout main
git pull origin main
git checkout develop -- configuration/ main-libraries/ menu.sh models/ README.md requirements.txt scripts/ CHANGELOG.md .gitignore
git rm -r docs/ 2>/dev/null || true
git commit -m "chore: despliegue de componentes para release vX.Y.Z en main
- Sincronización operativa de la versión vX.Y.Z desde develop.
- Exclusión intencional de docs/ para optimizar almacenamiento en campo."
git push origin main

# 3. Creación y publicación del Git Tag oficial anotado
git tag -a vX.Y.Z -m "Release vX.Y.Z: [Descripción concisa del objetivo del release]"
git push origin vX.Y.Z
```

---

### Paso 4: Entrega de Comandos para Estaciones Remotas (SSH Copy-Paste)

Guía al usuario en el despliegue escalonado en dos etapas:

#### Etapa A: Estación Piloto / Canario (ej. `CHA01` o `TEST-01`)
Proporciona los comandos para ejecutar en la terminal remota de la estación piloto:

```bash
# 1. Conectarse a la estación piloto
ssh rsa@<ip_o_nombre_estacion>

# 2. Navegar al repositorio, descargar el tag y actualizar
cd ~/git/RSA-Acelerografo
git fetch --tags
git checkout vX.Y.Z
./menu.sh    # Seleccionar Opción 3 (Actualizar el proyecto)

# 3. Reiniciar el pipeline y verificar adquisición
registrocontinuo restart
comprobar
```

#### Etapa B: Resto de la Flota (ej. `CHA02`, `EST02`, etc.)
Una vez validada la estación piloto, proporciona los comandos para actualizar las estaciones restantes secuencialmente repitiendo el bloque anterior con la IP correspondiente.

---

### Paso 5: Auditoría y Verificación Post-Despliegue

1. Solicita al usuario que pegue en `logs.tmp` la salida de verificación de la estación remota:
   ```bash
   # En la estación remota:
   tail -n 25 $PROJECT_LOCAL_ROOT/log-files/mqtt_coordinator.log
   ls -la $PROJECT_LOCAL_ROOT/log-files/
   ```
2. Lee `logs.tmp` y audita:
   - **Adquisición**: Latencia (`age < 2.0s`) y sincronización NTP (`wait_for_ntp` OK).
   - **Sensor**: Integridad triaxial (`[SENSOR_OK]` y ausencia de `[SENSOR_ANOMALY]`).
   - **Permisos**: Titularidad correcta `rsa:rsa` en todos los archivos de `$PROJECT_LOCAL_ROOT/log-files/` (validando la efectividad del blindaje de permisos de `update.sh`).
3. Entrega un informe de síntesis confirmando si la estación se encuentra en estado **Nominal**, o señalando anomalías específicas a corregir.
