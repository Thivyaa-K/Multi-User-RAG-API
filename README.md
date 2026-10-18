# Multi-User RAG REST API

A Python-based REST API that implements an end-to-end Retrieval-Augmented Generation (RAG) pipeline, allowing users to perform semantic search queries and generate context-aware, AI-grounded responses over custom documents.

---

## 🚀 Project Overview

Standard Large Language Models (LLMs) only know what they were trained on, often resulting in generic answers or "hallucinations" when queried about private files or specific personal data. To solve this, this project builds an **"open-book exam" for the AI**:
* **Semantic Search:** Uses vector embeddings to search documents based on conceptual meaning rather than rigid keyword matching.
* **Strict Grounding:** Restricts the AI's universe of knowledge exclusively to retrieved text chunks, preventing fabrication.
* **Multi-Tenant Scalability:** Extends into a multi-user directory where multiple profiles can be ingested and dynamically isolated using metadata filtering.

---

## 🛠️ Architecture & RAG Pipeline

The application processes data through three core phases:
1. **Ingestion & Chunking:** Unstructured profile documents are parsed and split into logical paragraph chunks.
2. **Vector Embedding & Storage:** Chunks are converted into 768-dimensional numerical vectors via Ollama's `nomic-embed-text` model and stored locally in **ChromaDB**.
3. **Retrieval, Augmentation, & Generation:** 
   * **FastAPI** handles incoming queries via REST endpoints.
   * ChromaDB fetches the top matching context chunks.
   * The prompt is augmented with the retrieved context and sent to a local chat model (`qwen2.5:0.5b`) to generate a precise, grounded answer.

---

## 📈 Scaling to a Multi-User Directory

To transition the app from a single-user tool to a multi-tenant platform:
* **Dynamic Ingestion (`POST /documents`):** Leverages **Pydantic** models to validate incoming profile data and store chunks with attached `user_name` metadata.
* **Metadata Filtering (`GET /ask`):** Uses ChromaDB `where` parameter filters to strictly isolate vector searches to a specific user profile, preventing cross-contamination and mixed-context confusion.

---

## 💻 Core Technologies Used

* **Web Framework:** FastAPI, Uvicorn
* **AI & Vector Database:** ChromaDB, Ollama (`qwen2.5:0.5b`, `nomic-embed-text`)
* **Data Validation:** Pydantic
* **Environment & Tools:** Python (`venv`), Git, Swagger UI

---

## ⚙️ Local Installation & Setup

Follow these steps to run the project locally on your machine:

1. **Clone the repository:**
   ```bash
   git clone https://github.com/Thivyaa-K/Multi-User-RAG-API.git
   cd Multi-User-RAG-API
    ```
2. **Create and activate a Python virtual environment (Python 3.12 recommended):**
   ```bash
   python -3.12 -m venv venv
   # On Windows PowerShell:
   Set-ExecutionPolicy -ExecutionPolicy Bypass -Scope Process
   .\venv\Scripts\activate
    ```
3. **Install dependencies:**
   ```bash
   pip install fastapi uvicorn chromadb ollama
    ```
4. **Ensure Ollama is running and pull required models:**
   ```bash
   ollama pull qwen2.5:0.5b
   ollama pull nomic-embed-text
    ```
5. **Run the FastAPI server:**
   ```bash
   uvicorn main:app --reload
    ```
6. **Explore the API:**
   Open the browser and navigate to http://127.0.0.1:8000/docs to test the interactive Swagger UI playground.
