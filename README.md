# RAG Agent with Flowise & Ollama

A fully local **Retrieval-Augmented Generation (RAG)** based document question-answering system built with **Flowise** and **Ollama**.

The system allows users to provide a PDF document and ask questions about its content. The entire RAG pipeline runs locally, using Ollama for both the language model and embeddings, eliminating the need for paid cloud LLM APIs.

---

## Overview

This project demonstrates how to build a local RAG pipeline using a visual workflow in Flowise.

The system processes a PDF document, divides it into smaller chunks, generates vector embeddings, stores them in an in-memory vector store, retrieves the most relevant information for a user's question, and generates an answer using a locally running LLM.

### Key Highlights

* Fully local RAG pipeline
* No paid LLM API required
* PDF-based document question answering
* Local LLM inference using Ollama
* Local text embeddings
* Conversational retrieval
* Context-based answer generation
* Flowise visual workflow
* Easy to reproduce on a local machine

---

## Architecture

```text
                         PDF DOCUMENT
                              │
                              ▼
                    ┌───────────────────┐
                    │    PDF Loader     │
                    └─────────┬─────────┘
                              │
                              ▼
              ┌─────────────────────────────┐
              │ Recursive Character Text    │
              │ Splitter                    │
              │                             │
              │ Chunk Size: 200             │
              │ Chunk Overlap: 50           │
              └──────────────┬──────────────┘
                             │
                             ▼
                  ┌─────────────────────┐
                  │  Ollama Embeddings  │
                  │  nomic-embed-text   │
                  └──────────┬──────────┘
                             │
                             ▼
                ┌─────────────────────────┐
                │ In-Memory Vector Store  │
                │                         │
                │ Top K: 3                │
                └────────────┬────────────┘
                             │
                             ▼
                    ┌────────────────┐
                    │    Retriever   │
                    └───────┬────────┘
                            │
                            ▼
              ┌─────────────────────────────┐
              │ Conversational Retrieval    │
              │ QA Chain                    │
              └──────────────┬──────────────┘
                             │
                             ▼
                    ┌─────────────────┐
                    │  Ollama LLM     │
                    │  Gemma 3 1B     │
                    └────────┬────────┘
                             │
                             ▼
                          ANSWER
```

---

## Flowise Workflow

The complete workflow is implemented as a Flowise Chatflow and is provided in this repository.

![Flowise RAG Workflow](https://github.com/Shreyash-dev06/RAG-Agent-Flowise/blob/main/rag-workflow.png)

The workflow contains the following major components:

1. PDF File Loader
2. Recursive Character Text Splitter
3. Ollama Embeddings
4. In-Memory Vector Store
5. Conversational Retrieval QA Chain
6. Ollama Chat Model

---

## Tech Stack

| Technology                        | Purpose                           |
| --------------------------------- | --------------------------------- |
| Flowise                           | Visual RAG workflow orchestration |
| Ollama                            | Local LLM and embedding runtime   |
| Gemma 3 1B                        | Local language model              |
| nomic-embed-text                  | Document embedding model          |
| RAG                               | Retrieval-Augmented Generation    |
| PDF Loader                        | Document ingestion                |
| Recursive Character Text Splitter | Document chunking                 |
| In-Memory Vector Store            | Vector storage and retrieval      |

---

## How the RAG Pipeline Works

### 1. PDF Ingestion

The user provides a PDF document to the Flowise PDF File Loader.

The loader extracts the document content and prepares it for further processing.

### 2. Text Splitting

The extracted document is divided into smaller chunks using the Recursive Character Text Splitter.

Current configuration:

```text
Chunk Size: 200
Chunk Overlap: 50
```

Chunking allows the retrieval system to work with smaller and more relevant sections of the document.

### 3. Embedding Generation

Each document chunk is converted into a numerical vector using the local Ollama embedding model:

```text
nomic-embed-text
```

No external embedding API is required.

### 4. Vector Storage

The generated embeddings are stored in Flowise's In-Memory Vector Store.

The current configuration retrieves:

```text
Top K: 3
```

relevant chunks for a query.

### 5. Retrieval

When the user asks a question, the system searches the vector store and retrieves the most relevant document chunks.

### 6. Answer Generation

The retrieved context is passed to the Conversational Retrieval QA Chain.

The language model used for generating the response is:

```text
Gemma 3 1B
```

running locally through Ollama.

### 7. Context Restriction

The response prompt instructs the model to use only the retrieved document context.

If the requested information is not available in the retrieved context, the configured response is:

```text
Answer not found in document.
```

This helps reduce unsupported or hallucinated answers.

---

# Step 1: Install Ollama

Download and install Ollama for your operating system from the official Ollama website.

After installation, verify that Ollama is available:

```bash
ollama --version
```

You should see the installed Ollama version.

---

# Step 2: Download the Required Ollama Models

This project uses two local models.

### Chat Model

```bash
ollama pull gemma3:1b
```

### Embedding Model

```bash
ollama pull nomic-embed-text
```

Verify the downloaded models:

```bash
ollama list
```

You should see both:

```text
gemma3:1b
nomic-embed-text
```

---

# Step 3: Start Ollama

Start the Ollama service:

```bash
ollama serve
```

Ollama normally exposes its local API through:

```text
http://localhost:11434
```

Keep Ollama running while using the Flowise RAG Agent.

---

# Step 4: Install Node.js

Install a current supported Node.js version on your system.

Verify the installation:

```bash
node --version
```

and:

```bash
npm --version
```

---

# Step 5: Install Flowise

Install Flowise using npm:

```bash
npm install -g flowise
```

Verify the installation:

```bash
flowise --version
```

---

# Step 6: Start Flowise

Start Flowise locally:

```bash
npx flowise start
```

Flowise will provide a local address in the terminal.

Open the displayed Flowise address in your browser.

---

# Step 7: Download This Repository

Clone the repository:

```bash
git clone https://github.com/Shreyash-dev06/RAG-Agent-Flowise.git
```

Move into the project directory:

```bash
cd RAG-Agent-Flowise
```

The repository contains the Flowise Chatflow JSON and project documentation.

---

# Step 8: Import the Flowise Chatflow

Open Flowise in your browser.

Inside Flowise:

1. Open the Chatflows section.
2. Select the option to import a Chatflow.
3. Select:

```text
rag-agent-chatflow.json
```

4. Import the Chatflow.
5. Open the imported workflow.

You should see the RAG pipeline containing the PDF loader, text splitter, embeddings, vector store, retrieval chain, and Ollama model.

---

# Step 9: Verify Ollama Configuration

Open the **ChatOllama** node.

The configuration should use:

```text
Base URL:
http://localhost:11434
```

and:

```text
Model:
gemma3:1b
```

Next, open the **Ollama Embeddings** node.

The configuration should use:

```text
Base URL:
http://localhost:11434
```

and:

```text
Model:
nomic-embed-text
```

These settings connect Flowise to your locally running Ollama instance.

---

# Step 10: Upload a PDF

Open the PDF File node in Flowise.

Upload a PDF document that you want the RAG Agent to use as its knowledge source.

The current workflow is configured to process the PDF and pass the resulting document data to the vector store.

---

# Step 11: Test the RAG Agent

After configuring the workflow:

1. Save the Chatflow.
2. Open the Flowise chat interface.
3. Ask a question related to the uploaded PDF.
4. Verify that the answer is based on the document.

For example:

```text
What is the main topic of this document?
```

If that information is not contained in the uploaded document, the configured system should respond:

```text
Answer not found in document.
```

---

# Configuration

The current Flowise workflow uses the following configuration:

### ChatOllama

```text
Provider: Ollama
Model: gemma3:1b
Base URL: http://localhost:11434
Temperature: 0
```

### Embeddings

```text
Provider: Ollama
Model: nomic-embed-text
Base URL: http://localhost:11434
```

### Text Splitter

```text
Type: Recursive Character Text Splitter
Chunk Size: 200
Chunk Overlap: 50
```

### Vector Store

```text
Type: In-Memory Vector Store
Top K: 3
```

### Document Loader

```text
Type: PDF File Loader
Usage: One document per page
```

---

# Why Local RAG?

Traditional RAG applications often rely on cloud-based LLM and embedding APIs.

This project uses Ollama locally instead.

### Advantages

* No paid LLM API required
* No API key required for inference
* Data can remain on the local machine
* Easy to experiment with different open-source models
* Useful for learning RAG architecture
* Suitable for local development and experimentation

### Limitations

Because the models run locally, performance depends on the computer's available CPU, RAM, GPU, and storage.

---

# License

This project is licensed under the [MIT License](LICENSE).

---

Developed by **[Shreyash Ahire](https://github.com/Shreyash-dev06)**.
