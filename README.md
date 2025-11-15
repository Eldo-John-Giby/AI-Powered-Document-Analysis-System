# Fast Insurance Q&A RAG System

A high-performance Retrieval-Augmented Generation (RAG) system designed for legal and insurance document analysis. Built with FastAPI, LangChain, and Weaviate for lightning-fast question answering on complex documents.

## 🚀 Features

- **Ultra-Fast Processing**: Optimized for sub-15 second response times
- **Multi-Document Support**: PDF, DOCX, and Email file processing
- **Semantic Search**: Hybrid search combining vector and keyword retrieval
- **Document Ingestion**: Persistent vector storage for uploaded documents
- **Concurrent Query Processing**: Handles up to 12 concurrent questions
- **Smart Chunking**: Semantic similarity-based document splitting
- **Professional Legal Responses**: 3-4 sentence answers with legal context

## 📋 Table of Contents

- [Architecture](#architecture)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Configuration](#configuration)
- [API Endpoints](#api-endpoints)
- [Usage Examples](#usage-examples)
- [Deployment](#deployment)
- [Performance Optimization](#performance-optimization)
- [Project Structure](#project-structure)
- [Contributing](#contributing)

## 🏗️ Architecture

### Core Components

1. **FastAPI Backend**: High-performance async API framework
2. **LangChain**: Document processing and RAG chain orchestration
3. **Weaviate**: Cloud-based vector database for semantic search
4. **Groq LLM**: Ultra-fast inference with Llama-3.1-8b-instant
5. **VoyageAI Embeddings**: High-quality semantic embeddings (voyage-3-large)

### Processing Pipeline

```
Document Upload → PDF/DOCX Processing → Smart Chunking → 
Semantic Embeddings → Vector Store → Hybrid Retrieval → 
LLM Generation → Legal-Formatted Response
```

## 📦 Prerequisites

- Python 3.10+
- API Keys:
  - Groq API Key (for LLM)
  - VoyageAI API Key (for embeddings)
  - Weaviate Cloud URL and API Key

## 🔧 Installation

### Local Setup

1. **Clone the repository**
```bash
git clone <repository-url>
cd Fast-Insurance-Q-A-RAG-HackRx-6.0--main
```

2. **Create virtual environment**
```bash
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
```

3. **Install dependencies**
```bash
pip install -r requirements_clean.txt
```

### Docker Setup

Build and run using Docker:

```bash
# Build the image
docker build -t insurance-rag-api .

# Run the container
docker run -p 10000:10000 \
  -e GROQ_API_KEY=your_groq_key \
  -e VOYAGE_API_KEY=your_voyage_key \
  -e WEAVIATE_URL=your_weaviate_url \
  -e WEAVIATE_API_KEY=your_weaviate_key \
  insurance-rag-api
```

## ⚙️ Configuration

### Environment Variables

Create a `.env` file in the project root:

```env
# LLM Configuration
GROQ_API_KEY=your_groq_api_key_here

# Embeddings Configuration
VOYAGE_API_KEY=your_voyage_api_key_here

# Vector Database Configuration
WEAVIATE_URL=your_weaviate_cloud_url
WEAVIATE_API_KEY=your_weaviate_api_key

# Optional: Server Configuration
PORT=10000
```

### API Authentication

The API uses token-based authentication. Include in request headers:

```
Authorization: Bearer 8ad62148045cbf8137a66e1d8c0974e14f62a970b4fa91afb850f461abfbadb8
```

## 🔌 API Endpoints

### 1. Health Check

```http
GET /health
```

**Response:**
```json
{
  "status": "ultra-optimized",
  "target": "sub-15s processing",
  "optimizations": [
    "llama-3.1-8b-instant",
    "massive chunk sizes",
    "128 batch embedding upload",
    "max 50 chunks per document",
    "12 concurrent questions",
    "background cleanup"
  ]
}
```

### 2. Quick Q&A (Document URL)

Process a document and answer multiple questions in one request.

```http
POST /api/v1/hackrx/run
Authorization: Bearer {TEAM_TOKEN}
Content-Type: application/json
```

**Request Body:**
```json
{
  "documents": "https://example.com/insurance-policy.pdf",
  "questions": [
    "What is the coverage limit?",
    "What are the exclusions?",
    "What is the claim process?"
  ]
}
```

**Response:**
```json
{
  "answers": [
    "The coverage limit is $1,000,000 per occurrence...",
    "Exclusions include pre-existing conditions...",
    "Claims must be filed within 30 days..."
  ]
}
```

### 3. Document Ingestion

Upload and permanently store a document in the vector database.

```http
POST /api/v1/documents/ingest
Authorization: Bearer {TEAM_TOKEN}
Content-Type: application/json
```

**Request Body:**
```json
{
  "document_url": "https://example.com/policy.pdf",
  "document_id": "policy-12345",
  "title": "Health Insurance Policy 2024"
}
```

**Response:**
```json
{
  "status": "success",
  "document_id": "policy-12345",
  "chunks_processed": 45,
  "message": "Document ingested successfully"
}
```

### 4. Query Ingested Documents

Ask questions about previously ingested documents.

```http
POST /api/v1/documents/query
Authorization: Bearer {TEAM_TOKEN}
Content-Type: application/json
```

**Request Body:**
```json
{
  "query": "What are the premium payment terms?",
  "document_id": "policy-12345"
}
```

**Response:**
```json
{
  "status": "success",
  "query": "What are the premium payment terms?",
  "answer": "Premium payments are due monthly on the first day...",
  "document_id": "policy-12345",
  "chunks_used": 3
}
```

### 5. Document Status

Check the processing status of an ingested document.

```http
GET /api/v1/documents/status/{document_id}
Authorization: Bearer {TEAM_TOKEN}
```

**Response:**
```json
{
  "document_id": "policy-12345",
  "status": "completed",
  "title": "Health Insurance Policy 2024",
  "chunks": 45,
  "collection_name": "Document_policy-12345",
  "completed_at": 1699999999.999
}
```

## 💡 Usage Examples

### Python Client Example

```python
import requests

API_URL = "http://localhost:10000"
HEADERS = {
    "Authorization": "Bearer 8ad62148045cbf8137a66e1d8c0974e14f62a970b4fa91afb850f461abfbadb8",
    "Content-Type": "application/json"
}

# Quick Q&A
response = requests.post(
    f"{API_URL}/api/v1/hackrx/run",
    headers=HEADERS,
    json={
        "documents": "https://example.com/policy.pdf",
        "questions": ["What is the deductible?", "What is covered?"]
    }
)
print(response.json())

# Document Ingestion
response = requests.post(
    f"{API_URL}/api/v1/documents/ingest",
    headers=HEADERS,
    json={
        "document_url": "https://example.com/policy.pdf",
        "document_id": "doc-001",
        "title": "Insurance Policy"
    }
)
print(response.json())

# Query Document
response = requests.post(
    f"{API_URL}/api/v1/documents/query",
    headers=HEADERS,
    json={
        "query": "What are the claim requirements?",
        "document_id": "doc-001"
    }
)
print(response.json())
```

### cURL Examples

**Quick Q&A:**
```bash
curl -X POST "http://localhost:10000/api/v1/hackrx/run" \
  -H "Authorization: Bearer 8ad62148045cbf8137a66e1d8c0974e14f62a970b4fa91afb850f461abfbadb8" \
  -H "Content-Type: application/json" \
  -d '{
    "documents": "https://example.com/policy.pdf",
    "questions": ["What is the coverage amount?"]
  }'
```

**Document Ingestion:**
```bash
curl -X POST "http://localhost:10000/api/v1/documents/ingest" \
  -H "Authorization: Bearer 8ad62148045cbf8137a66e1d8c0974e14f62a970b4fa91afb850f461abfbadb8" \
  -H "Content-Type: application/json" \
  -d '{
    "document_url": "https://example.com/policy.pdf",
    "document_id": "policy-001",
    "title": "My Insurance Policy"
  }'
```

## 🚢 Deployment

### Railway Deployment

1. Connect your GitHub repository to Railway
2. Set environment variables in Railway dashboard
3. Deploy using `render.yaml` configuration

### Render Deployment

The project includes a `render.yaml` for one-click deployment:

1. Push code to GitHub
2. Connect repository to Render
3. Environment variables are automatically configured
4. Deploy with the included build command

### Production Considerations

- **API Rate Limits**: Monitor Groq and VoyageAI usage
- **Vector Store Limits**: Check Weaviate cluster capacity
- **Memory Management**: Background cleanup handles temporary collections
- **Concurrent Processing**: Default limit is 10 concurrent requests
- **Timeout Configuration**: 15s LLM timeout, 30s document download

## ⚡ Performance Optimization

### Current Optimizations

1. **Lazy Initialization**: Components loaded on-demand
2. **Batch Embedding**: 128 documents per batch upload
3. **Smart Chunking**: Dynamic chunk sizes (1500-3500 chars)
4. **Concurrent Processing**: Up to 12 parallel questions
5. **Background Cleanup**: Automatic vector store cleanup
6. **Hybrid Search**: Combined vector + keyword retrieval (alpha=0.3)

### Processing Targets

- Single document processing: < 5 seconds
- Multiple questions (12 concurrent): < 15 seconds
- Document ingestion: < 10 seconds
- Query response time: < 2 seconds

## 📁 Project Structure

```
Fast-Insurance-Q-A-RAG-HackRx-6.0--main/
├── app/
│   ├── __init__.py
│   ├── main.py              # FastAPI application & endpoints
│   ├── qa_chain.py          # RAG chain & retrieval logic
│   ├── utils.py             # Document processing utilities
│   ├── ingest.py            # (Deprecated) Legacy ingestion
│   └── rerankers/           # Cross-encoder reranking (disabled)
├── Dockerfile               # Container configuration
├── requirements.txt         # Full dependency list
├── requirements_clean.txt   # Optimized dependencies
├── runtime.txt              # Python version specification
├── render.yaml              # Render deployment config
├── .env                     # Environment variables (local)
├── .gitignore               # Git ignore rules
└── README.md                # This file
```

### Key Files

- **main.py**: FastAPI routes, request/response handling, authentication
- **qa_chain.py**: LangChain setup, vector store, retrieval, LLM prompts
- **utils.py**: PDF/DOCX/Email parsing, semantic chunking, embeddings
- **Dockerfile**: Multi-stage build for production deployment

## 🔍 Features Deep Dive

### Smart Document Chunking

The system uses semantic similarity-based chunking:

- **Section Detection**: Identifies document structure (articles, sections, clauses)
- **Dynamic Sizing**: Adjusts chunk size based on document length (1500-3500 chars)
- **Semantic Sampling**: Selects most relevant chunks using embedding similarity
- **Overlap Strategy**: 200-500 char overlap for context preservation

### Hybrid Search

Combines multiple retrieval strategies:

- **Vector Search**: Semantic similarity via embeddings
- **Keyword Search**: BM25 ranking for exact matches
- **Fusion**: Ranked fusion with alpha=0.3 (30% keyword, 70% semantic)
- **Top-K Selection**: Retrieves 3-6 chunks based on document size

### LLM Prompting

Optimized prompt template for legal documents:

- Professional legal tone
- 3-4 sentence responses
- Specific references (articles, sections, dates)
- Complete accuracy with context-only answers
- Security safeguards against prompt injection

## 🐛 Troubleshooting

### Common Issues

**Issue**: `VOYAGE_API_KEY not found in environment`
- **Solution**: Add API key to `.env` file or environment variables

**Issue**: `Failed to connect to Weaviate`
- **Solution**: Verify Weaviate cluster URL and API key are correct

**Issue**: `Timeout: Question processing exceeded Xs`
- **Solution**: Reduce concurrent questions or increase timeout values

**Issue**: `No content extracted from PDF`
- **Solution**: Check PDF is not password-protected or corrupted

## 📊 Performance Metrics

### Benchmarks (Tested Configuration)

- Document Processing: 2-5 seconds
- Single Question: 1-3 seconds
- 10 Questions (concurrent): 8-12 seconds
- Embedding Generation: 500ms per batch (128 chunks)
- Vector Store Upload: 1-2 seconds

### Resource Usage

- Memory: ~500MB base, +200MB per concurrent request
- CPU: Minimal (LLM inference offloaded to Groq)
- Network: ~50KB/s embedding traffic, variable document downloads

## 🔐 Security

- Token-based authentication on all endpoints
- Environment variable secrets management
- Prompt injection safeguards in LLM template
- Temporary file cleanup after processing
- No sensitive data logging

## 🤝 Contributing

Contributions welcome! Please follow these guidelines:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/improvement`)
3. Commit changes (`git commit -am 'Add new feature'`)
4. Push to branch (`git push origin feature/improvement`)
5. Create Pull Request

### Development Setup

```bash
# Install development dependencies
pip install -r requirements.txt

# Run in development mode
uvicorn app.main:app --reload --port 10000
```

## 📝 License

This project is developed for the HackRx 6.0 hackathon.

## 🙏 Acknowledgments

- **LangChain**: Document processing framework
- **Weaviate**: Vector database infrastructure
- **Groq**: Ultra-fast LLM inference
- **VoyageAI**: High-quality embeddings
- **FastAPI**: Modern Python web framework

## 📞 Support

For issues, questions, or contributions:
- Create an issue on GitHub
- Contact the development team
- Review documentation and examples

---

**Built with ⚡ for HackRx 6.0** | Optimized for Legal & Insurance Document Analysis
