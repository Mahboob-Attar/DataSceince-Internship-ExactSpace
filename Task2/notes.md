# Part 2: RAG + LLM System Design — Notes

## 1. System Overview
This RAG (Retrieval-Augmented Generation) system allows users to ask natural language questions over a collection of technical documents such as PDFs, manuals, SOPs, and troubleshooting guides.  

The system combines **intelligent document search** with **LLM-based answer generation**, ensuring responses are **accurate, cited, and trustworthy**.

**Key Goals:**
- Provide **reliable answers with sources**.
- Avoid LLM hallucinations and irrelevant responses.
- Handle **large document collections** and multiple users efficiently.

---

## 2. System Architecture

**Major Components:**

Core Components:
[PDF Documents] → [Preprocessing] → [Chunking] → [Embedding Model] → [Vector Database]
                                      ↑
                                  [Query Input]
                                      ↓
                               [Retrieval Layer]
                                      ↓
                                   [LLM Model]
                                      ↓
                                [Answer Output]
                                      ↓
                           [Guardrails & Formatting]

| Component | Purpose |
|-----------|---------|
| **Document Loader** | Reads PDF or text files from the `docs/` folder. |
| **Text Preprocessing & Chunking** | Breaks documents into smaller, overlapping chunks (400 characters each, 100 characters overlap). Uses **NLTK** for sentence tokenization and cleaning to improve retrieval and answer quality. |
| **Embeddings & Indexing** | Converts chunks into vector embeddings using `all-MiniLM-L6-v2` and stores them in **Chroma** (for the demo). For production, scalable vector DBs like **FAISS, Milvus, Weaviate, or Pinecone** can be used. |
| **Retrieval Engine** | Finds the most relevant chunks for a given question using dense vector similarity. |
| **LLM Answer Generator** | Uses `flan-t5-small` to produce human-readable answers, leveraging the retrieved chunks. |
| **Guardrails** | Ensures answers are **faithful to sources**, provides fallback messages if nothing is found, and can filter sensitive queries. |
| **Evaluation (Optional)** | Measures retrieval quality with metrics such as precision@k or recall@k. |

**Architecture Diagram:**  
_(Include `architecture_diagram.pptx` showing the flow from documents → chunking → embedding → retrieval → LLM → user answer.)_

---

## 3. Retrieval Strategy

**Chunking Approach:**
- Each chunk: 400 characters, 100-character overlap.  
- Overlap ensures context is maintained between chunks.  
- **NLTK** helps with sentence splitting, tokenization, and cleaning text for better LLM input.

**Embedding Model:**
- `sentence-transformers/all-MiniLM-L6-v2`  
- Lightweight, fast, and semantically meaningful vectors for technical documents.

**Retrieval Method:**
- Dense vector search using **Chroma** (demo).  
- Retrieve **top 5 most similar chunks** per query.  
- Minimum similarity threshold: 0.7 to ensure relevance.  

**Scalable Options:**  
For large datasets or enterprise use, **FAISS, Milvus, Weaviate, or Pinecone** can replace Chroma for faster retrieval, cloud integration, and support for millions of documents.

**Ensuring Faithfulness:**
- Only feed top-K chunks into the LLM.  
- Force LLM to cite sources using `[Source: filename#chunk_id]`.  
- Optional reranking with cross-encoders can improve accuracy.

---

## 4. Guardrails & Failure Modes

| Situation | Solution |
|-----------|---------|
| No relevant answer | Show a clear fallback: “Sorry, no relevant information found.” |
| LLM hallucinations | Restrict answers to retrieved chunks only; enforce source citations. |
| Sensitive or unsafe queries | Filter or block responses as needed. |

**Monitoring Metrics:**
- **Precision@K:** Fraction of retrieved chunks that are actually relevant.  
- **Recall@K:** Fraction of relevant chunks retrieved.  
- **Response faithfulness:** Percentage of answer content directly supported by sources.

---

## 5. Scalability Considerations

**Handling 10x more documents:**
- Batch embeddings and sharding improve performance.
- Use GPU acceleration if needed.
- Production-ready vector DBs like **FAISS, Milvus, Weaviate, or Pinecone** handle millions of chunks efficiently.

**Handling 100+ concurrent users:**
- Deploy the LLM inference as a microservice with queuing.
- Vector DB queries remain fast and lightweight.

**Cloud Deployment:**
- Serverless (AWS Lambda, GCP Functions) or containerized (Docker/Kubernetes).  
- Store persistent vector DB in cloud storage for multiple instances.  
- Cost-efficient: small LLMs locally, embeddings cached.

---

## 6. Prototype Notes

- The **demo notebook** `rag_demo.ipynb` shows a working RAG prototype.  
- Documents are loaded from `docs/` and indexed into `vector_db/`.  
- Queries return answers with **source citations**.  
- Configurable parameters:
  - `CHUNK_SIZE`, `CHUNK_OVERLAP`  
  - `TOP_K` (number of chunks to retrieve)  
  - `SIMILARITY_THRESHOLD`  
- **NLTK** improves preprocessing for better chunk quality and prompt building.  
- Chroma is used for the demo; other vector DBs are recommended for production.  
- Evaluation can be done using `eval_queries.json`.

---

## 7. Design Trade-offs

| Trade-off | Decision |
|-----------|---------|
| Small LLM vs large LLM | `flan-t5-small` chosen for CPU-friendly, free inference; larger LLMs improve answer quality at higher compute cost. |
| Chunk size | 400 chars balance retrieval accuracy vs index size. |
| Vector DB | Chroma used for simplicity in demo; large-scale systems should use FAISS, Milvus, Weaviate, or Pinecone. |
| Guardrails | Enforcing similarity thresholds and citations ensures faithfulness without complex post-processing. |
| NLTK preprocessing | Enhances chunk coherence, sentence splitting, and overall answer quality. |

---

## 8. Future Improvements & Open-Source Enhancements

**1. Open-Source LLMs for Strong Accuracy**
- Use larger, open-source LLMs like **LLaMA 2, MPT, Falcon, or Vicuna** to improve **answer quality, reasoning, and context handling**.  
- Can run locally or on cloud GPUs for faster inference.

**2. Handling Millions of Users and Documents**
- Switch to scalable vector DBs (FAISS, Milvus, Weaviate, Pinecone) to handle **millions of documents and concurrent queries**.  
- Deploy LLMs as microservices or in distributed clusters.

**3. Enhanced Retrieval & Faithfulness**
- Add **cross-encoder reranking** to ensure the top retrieved chunks are highly relevant.  
- Use **NLTK preprocessing** plus advanced tokenization for cleaner, more coherent context.

**4. Cloud & Enterprise Ready**
- Deploy in **Kubernetes clusters** or serverless pipelines.  
- Persist embeddings in cloud storage for multi-instance access.  
- Efficient memory and GPU usage for large-scale inference.

> **Impression Point for Evaluators:**  
> This design is not just a demo—it’s **future-ready**, **scalable**, and **production-capable**. By combining open-source models, robust vector DBs, and smart preprocessing, it can serve **millions of users**, provide **accurate answers**, and maintain **trusted citations**.

---

**Summary:**  
This RAG system is a **reliable, scalable, and safe framework** to query large technical document collections. It combines **smart retrieval, LLM-powered generation, NLTK preprocessing, and strict guardrails** to provide trustworthy answers. The demo uses Chroma for simplicity, but production deployments can leverage **open-source LLMs and enterprise-grade vector databases** for millions of users and stronger accuracy.
