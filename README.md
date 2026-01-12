# RAG Solution — LangChain + Ollama

This project provides a **Retrieval-Augmented Generation (RAG)** system to index documents (PDF/TXT) and answer questions using a FAISS vector store. It includes:

- a **Streamlit UI** for indexing, search, Q&A, and chatbot use;
- a **CLI** for command-line workflows;
- centralized configuration in `config.yaml`.

The LLM is **Ollama** with **Llama 3.x** (for example, `llama3.1` in the Streamlit app).【F:src/streamlit_app.py†L1-L380】【F:cli.py†L1-L200】【F:config.yaml†L1-L17】

---

## Features

- **Indexing** PDF/TXT documents into chunks (size/overlap configurable).【F:src/indexation.py†L1-L87】
- **Semantic search** with FAISS and normalized scores.【F:src/retrieval.py†L1-L77】
- **Question answering** with RAG and LLM generation (Ollama).【F:src/llm.py†L1-L84】【F:src/streamlit_app.py†L206-L320】
- **RAG chatbot** with conversation history and a larger-k fallback.【F:src/chatbot.py†L1-L110】【F:src/streamlit_app.py†L322-L394】
- **Answer evaluation** for relevance and factuality.【F:src/evaluation.py†L1-L138】【F:src/streamlit_app.py†L259-L320】

---

## Prerequisites

- **Python 3.10+**
- **Ollama** installed locally (https://ollama.com/)
- Access to a Llama model (e.g., `llama3` or `llama3.1`)

> The Streamlit UI attempts to start Ollama and automatically download `llama3.1` if needed.【F:src/streamlit_app.py†L44-L140】

---

## Installation

1. Create a virtual environment (optional but recommended).
2. Install dependencies:

```bash
pip install -r requirements.txt
```

---

## Configuration

The `config.yaml` file controls the LLM model, embeddings, and indexing settings:

- `llm_model`: Ollama model name (e.g., `llama3`)
- `embeddings_model`: HuggingFace embeddings model
- `chunk_size`, `chunk_overlap`
- `persist_directory` (FAISS index location)
- `data_directory` (documents to index)

【F:config.yaml†L1-L17】

---

## Quick start (Streamlit)

```bash
python -m streamlit run src/streamlit_app.py
```

The app provides four pages:

1. **Indexing** (load and chunk documents)
2. **Search** (semantic queries)
3. **Question Answering** (RAG with Llama)
4. **Chatbot** (multi-turn conversation)

【F:src/streamlit_app.py†L146-L394】

---

## CLI usage

### Index documents
```bash
python cli.py index --data-dir data
```

### Search
```bash
python cli.py search "my query" --k 5
```

### Ask a question
```bash
python cli.py qa "What is the policy about ...?" --k 5
```

### Start the chatbot
```bash
python cli.py chat
```

### Launch Streamlit from the CLI
```bash
python cli.py streamlit
```

【F:cli.py†L1-L200】

---

## Project structure

```
.
├── cli.py
├── config.yaml
├── data/               # Documents to index (PDF/TXT)
├── requirements.txt
└── src/
    ├── streamlit_app.py
    ├── indexation.py
    ├── retrieval.py
    ├── llm.py
    ├── chatbot.py
    └── evaluation.py
```

---

## Notes & best practices

- Place your PDF/TXT files in `data/` before indexing.【F:src/indexation.py†L12-L45】
- The FAISS index is stored in `persist_directory` (defaults to `faiss_index`).【F:src/indexation.py†L56-L66】【F:config.yaml†L14-L17】
- If Ollama is not running, Streamlit attempts to start it automatically.【F:src/streamlit_app.py†L74-L140】

---

## Troubleshooting

- **Ollama not available**: run `ollama serve`, then install the model with `ollama pull llama3.1`.
- **No index found**: run indexing first (UI or CLI).

【F:src/streamlit_app.py†L74-L140】【F:src/indexation.py†L68-L87】
