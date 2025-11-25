## 🧠 Architecture Spotlight: Dynamic Few-Shot RAG
To bridge the gap between general Large Language Model (LLM) capabilities and domain-specific Requirements Engineering standards, this system implements a Retrieval-Augmented Generation (RAG) layer using ChromaDB.
Instead of relying on static, hard-coded examples in the prompt (which consume token space and lack context), the system employs a Dynamic Few-Shot strategy.

## 🔄 The Mechanism: Just-In-Time Context
For every incoming requirement (Input X), the system executes the following retrieval pipeline before generation:
1. Vector Embedding: The input X is embedded into a high-dimensional vector space.
2. Semantic Search: ChromaDB queries the "Golden Set" (a CSV of verified historical requirements) to find the k=3 nearest semantic neighbors.
3. Prompt Augmentation: These retrieved examples—complete with their correct Actor, Goal, Rationale, and NFR Classification—are dynamically injected into the system prompt.
4. Contextual Generation: The LLM (Gemini) generates Output Y,conditioned on these specific, relevant precedents.

## 🚀 Impact on System Performance
1. Nuanced Classification (FR vs. NFR)
General LLMs often struggle to distinguish between overlapping NFR subtypes (e.g., Reliability vs. Availability). By retrieving similar past examples, the model learns the specific criteria used in this project's history.
Result: If the input mentions "uptime," the system retrieves examples classifying similar metrics as "Reliability," guiding the model to the correct subtype with high confidence.
2. Handling Ambiguity in Extraction
In complex sentences where the "Actor" is implied or the "Rationale" is merged with the "Goal," zero-shot models often hallucinate or miss entities.
Result: By seeing 3 examples of how complex sentences were parsed in the past, the model mimics the extraction logic, significantly reducing parsing errors in Phase 2.

## 🛡️ Impact on Reliability & Standardization
1. Output Consistency (The "Mimicry" Effect)
One of the biggest challenges in AI engineering is variable output formats. The RAG layer ensures the model adheres to project-specific nomenclature (e.g., consistently extracting "Admin" rather than varying between "Administrator," "SysAdmin," and "SuperUser") because it is statistically likely to follow the pattern of the retrieved context.
2. Adaptive Learning without Fine-Tuning
The system improves automatically as the dataset grows. By simply adding a new, corrected example to the requirements.csv (and re-indexing ChromaDB), the AI instantly "learns" from that edge case without requiring expensive model fine-tuning or code changes.
