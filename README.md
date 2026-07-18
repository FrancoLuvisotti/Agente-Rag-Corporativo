# Agente RAG Corporativo

MVP de una base de conocimiento conversacional para colaboradores. Procesa documentos internos, recupera fragmentos relevantes y muestra las fuentes usadas. Está diseñado para comenzar localmente y evolucionar hacia una integración con un LLM, almacenamiento vectorial administrado y fuentes documentales corporativas.

## Funcionalidades

- Chat web con historial, filtro por categoría, citas y botones de feedback.
- Ingesta mediante carga manual y lectura de carpetas. Admite PDF, Word, Excel, PowerPoint, Markdown, CSV, JSON y HTML.
- Extracción específica por formato, limpieza, fragmentación con solapamiento y metadatos (categoría, archivo, ubicación y fecha).
- Recuperación semántica con embeddings locales (`all-MiniLM-L6-v2`) y ChromaDB persistente. Si la relevancia es baja, responde que no tiene respaldo documental.
- Tres modos de redacción: extractivo, Ollama local y Gemini Cloud (con clave secreta para despliegue web).
- Registro JSON Lines de cada consulta: fecha, filtro, fuentes, puntajes, respuesta y latencia.
- Datos de muestra para RH, Finanzas y Operaciones.
- Base documental ficticia y legible de NovaTech Solutions, organizada por área; cada archivo indica vigencia y responsable.

## Ejecutar localmente

```bash
python -m venv .venv
.venv\Scripts\activate
pip install -r requirements.txt
streamlit run app.py
```

Prueba con: `¿Cuántos días de vacaciones tengo?`, `¿Cuál es el límite de comidas de trabajo?` o `¿Qué hago ante una incidencia crítica?`.

También puedes probar: `¿Qué hago si recibo un correo de phishing?`, `¿Cuánto dura el reembolso?`, `¿Cuándo se lanza el módulo de automatización?` y `¿Quién aprueba un descuento mayor al 10 %?`.

## Respuestas más naturales con Ollama (opcional)

Instala Ollama y descarga un modelo local antes de iniciar la aplicación:

```bash
ollama pull llama3.2:3b
```

En la barra lateral activa **Usar LLM local (Ollama)**. El agente le envía al modelo solamente la pregunta, un historial corto y los fragmentos recuperados; las fuentes siguen apareciendo debajo de la respuesta. Si Ollama no está disponible, la aplicación vuelve automáticamente al modo extractivo.

## Gemini Cloud para la aplicación pública

1. Crea una clave en Google AI Studio.
2. En local, copia `.streamlit/secrets.toml.example` como `.streamlit/secrets.toml` y pega la clave.
3. En Streamlit Community Cloud, abre **Advanced settings → Secrets** y pega:

```toml
GEMINI_API_KEY = "tu_clave"
```

4. En la aplicación, selecciona **Gemini Cloud** y el modelo `gemini-3.5-flash`.

El archivo real `secrets.toml` está ignorado por Git y nunca debe publicarse.

## Estructura

```text
data/documents/<categoría>/  # documentos aprobados y vigentes
logs/execution.jsonl         # trazabilidad de consultas
app.py                       # interfaz, ingesta, búsqueda y registro
```

## Gobernanza propuesta

| Categoría | Responsable sugerido | Regla de curaduría |
| --- | --- | --- |
| RH | Equipo de Recursos Humanos | Solo políticas vigentes aprobadas |
| Finanzas | Control de gestión | Revisión mensual de políticas |
| Legal | Área Legal | Versionado y fecha de vigencia obligatorios |
| Operaciones | Líder de Operaciones | Revisión tras cambios de proceso |

Antes de indexar documentos reales, se deben identificar la fuente oficial, el responsable y el estado de vigencia; se deben excluir borradores y duplicados.

## Próximos pasos de escalabilidad

1. Conectar `answer` a un LLM con un prompt estricto: usar solo el contexto y citar sus fuentes.
2. Sincronizar Drive, SharePoint u otra fuente con una tarea programada.
3. Implementar reranking, evaluación con preguntas de prueba y tablero de métricas desde `logs/execution.jsonl`.

## Despliegue gratuito sugerido

El proyecto puede desplegarse en Streamlit Community Cloud: subir este directorio a un repositorio público de GitHub, crear una aplicación apuntando a `app.py` e instalará `requirements.txt` automáticamente. Tras publicarlo, agrega aquí una captura o video del agente en línea y la URL pública para cumplir con la evidencia de ejecución.

> Nota: los documentos incluidos son ficticios. No cargues contratos, datos personales ni información confidencial real en un repositorio público.
