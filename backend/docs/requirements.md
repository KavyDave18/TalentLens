# TalentLens — Requirements

## 1. Project Overview

**TalentLens** is an AI-powered Resume Intelligence and Career Assistant designed for students, fresh graduates, and job seekers.

The system allows users to provide their resume and a target Job Description (JD). TalentLens analyzes both documents using NLP, machine learning, rule-based techniques, and generative AI to provide an ATS compatibility score, job match score, skill-gap analysis, resume improvement suggestions, and personalized learning roadmaps.

The initial version will be **stateless**. Users will not need to create an account, and no database will be used.

---

## 2. Problem Statement

Job seekers often face several problems when applying for jobs:

* They do not know whether their resume is ATS-compatible.
* They do not know how well their resume matches a specific job.
* Simple keyword-based resume checkers fail to understand context and semantic similarity.
* Candidates may not know which required skills they are missing.
* Generic resume suggestions are often not specific to the target job.
* Candidates may identify a skill gap but not know what to learn next or where to start.
* Interview preparation is often generic rather than personalized.

TalentLens aims to solve these problems through a combination of NLP, machine learning, and generative AI.

---

## 3. Target Users

TalentLens is designed primarily for:

* College students
* Fresh graduates
* Job seekers
* Candidates applying for internships
* Professionals targeting a new role

---

## 4. Proposed Solution

The user provides:

**Resume + Job Description**

TalentLens processes the input through:

**Document Processing → NLP → ATS Analysis → Semantic Matching → Skill Gap Analysis → AI Recommendations**

The system produces:

* ATS Compatibility Score
* Job Match Score
* Matching Skills
* Missing/Weak Skills
* Keyword Gaps
* Resume Improvement Suggestions
* Personalized Learning Roadmap

---

# 5. Functional Requirements

## 5.1 Resume Input

The system shall allow users to:

* Paste resume text.
* Upload PDF resumes.
* Upload DOCX resumes.
* Replace the uploaded resume.
* Validate supported file types.

---

## 5.2 Job Description Input

The system shall allow users to:

* Paste a Job Description.
* Upload a JD as PDF.
* Upload a JD as DOCX.
* Replace the provided JD.
* Validate the input.

---

## 5.3 Resume Parsing

The system shall extract relevant information from resumes, including:

* Name
* Email
* Phone number
* Location
* Professional summary
* Skills
* Programming languages
* Frameworks
* Libraries
* Databases
* Cloud technologies
* Tools
* Education
* Work experience
* Projects
* Certifications
* GitHub
* LinkedIn
* Portfolio

The parser should handle variations in resume structure as reliably as possible.

---

## 5.4 Job Description Analysis

The system shall analyze the JD and identify:

* Job title
* Company name when available
* Required skills
* Preferred skills
* Technologies
* Responsibilities
* Experience requirements
* Education requirements
* Domain-specific requirements
* Soft skills
* Seniority level when identifiable

Required and preferred skills should be treated differently during matching.

---

## 5.5 ATS Compatibility Analysis

TalentLens shall calculate a **TalentLens ATS Compatibility Score** between 0 and 100.

The score may consider:

* Resume structure
* Section completeness
* Keyword coverage
* Skill coverage
* Experience relevance
* Readability
* Formatting compatibility
* Contact information

The score must be explainable through a breakdown rather than being an unexplained number.

The system must not claim that the score represents the actual ATS score of a particular company.

---

## 5.6 Resume-JD Matching

TalentLens shall calculate a separate **Job Match Score**.

The matching system should consider:

* Exact skill matches
* Normalized skill matches
* Semantic skill similarity
* Keyword coverage
* Experience relevance
* Education relevance
* Responsibility similarity
* Requirement importance

The system should not depend solely on exact keyword matching.

---

## 5.7 Skill Gap Analysis

The system shall compare the candidate's detected capabilities with the requirements of the JD.

Skills should be classified where possible as:

* Strong
* Moderate
* Weak
* Missing

For each important skill gap, TalentLens should provide an explanation based on evidence from the resume and JD.

---

## 5.8 Resume Improvement Suggestions

TalentLens shall provide actionable suggestions related to:

* Professional summary
* Skills section
* Project descriptions
* Experience descriptions
* Keywords
* Resume structure
* Missing relevant information
* Weak descriptions

The system must never invent:

* Work experience
* Achievements
* Metrics
* Technologies
* Certifications
* Responsibilities

Suggestions must remain grounded in the information provided by the user.

---

## 5.9 Personalized Learning Roadmap

Every important missing skill should be clickable.

Selecting a skill should open a dedicated roadmap.

The roadmap should contain:

* Why the skill is relevant to the target job
* Candidate's current related knowledge
* Prerequisites
* Beginner topics
* Intermediate topics
* Advanced topics
* Practical projects
* Recommended learning resources

The roadmap should be personalized based on the candidate's existing skills and target role.

---

## 5.10 Analysis Results

The results page shall display:

* ATS Compatibility Score
* ATS score breakdown
* Job Match Score
* Matching skills
* Missing/weak skills
* Keyword gaps
* Resume strengths
* Resume weaknesses
* AI suggestions
* Recommended skills to learn

---

# 6. Non-Functional Requirements

## Performance

* Analysis should complete within a reasonable time.
* The UI should show real analysis progress.
* The system should avoid unnecessary API calls.

## Accuracy

* NLP extraction should be tested using multiple resumes.
* Matching should combine semantic and deterministic techniques.
* ATS scoring should be explainable.
* AI output should be validated before being displayed.

## Usability

* Simple resume/JD input workflow.
* Clear results.
* Responsive interface.
* Easy navigation between analysis and roadmaps.

## Reliability

The system should gracefully handle:

* Invalid files
* Empty input
* Poorly formatted resumes
* API failures
* LLM failures
* Parsing failures

---

# 7. MVP Scope

The initial MVP includes:

1. Resume input
2. JD input
3. Resume parsing
4. JD analysis
5. NLP-based skill extraction
6. ATS compatibility scoring
7. Semantic resume-JD matching
8. Job match score
9. Skill gap analysis
10. AI resume suggestions
11. Personalized learning roadmap
12. Results dashboard

---

# 8. Out of Scope for MVP

The initial version will NOT include:

* User accounts
* Authentication
* Database
* Resume history
* Resume version tracking
* Recruiter dashboard
* Job application automation
* Payment system
* Company recruitment tools
* AI mock interviews

These can be added in future versions.

---

# 9. Future Scope

Possible future improvements include:

* User accounts and database
* Resume history
* Resume version comparison
* Resume tailoring for specific jobs
* AI mock interviews
* Resume-based interview questions
* Company-specific interview preparation
* AI evaluation of interview answers
* Career recommendations
* Verified learning-resource recommendations
* Improved skill ontology
* More advanced ML matching models

---

# 10. Success Criteria

TalentLens will be considered successful when a user can:

**Provide a Resume + JD**

↓

**Receive an explainable ATS Compatibility Score**

↓

**Receive a meaningful Job Match Score**

↓

**Understand which skills match and which are missing**

↓

**Receive evidence-based resume improvement suggestions**

↓

**Click a missing skill**

↓

**Receive a personalized learning roadmap**

The system should provide meaningful results across different resumes and job descriptions rather than relying on hardcoded or keyword-only analysis.
