
   UPDATED: END-TO-END PROCESS FOCUS (NO SRS GENERATION)
   GAI = Process Enhancement, NOT Document Generation



   1. SYSTEM ARCHITECTURE - CLASS DIAGRAM
 

@startuml REMP_Class_Diagram
!theme cerulean-outline
skinparam linetype ortho

title GAI-Enhanced End-to-End REMP - Class Diagram

package "Core Processing Models" {
    class BERTNERExtractor {
        - model: BertForTokenClassification
        - tokenizer: BertTokenizer
        - confidence_threshold: float = 0.90
        --
        + extract_agr(text: str): AGRTriplet
        + get_confidence(): float
        - preprocess(text: str): List[Token]
    }

    class NoRBERTClassifier {
        - model: BertForSequenceClassification
        - label_map: Dict
        - nfr_subtypes: List[str]
        --
        + classify(goal: str): Classification
        + get_probabilities(): Dict
        - apply_sigmoid(logits): ndarray
    }

    class Neo4jGraphDB {
        - driver: GraphDatabase
        - uri: str
        --
        + add_requirement(req: Requirement): str
        + add_relationship(req1, req2, type): void
        + query_cypher(query: str): List
        + find_similar_requirements(req_id): List
        + get_traceability_graph(): Graph
    }
}

package "GAI Process Enhancement Layer" {
    class GAIProcessEnhancer {
        - api_key: str
        - model: str = "gpt-4"
        - use_local: bool = False
        --
        + complete_partial_input(text: str): str
        + clarify_ambiguity(vague: str): str
        + validate_classification(req, pred): ValidationResult
        + improve_quality(req: str): str
        + discover_relationships(reqs: List): List[Relationship]
        + generate_cypher_from_nl(query: str): str
    }

    class LocalLLM {
        - model: str = "llama3"
        - ollama_url: str
        --
        + generate(prompt: str): str
        + is_available(): bool
    }
    
    note right of GAIProcessEnhancer
      GAI Role: Process Enhancement
      NOT document generation
    end note
}

package "Data Models" {
    class Requirement {
        + id: str
        + text: str
        + actor: str
        + goal: str
        + rationale: str
        + type: str
        + nfr_subtype: str
        + confidence: float
        + shap_values: Dict
        + quality_score: float
        --
        + to_neo4j_node(): Dict
        + is_high_quality(): bool
    }

    class AGRTriplet {
        + actor: str
        + goal: str
        + rationale: str
        + confidence_scores: Dict
        --
        + is_complete(): bool
        + get_weighted_confidence(): float
    }

    class Classification {
        + type: str
        + subtype: str
        + confidence: float
        + gai_validated: bool
        + explanation: str
    }
    
    class TraceabilityLink {
        + from_req: str
        + to_req: str
        + relationship_type: str
        + confidence: float
        + reasoning: str
    }
}

package "End-to-End Pipeline" {
    class REMPEndToEndPipeline {
        - bert_extractor: BERTNERExtractor
        - nobert_classifier: NoRBERTClassifier
        - gai_enhancer: GAIProcessEnhancer
        - neo4j_db: Neo4jGraphDB
        - validator: RequirementValidator
        --
        + process_requirement(text: str): Requirement
        + process_batch(texts: List[str]): List[Requirement]
        - enhance_input_quality(text): str
        - store_in_knowledge_graph(req): str
        + query_graph_nl(query: str): List
    }

    class RequirementValidator {
        - completeness_threshold: float
        - quality_rules: List
        --
        + validate(req: Requirement): ValidationReport
        + check_ambiguity(text: str): List[Issue]
        + check_smart_criteria(req): float
    }
}

' Relationships
REMPEndToEndPipeline --> BERTNERExtractor : uses
REMPEndToEndPipeline --> NoRBERTClassifier : uses
REMPEndToEndPipeline --> GAIProcessEnhancer : enhances with
REMPEndToEndPipeline --> Neo4jGraphDB : stores in
REMPEndToEndPipeline --> RequirementValidator : validates
REMPEndToEndPipeline --> TraceabilityLink : creates

BERTNERExtractor --> AGRTriplet : produces
NoRBERTClassifier --> Classification : produces
GAIProcessEnhancer --> LocalLLM : fallback

REMPEndToEndPipeline --> Requirement : creates
Requirement --> AGRTriplet : contains
Requirement --> Classification : contains

note bottom of REMPEndToEndPipeline
  Output: Neo4j Knowledge Graph
  NOT: Document files
end note

@enduml


   2. SEQUENCE DIAGRAM - END-TO-END PROCESS FLOW


@startuml REMP_Sequence_EndToEnd
!theme cerulean-outline

title GAI-Enhanced End-to-End REMP - Process Flow

actor User
participant "Pipeline" as Pipeline
participant "GAI\nEnhancer" as GAI
participant "BERT\nExtractor" as BERT
participant "NoRBERT\nClassifier" as NoRBERT
participant "Validator" as Validator
participant "Neo4j\nGraph" as Neo4j

User -> Pipeline: process_requirement(text)
activate Pipeline

' Phase 1: Input Quality Enhancement
Pipeline -> Pipeline: check_input_completeness()
alt Input is incomplete/partial
    Pipeline -> GAI: complete_partial_input(text)
    activate GAI
    note right: Generate complete requirement
    GAI --> Pipeline: complete_requirement
    deactivate GAI
end

' Phase 2: Extraction
Pipeline -> BERT: extract_agr(text)
activate BERT
BERT -> BERT: dual_ner_extraction()
BERT --> Pipeline: agr_triplet
deactivate BERT

' Phase 3: Ambiguity Check
alt Extraction confidence < 0.90
    Pipeline -> GAI: clarify_ambiguity(text)
    activate GAI
    note right: Make requirement specific
    GAI --> Pipeline: clarified_text
    deactivate GAI
    
    Pipeline -> BERT: extract_agr(clarified_text)
    activate BERT
    BERT --> Pipeline: improved_agr
    deactivate BERT
end

' Phase 4: Classification
Pipeline -> NoRBERT: classify(agr.goal)
activate NoRBERT
NoRBERT --> Pipeline: classification
deactivate NoRBERT

' Phase 5: Classification Validation
alt Classification confidence < 0.90
    Pipeline -> GAI: validate_classification(req, pred)
    activate GAI
    note right: Provide reasoning + boost confidence
    GAI --> Pipeline: validation_result
    deactivate GAI
    
    Pipeline -> Pipeline: combine_predictions()
end

' Phase 6: Quality Validation
Pipeline -> Validator: validate_requirement(req)
activate Validator
Validator --> Pipeline: validation_report
deactivate Validator

alt Quality score < 0.90
    Pipeline -> GAI: improve_quality(req)
    activate GAI
    note right: Suggest SMART improvements
    GAI --> Pipeline: improved_requirement
    deactivate GAI
end

' Phase 7: Store in Neo4j Graph
Pipeline -> Neo4j: create_requirement_node(req)
activate Neo4j
Neo4j -> Neo4j: create_agr_nodes()
Neo4j -> Neo4j: establish_relationships()
Neo4j --> Pipeline: node_id
deactivate Neo4j

' Phase 8: Discover Traceability
Pipeline -> GAI: discover_relationships([requirements])
activate GAI
note right: Find semantic links
GAI --> Pipeline: traceability_links
deactivate GAI

Pipeline -> Neo4j: add_traceability_links(links)
activate Neo4j
Neo4j --> Pipeline: success
deactivate Neo4j

Pipeline --> User: neo4j_node_id + metadata
note left
  Output: Neo4j Graph Node
  NOT: Document file
end note

deactivate Pipeline

@enduml


   3. ACTIVITY DIAGRAM - END-TO-END WORKFLOW
  

@startuml REMP_Activity_EndToEnd
!theme cerulean-outline

title GAI-Enhanced End-to-End REMP - Workflow

start

:Receive Raw Requirement Text;
note right: INPUT

partition "Phase 1: Input Quality" {
    if (Input Complete & Clear?) then (no)
        :GAI: Complete/Clarify Input;
        note right
          Generate complete requirement
          Make vague requirements specific
        end note
    else (yes)
    endif
}

partition "Phase 2: Extraction (BERT NER)" {
    :Extract Actor-Goal-Rationale;
    
    if (Confidence >= 0.90?) then (yes)
    else (no)
        :GAI: Clarify Ambiguity;
        :Re-extract with BERT;
    endif
    
    :Calculate Weighted Confidence;
}

partition "Phase 3: Classification (NoRBERT)" {
    :Classify FR/NFR + Subtypes;
    
    if (Confidence >= 0.90?) then (yes)
    else (no)
        :GAI: Validate with Reasoning;
        :Boost Combined Confidence;
    endif
}

partition "Phase 4: Quality Validation" {
    :Check SMART Criteria;
    :Check Completeness;
    :Check Ambiguity;
    
    if (Quality Score >= 0.90?) then (yes)
    else (no)
        :GAI: Suggest Improvements;
        if (Accept?) then (yes)
            :Apply Improvements;
        else (no)
            :Flag for Manual Review;
        endif
    endif
}

partition "Phase 5: Neo4j Graph Storage" {
    :Create Requirement Node;
    :Create AGR Sub-nodes;
    :Add Classification Properties;
    :Store Confidence & Quality Scores;
}

partition "Phase 6: Traceability Discovery" {
    :GAI: Discover Semantic Links;
    :Create Relationship Edges;
    :Store in Graph;
}

:Return Neo4j Node ID;
note right: OUTPUT: Graph Node

stop

note right of "Neo4j Graph Storage"
  PRIMARY OUTPUT:
  Queryable Knowledge Graph
  NOT documents
end note

@enduml


   4. COMPONENT DIAGRAM - SYSTEM ARCHITECTURE


@startuml REMP_Component_EndToEnd
!theme cerulean-outline

title GAI-Enhanced End-to-End REMP - System Architecture

package "User Interface Layer" {
    [Web Dashboard\n(Streamlit)] as WebUI
    [REST API\n(FastAPI)] as API
    [NL Query Interface] as NLQuery
}

package "Business Logic Layer" {
    component "End-to-End Pipeline" as Pipeline {
        [Process Orchestrator] as Orch
        [Input Quality Gate] as InputQG
        [Output Quality Gate] as OutputQG
    }
}

package "AI Processing Layer" {
    component "Extraction Module" {
        [BERT NER] as BERT
        [spaCy (Fallback)] as spaCy
    }
    
    component "Classification Module" {
        [NoRBERT] as NoRBERT
        [Multi-Label Processor] as MultiLabel
    }
    
    component "Validation Module" {
        [SMART Checker] as SMART
        [Ambiguity Detector] as Ambiguity
    }
}

package "GAI Enhancement Layer" {
    component "Process Enhancer" {
        [Input Completer] as Complete
        [Ambiguity Clarifier] as Clarify
        [Classification Validator] as Validate
        [Quality Improver] as Improve
        [Relationship Discoverer] as Discover
        [NL-to-Cypher] as NLCypher
    }
}

package "Knowledge Graph Layer" {
    database "Neo4j\nKnowledge Graph" as Neo4j {
        [Requirement Nodes] as ReqNodes
        [AGR Components] as AGRNodes
        [Traceability Links] as TraceLinks
    }
    
    database "Vector Store\n(Embeddings)" as Vector
}

package "External Services" {
    [GPT-4 API\n(Optional)] as GPT4
    [Ollama\n(Local LLM)] as Ollama
}

' Connections
WebUI --> API
NLQuery --> API
API --> Pipeline

Pipeline --> BERT
Pipeline --> NoRBERT
Pipeline --> SMART
Pipeline --> Complete
Pipeline --> Clarify
Pipeline --> Validate

Complete --> GPT4 : cloud
Complete --> Ollama : local fallback

Pipeline --> Neo4j : store
Pipeline --> Vector : embeddings
Discover --> Neo4j : read/write links
NLCypher --> Neo4j : query

note right of Pipeline
  Core Output:
  Neo4j Knowledge Graph
  with structured requirements
end note

note right of Complete
  GAI Role:
  Process Enhancement
  NOT document creation
end note

@enduml


   5. USE CASE DIAGRAM


@startuml REMP_UseCase_EndToEnd
!theme cerulean-outline

title GAI-Enhanced End-to-End REMP - Use Cases

left to right direction

actor "Stakeholder" as Stakeholder
actor "Requirements Engineer" as RE
actor "Project Manager" as PM
actor "Developer" as Dev

rectangle "End-to-End REMP System" {
    usecase "Submit Partial\nRequirement" as UC1
    usecase "Complete Input\n(GAI)" as UC2
    usecase "Extract AGR\nComponents" as UC3
    usecase "Classify FR/NFR" as UC4
    usecase "Validate Quality" as UC5
    usecase "Review Flagged\nRequirements" as UC6
    usecase "Store in Neo4j\nGraph" as UC7
    usecase "Discover Traceability\n(GAI)" as UC8
    usecase "Query Graph\n(Natural Language)" as UC9
    usecase "Export Graph Data" as UC10
    usecase "Visualize Traceability" as UC11
}

' Stakeholder use cases
Stakeholder --> UC1
UC1 ..> UC2 : <<extend>>\nif incomplete

' Requirements Engineer use cases
RE --> UC3
RE --> UC4
RE --> UC5
RE --> UC6
RE --> UC7

' Project Manager use cases
PM --> UC8
PM --> UC9
PM --> UC11

' Developer use cases
Dev --> UC9
Dev --> UC10
Dev --> UC11

' System relationships
UC2 --> UC3 : triggers
UC3 --> UC4 : triggers
UC4 --> UC5 : triggers
UC5 --> UC7 : if validated
UC5 ..> UC6 : <<extend>>\nif issues
UC7 --> UC8 : triggers
UC8 --> UC9 : enables

note right of UC2
  GAI Enhancement:
  Completes partial input
  NOT document generation
end note

note right of UC8
  GAI Enhancement:
  Discovers semantic links
  in Neo4j graph
end note

note bottom of UC7
  PRIMARY OUTPUT:
  Neo4j Knowledge Graph
end note

@enduml


   6. STATE DIAGRAM - REQUIREMENT LIFECYCLE
   

@startuml REMP_State_EndToEnd
!theme cerulean-outline

title Requirement Lifecycle - End-to-End Process

[*] --> Submitted : User submits text

Submitted --> InputQualityCheck : Start processing

InputQualityCheck --> GAICompletion : If incomplete/vague
GAICompletion --> Extraction : Enhanced input

InputQualityCheck --> Extraction : If complete

Extraction --> ExtractionValidation : AGR extracted

ExtractionValidation --> GAIClarification : If confidence < 0.90
GAIClarification --> Extraction : Re-extract

ExtractionValidation --> Classification : If confidence >= 0.90

Classification --> ClassificationValidation : Type assigned

ClassificationValidation --> GAIValidation : If confidence < 0.90
GAIValidation --> QualityCheck : Boosted confidence

ClassificationValidation --> QualityCheck : If confidence >= 0.90

QualityCheck --> GAIImprovement : If quality < 0.90
GAIImprovement --> QualityCheck : Improved

QualityCheck --> ManualReview : If critical issues
QualityCheck --> Neo4jStorage : If quality >= 0.90

ManualReview --> Neo4jStorage : After review
ManualReview --> Rejected : If invalid

Neo4jStorage --> TraceabilityDiscovery : Node created

TraceabilityDiscovery --> GAIRelationships : Discover links
GAIRelationships --> GraphComplete : Links stored

GraphComplete --> [*] : Available for queries

Rejected --> [*]

note right of Neo4jStorage
  PRIMARY OUTPUT:
  Neo4j Graph Node
  with all metadata
end note

note right of GAIRelationships
  GAI discovers semantic
  relationships, NOT
  generates documents
end note

@enduml


   7. DEPLOYMENT DIAGRAM - PROCESS INFRASTRUCTURE
   

@startuml REMP_Deployment_EndToEnd
!theme cerulean-outline

title End-to-End REMP - Deployment Architecture

node "User Device" {
    [Web Browser] as Browser
}

node "Application Server\n(Docker Container)" {
    [Streamlit UI] as Frontend
    [FastAPI Backend] as Backend
    [Celery Workers] as Workers
}

node "AI Model Server\n(GPU Instance)" {
    [BERT NER Model] as BERTModel
    [NoRBERT Classifier] as NoRBERTModel
    [Model Cache] as Cache
}

node "GAI Enhancement Server" {
    component "Local LLM\n(Ollama)" as Ollama {
        [Llama 3 70B] as Llama
    }
    component "Fallback API" as API {
        [GPT-4] as GPT4
    }
}

node "Graph Database Cluster" {
    database "Neo4j\n(Port 7687)" as Neo4j {
        [Requirement Nodes]
        [Traceability Graph]
        [Vector Index]
    }
    database "PostgreSQL\n(Metadata)" as Postgres
    database "Redis\n(Queue)" as Redis
}

cloud "External Services" {
    [OpenAI API] as OpenAI
    [HuggingFace] as HF
}

' Connections
Browser --> Frontend : HTTPS
Frontend --> Backend : REST API
Backend --> Workers : Async Tasks
Backend --> BERTModel : gRPC
Backend --> NoRBERTModel : gRPC
Backend --> Ollama : HTTP (Primary)
Backend --> API : HTTPS (Fallback)
Backend --> Neo4j : Bolt Protocol
Backend --> Postgres : SQL
Workers --> Redis : Queue

API ..> OpenAI : API Call
BERTModel ..> HF : Download

note right of Neo4j
  PRIMARY OUTPUT STORAGE:
  - Structured requirements
  - AGR components
  - Classifications
  - Traceability links
  - Quality scores
  NOT document files
end note

note right of Ollama
  Privacy-First:
  Local GAI processing
  No data leaves server
end note

@enduml


   8. DATA FLOW DIAGRAM - END-TO-END PROCESS
 

@startuml REMP_DataFlow_EndToEnd
!theme cerulean-outline

title End-to-End REMP - Data Flow Diagram

' External Entities
rectangle "User/Stakeholder" as User

' Processes
rectangle "1.0\nInput Enhancement\n(GAI)" as P1
rectangle "2.0\nAGR Extraction\n(BERT NER)" as P2
rectangle "3.0\nClassification\n(NoRBERT)" as P3
rectangle "4.0\nQuality Validation" as P4
rectangle "5.0\nNeo4j Storage" as P5
rectangle "6.0\nTraceability\nDiscovery (GAI)" as P6

' Data Stores
database "D1: Neo4j\nKnowledge Graph" as D1
database "D2: Vector\nEmbeddings" as D2
database "D3: Quality\nRules" as D3

' Data Flows
User --> P1 : Raw requirement text\n(incomplete/vague)
P1 --> P2 : Complete requirement text
P2 --> P3 : AGR triplet\n(Actor-Goal-Rationale)
P3 --> P4 : Classification\n(FR/NFR + subtype)
P4 --> P5 : Validated requirement\n(quality >= 0.90)
P5 --> D1 : Store requirement node
P5 --> D2 : Store embeddings
P5 --> P6 : Requirement data
P6 --> D1 : Store traceability links

' Feedback loops
P2 ..> P1 : Clarification needed
P3 ..> P1 : Ambiguity detected
P4 ..> P1 : Quality improvement
D3 --> P4 : Quality rules

' Output
D1 --> User : Query results\n(Cypher/NL)

note right of P1
  GAI Enhancement:
  - Complete partial input
  - Clarify ambiguities
  - Improve quality
end note

note right of D1
  PRIMARY OUTPUT:
  Queryable graph with:
  - Structured requirements
  - Classifications
  - Traceability
end note

@enduml

/' ============================================================
   END OF UML DIAGRAMS
   All diagrams focus on END-TO-END PROCESS
   GAI = Process Enhancement, NOT Document Generation
   Output = Neo4j Knowledge Graph, NOT Documents
   ============================================================ '/
