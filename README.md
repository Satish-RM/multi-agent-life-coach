# Multi-Agent Life Coach – LangFlow Prototypes

This repository accompanies the MSc Data Science dissertation:

> **“Design and Evaluation of Multi-Agent Conversational Life Coaches Grounded in Behavioural Psychology”**  
> University of Surrey, 2024–2025  
> Author: Satish Ranganathan Mohan

The project explores how to design and evaluate **multi-agent conversational “life coach” systems** using LangFlow, Retrieval-Augmented Generation (RAG), and psychologically grounded evaluation.

Sixteen prototypes (v1.1 … v8.2) were developed, evolving from simple single-agent prompts to **multi-agent architectures** combining:

- **Cognitive Behavioural Therapy (CBT)**
- **Performance psychology**
- **Cultural wisdom from the Thirukkural**

and evaluated with adapted **CARE** (Communication, Autonomy, Respect, Empathy) and **Psychological Realism** rubrics, plus readability and NLP metrics.

---

## 1. Project goals

This repo aims to:

1. **Showcase the research artefact** behind the dissertation, including the prototype designs and evaluation pipeline.
2. Provide **exported LangFlow flows** that demonstrate:
   - Single-agent baselines
   - RAG-enhanced agents
   - Multi-agent aggregators (blend and bias modes)
3. Document an **evaluation framework** for psychological chatbots using:
   - CARE
   - Psychological Realism
   - Readability + NLP metrics (repetition, consistency, etc.)

At this stage the repository is **research-focused**. It is not a polished product or a clinical tool.

---

## 2. High-level architecture

The prototypes were built in **LangFlow** on top of **LangChain**, using a modular pipeline:

- **Chat Input** – user message entry
- **Prompt blocks** – system messages + templates that define each agent’s “voice”
- **SplitText** – chunking texts into ~450-character segments with 80-character overlap for RAG
- **AstraDB** – vector store for CBT, performance psychology, and Thirukkural corpora
- **Stringify parser** – preserve text integrity in retrieval
- **Message History** – short-term memory within a session
- **Model backends**:
  - General foundation model via **OpenRouter** (e.g. Qwen)
  - Fine-tuned **alientelligence/lifecoach** via Ollama for coaching-style dialogue
- **Aggregator** – coordinates multiple agents (CBT + Performance Psychology, optionally Thirukkural) in blend or bias modes

Conceptually, there are three core agents:

- **CBT Agent** – cognitive restructuring, automatic thoughts, behavioural activation
- **Performance Agent** – goal setting, motivation, habit formation, productivity
- **Cultural Agent (Thirukkural)** – culturally resonant moral and behavioural guidance

The **Aggregator Agent** acts as a meta-layer that:

- Monitors emotional tone and problem type
- Chooses whether CBT or performance framing is primary
- Ensures smooth transitions between frameworks

---

## 3. Prototype evolution

The dissertation developed **16 prototypes** (v1.1 … v8.2):

- **v1.x** – Single agent, no RAG, no memory  
  *Baseline: prompt-only “friendly coach”*
- **v2.x** – Single agent + **Message History**  
  *Adds in-session memory for name and context recall.*
- **v3.x–v4.x** – **RAG + memory** over the Thirukkural and other sources  
  *Culturally grounded, but single-framework.*
- **v5.x–v6.x** – **Multi-agent with Aggregator (blend mode)**  
  *Combines CBT + Performance perspectives in one response.*
- **v7.x–v8.x** – **Multi-agent with Aggregator (bias mode)**  
  *Dynamically leans towards CBT *or* Performance depending on context.*

Each prototype was evaluated on a fixed benchmark of 5 prompts (Q1–Q5) related to dissertation anxiety, self-worth, introspection, milestones, and memory of the user’s name.

---

## 4. Evaluation framework

### 4.1 CARE

Adapted from the Consultation and Relational Empathy measure, CARE is used here as:

- **Communication** – clarity and coherence
- **Autonomy** – supports user’s agency
- **Respect** – non-judgemental, validating tone
- **Empathy** – emotional attunement and warmth

Each dimension is rated on a 1–5 scale for each response.

### 4.2 Psychological Realism

A bespoke rubric focusing on:

- **Groundedness** – alignment with CBT/performance/Thirukkural frameworks
- **Cognitive Insight** – illuminating patterns and alternatives
- **Emotional Resonance** – does it *feel* like a believable human response?
- **Forward Movement** – helps the user move somewhere, not just talk in circles

Again, each on a 1–5 scale.

### 4.3 Readability & NLP metrics

To go beyond “vibes”, the project also measures:

- **Word count**
- **Flesch Reading Ease**
- **Flesch–Kincaid Grade Level**
- **N-gram repetitiveness** (bigrams/trigrams)
- **Consistency** (cosine similarity across multiple runs for the same prompt)

A key empirical finding: **the best responses cluster around**:

- **70–100 words**
- **Flesch–Kincaid Grade Level ≈ 7–9**
- **Flesch Reading Ease ≈ 65–75**

i.e. concise, accessible, but not oversimplified.

---

## 5. What’s in this repo?

Planned contents:

- `report/dissertation.pdf` – full MSc thesis (for those who want the deep dive)
- `report/dissertation-summary.md` – 1–2 page summary of aims, methods, and findings
- `flows/` – exported LangFlow pipelines:
  - `v1_baseline.json` – single agent, no RAG
  - `v3_thirukkural_rag.json` – RAG + Thirukkural
  - `v6_blend_aggregator.json` – CBT + Performance, blend mode
  - `v8_bias_aggregator.json` – CBT + Performance, bias mode
- `data/sample_prompts.json` – benchmark prompts Q1–Q5
- `notebooks/` – example notebooks for:
  - CARE/Realism scoring template
  - Readability + NLP metric calculation
- `docs/` – architecture, evaluation details, and future work.

> **Note:** The original implementation was built in **LangFlow** with AstraDB and external model endpoints (OpenRouter, Ollama). This repo focuses on documentation and reproducibility of the *design and evaluation*, rather than shipping a production-ready API.

---

## 6. How to use this repo

Right now, this repo is mainly for **reading and reuse**, not “pip install and deploy to production”.

Suggested uses:

- **Researchers / students**  
  - Inspect the LangFlow graphs and recreate or extend them in your own environment.
  - Reuse the evaluation framework (CARE + Realism + metrics) for your own psychological chatbots.

- **Engineers / builders**
  - Port the LangFlow designs into pure code (LangChain, Haystack, etc.).
  - Swap in different models or knowledge bases while keeping the same aggregator logic.

- **Supervisors / interviewers**
  - See concrete evidence of:
    - multi-agent architecture design,
    - RAG setup with real corpora,
    - psychologically-informed evaluation.

---

## 7. Limitations & ethics

This work explicitly **does not** claim to replace human therapists or coaches. The dissertation highlights several limitations:

- Responses can be **repetitive** or **overly generic**.
- Cultural grounding via the Thirukkural is meaningful but sometimes **contextually imprecise**.
- Multi-agent aggregation can:
  - Improve richness (blend mode),
  - But also cause **verbosity or outright failure** when orchestration breaks (3.3% null outputs in aggregator prototypes).

**This repository is for research and educational purposes only.  
It is not a medical device and must not be used for emergency or crisis support.**

If you are in distress, please contact appropriate local mental health services or emergency support.

---

## 8. Future directions

Some natural extensions:

- Reimplement the LangFlow pipelines as **fully code-based agents** (e.g. LangGraph).
- Integrate **user-level memory** (longitudinal coaching), not just short-term session history.
- Expand cultural corpora beyond the Thirukkural to test cross-cultural generalisation.
- Run **larger, user-facing evaluations** to test engagement, trust, and alliance over time.

See [`docs/future-work.md`](docs/future-work.md) for more detail (coming soon).

---
