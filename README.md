# 🌋 Hybrid GraphRAG on a Volcanology PhD Thesis

> Final-year academic project (PFA) – **EMSI**, Data & Artificial Intelligence Engineering, 2025–2026
> Authors: **Khadija Boumaji** & **Inas Mourid** · Supervisor: Dr. El Mkhalet Mouna

A hybrid **Retrieval-Augmented Generation (RAG)** system that turns a 400+ page doctoral thesis on the Piton des Neiges volcano (Réunion Island) into a knowledge base you can question in natural language, by combining **semantic vector search** and a **knowledge graph**.

## ✨ Key results

| Step | Result |
|---|---|
| Chunking (7 methods compared) | Sliding Window selected – best intra-similarity 0.511 |
| Retrieval (6 methods compared) | BM25 selected – best global score 0.660 |
| Knowledge graph (Neo4j Aura) | 1,016 entities · 5,200 relationships |
| Community detection (Louvain) | 26 communities · modularity 0.906 |
| Graph + Vector fusion | Final fusion score **0.87** (α = 0.60) |

## 🏗️ Architecture

```
Thesis PDF ──► Cleaning ──► Chunking ──► Embeddings (all-mpnet-base-v2) ──► Vector store
                                   └───► Entity & relation extraction ──► Neo4j graph ──► Louvain communities
                                                         │
User question ──► Q-Learning agent (routes: vector / graph / hybrid) ──► Fusion ──► LLM (Llama 3.3 70B via Groq) ──► Answer
```

## 🛠️ Tech stack

Python · FastAPI · Sentence-Transformers · FAISS · BM25 / TF-IDF · Neo4j Aura · NetworkX · Louvain · Q-Learning · Groq (Llama 3.3) · HTML/JS frontend · Docker

## 📂 Pipeline

| File | Role |
|---|---|
| `step1_chunking.py` | Compares 7 chunking strategies |
| `step2_embedding_retrieval.py` | Embeddings + comparison of 6 retrieval methods |
| `step3_graph_rag.py` | Builds the knowledge graph in Neo4j |
| `step4_louvain_neo4j.py` / `step4b_extract_relations.py` | Relations + Louvain community detection |
| `step5_agentic_rag.py` | Q-Learning agent for query routing |
| `backend.py` | FastAPI API (`/query`, `/vectorial`, `/graph`, `/agentic`, `/graph/louvain`) |
| `frontend.html` | Interactive UI: graph, communities, agent decision paths |

## 🚀 Run locally

```bash
pip install -r requirements.txt
cp .env.example .env        # then add your Neo4j and Groq credentials
uvicorn backend:app --reload --port 8000
```

Then open http://localhost:8000/frontend.html

## 📄 Source corpus

C. Paquez (2021), *Structure interne d'un volcan basaltique (Le Piton des Neiges, Île de La Réunion), pour l'évaluation de son potentiel géothermique*, Université de La Réunion – HAL tel-03934766.

## 📑 Full report

The complete project report (72 pages) is available in [`report.pdf`](report.pdf).
