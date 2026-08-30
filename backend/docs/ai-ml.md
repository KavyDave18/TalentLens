# TalentLens — AI/ML Architecture

## 1. Overview

TalentLens uses a **hybrid AI architecture**.

It combines:

1. Rule-based programming
2. NLP
3. Machine learning
4. Generative AI

The goal is to use each technique where it is most reliable.

The system should not send the entire resume and JD to an LLM and blindly accept its output.

---

# 2. AI/ML Pipeline

```text
Resume + JD
     ↓
Text Processing
     ↓
NLP Extraction
     ↓
Structured Information
     ↓
┌───────────────┬────────────────┐
│               │                │
▼               ▼                ▼
ATS Engine    ML Matching     Skill Gap
│               │                │
└───────────────┴────────────────┘
                ↓
         Analysis Results
                ↓
           Generative AI
                ↓
     Suggestions + Roadmap
```

---

# 3. Rule-Based Processing

Traditional programming will be used for information that can be extracted reliably through deterministic methods.

### Regex

Used for:

* Email
* Phone number
* URLs
* Dates

### Rule-Based Logic

Used for:

* File validation
* Section detection
* ATS calculations
* Score calculation
* Skill set comparison
* Required/preferred classification where possible

These methods provide predictable and explainable results.

---

# 4. NLP Processing

Technology:

**spaCy**

NLP will be used to process resume and JD text.

Tasks include:

* Tokenization
* Sentence segmentation
* Linguistic processing
* Named Entity Recognition
* Phrase matching
* Skill extraction
* Section processing

Example:

```text
"Rahul worked at ABC Technologies as a
Machine Learning Engineer."
```

NLP can help identify:

```text
Person → Rahul
Organization → ABC Technologies
Job Role → Machine Learning Engineer
```

---

# 5. Skill Extraction

TalentLens will maintain a structured skill taxonomy containing categories such as:

```text
Programming Languages
Frameworks
Libraries
Databases
Cloud
DevOps
Machine Learning
Data Science
Web Development
Cybersecurity
Tools
```

The system combines:

```text
Skill Dictionary
+
Phrase Matching
+
NLP
+
Semantic Similarity
```

to identify skills.

---

# 6. Skill Normalization

Different names for the same technology should be mapped to a canonical representation.

Example:

```text
ML
Machine Learning
machine-learning
```

↓

```text
Machine Learning
```

Another example:

```text
JS
JavaScript
```

↓

```text
JavaScript
```

This improves matching accuracy.

The normalization system should be maintainable and expandable.

---

# 7. Embeddings

The core ML component is semantic text representation.

A Sentence Transformer converts text into a numerical vector.

Conceptually:

```text
"Machine Learning Engineer"
          ↓
Embedding Model
          ↓
[0.21, -0.14, 0.73, ...]
```

The vector represents semantic information about the text.

---

# 8. Sentence Transformers

Initial technology:

**Sentence Transformers**

Initial model:

```text
all-MiniLM-L6-v2
```

The model is pretrained, so TalentLens does not need to train a Transformer from scratch.

The model will be used to generate embeddings for:

* Resume sections
* JD requirements
* Skills
* Responsibilities
* Project descriptions

---

# 9. Semantic Similarity

After generating embeddings for two pieces of text, TalentLens can calculate their similarity.

Example:

Resume:

> Developed predictive models using Python.

JD:

> Experience building machine learning systems using Python.

Their wording is different, but the semantic meaning is related.

The system generates:

```text
Resume → Vector A

JD → Vector B
```

Then:

```text
Vector A
    +
Vector B
    ↓
Cosine Similarity
    ↓
Semantic Similarity Score
```

A higher similarity indicates that the meanings are more closely related.

---

# 10. Section-Level Semantic Matching

TalentLens should not rely only on:

```text
Entire Resume ↔ Entire JD
```

Instead, compare relevant sections.

```text
Resume Experience
        ↕
JD Responsibilities

Resume Skills
        ↕
JD Required Skills

Resume Projects
        ↕
JD Responsibilities
```

This produces more useful matching signals.

---

# 11. Hybrid Matching Algorithm

The final Job Match Score should combine several signals.

Conceptually:

```text
Job Match Score =
    Skill Match
    +
    Keyword Coverage
    +
    Semantic Similarity
    +
    Experience Match
    +
    Education Match
    +
    Requirement Importance
```

The weights should be configurable.

For example:

```text
Skill Match          35%
Semantic Match       25%
Keyword Coverage     15%
Experience Match     15%
Education Match      10%
```

These are initial design values, not scientifically validated weights.

They should be evaluated and adjusted using test data.

---

# 12. ATS Analysis

ATS analysis will primarily be rule-based.

Possible components:

```text
Section Completeness
Keyword Coverage
Skill Coverage
Resume Structure
Readability
Formatting Compatibility
Experience Relevance
```

The result is:

```text
TalentLens ATS Compatibility Score
```

This should be different from:

```text
Job Match Score
```

### Example

A resume can have:

```text
ATS Compatibility = 92
Job Match = 61
```

This means the resume is well structured for ATS processing but is not a strong match for that particular job.

---

# 13. Skill Gap Analysis

The system compares:

```text
Required JD Skills
        -
Candidate Skills
        ↓
Skill Gap
```

But it should not only check exact words.

Example:

```text
JD:
Deep Learning

Resume:
PyTorch + Neural Networks
```

The system should recognize that the candidate has evidence related to deep learning.

However, semantic similarity should not automatically claim that the candidate has a specific technology.

For example:

```text
JD:
PyTorch

Resume:
TensorFlow
```

should not automatically become:

```text
PyTorch = Present
```

Instead:

```text
PyTorch
Status: Missing
Related Experience: TensorFlow
```

This distinction is important for accuracy.

---

# 14. Generative AI

Technology:

**Gemini API**

Generative AI will be used for tasks that require explanation and generation.

### Resume Suggestions

Input:

```text
Resume analysis
+
JD requirements
+
Detected weaknesses
```

Output:

* Improvement suggestions
* Better wording
* Summary recommendations
* Project-description recommendations

---

# 15. AI Hallucination Control

The LLM must not invent information.

For example, if the resume says:

```text
Built a Flask API.
```

the AI must not generate:

```text
Built a Flask API handling 10,000 requests per second.
```

unless the user provided that information.

AI output should be:

```text
LLM
 ↓
Structured JSON
 ↓
Validation
 ↓
Evidence checking where practical
 ↓
Frontend
```

---

# 16. Personalized Learning Roadmap

Input:

```text
Current Skills
+
Missing Skill
+
Target Role
+
JD Context
```

Example:

```text
Current:
Python
Machine Learning
TensorFlow

Missing:
PyTorch

Target:
ML Engineer
```

The LLM generates:

```text
PyTorch Fundamentals
        ↓
Tensors
        ↓
Datasets/DataLoaders
        ↓
Neural Networks
        ↓
Training
        ↓
Advanced Topics
        ↓
Practical Project
```

The roadmap should avoid teaching concepts the user already clearly knows.

---

# 17. AI/ML Responsibility Split

| Task                       | Method                      |
| -------------------------- | --------------------------- |
| Email extraction           | Regex                       |
| Phone extraction           | Regex                       |
| Section detection          | NLP + rules                 |
| Skill extraction           | NLP + skill taxonomy        |
| Skill normalization        | Rules + taxonomy            |
| ATS score                  | Rule-based engine           |
| Keyword coverage           | NLP / information retrieval |
| Semantic similarity        | Sentence Transformers       |
| Skill matching             | Rules + ML                  |
| Experience matching        | NLP + rules                 |
| Skill gap                  | Matching engine             |
| Resume suggestions         | Gemini                      |
| Roadmap                    | Gemini                      |
| Future interview questions | Gemini                      |

---

# 18. ML Evaluation

TalentLens should not assume that the ML pipeline is accurate.

Create a test dataset containing:

* Multiple resumes
* Multiple JDs
* Strong matches
* Partial matches
* Weak matches
* Different industries and roles

Evaluate:

### Skill Extraction

How often are skills correctly detected?

### Skill Gap

Are genuinely missing skills identified?

### Semantic Matching

Does a high similarity score actually represent relevant experience?

### Final Match Score

Does the score correspond reasonably to human judgment?

The evaluation results should be used to tune:

* Similarity thresholds
* Skill normalization
* Matching weights
* ATS weights

---

# 19. Important Limitation

TalentLens cannot know exactly how every company's proprietary ATS works.

Therefore:

**ATS Compatibility Score ≠ Actual Company ATS Score**

TalentLens provides an **estimated ATS compatibility assessment** based on transparent criteria.

Similarly:

**Job Match Score ≠ Probability of Getting Hired**

It measures compatibility between the provided resume and JD.

---

# 20. Final AI/ML Architecture

```text
                    RESUME
                       │
                       ▼
                Text Processing
                       │
                       ▼
                 NLP Pipeline
                       │
              ┌────────┴────────┐
              ▼                 ▼
        Skill Extraction    Sections
              │                 │
              └────────┬────────┘
                       │
                       ▼
                Structured Resume
                       │
                       │
                       │        JOB DESCRIPTION
                       │               │
                       │               ▼
                       │          NLP Pipeline
                       │               │
                       │               ▼
                       │          JD Structure
                       │               │
                       └───────┬───────┘
                               ▼
                       Matching Engine
                               │
             ┌─────────────────┼────────────────┐
             ▼                 ▼                ▼
        Skill Matching    Embeddings       Keywords
                               │
                               ▼
                      Semantic Similarity
                               │
                               ▼
                        Job Match Score
                               │
                               ▼
                         Skill Gap
                               │
              ┌────────────────┴────────────────┐
              ▼                                 ▼
       ATS Compatibility                  Gemini AI
              │                                 │
              │                    ┌────────────┴──────────┐
              │                    ▼                       ▼
              │               Suggestions              Roadmap
              │                    │                       │
              └────────────────────┴───────────────────────┘
                                   │
                                   ▼
                            FINAL REPORT
```

## Core Principle

TalentLens should be built around this principle:

> **Use deterministic methods when the answer should be deterministic, ML when semantic understanding is required, and generative AI when explanation or content generation is required.**

This separation is the foundation of the project's technical design.
