# RAG PoC

This project implements a Retrieval-Augmented Generation (RAG) system using Ollama for local LLM inference and ChromaDB for vector storage.

## Prerequisites

- macOS (this guide uses Homebrew)
- Python 3.8 or higher
- Git

## Installation and Setup

### 1. Install Ollama using Homebrew

```bash
# Install Ollama
brew install ollama
```

### 2. Start Ollama Server

```bash
# Start the Ollama service
brew services start ollama

# Or run it manually (will run in the foreground)
ollama serve
```

### 3. Pull Required Models

The RAG system uses two models:
- **llama3.2**: For text generation and question answering
- **nomic-embed-text**: For creating document embeddings

```bash
# Pull the main language model
ollama pull llama3.2

# Pull the embedding model
ollama pull nomic-embed-text
```

### 4. Verify Models Installation

```bash
# List all installed models
ollama list
```

You should see both `llama3.2` and `nomic-embed-text` in the output.

### 5. Set Up Python Environment

```bash
# Create a virtual environment (optional but recommended)
python -m venv rag_env
source rag_env/bin/activate

# Install required Python packages
pip install --upgrade pydantic-core pydantic
pip install langchain langchain-core langchain-community langchain-ollama langchain-chroma pypdf docx2txt
```

## Project Structure

```
RAG_PoC/
├── README.md
├── SimpleRag.ipynb          # Main notebook with RAG implementation
├── Documents/               # Place your PDF/DOCX files here
└── vector_db/              # ChromaDB storage (created automatically)
    ├── chroma.sqlite3
    └── e8bba4da-9672-4333-a9fc-fb4975cf1314/
```

## Usage

### 1. Add Documents

Place your PDF or Word documents in the `Documents/` folder. The system supports:
- `.pdf` files
- `.docx` files

### 2. Run the RAG System

Open and run the `SimpleRag.ipynb` notebook:

```bash
# Start Jupyter notebook
jupyter notebook SimpleRag.ipynb
```

Or run it in VS Code with the Jupyter extension.

### 3. How It Works

1. **Document Loading**: The system loads all PDF and DOCX files from the `Documents/` folder
2. **Text Chunking**: Documents are split into chunks of 1000 characters with 200 character overlap
3. **Embedding Creation**: Each chunk is converted to embeddings using `nomic-embed-text`
4. **Vector Storage**: Embeddings are stored in ChromaDB for fast retrieval
5. **Question Answering**: When you ask a question, the system:
   - Finds the 3 most relevant document chunks
   - Passes them to `llama3.2` along with your question
   - Returns an answer based only on the provided context

### 4. Example Query

```python
query = "Hi"
response = rag_chain.invoke({"input": query})
print("Response:", response.get('answer'))
```

## Configuration

Key configuration variables in `SimpleRag.ipynb`:

```python
DOC_PATH = "/path/to/your/documents"  # Documents folder path
DB_PATH = "./vector_db"               # Vector database storage
MODEL_NAME = "llama3.2"               # Main LLM model
EMBEDDING_MODEL = "nomic-embed-text"  # Embedding model
```

## Troubleshooting

### Ollama Server Issues

```bash
# Check if Ollama is running
brew services list | grep ollama

# Restart Ollama service
brew services restart ollama

# Check Ollama logs
brew services info ollama
```

### Model Not Found

```bash
# Verify models are installed
ollama list

# Re-pull models if needed
ollama pull llama3.2
ollama pull nomic-embed-text
```

### Python Package Issues

```bash
# Upgrade packages
pip install --upgrade langchain langchain-ollama langchain-chroma

# Check package versions
pip list | grep langchain
```



