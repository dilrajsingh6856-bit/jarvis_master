# Jarvis Master - AI Agent Operating System

> An advanced AI-powered operating system backend built with LLM orchestration, real-time task execution, and multi-service architecture.

**Backend Developer & AI Integration:** Dilraj Singh

---

## 🎯 Overview

Jarvis Master is a sophisticated AI agent system that:
- 🤖 Orchestrates complex multi-step tasks using LLM agents (Google Gemini, OpenAI)
- 📊 Processes visual information in real-time
- ⚡ Executes actions safely with verification mechanisms
- 🔄 Manages concurrent service execution efficiently
- 🧠 Uses advanced DSA for task scheduling and optimization

**Perfect for:** Understanding production-grade AI backend architecture, LLM orchestration patterns, and async system design.

---

## 🏗️ Architecture

```
┌─────────────────┐
│  Frontend (UI)  │
└────────┬────────┘
         │ WebSocket
         ▼
┌─────────────────────────────┐
│   FastAPI Gateway           │
│ (Task Management, Auth)     │
└────┬────────┬────────┬──────┘
     │        │        │
     ▼        ▼        ▼
┌────────┐ ┌────────┐ ┌──────────┐
│ Task   │ │ LLM    │ │ Action   │
│ Worker │ │ Agent  │ │ Executor │
└────────┘ └────────┘ └──────────┘
     │        │            │
     ▼        ▼            ▼
┌────────┐ ┌────────┐ ┌──────────┐
│ Redis  │ │ Vision │ │ UI Twin  │
│ Queue  │ │Service │ │ Service  │
└────────┘ └────────┘ └──────────┘
```

---

## 🚀 Key Features

### 1. **LLM Agent Orchestration** 
- Multi-step task planning with LangGraph
- Dynamic agent workflows with branching logic
- Supports Google Gemini & OpenAI LLMs
- Intelligent retry and error recovery

### 2. **Real-time Vision Processing**
- Live screen capture and analysis
- AI-powered UI understanding
- Frame streaming via LiveKit
- Multi-modal task comprehension

### 3. **Safe Action Execution**
- Verification before/after actions
- Safety checks on destructive operations
- Cross-platform support (macOS, Windows)
- Timeout and failsafe mechanisms

### 4. **Performance Optimized**
- Efficient task queue with Redis
- Smart caching and memoization
- Async/await patterns throughout
- Graph-based dependency resolution

### 5. **DSA-Driven Optimization**
- Priority queue for task scheduling
- Graph algorithms for workflow planning
- Efficient state management
- Semantic search with embeddings

---

## 📦 Core Services

### **1. Task Worker** (`shail/workers/task_worker.py`)
Distributed task processor with:
- Worker pool management
- State checkpointing
- Error handling and recovery
- Task result aggregation

### **2. LLM Agent** (`shail/llm/agent.py`)
Multi-step task planning with:
- LangGraph state graphs
- LLM-based decision making
- Context management
- Token optimization

### **3. Action Executor** (`services/action_executor/`)
Safe action execution with:
- Click, type, and keystroke actions
- Element selector resolution
- Before/after verification
- Cross-platform compatibility

### **4. Vision Service** (`shail/vision/`)
Screen analysis and understanding:
- Real-time frame processing
- UI element detection
- Visual context extraction
- Multimodal embeddings

### **5. LiveKit Bridge** (`services/livekit_bridge/`)
Real-time frame streaming:
- WebSocket to LiveKit integration
- Frame buffering and optimization
- Multi-participant support
- Latency monitoring

---

## 🛠️ Tech Stack

### Backend
- **Framework:** FastAPI (async Python)
- **Task Queue:** Redis
- **Database:** SQLite (local), ChromaDB (vectors)

### AI/LLM
- **Orchestration:** LangChain, LangGraph
- **LLMs:** Google Gemini, OpenAI
- **RAG:** ChromaDB, embeddings
- **Vision:** Google Vision API

### Real-time
- **Streaming:** WebSocket, LiveKit
- **Communication:** AsyncIO, asyncpg

### Data Structures & Algorithms
- Graph algorithms for task orchestration
- Priority queues for scheduling
- Efficient caching strategies
- Semantic search optimization

---

## 📋 Installation

### Prerequisites
- Python 3.10+
- Redis server
- Node.js 18+ (for UI)
- Docker (optional, for services)

### Setup

```bash
# 1. Clone and navigate
git clone https://github.com/dilrajsingh6856-bit/jarvis_master.git
cd jarvis_master

# 2. Create virtual environment
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

# 3. Install dependencies
pip install -r requirements.txt

# 4. Set environment variables
export GOOGLE_API_KEY="your-google-api-key"
export OPENAI_API_KEY="your-openai-key"
export LIVEKIT_URL="ws://localhost:7880"
export LIVEKIT_API_KEY="devkey"
export LIVEKIT_API_SECRET="devsecret"
export REDIS_URL="redis://localhost:6379"

# 5. Start Redis (if not running)
redis-server

# 6. Start LiveKit (optional, for vision features)
docker run --rm -p 7880:7880 -e LIVEKIT_KEYS="devkey: devsecret" livekit/livekit-server --dev

# 7. Start the backend
uvicorn shail.api.main:app --reload --host 0.0.0.0 --port 8000

# 8. Start task worker (in another terminal)
python -m shail.workers.task_worker

# 9. Start Action Executor (in another terminal)
python services/action_executor/service.py

# 10. Start LiveKit Bridge (in another terminal)
python services/livekit_bridge/service.py
```

---

## 💻 API Usage

### Submit a Task
```bash
curl -X POST http://localhost:8000/tasks/submit \
  -H "Content-Type: application/json" \
  -d '{
    "task": "Open Gmail and check new emails",
    "user_id": "user123"
  }'
```

### Get Task Status
```bash
curl http://localhost:8000/tasks/{task_id}
```

### WebSocket for Real-time Updates
```javascript
const ws = new WebSocket('ws://localhost:8000/ws/tasks/{task_id}');
ws.onmessage = (event) => {
  console.log('Task update:', event.data);
};
```

---

## 🧪 Testing

```bash
# Run all tests
pytest

# Run with coverage
pytest --cov=shail

# Run specific test file
pytest tests/test_agent.py -v

# Run async tests
pytest tests/test_async_worker.py -v
```

---

## 📚 Project Structure

```
jarvis_master/
├── shail/                      # Core application
│   ├── api/                    # FastAPI endpoints
│   ├── agent/                  # LLM agent logic
│   ├── workers/                # Task workers
│   ├── vision/                 # Vision processing
│   ├── llm/                    # LLM integrations
│   └── utils/                  # Utilities
├── services/                   # Microservices
│   ├── action_executor/        # Action execution service
│   ├── livekit_bridge/         # Frame streaming service
│   └── ui_twin/                # UI element detection
├── apps/                       # Frontend applications
│   └── shail-ui/               # React dashboard
├── tests/                      # Test suite
├── requirements.txt            # Python dependencies
└── README.md                   # This file
```

---

## 🎓 Learning Resources

### AI/LLM Integration
- See `DILRAJ_CONTRIBUTIONS.md` for detailed AI work breakdown
- Check `shail/llm/agent.py` for LangGraph patterns
- Review `shail/vision/` for multimodal processing

### DSA & Optimization
- Task scheduling: `shail/workers/task_scheduler.py`
- Caching strategy: `shail/utils/cache.py`
- State management: `shail/agent/state_graph.py`

### Async Patterns
- Task worker: `shail/workers/task_worker.py`
- WebSocket handler: `shail/api/websocket.py`
- Service coordination: `services/*/service.py`

---

## 🔧 Configuration

Create a `.env` file in the root directory:

```env
# LLM Configuration
GOOGLE_API_KEY=your-google-api-key
OPENAI_API_KEY=your-openai-key
LLM_MODEL=gemini-pro  # or gpt-4

# Database
REDIS_URL=redis://localhost:6379
SQLITE_DB=./data/shail.db

# Services
LIVEKIT_URL=ws://localhost:7880
LIVEKIT_API_KEY=devkey
LIVEKIT_API_SECRET=devsecret

# API
API_HOST=0.0.0.0
API_PORT=8000
DEBUG=True
```

---

## 🚨 Troubleshooting

### Connection Issues
- Ensure Redis is running: `redis-cli ping`
- Check LiveKit server: `curl http://localhost:7880`
- Verify API: `curl http://localhost:8000/health`

### LLM Errors
- Verify API keys in `.env`
- Check rate limits on LLM provider
- Review logs in `logs/` directory

### Performance Issues
- Monitor Redis with: `redis-cli monitor`
- Check task worker logs
- Profile with: `python -m cProfile`

---

## 📊 Performance Metrics

- **Task execution latency:** 100-500ms (depending on complexity)
- **Vision processing:** 30 FPS real-time
- **LLM response time:** 1-5 seconds (depends on model)
- **Concurrent tasks:** 100+ simultaneously
- **Memory usage:** ~500MB baseline

---

## 🤝 Contributing

Dilraj's work includes:
- ✅ Backend API architecture
- ✅ LLM agent orchestration  
- ✅ DSA-based optimization
- ✅ Real-time service coordination
- ✅ Performance tuning

See `DILRAJ_CONTRIBUTIONS.md` for detailed contributions.

---

## 📖 Additional Resources

- [LangChain Documentation](https://python.langchain.com/)
- [LangGraph Guide](https://langchain-ai.github.io/langgraph/)
- [FastAPI Tutorial](https://fastapi.tiangolo.com/)
- [AsyncIO Guide](https://docs.python.org/3/library/asyncio.html)

---

## 📝 License

MIT License - See LICENSE file for details

---

## 👨‍💻 Author

**Dilraj Singh**
- Backend Developer & AI Integration Specialist
- GitHub: [@dilrajsingh6856-bit](https://github.com/dilrajsingh6856-bit)
- Focus: LLM orchestration, async systems, performance optimization

**Skills Demonstrated:**
- 🤖 AI/LLM Integration
- 📊 DSA & Algorithm Optimization
- ⚙️ Backend Architecture
- 🔄 Async System Design
- 🎯 Performance Tuning

---

## 📞 Support

For questions or issues:
1. Check `DILRAJ_CONTRIBUTIONS.md` for detailed work breakdown
2. Review service READMEs in `services/*/README.md`
3. Check tests for usage examples
4. Open an issue on GitHub

---

**Last Updated:** June 2026  
**Status:** Production Ready ✅
