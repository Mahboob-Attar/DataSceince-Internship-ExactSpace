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
ask2/
└── prototype/
├── docs/                                           # PDF manuals and technical documents
│ └── (your PDFs go here)
├── outputs/                                        # (optional for future results)
├── vector_db/                                      # generated embeddings storage
├── README.md                                       # This README file
├── rag_prototype.ipynb                             # Prototype notebook with full workflow
├── requirements_task2.txt                          # Python dependencies
├── architecture_diagram.pptx
└── notes.md                                        # System design notes and explanations


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
    pip install -r requirements_task2.txt
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
    ```
    What are the safety precautions and features of cyclone?
    ```
    - Answers will include cited bullet-pointed results if u want ask other wuestions related to the project lie what is cyclone?

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
