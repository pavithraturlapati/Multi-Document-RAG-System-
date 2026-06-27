# 📚 Multi-Document RAG System for Math Textbooks

> Ask questions about your math textbooks in plain English — powered by a fully local, offline AI.

A **Retrieval-Augmented Generation (RAG)** system built with Python and LangChain. Point it at a folder of mathematical PDF textbooks — *Statistical Probability*, *Permutations & Combinations*, *Linear Algebra* — and get precise, grounded answers without any cloud dependency or hallucinations.

---

## ✨ Highlights

- 🔒 **100% Local & Private** — Llama 3 runs fully offline via Ollama; no data leaves your machine
- 🧠 **Grounded Answers** — Responses are generated strictly from your textbooks, never from outside knowledge
- ⚡ **Fast Retrieval** — Chroma vector database is built once and reused on every subsequent run
- 📄 **Fault-Tolerant Loading** — Gracefully skips corrupt or non-PDF files without crashing

---

## 🏗️ Architecture

```
PDF Textbooks (Input/)
        │
        ▼
  [1] Load          PyPDFLoader reads all valid .pdf files
        │
        ▼
  [2] Chunk         RecursiveCharacterTextSplitter splits by paragraph → sentence
        │            Sliding window preserves context across boundaries
        ▼
  [3] Embed         all-MiniLM-L6-v2 converts each chunk to a semantic vector
        │
        ▼
  [4] Store         Chroma persists all vectors locally to ./chroma_db
        │
        ▼
  [5] Retrieve      Top 3 most semantically relevant chunks (k=3) fetched per query
        │
        ▼
  [6] Generate      Retrieved chunks injected into a strict prompt → Llama 3 responds
```

### Component Overview

| Layer | Technology |
|-------|------------|
| Orchestration | LangChain |
| Document Loading | `PyPDFLoader` |
| Text Splitting | `RecursiveCharacterTextSplitter` |
| Embedding Model | `all-MiniLM-L6-v2` — lightweight, open-source, CPU-friendly |
| Vector Database | Chroma (persisted locally) |
| LLM | Llama 3 via Ollama (fully offline) |

---

## 📥 Input & Output

**Input**
- `.pdf` textbooks placed in an `Input/` folder
- Natural language questions typed into the terminal chat loop

**Output**
- Accurate, grounded answers generated strictly from your provided textbooks — no guessing, no outside knowledge

---

## 🛠️ Prerequisites

### 1. Python Packages

> Python 3.11 is recommended for best compatibility.

```bash
pip install langchain langchain-community langchain-huggingface \
            langchain-chroma langchain-ollama pypdf
```

### 2. Ollama & Llama 3

1. Download and install [Ollama](https://ollama.com/)
2. Pull the Llama 3 model:

```bash
ollama pull llama3
```

> ⚠️ Ollama must be running in the background before launching the script.

---

## 🚀 Usage

### Step 1 — Add your textbooks

Create an `Input/` folder in the project directory and place your PDF files inside:

```
project/
├── Input/
│   ├── statistical_probability.pdf
│   ├── permutations_combinations.pdf
│   └── linear_algebra.pdf
├── chroma_db/        ← auto-created on first run
└── rag.py
```

### Step 2 — Run the script

```bash
python rag.py
```

On **first run**, the system reads your PDFs and builds the Chroma vector database. **Subsequent runs** skip this step and load the existing database directly — much faster.

### Step 3 — Ask questions

An interactive chat loop starts in your terminal once the database is ready:

```
> Explain the binomial theorem
> What is the difference between permutation and combination?
> What's Taylor's theorem? Can you explain with an example?
```

The system retrieves the most relevant passages from your textbooks and generates a precise, sourced answer.

---

## 📁 Project Structure

```
project/
├── Input/          # Place your PDF textbooks here
├── chroma_db/      # Auto-generated vector database (do not edit manually)
└── rag.py          # Main application script
```

---

## 🔮 Potential Enhancements

- [ ] **Swap the LLM** — Plug in any Ollama-compatible model (Mistral, Gemma, Phi-3) for different speed/quality tradeoffs
- [ ] **Increase retrieval depth** — Raise `k` beyond 3 for broader context on complex questions
- [ ] **Add a web UI** — Wrap the terminal loop in a Streamlit or Gradio interface
- [ ] **Source citations** — Surface the exact page and document each answer was pulled from
- [ ] **Multi-modal support** — Extend to handle textbook diagrams and equations via vision models

---

## 📄 License

This project is licensed under the MIT License. See `LICENSE` for details.
