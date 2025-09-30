# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Development Commands

### Starting the Application
```bash
# Quick start using the provided script
./run.sh

# Manual start
cd backend && uv run uvicorn app:app --reload --port 8000
```

### Dependency Management
```bash
# Install/sync dependencies
uv sync

# Install uv (if needed)
curl -LsSf https://astral.sh/uv/install.sh | sh
```

### Environment Setup
- Create `.env` file in root directory with `ANTHROPIC_API_KEY=your_key_here`
- Python 3.13+ required
- Uses uv for package management

## Project Architecture

### Core Components
This is a RAG (Retrieval-Augmented Generation) system with the following architecture:

- **FastAPI Backend** (`backend/app.py`): Main API server serving both REST endpoints and static frontend
- **RAG System** (`backend/rag_system.py`): Central orchestrator coordinating all components
- **Vector Store** (`backend/vector_store.py`): ChromaDB integration for semantic search
- **AI Generator** (`backend/ai_generator.py`): Anthropic Claude integration with tool support
- **Document Processor** (`backend/document_processor.py`): Handles PDF/DOCX/TXT file processing
- **Session Manager** (`backend/session_manager.py`): Manages conversation history
- **Search Tools** (`backend/search_tools.py`): Tool-based search functionality for AI
- **Models** (`backend/models.py`): Pydantic models for Course, Lesson, and CourseChunk

### Key Architecture Patterns
- **Tool-based AI**: The AI uses search tools to query the vector store rather than direct context injection
- **Session-based Conversations**: Maintains conversation history with configurable limits
- **Modular Design**: Each component is isolated with clear interfaces
- **Course-centric Data Model**: Documents are processed as courses containing lessons and chunks

### Data Flow
1. Documents in `docs/` folder are processed into Course objects with lessons
2. Content is chunked and stored in ChromaDB with embeddings
3. User queries trigger AI generation with access to search tools
4. AI uses tools to search relevant content and generates responses
5. Conversation history is maintained per session

### Configuration
- All settings in `backend/config.py` using environment variables
- Default model: `claude-sonnet-4-20250514`
- Chunk size: 800 characters with 100 character overlap
- Max search results: 5
- Conversation history: 2 exchanges

### API Endpoints
- `POST /api/query`: Process user queries with optional session ID
- `GET /api/courses`: Get course statistics and titles
- Frontend served at root `/`

### File Structure
- `backend/`: All Python backend code
- `frontend/`: Static HTML/CSS/JS files
- `docs/`: Course documents (auto-loaded on startup)
- `chroma_db/`: Vector database storage (auto-created)

## Source code management

### git repository

When I ask you to push the changes to the git repo, will run this:
```
eval `ssh-agent` && ssh-add ~/.ssh/id_rsa && git push aldian
```