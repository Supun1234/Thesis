# Requirements Engineering Management AI Agent
## End-to-End Prompting Examples

### System Conceptualization
AI Agent Role: Requirements Engineering Management Processor (REMP-AI)
Purpose: An intelligent agent that transforms raw, unstructured requirement statements into structured, classified, and explainable requirement graphs with full traceability.
#### Processing Pipeline:

1. Input Reception → Parse raw requirement text
2. Data Understanding → Identify requirement structure and semantics
3. Entity Extraction → Extract Actor, Goal, and Rationale (AGR triplets)
4. Classification → Determine Functional vs Non-Functional type and subtypes
5. Confidence Assessment → Calculate prediction confidence scores
6. Explainability → Identify key tokens influencing classification
7. Graph Construction → Generate Neo4j semantic graph structure
8. Output Generation → Return structured, validated requirement specification


## PROMPT 1: Zero-Shot Example (Simple)
### User Input:
---
You are REMP-AI, an expert Requirements Engineering Management agent. Your task is to process raw requirement statements and transform them into structured, classified requirement specifications.

Given the following requirement statement X, perform end-to-end analysis and generate output Y:

INPUT (X):
"As a mobile app user, I want the app to work offline so that I can access my data without internet connection"

TASK:
Analyze this requirement and provide:
1. Requirement ID
2. Structured Components (Actor, Goal, Rationale)
3. Classification (FR/NFR with subtype)
4. Confidence Score
5. Explainability (key tokens with influence scores)
6. Neo4j Graph Structure
7. Validation Status
Generate the complete structured output Y.
---

### AI Agent Output (Y):
---
Here is the end-to-end analysis and generated output:

1. Requirement ID

REQ-20251027-001

This ID is generated using the format REQ-YYYYMMDD-NNN, ensuring uniqueness and chronological order.

2. Structured Components

Actor: Mobile app user

Goal: I want the app to work offline.

Rationale: So that I can access my data without an internet connection.

3. Classification

Type: Non-Functional Requirement (NFR)

Subtype: Availability/Reliability

This requirement describes a quality attribute of the system (how it should be) rather than a specific function (what it should do). It specifically relates to the system's ability to be operational and accessible under certain conditions.

4. Confidence Score

85%

The confidence score represents the certainty in the accuracy and completeness of the requirement. A score of 85% indicates a high level of confidence, as the requirement is well-stated and clear. However, it is not 100% because it lacks specific details about which data and functionalities should be available offline.

5. Explainability

mobile app user (Actor): 0.85 - This token clearly defines who will interact with the system.

work offline (Goal): 0.95 - This is the core desired behavior of the system.

access my data (Rationale): 0.90 - This clarifies the primary purpose of the offline capability.

without internet connection (Constraint): 0.92 - This specifies the condition under which the offline functionality is required.

These influence scores, on a scale of 0 to 1, highlight the key terms that contribute most to the requirement's meaning and intent.

6. Neo4j Graph Structure

Nodes:

(r:Requirement {id: 'REQ-20251027-001', text: 'As a mobile app user, I want the app to work offline so that I can access my data without internet connection'})

(a:Actor {name: 'Mobile app user'})

(g:Goal {text: 'The app to work offline'})

(rt:Rationale {text: 'Access data without internet connection'})

(c:Classification {type: 'NFR', subtype: 'Availability'})

Relationships:

(r)-[:HAS_ACTOR]->(a)

(r)-[:HAS_GOAL]->(g)

(r)-[:HAS_RATIONALE]->(rt)

(r)-[:IS_CLASSIFIED_AS]->(c)


7. Validation Status
Pending
The initial status is "Pending" as the requirement has been captured but not yet reviewed and approved by stakeholders. The validation process will confirm that the requirement is correct, complete, and aligns with the project's objectives.
---


## PROMPT 2: Few-Shot Example (Advanced)
### User Input:

---
You are REMP-AI, an expert Requirements Engineering Management agent. Your task is to process raw requirement statements and transform them into structured, classified requirement specifications.

I will provide you with several example (X, y) pairs demonstrating the expected analysis format, followed by a new requirement for you to process.


EXAMPLE 1

INPUT (X₁):

"As a user, I want to login quickly so that I can access my account"

OUTPUT (Y₁):

1. Requirement ID: REQ_001

2. Structured Components:
   
Actor: "user"

Goal: "login quickly"

Rationale: "access my account"

3. Classification:

Type: Non-Functional (NFR)

Subtype: Performance

Confidence: 0.92

4. Explainability:
   
Key tokens: "quickly" (0.31), "login" (0.24), "access" (0.18)

5. Neo4j Graph:
   
(REQ_001)-[:HAS_ACTOR]→(Actor {name: "user"})

(REQ_001)-[:HAS_GOAL]→(Goal {text: "login quickly"})

(REQ_001)-[:HAS_RATIONALE]→(Rationale {text: "access my account"})

(REQ_001)-[:CLASSIFIED_AS]→(Performance {confidence: 0.92})

6. Status: ✓ Validated




EXAMPLE 2

INPUT (X₂):

"The system shall encrypt all user data using AES-256 encryption"

OUTPUT (Y₂):

1. Requirement ID: REQ_002

2. Structured Components:

Actor: "system"

Goal: "encrypt all user data"

Rationale: "using AES-256 encryption"

3. Classification:

Type: Non-Functional (NFR)

Subtype: Security

Confidence: 0.96

4. Explainability:

Key tokens: "encrypt" (0.38), "AES-256" (0.29), "data" (0.22)

5. Neo4j Graph:

(REQ_002)-[:HAS_ACTOR]→(Actor {name: "system"})

(REQ_002)-[:HAS_GOAL]→(Goal {text: "encrypt all user data"})

(REQ_002)-[:HAS_RATIONALE]→(Rationale {text: "using AES-256 encryption"})

(REQ_002)-[:CLASSIFIED_AS]→(Security {confidence: 0.96})

6. Status: ✓ Validated




EXAMPLE 3

INPUT (X₃):

"As an admin, I want to delete user accounts so that I can manage the database"

OUTPUT (Y₃):

1. Requirement ID: REQ_003

2. Structured Components:

Actor: "admin"

Goal: "delete user accounts"

Rationale: "manage the database"

3. Classification:

Type: Functional (FR)

Subtype: N/A

Confidence: 0.89

4. Explainability:

Key tokens: "delete" (0.35), "accounts" (0.27), "admin" (0.19)

5. Neo4j Graph:

(REQ_003)-[:HAS_ACTOR]→(Actor {name: "admin"})

(REQ_003)-[:HAS_GOAL]→(Goal {text: "delete user accounts"})

(REQ_003)-[:HAS_RATIONALE]→(Rationale {text: "manage the database"})

(REQ_003)-[:CLASSIFIED_AS]→(Functional {confidence: 0.89})

6. Status: ✓ Validated


NEW REQUIREMENT TO ANALYZE


INPUT (X_new):

"As a premium subscriber, I want the system to be available 99.9% of the time so that I can rely on the service for critical business operations"

TASK:
Following the pattern demonstrated in the examples above, perform complete end-to-end analysis and generate the structured output Y_new with:
1. Requirement ID
2. Structured Components (Actor, Goal, Rationale)
3. Classification (FR/NFR with subtype and confidence)
4. Explainability (key tokens with SHAP scores)
5. Neo4j Graph Structure (nodes and relationships)
6. Validation Status
Generate the complete output now.
---


### AI Agent Output (Y):

---
OUTPUT (Y_new):

1. Requirement ID: REQ_004

2. Structured Components:

Actor: "premium subscriber"

Goal: "the system to be available 99.9% of the time"

Rationale: "rely on the service for critical business operations"

3. Classification:

Type: Non-Functional (NFR)

Subtype: Availability

Confidence: 0.97

4. Explainability:

Key tokens: "available" (0.36), "99.9%" (0.32), "rely" (0.17)

5. Neo4j Graph:
   
(REQ_004)-[:HAS_ACTOR]→(Actor {name: "premium subscriber"})

(REQ_004)-[:HAS_GOAL]→(Goal {text: "the system to be available 99.9% of the time"})

(REQ_004)-[:HAS_RATIONALE]→(Rationale {text: "rely on the service for critical business operations"})

(REQ_004)-[:CLASSIFIED_AS]→(Availability {confidence: 0.97})

6. Status: ✓ Validated

---
