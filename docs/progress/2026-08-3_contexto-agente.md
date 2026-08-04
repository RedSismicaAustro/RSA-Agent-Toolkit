# Resumen de Contexto: Arquitectura Híbrida de Planificación (Claude API) y Ejecución (Gemini Flash en Antigravity IDE)

**Fecha de creación:** 3 de agosto de 2026  
**Destinatario:** Agente de IA / Contexto de Sesión Futura  
**Entorno del usuario:** Windows 11 + WSL (Ubuntu/Linux) para desarrollo destinado a Raspberry Pi.  
**Estado de Suscripción:** Suscripción activa a Google Antigravity Pro (~6 meses restantes).

---

## 1. Contexto y Problema

El usuario cuenta con una suscripción activa a **Google Antigravity Pro**, lo que le otorga acceso al agente nativo de Antigravity alimentado por modelos Gemini (Gemini 3.6 Flash y Gemini 3 Pro) sin coste adicional por token dentro de la aplicación.

### Diagnóstico de Modelos:
* **Gemini 3.6 Flash:** Extremadamente veloz (2º puesto en velocidad según benchmarks de *Artificial Analysis*) y eficiente para uso de herramientas, edición de código y ejecución de comandos en la terminal de WSL. Sin embargo, su capacidad de razonamiento abstracto para arquitectura y planificación profunda es limitada en proyectos complejos.
* **Modelos de Razonamiento Superior (Claude Sonnet / Opus):** Tienen un rendimiento óptimo en diseño de arquitectura y planificación de tareas paso a paso, pero el uso de la interfaz web gratuita no es viable debido a la necesidad de pasar contexto masivo de archivos del proyecto. Pagar suscripciones web adicionales resulta redundante y costoso.

---

## 2. Requerimientos Técnicos

1. **Aprovechamiento de Recursos Existentes:** Maximizar el uso de la suscripción Antigravity Pro para toda la fase pesada de escritura de código, modificación de archivos y ejecución en la terminal WSL (coste $0 adicional).
2. **Planificación de Alta Inteligencia:** Integrar la API de Anthropic (Claude) exclusivamente para la fase de arquitectura y generación de planes de implementación en formato Markdown (`implementation_plan.md`).
3. **Flujo 100% Nativo en Antigravity IDE:** Evitar la instalación de extensiones secundarias de terceros de VS Code (como Roo Code o Cline) o copiado/pegado manual desde interfaces web.
4. **Seguridad y Gestión de Credenciales:** Manejo seguro de la `ANTHROPIC_API_KEY` mediante archivos de entorno locales (`~/.env`) en el entorno WSL.
5. **Costo-Efectividad:** La API de Anthropic se utilizará bajo el esquema *Pay-as-you-go* únicamente para las llamadas de planificación (coste estimado de $0.02 a $0.05 USD por plan generado), manteniendo la ejecución masiva dentro de la tarifa plana de Antigravity.

---

## 3. Propuesta de Solución: Skill Personalizada (`claude-planner`)

La solución consiste en construir una **Skill personalizada** dentro de la carpeta `.agents/skills/` del proyecto en Antigravity. Las Skills en Antigravity permiten extender las capacidades del agente Gemini mediante instrucciones y scripts ejecutables locales.

### Estructura de la Skill Propuesta
* `.agents/skills/claude-planner/SKILL.md`: Documento de definición de la Skill que indica a Gemini cuándo y cómo invocar la fase de planificación de Claude.
* `.agents/scripts/generate_plan.py`: Script en Python ejecutado por el agente Gemini que:
  1. Carga de forma segura la clave `ANTHROPIC_API_KEY` desde `~/.env`.
  2. Recopila el prompt y el contexto del requerimiento.
  3. Realiza la llamada a la API de Anthropic (`claude-3-5-sonnet` o `claude-3-opus`).
  4. Escribe la respuesta estructurada directamente en el archivo `implementation_plan.md` en la raíz del espacio de trabajo.

### Flujo de Trabajo Operativo (Workflow)
1. **Invocación:** El usuario solicita a Antigravity la planificación de una característica o arquitectura indicando el uso de la skill (ej. *"Planifica esta arquitectura usando la skill claude-planner"*).
2. **Generación del Plan:** El agente Gemini en Antigravity lanza el script de la skill. Claude procesa la solicitud vía API por unos centavos y guarda el archivo `implementation_plan.md`.
3. **Ejecución Autónoma:** El agente nativo de Antigravity (configurado con Gemini 3.6 Flash bajo la suscripción Pro) toma el archivo `implementation_plan.md` generado por Claude y procede a ejecutar todas las modificaciones de código, creación de archivos y comandos bash en la terminal WSL a máxima velocidad y con coste cero adicional.

---

## 4. Estado de Decisiones y Próximos Pasos para el Agente

* **Decisión tomada:** El usuario ha optado por implementar la Skill personalizada como la solución óptima que combina la inteligencia de arquitectura de Claude vía API con la velocidad y cobertura de suscripción de Gemini Flash en Antigravity IDE.
* **Entorno de ejecución:** Windows 11 con WSL (Ubuntu). Todo comando terminal y desarrollo debe ejecutarse considerando la compatibilidad Linux para Raspberry Pi.
