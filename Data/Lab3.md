# Companies Best Suited for This Solution:
1. Software Development Companies

Need to manage hundreds of requirements efficiently
Example: Accenture, Infosys, TCS, Capgemini

2. Requirements Management Tool Vendors

Can integrate AI into their platforms
Example: IBM (DOORS Next), Jama Software, PTC (Codebeamer)

3. Enterprise Software Companies

Handle complex requirement specifications
Example: SAP, Oracle, Microsoft, Salesforce

4. Financial/Banking Institutions

Heavy requirements documentation & compliance
Example: Banks, Insurance companies (regulatory requirements)

5. Healthcare/Medical Software

Strict requirement traceability needed
Example: Epic Systems, Cerner, Philips Healthcare

## BEST MATCH: IBM or Jama Software

Already have requirements management tools (IBM DOORS, Jama Connect)
Need AI to enhance automation
Large customer base needing better RE processes
Neo4j graph + AI classification directly improves their products

# How GAI is integrated into this system

## **Architecture Diagram:**
```
┌─────────────────────────────────────────────────┐
│           INPUT: Raw Requirement Text           │
└─────────────────┬───────────────────────────────┘
                  ↓
         ┌────────────────┐
         │ GAI Layer #1:  │
         │ Completion     │ ← GPT-4 / Llama
         └────────┬───────┘
                  ↓
┌─────────────────────────────────────────────────┐
│      BERT NER Extraction (My Core Model)        │
└─────────────────┬───────────────────────────────┘
                  ↓
         ┌────────────────┐
         │ GAI Layer #2:  │
         │ Clarification  │ ← GPT-4 / Llama
         └────────┬───────┘
                  ↓
┌─────────────────────────────────────────────────┐
│   NoRBERT Classification (My Core Model)        │
└─────────────────┬───────────────────────────────┘
                  ↓
         ┌────────────────┐
         │ GAI Layer #3:  │
         │ Validation     │ ← GPT-4 / Llama
         └────────┬───────┘
                  ↓
┌─────────────────────────────────────────────────┐
│        Neo4j Graph Storage + Traceability       │
└─────────────────┬───────────────────────────────┘
                  ↓
         ┌────────────────┐
         │ GAI Layer #4:  │
         │ Documentation  │ ← GPT-4 / Llama
         └────────┬───────┘
                  ↓
┌─────────────────────────────────────────────────┐
│         OUTPUT: IEEE 830 SRS Document           │
└─────────────────────────────────────────────────┘

