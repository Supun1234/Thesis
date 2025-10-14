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

## Example 1

### X (INPUT):
```
"As a user, I want to login quickly so that I can access my account"
```

### BLACK BOX PROCESSING:
```
```

### Y (OUTPUT):
```
Requirement ID: REQ_001

Structured Components:
├─ Actor: "user"
├─ Goal: "login quickly"
└─ Rationale: "access my account"

Classification:
├─ Type: Non-Functional (NFR)
├─ Subtype: Performance
└─ Confidence: 0.92

Explainability (SHAP):
└─ Key tokens: "quickly" (0.31), "login" (0.24), "access" (0.18)

Neo4j Graph Structure:
(REQ_001:Requirement)-[:HAS_ACTOR]→(Actor {name: "user"})
(REQ_001:Requirement)-[:HAS_GOAL]→(Goal {text: "login quickly"})
(REQ_001:Requirement)-[:HAS_RATIONALE]→(Rationale {text: "access my account"})
(REQ_001:Requirement)-[:CLASSIFIED_AS]→(Performance {confidence: 0.92})

Status: ✓ Validated and stored in Neo4j
```

---

## Example 2

### X (INPUT):
```
"The system shall encrypt all user data using AES-256 encryption"
```

### BLACK BOX PROCESSING:
```
```

### Y (OUTPUT):
```
Requirement ID: REQ_002

Structured Components:
├─ Actor: "system"
├─ Goal: "encrypt all user data"
└─ Rationale: "using AES-256 encryption"

Classification:
├─ Type: Non-Functional (NFR)
├─ Subtype: Security
└─ Confidence: 0.96

Explainability (SHAP):
└─ Key tokens: "encrypt" (0.38), "AES-256" (0.29), "data" (0.22)

Neo4j Graph Structure:
(REQ_002:Requirement)-[:HAS_ACTOR]→(Actor {name: "system"})
(REQ_002:Requirement)-[:HAS_GOAL]→(Goal {text: "encrypt all user data"})
(REQ_002:Requirement)-[:HAS_RATIONALE]→(Rationale {text: "using AES-256 encryption"})
(REQ_002:Requirement)-[:CLASSIFIED_AS]→(Security {confidence: 0.96})

Status: ✓ Validated and stored in Neo4j
```

---

## Example 3

### X (INPUT):
```
"As an admin, I want to delete user accounts so that I can manage the database"
```

### BLACK BOX PROCESSING:
```
```

### Y (OUTPUT):
```
Requirement ID: REQ_003

Structured Components:
├─ Actor: "admin"
├─ Goal: "delete user accounts"
└─ Rationale: "manage the database"

Classification:
├─ Type: Functional (FR)
├─ Subtype: N/A (Functional requirement)
└─ Confidence: 0.89

Explainability (SHAP):
└─ Key tokens: "delete" (0.35), "accounts" (0.27), "admin" (0.19)

Neo4j Graph Structure:
(REQ_003:Requirement)-[:HAS_ACTOR]→(Actor {name: "admin"})
(REQ_003:Requirement)-[:HAS_GOAL]→(Goal {text: "delete user accounts"})
(REQ_003:Requirement)-[:HAS_RATIONALE]→(Rationale {text: "manage the database"})
(REQ_003:Requirement)-[:CLASSIFIED_AS]→(Functional {confidence: 0.89})

Status: ✓ Validated and stored in Neo4j
```

---

## Example 4

### X (INPUT):
```
"The system must respond to user requests within 2 seconds"
```

### BLACK BOX PROCESSING:
```
```

### Y (OUTPUT):
```
Requirement ID: REQ_004

Structured Components:
├─ Actor: "system"
├─ Goal: "respond to user requests"
└─ Rationale: "within 2 seconds"

Classification:
├─ Type: Non-Functional (NFR)
├─ Subtype: Performance
└─ Confidence: 0.94

Explainability (SHAP):
└─ Key tokens: "2 seconds" (0.42), "respond" (0.31), "requests" (0.18)

Neo4j Graph Structure:
(REQ_004:Requirement)-[:HAS_ACTOR]→(Actor {name: "system"})
(REQ_004:Requirement)-[:HAS_GOAL]→(Goal {text: "respond to user requests"})
(REQ_004:Requirement)-[:HAS_RATIONALE]→(Rationale {text: "within 2 seconds"})
(REQ_004:Requirement)-[:CLASSIFIED_AS]→(Performance {confidence: 0.94})

Status: ✓ Validated and stored in Neo4j
```

---

## Example 5

### X (INPUT):
```
"As a customer, I want to view my order history so that I can track purchases"
```

### BLACK BOX PROCESSING:
```
```

### Y (OUTPUT):
```
Requirement ID: REQ_005

Structured Components:
├─ Actor: "customer"
├─ Goal: "view order history"
└─ Rationale: "track purchases"

Classification:
├─ Type: Functional (FR)
├─ Subtype: N/A (Functional requirement)
└─ Confidence: 0.91

Explainability (SHAP):
└─ Key tokens: "view" (0.33), "order history" (0.28), "track" (0.21)

Neo4j Graph Structure:
(REQ_005:Requirement)-[:HAS_ACTOR]→(Actor {name: "customer"})
(REQ_005:Requirement)-[:HAS_GOAL]→(Goal {text: "view order history"})
(REQ_005:Requirement)-[:HAS_RATIONALE]→(Rationale {text: "track purchases"})
(REQ_005:Requirement)-[:CLASSIFIED_AS]→(Functional {confidence: 0.91})

Status: ✓ Validated and stored in Neo4j
```

---
---


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
