Thesis: Application of Artificial Intelligence Methods in the Requirements Engineering Management Process
🔹 Overview

This repository supports my MSc thesis:
“Application of Artificial Intelligence Methods in the Requirements Engineering Management Process.”
The research explores how AI techniques — particularly NLP, transformer-based models, and graph-based representations — can enhance the extraction, classification, and management of requirements.
The core contribution is an end-to-end pipeline that transforms raw elicited requirements into a semantic requirement graph, enabling automation, traceability, and explainability in the Requirements Engineering Management Process (REMP).

🔹 End-to-End Process

Input: Raw elicited requirement sentences (unstructured text)
Output: Structured, classified, and explainable requirement graphs (Neo4j)

📌 Pipeline Steps

Preprocessing – text normalization and cleaning.
Requirement Extraction (Dual NER) – spaCy + BERT/RemBERT extract Actor, Goal, Rationale (AGR).
Completeness & Confidence – slot-level evaluation + weighted confidence scoring.
Graph Construction – AGR triplets stored in a Neo4j semantic graph.
Classification – Functional vs Non-Functional; Multi-aspect NFR classification using fine-tuned BERT.
Explainability – SHAP/LIME highlights key tokens influencing classification.
Validation & Feedback Loop – low-confidence cases flagged for manual review.

🔹 Datasets Used

PROMISE NFR Dataset – Functional vs Non-Functional requirements.
NoRBERT (PROMISE variant) – Multi-label NFR dimensions.
REMBERT-AGR Dataset – Actor, Goal, Rationale triplets.
Custom Dataset – manually curated requirements for evaluation.

🔹 Tools & Frameworks

Python (Google Colab)
spaCy + Transformers (NER & preprocessing)
Hugging Face BERT/RemBERT (fine-tuned models)
Neo4j (semantic graph construction)
SHAP / LIME (explainability layer) 
