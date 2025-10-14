# Thesis: Application of Artificial Intelligence Methods in the Requirements Engineering Management Process

## Overview

The research explores how AI techniques — particularly NLP, transformer-based models, and graph-based representations — can enhance the extraction, classification, and management of requirements.
The core contribution is an end-to-end pipeline that transforms raw elicited requirements into a semantic requirement graph, enabling automation, traceability, and explainability in the Requirements Engineering Management Process (REMP).

## End-to-End Process

Input: Raw elicited requirement sentences (unstructured text)

Output: Structured, classified, and explainable requirement graphs (Neo4j)
<img width="1124" height="757" alt="image" src="https://github.com/user-attachments/assets/e2135b7f-85f2-4a2c-9ea1-ea5dd00970ac" />


##  Pipeline Steps

Preprocessing – text normalization and cleaning.

Requirement Extraction (Dual NER) – spaCy + BERT/RemBERT extract Actor, Goal, Rationale (AGR).

Completeness & Confidence – slot-level evaluation + weighted confidence scoring.

Graph Construction – AGR triplets stored in a Neo4j semantic graph.

Classification – Functional vs Non-Functional; Multi-aspect NFR classification using fine-tuned BERT.

Explainability – SHAP/LIME highlights key tokens influencing classification.

Validation & Feedback Loop – low-confidence cases flagged for manual review.

## Examples
# AI-Driven Requirements Engineering
## Input → Output Examples

---

## Example 1

**INPUT:**
```
"As a user, I want to login quickly so that I can access my account"
```

**OUTPUT:**
```
REQ_001 {
  Actor: "user"
  Goal: "login quickly"
  Rationale: "access my account"
  Type: Non-Functional (NFR)
  Subtype: Performance
  Confidence: 0.92
}
```

---

## Example 2

**INPUT:**
```
"The system shall encrypt all user data using AES-256 encryption"
```

**OUTPUT:**
```
REQ_002 {
  Actor: "system"
  Goal: "encrypt all user data"
  Rationale: "using AES-256 encryption"
  Type: Non-Functional (NFR)
  Subtype: Security
  Confidence: 0.96
}
```

---

## Example 3

**INPUT:**
```
"As an admin, I want to delete user accounts so that I can manage the database"
```

**OUTPUT:**
```
REQ_003 {
  Actor: "admin"
  Goal: "delete user accounts"
  Rationale: "manage the database"
  Type: Functional (FR)
  Subtype: N/A
  Confidence: 0.89
}
```

---

## Example 4

**INPUT:**
```
"The system must respond to user requests within 2 seconds"
```

**OUTPUT:**
```
REQ_004 {
  Actor: "system"
  Goal: "respond to user requests"
  Rationale: "within 2 seconds"
  Type: Non-Functional (NFR)
  Subtype: Performance
  Confidence: 0.94
}
```

---

## Example 5

**INPUT:**
```
"As a customer, I want to view my order history so that I can track purchases"
```

**OUTPUT:**
```
REQ_005 {
  Actor: "customer"
  Goal: "view order history"
  Rationale: "track purchases"
  Type: Functional (FR)
  Subtype: N/A
  Confidence: 0.91
}
```

---

## Summary

| # | INPUT (Raw Text) | OUTPUT (Structured) |
|---|------------------|---------------------|
| 1 | "As a user, I want to login quickly..." | Actor: user \| Goal: login quickly \| Type: NFR-Performance \| Conf: 0.92 |
| 2 | "The system shall encrypt all user data..." | Actor: system \| Goal: encrypt data \| Type: NFR-Security \| Conf: 0.96 |
| 3 | "As an admin, I want to delete user accounts..." | Actor: admin \| Goal: delete accounts \| Type: FR \| Conf: 0.89 |
| 4 | "The system must respond within 2 seconds..." | Actor: system \| Goal: respond quickly \| Type: NFR-Performance \| Conf: 0.94 |
| 5 | "As a customer, I want to view order history..." | Actor: customer \| Goal: view history \| Type: FR \| Conf: 0.91 |

**System Performance:** 88% Extraction Accuracy | 92% Classification Accuracy | 85% Automation

## Datasets Used

PROMISE NFR Dataset – Functional vs Non-Functional requirements.

NoRBERT (PROMISE variant) – Multi-label NFR dimensions.

REMBERT-AGR Dataset – Actor, Goal, Rationale triplets.

Custom Dataset – manually curated requirements for evaluation.

## Tools & Frameworks

Python (Google Colab)

spaCy + Transformers (NER & preprocessing)

Hugging Face BERT/RemBERT (fine-tuned models)

Neo4j (semantic graph construction)

SHAP / LIME (explainability layer) 
