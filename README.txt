Multi-Document RAG System for Textbooks
This project is a Multi-Document Retrieval-Augmented Generation (RAG) system built with Python and LangChain
. It is designed to read a local folder of PDF textbooks (like Statistical Probability), process the text, and allow the user to ask interactive questions about the material using a local AI model
.
Features
PDF Processing: Uses PyPDFLoader to extract text from PDF files
.
Smart Chunking: Employs RecursiveCharacterTextSplitter to break massive textbook chapters into smaller, logical paragraphs and sentences
.
Semantic Search: Converts text into numerical vectors using the open-source all-MiniLM-L6-v2 embedding model and stores them locally using Chroma
.
Strict Answering: Retrieves the top 3 most relevant text chunks to answer user questions and forces the AI to answer only based on the textbook context, preventing hallucinations
.
Tech Stack
Python (Version 3.11 recommended for stability)
LangChain (for orchestration)
Chroma (for the vector database)
Ollama (to run the Llama 3 model locally)
Prerequisites
Before running this code, you must install the required Python libraries and set up the local AI model.
Install Python Packages: Run the following command in your terminal: pip install langchain langchain-community langchain-huggingface langchain-chroma langchain-ollama pypdf
Install and Run Ollama:
Download and install the Ollama desktop application.
Open your terminal and run the following command to download the AI brain
: ollama pull llama3
How to Use
Add Your Textbooks: Create a folder named Input (or update the folder_path in the code to match your chosen folder name). Place your .pdf textbooks inside this folder
.
Run the Script: Execute the Python script. If it is your first time running it, the system will read your PDFs and build a vector database saved in a ./chroma_db folder
.
Ask Questions: Once the database is loaded, an interactive chat loop will start
. Type in your conceptual questions (e.g., "Explain the binomial theory") and the system will pull the exact answer from your textbook!