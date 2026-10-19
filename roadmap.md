# 80/20 Rule Roadmap: AI Engineering (25 Days)

Given what I know about your goals, this roadmap fits well. You're already comfortable with React, you've worked with Node.js, Python basics, AI APIs, and you're aiming to become employable quickly in AI engineering. 

**One small change:**
> Don't learn LangChain first. Learn **FastAPI alongside LangGraph**. Companies hire for **AI + Backend**, not just prompt engineering.

* **The Goal:** The goal isn't to learn every feature. The goal is to learn the **20%** that gives you **80%** of real-world AI engineering skills.

---

## Phase 1: Week 1 (Days 1–7) — FastAPI (30%)
*Build APIs because every AI app needs them.*

### Day 1
* **Learn:**
  * FastAPI basics
  * Routes (`GET`, `POST`)
  * Request Body
* **Project:** Todo API

### Day 2
* **Learn:**
  * Path Parameters
  * Query Parameters
  * Response Models
  * Validation (Pydantic)
* **Project:** Book API

### Day 3
* **Learn:**
  * CRUD operations
  * Async programming
  * Status Codes
  * Error Handling
* **Project:** Notes API

### Day 4
* **Learn:**
  * File Upload
  * Reading PDFs
  * Uploading Images
* **Project:** PDF Upload API

### Day 5
* **Learn:**
  * Environment Variables
  * API Keys
  * CORS
  * Middleware

### Day 6
* **Learn (Authentication):**
  * JWT
  * OAuth basics
* **Project:** Login API

### Day 7: Mini Project
* **Project:** AI Chat API
  * `POST /chat`
  * Accept prompt
  * Return AI response

---

## Phase 2: Week 2 (Days 8–14) — LangChain (20%)
*Don't learn every module. Only learn these:*

### Day 8: LLMs
* **Learn:**
  * Chat Models
  * Prompt Templates
* **Project:** Simple AI Chat

### Day 9: Chains
* **Learn:**
  * Sequential Chains
  * LCEL basics
* **Project:** Email Generator

### Day 10: Output Parsers
* **Learn:** Output Parsers
* **Project:** Resume Analyzer

### Day 11: Memory
* **Learn:** Conversation Memory
* **Project:** Chatbot with Memory

### Day 12: Documents
* **Learn:**
  * PDF Loader
  * Text Splitter

### Day 13: Embeddings
* **Learn:**
  * Vector Store
  * ChromaDB
* **Project:** Semantic Search

### Day 14: Mini Project
* **Project:** PDF Question Answering

---

## Phase 3: Week 3 (Days 15–21) — LangGraph (40%)
*This is your main skill. Spend the most time here.*

### Day 15: Fundamentals
* **Learn:**
  * Nodes
  * Edges
  * State
  * Flow
* **Project:** Simple Graph (`User` $\rightarrow$ `LLM` $\rightarrow$ `Output`)

### Day 16: Control Flow
* **Learn:**
  * Conditional Edges
  * Loops
  * Decision Making
* **Project:** Customer Support Bot

### Day 17: Tool Calling
* **Learn:**
  * Search Tool
  * Calculator
  * Python Tool
* **Project:** Tool Calling Agent

### Day 18: Persistence
* **Learn:**
  * Memory
  * State Management
  * Checkpointing

### Day 19: Multi-Agent Workflows
* **Learn:**
  * Supervisor
  * Worker
  * Planner
  * Executor
* **Project:** Research Assistant

### Day 20: Human-in-the-loop
* **Learn:**
  * Interrupt
  * Resume
  * Approval

### Day 21: Big Project
* **Project:** Multi-Agent Research Assistant
  * **Flow:** `Planner` $\rightarrow$ `Researcher` $\rightarrow$ `Summarizer` $\rightarrow$ `Writer`

---

## Phase 4: Week 4 (Days 22–25) — Deployment + Docker (10%)
*Only learn what's required.*

### Day 22: Docker
* **Learn:**
  * `Dockerfile`
  * `docker build`
  * `docker run`

### Day 23: Docker Compose
* **Learn:** Orchestrating multi-container apps:
  * FastAPI
  * Redis
  * Postgres

### Day 24: Deployment Basics
* **Learn:**
  * Render
  * Railway
  * VPS basics
  * Environment Variables

### Day 25: Final Deployment
* **Task:** Deploy FastAPI AI Service
  * Public API
  * GitHub repository
  * Dockerized setup
  * Live URL

---

## 🏆 Final Portfolio Projects

### 1. Multi-Agent Research Assistant ⭐⭐⭐⭐⭐
* **Skills:** LangGraph, FastAPI, Tool Calling, Memory

### 2. RAG Chatbot ⭐⭐⭐⭐⭐
* **Skills:** ChromaDB, Embeddings, Retrieval, LangChain

### 3. PDF Chatbot ⭐⭐⭐⭐
* **Features:** Upload PDF, Ask Questions, Citations

### 4. AI Agent with Tools ⭐⭐⭐⭐
* **Tools:** Weather, Calculator, Search, Wikipedia

### 5. AI Backend Service ⭐⭐⭐⭐⭐
* **Skills:** FastAPI, Authentication, Streaming Responses, Docker, Deployment

---

## ⏱️ Daily Schedule (3–4 Hours)

| Time | Task |
| :--- | :--- |
| **45 min** | Learn concepts |
| **90 min** | Build while learning |
| **30 min** | Read documentation |
| **45 min** | Improve project / refactor |
| **15 min** | Push to GitHub & write notes |

---

## 🚫 What to Ignore (For Now)
Don't spend time on these until *after* you're employable:
* ❌ Custom LangChain components
* ❌ Advanced graph optimizations
* ❌ Kubernetes
* ❌ Microservices
* ❌ Fine-tuning LLMs
* ❌ Multi-modal AI
* ❌ LangSmith advanced observability
* ❌ Complex vector database tuning
* ❌ Distributed agent architectures

---

## 📚 Resources (Learn in This Order)
1. **FastAPI:** Official FastAPI documentation and build small APIs from scratch.
2. **LangChain:** Focus on prompts, chains, document loading, embeddings, and retrieval in the official docs.
3. **LangGraph:** Work through the official tutorials on state graphs, tool calling, memory, and multi-agent workflows.
4. **Docker:** Learn Dockerfiles, images, containers, and Docker Compose through the official documentation.
5. **Deployment:** Deploy your FastAPI projects to services like Render or Railway after containerizing them.

---

## 🎯 Outcome
By the end of these 25 days, you should have:
* **5 production-style AI backend projects** on GitHub
* **Hands-on experience** with FastAPI, LangChain, LangGraph, Docker, and deployment
* **Practical experience** building RAG systems, AI agents, and multi-agent workflows
* **A competitive portfolio** that aligns well with entry-level AI Engineer or AI Application Developer roles rather than just demonstrating tutorial-level knowledge.