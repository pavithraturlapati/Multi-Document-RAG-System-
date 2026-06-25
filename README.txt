📚 Multi-Document RAG System for Math Textbooks

A **Retrieval-Augmented Generation (RAG)** system built with Python and LangChain. It reads a local folder of mathematical PDF textbooks — *Statistical Probability*, *Permutations & Combinations*, and *Linear Algebra* — and lets you ask interactive questions about the material using a fully local AI model.

🏗️ Core Architecture

| Component | Technology |
|---|---|
| Orchestration | LangChain |
| Document Loading | PyPDFLoader |
| Text Splitting | RecursiveCharacterTextSplitter |
| Embedding Model | `all-MiniLM-L6-v2` (lightweight, open-source, CPU-friendly) |
| Vector Database | Chroma (persisted locally) |
| LLM | Llama 3 (via Ollama, runs fully offline) |

⚙️ How It Works

The system processes information in six logical steps:

**1. Load** — `PyPDFLoader` reads all valid `.pdf` files in the `Input` folder, gracefully skipping any corrupt or non-PDF files.

**2. Chunk** — `RecursiveCharacterTextSplitter` breaks large textbook chapters into smaller pieces, splitting first by paragraph then by sentence so related content stays together. A sliding window (chunk overlap) preserves context across boundaries.

**3. Embed** — Each chunk is converted into a numerical vector using the `all-MiniLM-L6-v2` model, capturing the semantic meaning of the text.

**4. Store** — All vectors are saved into a local Chroma vector database (`./chroma_db`), built once and reused on subsequent runs.

**5. Retrieve** — When you ask a question, the system searches the database and fetches the top 3 most semantically relevant text chunks (`k=3`).

**6. Generate** — The 3 retrieved chunks are injected into a strict prompt (`"Answer ONLY using the context below"`) and passed to Llama 3, producing an accurate, grounded response with no hallucinations.

---

📥 Input & Output

**Input**
- **Knowledge Base:** `.pdf` textbooks placed in an `Input/` folder
- **User Query:** Conceptual questions typed into a terminal chat loop (e.g., *"Explain the binomial theorem"*)

**Output**
- A natural language response generated strictly from the content of your provided textbooks — no outside knowledge, no guessing.

🛠️ Prerequisites

1. Install Python Packages

```bash
pip install langchain langchain-community langchain-huggingface langchain-chroma langchain-ollama pypdf
```

> Python **3.11** is recommended for best stability.

2. Install Ollama & Download the Model

1. Download and install [Ollama](https://ollama.com/)
2. Pull the Llama 3 model:

```bash
ollama pull llama3
```

> Ollama must be running in the background before you start the script.


🚀 How to Use

**Step 1 — Add your textbooks**

Create a folder named `Input` in the project directory and place your `.pdf` files inside it.

```
project/
├── Input/
│   ├── statistical_probability.pdf
│   ├── permutations_combinations.pdf
│   └── linear_algebra.pdf
├── chroma_db/        ← auto-created on first run
└── rag.py
```

**Step 2 — Run the script**

```bash
python rag.py
```

On first run, the system will read your PDFs and build the Chroma vector database. Subsequent runs skip this step and load the existing database directly.

**Step 3 — Ask questions**

Once the database is ready, an interactive chat loop starts in your terminal:

```
> Explain the binomial theorem
> What is the difference between permutation and combination?
> How do you find the determinant of a 3x3 matrix?
```

The system will pull the most relevant passages from your textbooks and generate a precise answer.

📁 Project Structure

```
project/
├── Input/          # Place your PDF textbooks here
├── chroma_db/      # Auto-generated vector database (do not edit)
└── rag.py          # Main application script
```
