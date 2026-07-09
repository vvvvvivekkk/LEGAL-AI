---
title: LEGAL-AI
emoji: "⚖️"
colorFrom: slate
colorTo: indigo
sdk: docker
app_port: 7860
pinned: true
author: vvvvvivekkk
---

[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://www.apache.org/licenses/LICENSE-2.0)
LEGAL-AI is an enterprise-grade AI platform for legal research, statutory search, and intelligent document analysis.

The platform combines Retrieval-Augmented Generation (RAG), Hybrid Search, BM25, Dense Retrieval, Vector Databases, and Large Language Models to provide accurate, explainable, and citation-backed legal assistance.

Designed with production-ready architecture, LEGAL-AI supports secure document ingestion, semantic search, hybrid retrieval, and scalable deployment for legal professionals, researchers, enterprises, and educational institutions.

- Ingests legal text and PDFs into structured chunks.
- Builds dense, sparse, and optional ColBERT indexes.
- Uses a router to choose between direct retrieval and graph-assisted retrieval.
- Surfaces citations so answers remain auditable.
- Exposes a browser UI and REST API for interactive use.

## Architecture

LEGAL-AI is organized into four layers:

1. Offline build
   Preprocess source text, build indexes, and generate graph artifacts.
2. Read models
   Serve FAISS, BM25, and graph artifacts as immutable retrieval inputs.
3. Online ingestion
   Parse uploaded PDFs and update the indexes incrementally.
4. Online serving
   Route the query, retrieve evidence, and generate a grounded response.

See [docs/architecture.mmd](docs/architecture.mmd) for the high-level flow.

## Quickstart

```bash
git clone https://github.com/vvvvvivekkk/LEGAL-AI.git
cd LEGAL-AI
pip install -r requirements.txt
```

Build the default corpora and indexes:

```bash
python -m scripts.preprocess_law
python -m scripts.build_index
python -m scripts.build_graph
```

Start the API and UI:

```bash
python -m uvicorn legalrag.api.server:app --host 127.0.0.1 --port 8000
```

Then open http://127.0.0.1:8000/ or http://127.0.0.1:8000/ui/.

## Example

```python
from legalrag.config import AppConfig
from legalrag.pipeline.rag_pipeline import RagPipeline

cfg = AppConfig.load()
pipeline = RagPipeline(cfg)

question = "What standards must goods satisfy to be merchantable?"
answer = pipeline.answer(question)

print(answer.answer)
```

## Configuration

- `OPENAI_API_KEY` enables OpenAI-compatible generation when no local GPU model is selected.
- `QWEN_MODEL` and `OPENAI_MODEL` override the default model names.
- `LEGALAI_INDEX_VERSION` selects a named index version when multiple builds exist.

## Repository Layout

```text
LEGAL-AI/
├── legalrag/
├── scripts/
├── notebooks/
├── data/
├── docs/
├── ui/
├── tests/
├── README.md
├── README-zh.md
├── pyproject.toml
├── requirements.txt
├── Dockerfile
└── docker-compose.yml
```

## Notes

- The Python import path remains `legalrag` for compatibility with the existing codebase.
- The repository does not ship model weights or hosted demos.
- LEGAL-AI is intended for research and information assistance, not legal advice.

## License

Apache-2.0. See [LICENSE](LICENSE) for the full text.
"# LEGAL-AI" 
