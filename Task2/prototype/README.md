# RAG + LLM Prototype — Technical Document Q&A System

## 📌 Project Overview

This project implements a **Retrieval-Augmented Generation (RAG) system** to query a collection of **technical documents**—PDFs, manuals, SOPs, troubleshooting guides—using **natural language**.  

It combines **semantic retrieval** with **LLM-based answer generation** to provide **accurate, reliable, and cited answers** for operational or technical queries.

**Key objectives:**
- Provide **precise answers with source citations**.
- Avoid LLM hallucinations and irrelevant outputs.
- Handle **large document collections efficiently**.
- Support **interactive Q&A** for live queries.

**Why this matters:**  
Users can quickly extract meaningful insights from large technical documentation, reducing time spent searching manuals, avoiding mistakes, and ensuring operational accuracy.

---
## 📂 Folder Structure
```
ask2/
└── prototype/
├── docs/                                           # PDF manuals and technical documents
│ └── (your PDFs go here)
├── Evaluation_Mtarics/                             # (optional for future results)
├── vector_db/                                      # Auto generated embeddings storage
├── README.md                                       # This README file
├── rag_prototype.ipynb                             # Prototype notebook with full workflow
├── requirements_task2.txt                          # Python dependencies
├── architecture_diagram.pptx
└── notes.md                                        # System design notes and explanations
```

---

## 🏗️ System Architecture (Summary)

The system consists of the following components:  
1. **Document Ingestion & Preprocessing** → Load PDFs, extract text, clean formatting  
2. **Chunking Strategy** → Sentence-level chunks (~1–2 sentences per chunk, no overlap)  
3. **Embedding & Indexing** → Encode with `all-MiniLM-L6-v2`, store in Chroma DB  
4. **Retrieval Layer** → Top-K semantic search (default K=5)  
5. **LLM Layer** → Answer generation using retrieved context (Flan-T5)  
6. **Guardrails** → Source enforcement, sensitive query filter, fallback handling  

📊 Visual reference: See `architecture_diagram.pptx`

## 🔍 Retrieval Strategy

- **Chunking**: Sentence-level granularity for precision (no overlap)  
- **Embedding**: `all-MiniLM-L6-v2` (fast, lightweight, free)  
- **Retrieval Method**: Dense vector search with Chroma (top-K=5)  
- **Optional Extensions**: Hybrid retrieval (BM25 + semantic), reranking with cross-encoders  

---

## 🛡️ Guardrails & Failure Modes

- **No Relevant Answers** → Return fallback: *"No relevant information found in the documents."*  
- **Hallucinations** → Every generated answer must include source citations  
- **Sensitive Queries** → Block or filter inappropriate/sensitive queries  
- **Monitoring Metrics** → Track `precision@k`, `recall@k`, `faithfulness`  

---

## ⚖️ Scalability Considerations

- **10x more documents** → Use FAISS/Milvus/Weaviate in distributed mode  
- **100+ concurrent users** → Scale with async APIs + caching (e.g., FastAPI, Redis)  
- **Cloud Deployment** → Serverless or GPU-efficient scaling, quantized LLMs for cost control  

---


## ⚡ Key Features

### Sentence-Level Retrieval
- Splits documents into **independent sentences** for fine-grained semantic search
- Returns only the most **relevant sentences** to each user query
- Each result cites its source: `[Source: filename#sentence_id]`

### Embedding & Vector Database
- Encodes each sentence using `all-MiniLM-L6-v2` for semantic understanding
- Stores sentence embeddings in **Chroma** (easy to swap for FAISS/Milvus/Weaviate)
- Retrieves top-K sentences by vector similarity, ensuring high relevance

### LLM Answer Generation
- Generates answers using **only** the retrieved context to avoid hallucinations
- Produces clear, readable text with source references
- Built with `flan-t5-small` for speed and efficiency; easily upgradable for larger LLMs

### Guardrails & Reliability
- Returns fallback messages if no relevant information is found
- Uses a similarity threshold and sensitive-content filter for safe, relevant results
- Allows tracking of **precision@k**, **recall@k**, and **faithfulness** for quality monitoring

### Visual & Interactive Demo
- Upload and process multiple technical documents
- Interactive query-and-answer in real time, with citations
- **Portfolio Standout:** Robustly handles complex document Q&A with an intuitive notebook interface

---

## 🛠️ Setup Instructions

1. **Clone the repository**
    git clone <your-repo-url>
    ```
    cd Task2/prototype
    ```

3. **Upgrade pip**
    ```
    pip install --upgrade pip
    ```

4. **Install requirements**
    ```
    pip install -r task2/prototype/requirements_task2.txt
    ```

5. **Add your technical documents**
    - Place PDFs, manuals, or guides in `docs/`

6. **Run the prototype**
    - **Jupyter notebook**
        ```
        jupyter notebook rag_prototype.ipynb
        ```
    - **Python script** (if available)
        ```
        python rag_prototype.py
        ```

7. **Test a query**

    - Auto query: What are the safety precautions and features of cyclone?
   
    - if u want ask other questions related to the project like what is cyclone? before it enable last code to ask question conyineously in .ipynb file 
    - 📝 Note: The Chroma vector database is automatically created the first time you run a query.

---

## 📊 System Outputs

| Output              | Description                                         |
|---------------------|-----------------------------------------------------|
| vector_db/          | Stores sentence embeddings for fast semantic search |
| outputs/            | (Optional) Logs, metrics, and exportable results    |
| Interactive Answers | Human-readable Q&A with `[Source: filename#id]`     |
| Notes               | Precision/recall/faithfulness metrics if enabled    |

---

## 🔮 Future Improvements

- Integrate stronger LLMs (LLaMA 2, MPT, Falcon, Vicuna)
- Add cross-encoder reranking for better sentence relevance
- Implement hybrid retrieval (BM25 + semantic)
- Prepare for enterprise deployment (distributed vector DB, scalable inference)
- Enhance the UI for more advanced document interaction

---

## 📞 Support & Contributions

For issues, feature requests, or contributions, please open an issue or pull request in this repository.

---

*This prototype demonstrates a scalable, precise, and robust RAG system for querying technical documentation—ideal for portfolio projects, research, and real-world applications.*
