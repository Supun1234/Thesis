# GAI-Enhanced Requirements Engineering Management Process (REMP)

## Overview

This system implements an end-to-end AI agent for automating requirements engineering management. It takes raw requirement text as input and processes it through a 6-phase pipeline: (1) Input Quality Enhancement, (2) Actor-Goal-Rationale Extraction, (3) FR/NFR Classification, (4) Quality Validation, (5) Neo4j Graph Storage, and (6) Traceability Discovery. The system outputs structured requirements stored as a queryable knowledge graph in Neo4j format, complete with confidence scores, quality metrics, and semantic traceability links between related requirements.

## Technology Stack

- **Gemini 2.5 pro**: Used as GAI (Generative AI) for intelligent interventions
- **BERT/RemBERT**: NER model for Actor-Goal-Rationale extraction
- **NoRBERT**: Fine-tuned model for FR/NFR classification
- **Neo4j**: Knowledge graph database for requirement storage
- **Python**: Implementation in Google Colab

## How Gemini & Prompt Engineering Were Used

Gemini API is strategically integrated at four critical decision points in the pipeline where confidence thresholds (0.90) are not met:

1. **Phase 1 (Input Quality)**: Few-shot prompting to complete incomplete requirements while preserving original meaning and avoiding over-engineering
2. **Phase 2 (Extraction)**: Few-shot validation to clarify ambiguous Actor-Goal-Rationale extractions when BERT confidence < 0.85
3. **Phase 3 (Classification)**: Few-shot reasoning to validate FR/NFR classification when NoRBERT confidence < 0.85
4. **Phase 6 (Traceability)**: Semantic analysis to discover relationships (DEPENDS_ON, RELATES_TO, REFINES) between requirements

The prompt engineering approach emphasizes: (a) keeping enhancements minimal and contextual, (b) JSON-structured outputs for reliable parsing, (c) explicit constraints to prevent hallucination, and (d) length validation to avoid over-detailed responses.

## Reflection

**What Worked Well:**
The hybrid approach combining traditional NLP models (BERT/NoRBERT) with GAI validation proved highly effective, achieving quality scores above 0.90 while maintaining semantic accuracy. The confidence-based triggering of GAI interventions creates an efficient system that only uses expensive LLM calls when necessary. The Neo4j graph structure enables powerful querying and traceability that would be impossible with document-based storage. The modular pipeline design allows easy switching between prototype (regex-based) and production (fine-tuned models) modes.

**What Could Be Improved:**
Phase 1 occasionally struggles with over-enhancement despite prompt constraints, requiring additional length validation checks. The system currently lacks human-in-the-loop feedback for borderline cases (quality scores 0.85-0.90), which could improve accuracy through iterative refinement. Real-time Neo4j database integration would be more practical than generating Cypher queries as text output. The traceability discovery in Phase 6 is limited to the last 3-5 requirements due to context window constraints, missing potential long-range dependencies. Adding conflict detection and automated resolution suggestions would make the system more robust for real-world requirement sets with contradictions.

## Future Work

- Integrate actual Neo4j database with real-time graph updates
- Implement human-in-the-loop validation interface for edge cases
- Expand traceability to handle larger requirement sets using vector embeddings
- Add conflict detection and resolution mechanisms
- Fine-tune domain-specific models for your specific project context
- Implement batch processing optimizations for large requirement documents
