# TalentLens — System Architecture

## 1. Architecture Overview

TalentLens follows a modular architecture combining:

* Frontend application
* Backend REST API
* Document processing
* NLP pipeline
* Rule-based ATS engine
* Machine learning matching engine
* Generative AI service

The initial MVP is stateless and does not require a database.

---

# 2. High-Level Architecture

```text
                    USER
                      │
                      ▼
              React Frontend
                      │
                 REST API
                      │
                      ▼
              FastAPI Backend
                      │
        ┌─────────────┼──────────────┐
        │             │              │
        ▼             ▼              ▼
 Document         NLP/ML         AI Service
 Processing       Services       Gemini API
        │             │              │
        └─────────────┼──────────────┘
                      ▼
                Analysis Engine
                      │
                      ▼
                JSON Response
                      │
                      ▼
              React Results Page
```

---

# 3. Frontend

Technology:

* React
* TypeScript
* Tailwind CSS

Responsibilities:

* Resume input
* JD input
* File upload
* Analysis initiation
* Progress display
* Results visualization
* Skill-gap interaction
* Roadmap display
* Error handling

The frontend should not contain core ATS or ML logic.

It communicates with the backend through REST APIs.

---

# 4. Backend

Technology:

* Python
* FastAPI

The backend is responsible for coordinating the complete analysis pipeline.

Main responsibilities:

* Receive resume and JD
* Validate input
* Extract document text
* Run NLP processing
* Run ATS analysis
* Run semantic matching
* Calculate skill gaps
* Request AI-generated recommendations
* Validate AI output
* Return structured results

---

# 5. Document Processing Layer

Supported formats:

* Plain text
* PDF
* DOCX

Pipeline:

```text
PDF/DOCX
   ↓
Text Extraction
   ↓
Text Cleaning
   ↓
Normalized Text
```

Libraries:

* pdfplumber
* python-docx

For pasted text, document extraction is skipped.

---

# 6. Resume NLP Pipeline

```text
Resume Text
     ↓
Cleaning
     ↓
Section Detection
     ↓
Entity Extraction
     ↓
Skill Extraction
     ↓
Skill Normalization
     ↓
Structured Resume
```

The structured representation may contain:

```text
Personal Information
Summary
Skills
Education
Experience
Projects
Certifications
Links
```

---

# 7. Job Description NLP Pipeline

```text
Job Description
      ↓
Cleaning
      ↓
Section/Requirement Detection
      ↓
Skill Extraction
      ↓
Skill Normalization
      ↓
Requirement Classification
      ↓
Structured JD
```

The system should distinguish:

```text
Required Skills
Preferred Skills
Experience Requirements
Education Requirements
Responsibilities
```

---

# 8. ATS Engine

The ATS engine uses deterministic and explainable rules.

```text
Structured Resume
       ↓
ATS Checks
       ↓
Structure
Keywords
Skills
Experience
Readability
Formatting
       ↓
Weighted Score
       ↓
ATS Compatibility Score
```

Example:

```text
ATS Score
├── Structure
├── Keyword Coverage
├── Skill Coverage
├── Experience Relevance
├── Readability
└── Formatting
```

The score should be configurable so that the weighting can be improved through testing.

---

# 9. Machine Learning Matching Engine

The ML engine is responsible primarily for semantic understanding.

```text
Resume Sections
      ↓
Embedding Model
      ↓
Resume Vectors


JD Requirements
      ↓
Embedding Model
      ↓
JD Vectors

Resume Vectors + JD Vectors
              ↓
       Cosine Similarity
              ↓
      Semantic Match Score
```

Sentence Transformers will initially be used for generating embeddings.

A lightweight initial model can be:

```text
all-MiniLM-L6-v2
```

---

# 10. Hybrid Matching

Semantic similarity alone is insufficient.

The matching engine combines:

```text
Exact Skill Matching
        +
Skill Normalization
        +
Semantic Similarity
        +
Keyword Coverage
        +
Experience Matching
        +
Education Matching
        +
Requirement Importance
```

These signals are combined to produce:

**Job Match Score**

This score is separate from the ATS Compatibility Score.

---

# 11. Skill Gap Engine

The skill-gap engine compares:

```text
JD Requirements
       ↓
Candidate Capabilities
       ↓
Comparison
       ↓
Skill Status
```

Possible statuses:

```text
Strong
Moderate
Weak
Missing
```

The engine should consider semantic evidence rather than simply checking whether an exact word exists.

---

# 12. Generative AI Layer

Technology:

**Gemini API**

The LLM is used only for tasks requiring generation or explanation.

Examples:

* Resume improvement suggestions
* Explaining weaknesses
* Explaining skill gaps
* Personalized learning roadmaps

The LLM should receive structured information from the analysis pipeline rather than being responsible for the entire analysis.

---

# 13. Complete Analysis Pipeline

```text
             Resume + JD
                  │
                  ▼
            Input Validation
                  │
        ┌─────────┴─────────┐
        ▼                   ▼
   Resume Parser          JD Parser
        │                   │
        ▼                   ▼
   Resume NLP             JD NLP
        │                   │
        └─────────┬─────────┘
                  ▼
           Structured Data
                  │
        ┌─────────┼──────────┐
        ▼         ▼          ▼
      ATS      Matching    Skill Gap
     Engine     Engine      Engine
        │         │          │
        └─────────┼──────────┘
                  ▼
           Analysis Results
                  │
                  ▼
             Gemini API
                  │
        ┌─────────┴─────────┐
        ▼                   ▼
   Suggestions           Roadmap
        │                   │
        └─────────┬─────────┘
                  ▼
             Final JSON
                  │
                  ▼
             React UI
```

---

# 14. API Design

Initial APIs:

```text
POST /api/analyze
```

Main endpoint that receives the resume and JD and performs the complete analysis.

```text
POST /api/roadmap
```

Generates a roadmap for a selected skill.

Potential supporting endpoints:

```text
POST /api/resume/parse
POST /api/jd/parse
GET  /api/health
```

These can be used during development and testing.

---

# 15. Backend Module Structure

```text
backend/
│
├── app/
│   ├── api/
│   ├── services/
│   ├── parser/
│   ├── nlp/
│   ├── ml/
│   ├── ats/
│   ├── ai/
│   ├── schemas/
│   ├── utils/
│   └── main.py
│
└── tests/
```

Responsibilities:

### `parser/`

PDF/DOCX extraction.

### `nlp/`

Text processing, section detection, entities and skills.

### `ml/`

Embeddings and semantic similarity.

### `ats/`

ATS scoring and rule-based analysis.

### `ai/`

Gemini integration and prompt management.

### `services/`

Orchestrates the different components.

### `schemas/`

Defines API request and response structures.

---

# 16. Stateless MVP

The initial application does not use a database.

```text
User
 ↓
Resume + JD
 ↓
FastAPI
 ↓
Analysis
 ↓
Result
```

The analysis exists only for the current request/session.

A database can later be introduced for:

* User accounts
* Resume history
* Saved analyses
* Resume versions
* Learning progress
* Interview history

---

# 17. Future Architecture

After adding persistence:

```text
React
  ↓
FastAPI
  ↓
Services
  ├── NLP/ML
  ├── ATS
  └── AI
  ↓
PostgreSQL
```

The core analysis architecture does not need to change significantly.
