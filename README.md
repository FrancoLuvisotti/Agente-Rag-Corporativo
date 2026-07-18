# Agente RAG Corporativo

Este proyecto es un prototipo de asistente de conocimiento para empresas que necesita responder preguntas sobre políticas, procesos y documentos internos de forma rápida y trazable. La aplicación combina búsqueda semántica con una interfaz web simple para que cualquier colaborador pueda consultar información corporativa sin abrir manualmente múltiples carpetas o documentos.

## ¿Qué hace esta aplicación?

La app indexa documentos locales en una base vectorial persistente, recupera los fragmentos más relevantes para una pregunta y ofrece una respuesta respaldada por fuentes. Está pensada como un MVP de base de conocimiento conversacional para uso interno y para demostrar cómo puede evolucionar hacia un sistema más robusto con integración a herramientas de empresa.

## ¿Cómo funciona?

1. Se leen los documentos incluidos en la carpeta de datos.
2. El contenido se limpia, se fragmenta en bloques y se convierte en embeddings locales.
3. Cuando el usuario hace una pregunta, la app realiza una búsqueda semántica por similitud.
4. Se recuperan los fragmentos más relevantes y se muestran como fuentes.
5. La respuesta puede generarse en tres modos:
   - Extractivo: devuelve el contenido recuperado de manera estricta.
   - Ollama local: sintetiza la respuesta usando un modelo local.
   - Gemini Cloud: usa un modelo remoto cuando existe configuración de API.

## Características principales

- Interfaz web en Streamlit con historial de conversación.
- Filtro por categoría de documentos para enfocar la búsqueda.
- Carga manual de nuevos archivos y reconstrucción del índice vectorial.
- Soporte para PDF, Word, Excel, PowerPoint, Markdown, CSV, JSON y HTML.
- Recuperación semántica con embeddings locales y ChromaDB persistente.
- Registro local de consultas en JSON Lines para trazabilidad y métricas.
- Datos de ejemplo organizados por áreas como RH, Finanzas, Operaciones y Legal.

## Requisitos

- Python 3.10 o superior.
- Dependencias listadas en requirements.txt.
- Opcional: Ollama instalado localmente para usar modelos LLM en el equipo.
- Opcional: una clave de Gemini para usar el modo Cloud en despliegues web.

## Ejecución local

```bash
python -m venv .venv
.venv\Scripts\activate
pip install -r requirements.txt
streamlit run app.py
```

## Demo pública

Puedes probar la aplicación directamente sin instalar nada en:

https://agente-rag-corporativo-suk7lrbaqvafqyju8qenri.streamlit.app/

## Ejemplos de consultas

Estas son algunas preguntas que puedes probar para explorar el comportamiento de la app:

1. ¿Cuántos días de vacaciones tengo?
2. ¿Cuál es el límite de comidas de trabajo?
3. ¿Qué hago ante una incidencia crítica?
4. ¿Qué hago si recibo un correo de phishing?
5. ¿Cuánto dura el reembolso?
6. ¿Quién aprueba un descuento mayor al 10 %?
7. ¿Cuándo se lanza el módulo de automatización?
8. ¿Qué documentación necesito para un viaje de negocio?
9. ¿Qué pasa si se detecta una fuga de datos?
10. ¿Cuál es el procedimiento para solicitar un beneficio?
11. ¿Dónde puedo consultar la política de privacidad y seguridad?
12. ¿Qué debo hacer si un sistema deja de funcionar?
13. ¿Cuál es el proceso de onboarding para nuevos colaboradores?
14. ¿Qué información se debe incluir en una auditoría?
15. ¿Qué acciones se recomiendan ante una incidencia de seguridad?

## Modos de respuesta

### 1. Extractivo (por defecto)

No necesita claves externas. Es el modo más simple y seguro para probar el flujo RAG sin depender de servicios externos.

### 2. Ollama local (opcional)

Si tienes Ollama instalado, puedes descargar un modelo local y activarlo desde la barra lateral.

```bash
ollama pull llama3.2:3b
```

En la app, selecciona el proveedor Ollama local. Si el servicio no está disponible, la aplicación vuelve automáticamente al modo extractivo.

### 3. Gemini Cloud (opcional)

Para usar Gemini en un despliegue web, crea una clave en Google AI Studio y configura el secreto de Streamlit.

1. Genera una clave en Google AI Studio.
2. Crea el archivo .streamlit/secrets.toml con el siguiente contenido:

```toml
GEMINI_API_KEY = "tu_clave"
```

3. En Streamlit Community Cloud, añade ese valor en Advanced settings → Secrets.
4. En la aplicación, elige el proveedor Gemini Cloud.

> El archivo real secrets.toml debe mantenerse fuera del control de versiones y nunca debe publicarse.

## Estructura del proyecto

```text
data/
  documents/        # documentos fuente organizados por categoría
  chroma/           # base vectorial persistente
logs/
  execution.jsonl   # trazabilidad de consultas
app.py              # interfaz, ingesta, búsqueda semántica y registro
requirements.txt    # dependencias del proyecto
```

## Gobernanza sugerida para documentos reales

Antes de usar documentos reales, conviene definir:

- fuente oficial del documento
- responsable de aprobación
- fecha de vigencia
- política de revisión y actualización
- exclusión de borradores, copias duplicadas o información sensible

## Limitaciones y próximos pasos

Este MVP está pensado para demostrar el flujo completo de un sistema RAG corporativo. Algunas mejoras futuras recomendadas son:

- conectar el generador de respuestas con prompts más estrictos para evitar inventar información
- integrar origenes como SharePoint, Drive o sistemas internos con tareas programadas
- añadir reranking, evaluación con preguntas de prueba y dashboards de métricas
- mejorar la curaduría documental y la gestión de permisos

## Nota importante

Los documentos incluidos en este repositorio son ficticios y están pensados para demostración. No cargues contratos, datos personales ni información confidencial real en un repositorio público.
