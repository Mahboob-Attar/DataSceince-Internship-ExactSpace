# 📌 Internship Project – Dual Tasks (Data Science + ML + NLP)

This repository contains **two completely independent tasks** completed as part of the internship.  
Both tasks showcase different areas of applied AI/ML — one in **time-series machine data analysis** and the other in **LLM-based information retrieval**.

---

##  Important Note

✅ **Task 1 (Machine Data Analysis)** and **Task 2 (RAG + LLM)** are **completely separate**.  

- **Task 1** = Time-series sensor analysis (**shutdown detection, clustering, anomalies, forecasting**)  
- **Task 2** = Information retrieval + LLM answering system (**working with PDFs, embeddings, RAG pipeline**)  

There is **no direct dependency** between them:  

- Task 1 is **data science / ML on machine sensor data**  
- Task 2 is **NLP / system design for document search & Q&A**  

Both tasks together demonstrate **end-to-end applied AI skills** in **Data Science + NLP/LLMs**.

---

# 🏭 Task 1: Machine Data Analysis – Cyclone Sensor Data

## 📌 Overview
This task performs a **comprehensive analysis of 3 years of cyclone machine sensor data** (~370,000 records at 5-minute intervals).  
The main goal is to extract insights, detect unusual behavior, and forecast future states.

## 🔑 Key Objectives
- **Data Exploration** – Understand sensor distributions, detect missing values.  
- **Shutdown / Idle Period Detection** – When most sensors fall below their 5th percentile values.  
- **Clustering Analysis** – Group sensor behavior patterns using K-Means.  
- **Anomaly Detection** – Identify abnormal sensor readings.  
- **Forecasting** – Compare **Persistence Baseline vs ARIMA** models.  

## 📂 Files
- `task1_analysis.ipynb` → Jupyter Notebook with complete analysis  
- `cyclone_data.csv` → Dataset (370k records, 5-min intervals, multiple sensors)
---

  🤖 Task 2 – RAG + LLM System Design
📌 Overview

Design and prototype a Retrieval-Augmented Generation (RAG) system that allows operators to query 50+ technical PDFs (manuals, SOPs, troubleshooting guides) in natural language.
The system provides reliable, cited answers with guardrails against hallucinations.

🛠 Workflow

Document Ingestion & Preprocessing → Extract + clean text from PDFs.

Chunking → Split text into 500-token segments with overlap.

Embeddings + Vector Indexing → Use Hugging Face embedding model (all-MiniLM-L6-v2) with FAISS/Chroma.

Retrieval Layer → Semantic search to fetch top-k relevant chunks.

LLM Answering → Open-source LLM (Flan-T5, Llama-2, etc.) generates answer with citations.

Guardrails → fallback for no-answer, prevent hallucinations, filter sensitive queries.

Scalability → handle 10x docs & 100+ users via cloud deployment.

📂 Deliverables

architecture_diagram.pptx (visual system flow).

notes.md (design trade-offs, retrieval strategy, scaling).

prototype/ folder:

rag_prototype.py or rag_demo.ipynb

docs/ → sample PDFs

README.md with setup instructions

---

✅ Final Takeaway

This project highlights two separate but complementary AI/ML capabilities:

🏭 Task 1 → Industrial time-series data science (shutdown detection, clustering, anomalies, forecasting).

📚 Task 2 → Modern NLP/LLM pipeline with RAG for document-based Q&A.

Together, they showcase the ability to handle both data-heavy machine learning and advanced NLP system design in real-world applications.
