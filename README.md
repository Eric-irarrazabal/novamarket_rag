# NovaMarket RAG

Proyecto académico desarrollado para la asignatura **Ingeniería de Soluciones con Inteligencia Artificial (ISY0101)**.

NovaMarket es una empresa ficticia de comercio electrónico. El proyecto consiste en un asistente de atención al cliente que utiliza un **LLM junto con RAG (Retrieval-Augmented Generation)** para responder consultas frecuentes utilizando información almacenada en documentos internos y una fuente externa de SERNAC.

## Objetivo

El objetivo del proyecto es responder consultas relacionadas con productos, stock, despachos, medios de pago, devoluciones, garantías y políticas de compra.

Antes de generar una respuesta, el sistema busca información relacionada con la pregunta en los documentos disponibles. Si no encuentra información suficiente o el caso necesita una revisión más específica, el asistente lo indica en lugar de inventar una respuesta.

## Tecnologías utilizadas

- Python
- Google Colab
- Qwen mediante Groq
- Sentence Transformers
- MiniLM multilingüe
- Embeddings
- Similitud coseno
- RAG

## Fuentes de información

El proyecto utiliza 9 documentos:

- 8 fuentes internas simuladas de NovaMarket.
- 1 fuente externa basada en información pública de SERNAC sobre garantía legal.

Las fuentes internas contienen información sobre:

- Productos
- Stock
- Despachos
- Medios de pago
- Devoluciones
- Garantías
- Preguntas frecuentes
- Políticas de compra

## Estructura del repositorio

```text
novamarket_rag/
│
├── data/
│   ├── internas/
│   └── externas/
│
├── docs/
│   └── arquitectura_novamarket.png
│
├── evidencias/
│   ├── pruebas_embeddings.json
│   └── capturas de los casos de prueba
│
├── novamarket_rag.ipynb
└── README.md
```

### `data/`

Contiene los documentos utilizados por el sistema RAG.

`data/internas/` contiene los documentos simulados de NovaMarket y `data/externas/` contiene la fuente externa de SERNAC.

### `docs/`

Contiene el diagrama de arquitectura del asistente.

### `evidencias/`

Contiene las evidencias obtenidas durante las pruebas del sistema, incluyendo capturas y el archivo `pruebas_embeddings.json`.

### `novamarket_rag.ipynb`

Notebook principal que contiene la implementación del asistente, recuperación de documentos, generación de respuestas y casos de prueba.

## Funcionamiento general

El flujo principal del sistema es el siguiente:

1. Se cargan los documentos internos y externos.
2. Los textos se dividen en fragmentos.
3. Los fragmentos se transforman en embeddings utilizando MiniLM multilingüe.
4. La pregunta del usuario también se transforma en un embedding.
5. Se comparan los embeddings mediante similitud coseno.
6. Se recuperan hasta 3 documentos relacionados con la consulta.
7. Los documentos recuperados se utilizan como contexto para el LLM.
8. Qwen mediante Groq genera la respuesta.
9. El sistema valida las fuentes utilizadas y guarda el resultado.

Actualmente se utilizan 25 fragmentos y un umbral mínimo de similitud de 0,43.

## Cómo ejecutar el proyecto

El proyecto está preparado para ejecutarse en **Google Colab**.

1. Abrir el archivo `novamarket_rag.ipynb` en Google Colab.
2. Verificar que las carpetas `data/internas` y `data/externas` estén disponibles.
3. Crear un secreto en Google Colab llamado:

```text
GROQ_API_KEY
```

4. Ingresar una API Key válida de Groq y habilitar el acceso del notebook al secreto.
5. Ejecutar las celdas del notebook en orden.
6. Ejecutar los casos de prueba.
7. Al finalizar, el notebook puede generar el archivo:

```text
evidencias/pruebas_embeddings.json
```

> La API Key no está incluida en este repositorio y debe ser configurada por cada usuario.

## Casos de prueba

Se probaron cinco situaciones diferentes:

| Caso | Consulta | Resultado esperado |
|---|---|---|
| A | Plazo de despacho | RESPONDIDO |
| B | Llegada de una compra | RESPONDIDO |
| C | Programa de puntos | SIN_INFORMACION |
| D | Cobro duplicado | DERIVAR |
| E | Garantía legal | RESPONDIDO |

Los casos A, B y E utilizan el modelo mediante Groq. Los casos C y D pueden resolverse sin realizar una llamada al LLM.

## Limitaciones

Este proyecto corresponde a un prototipo académico.

- Utiliza datos internos simulados.
- No está conectado a pedidos ni inventario real.
- El stock y los documentos no se actualizan automáticamente.
- La derivación a atención humana muestra un aviso, pero no crea un ticket.
- Las pruebas realizadas corresponden a un conjunto pequeño de consultas.
- Las respuestas generadas por el LLM deben ser revisadas, ya que el modelo puede cometer errores.
