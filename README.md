# 🤖 DocRAG - Agentic Retrieval Augmented Generation System

A sophisticated **Retrieval Augmented Generation (RAG)** system built with **LangGraph**, **LangChain**, and **OpenAI**, featuring an intelligent agent-based architecture for semantic document search and question-answering.

## 📋 Overview

DocRAG is a production-ready RAG system that combines document retrieval with agentic intelligence to answer complex questions about loaded documents. It leverages modern AI frameworks to provide accurate, context-aware responses with source attribution.

**Key Innovation**: Integrates ReAct (Reasoning + Acting) agent pattern within a LangGraph workflow for enhanced reasoning capabilities over retrieved documents.

---

## ✨ Features

- **🔍 Semantic Search**: FAISS-powered vector similarity search for intelligent document retrieval
- **🧠 Agentic Architecture**: Built-in ReAct agent for multi-step reasoning and action planning
- **📄 Multi-format Support**: Process documents from various sources (URLs, PDFs, text)
- **⚡ Streaming Support**: Real-time response generation with streaming capabilities
- **📊 Response Tracking**: Built-in history tracking and performance metrics
- **🎨 Interactive UI**: Streamlit-based web interface for easy interaction
- **🔗 Source Attribution**: Retrieve and display relevant source documents for verification
- **⚙️ Configurable**: Easy customization of chunk sizes, overlap, and LLM parameters

---

## 🏗️ Architecture

The system follows a sophisticated multi-layer architecture:

```
User Input
    ↓
Streamlit UI (streamlit_app.py)
    ↓
Graph Builder (LangGraph StateGraph)
    ├─→ Retriever Node (Vector Search)
    │   └─→ FAISS Vector Store
    │
    └─→ Responder Node (ReAct Agent)
        └─→ OpenAI GPT-4o LLM
            ├─→ Wikipedia Tool
            └─→ Wikidata Tool
    ↓
Formatted Response + Source Docs
```

### Core Components

| Component | Purpose |
|-----------|---------|
| **document_processor.py** | Ingests and chunks documents from URLs |
| **vectorstore.py** | FAISS-based vector database management |
| **graph_builder.py** | Orchestrates RAG workflow with LangGraph |
| **reactnode.py** | Implements ReAct agent with retrieval tools |
| **rag_state.py** | Defines application state schema |
| **streamlit_app.py** | Interactive web UI for end users |

---

## 🛠️ Tech Stack

### Core Frameworks
- **LangGraph** - Workflow orchestration and state management
- **LangChain** - LLM integration and chain building
- **OpenAI GPT-4o** - Large Language Model

### Data Processing
- **FAISS** - Fast approximate nearest neighbor search
- **Pydantic** - Data validation and settings management

### Web & Integration
- **Streamlit** - Interactive web interface
- **BeautifulSoup4** - Web scraping for document extraction
- **Requests** - HTTP client for URL content fetching

### Knowledge Sources
- **Wikipedia API** - External knowledge retrieval
- **Wikidata** - Structured knowledge integration

---

## 📦 Installation

### Prerequisites
- Python 3.9+
- OpenAI API Key

### Setup Steps

1. **Clone the repository**
```bash
git clone https://github.com/yourusername/docRAG.git
cd docRAG
```

2. **Create virtual environment**
```bash
python -m venv .venv
.venv\Scripts\activate  # Windows
source .venv/bin/activate  # macOS/Linux
```

3. **Install dependencies**
```bash
pip install -r requirements.txt
```

4. **Configure environment**
```bash
# Create .env file
echo OPENAI_API_KEY=your_api_key_here > .env
```

5. **Verify installation**
```bash
python -c "from src.config.config import Config; print('✓ Installation successful')"
```

---

## 🚀 Usage

### Web Interface (Recommended)

```bash
streamlit run streamlit_app.py
```

The app will:
- Initialize the RAG system with default documents
- Load 2 articles on LLM agents and diffusion models
- Create a vector database with document chunks
- Present an interactive search interface

**Features in UI:**
- Real-time question input
- Source document visualization
- Response time tracking
- Search history (last 3 queries)

### Programmatic Usage

```python
from src.config.config import Config
from src.document_ingestion.document_processor import DocumentProcessor
from src.vectorstore.vectorstore import VectorStore
from src.graph_builder.graph_builder import GraphBuilder

# Initialize components
llm = Config.get_llm()
doc_processor = DocumentProcessor()
vector_store = VectorStore()

# Process documents
urls = ["https://example.com/article1", "https://example.com/article2"]
documents = doc_processor.process_urls(urls)
vector_store.create_vectorstore(documents)

# Build and run RAG workflow
graph = GraphBuilder(
    retriever=vector_store.as_retriever(),
    llm=llm
)
graph.build()

# Query
result = graph.run("Your question here")
print(result["answer"])
```

### Custom Configuration

Edit `src/config/config.py`:

```python
class Config:
    CHUNK_SIZE = 500           # Document chunk size
    CHUNK_OVERLAP = 50         # Overlap between chunks
    LLM_MODEL = "openai:gpt-4o"  # Model selection
    Default_URLS = [...]       # Default documents
```

---

## 📊 Project Structure

```
docRAG/
├── src/
│   ├── config/              # Configuration management
│   ├── document_ingestion/  # Document processing pipeline
│   ├── vectorstore/         # Vector database operations
│   ├── graph_builder/       # LangGraph workflow builder
│   ├── nodes/               # Graph nodes (retrieval, generation)
│   └── state/               # State definitions
├── data/
│   └── url.txt              # Sample URLs for processing
├── streamlit_app.py         # Web interface
├── main.py                  # CLI entry point
├── requirements.txt         # Python dependencies
├── pyproject.toml           # Project metadata
└── README.md                # This file
```

---

## 🔑 Key Implementation Details

### State Management (RAG State)
- Maintains conversation history
- Tracks retrieved documents
- Stores intermediate results
- Manages agent state

### Retrieval Pipeline
1. **Document Ingestion**: Fetch and parse documents from URLs
2. **Chunking**: Split documents into overlapping chunks
3. **Embedding**: Convert chunks to vector embeddings
4. **Indexing**: Store in FAISS for fast retrieval

### Generation Pipeline
1. **Query Embedding**: Convert user question to embedding
2. **Retrieval**: Find top-K similar documents (default K=3-5)
3. **ReAct Agent**: Multi-step reasoning with access to:
   - Retrieved context
   - Wikipedia for external knowledge
   - Wikidata for structured facts
4. **Response Generation**: Synthesize final answer from reasoning steps

---

## 🎯 Use Cases

- **Documentation Search**: Intelligent QA over technical documentation
- **Research Assistant**: Quick answers from academic papers
- **Knowledge Base**: Internal company documentation search
- **Educational Tool**: Learning platform for complex topics
- **Content Analysis**: Extract insights from document collections

---

## 📈 Performance Metrics

The system tracks:
- **Response Time**: Total latency from query to answer
- **Retrieval Quality**: Relevance of retrieved documents
- **Token Usage**: Input/output tokens for cost tracking
- **Error Rates**: System reliability metrics

---

## 🔄 Workflow Execution

```
1. User submits question
   ↓
2. Query embedding generation
   ↓
3. Vector similarity search (FAISS)
   ↓
4. Top documents retrieved
   ↓
5. ReAct Agent initialization
   ↓
6. Multi-step reasoning:
   - Analyze question
   - Search retrieved docs
   - Query external tools (Wikipedia/Wikidata)
   - Synthesize information
   ↓
7. Generate structured response
   ↓
8. Return answer + source attribution + metrics
```

---

## 🚦 Error Handling

The system includes robust error handling for:
- Missing API keys
- Invalid URLs
- Document parsing failures
- Vector store initialization errors
- LLM API rate limits

All errors are caught and user-friendly messages are displayed in the Streamlit UI.

---

## 🔐 Security Considerations

- API keys stored in `.env` file (not committed)
- Input validation on user queries
- Rate limiting for API calls
- Secure document storage

---

## 🎓 Learning Outcomes

This project demonstrates:
- **LangGraph** state graph orchestration
- **ReAct Pattern** implementation (Reasoning + Acting)
- **Vector Database** usage (FAISS)
- **LLM Integration** with LangChain
- **Agentic AI** architecture
- **Streamlit** application development
- **Full-stack RAG** system design