# RAG Architecture Patterns — Sprint 2 Reference

**Purpose:** Reference material for Sprint 2 planning
**Target Outcome:** Integrate a RAG pattern with the flower classification project
**Status:** Captured, pending brainstorm after Sprint 1
**Source:** [jamwithai.dev](https://www.jamwithai.dev/) diagrams

---

## Four RAG Patterns

### 1. Traditional RAG
User Query → Generate Embeddings (map to vector space) → Similarity Search against Vector DB (stored chunks) → LLM & Context Synthesis (construct final prompt) → Final Response

### 2. Vectorless RAG
User Query → Analyze & Parse Query (extract search terms) → Keyword Retrieval against Keyword Index (full-text records) → LLM & Context Synthesis (construct final prompt) → Final Response

### 3. Graph RAG (Knowledge Graph)
User Query → Entity & Relation Extraction → Query Knowledge Graph (entities, relationships, attributes) → Traverse Graph & Extract Sub-graph → LLM + Graph Context → Structured Response

### 4. Agentic RAG
User Query → Agent Analysis & Planning (formulate plan & sub-queries) → Execute Retrieval & Tool Use (vector DB, web search, APIs) → Agent Validation & Synthesis → Enriched Response
- Feedback loop: inadequate results → refine strategy → back to planning

---

## Open Questions for Sprint 2 Brainstorm
- Which pattern best complements a flower classification model?
- Possible directions: flower knowledge base (botanical info per species), visual search + retrieval, multi-modal RAG
- How does the classified flower feed into a retrieval pipeline?
- Which pattern is most portfolio-impressive while being feasible?
**Outcome:** Not pursued, project finalized after Sprint 1, Sprint 2 will not take place (archived 2026-04-07).
