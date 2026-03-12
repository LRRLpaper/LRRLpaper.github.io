---
layout: default
title: LLM Rubrics for Representation Learning
---

# LLMs can construct powerful representations for sample-efficient supervised learning

**Ilker Demirel, Larry Shi, Zeshan Hussain, David Sontag**

---

## Overview

Many real-world datasets contain heterogeneous inputs such as structured records, time-series, and free text. Designing effective representations for such data is often the main bottleneck in supervised learning pipelines, requiring substantial domain expertise and feature engineering.

In this paper, we show how large language models (LLMs) can automate powerful input representation design.

We introduce an agentic pipeline where an LLM analyzes text-serialized examples and constructs structured rubric representations that improve downstream learning.

The key idea is to transform naive text representations via **rubrics** that systematically extract predictive evidence from complex records.

---

## Method

### Text Serialization

Structured inputs (e.g., electronic health records) are first converted into a naive text representation.

Example components include:

- Patient demographics
- Detailed visit histories
- Diagnoses, medications, procedures, labs

These text serializations serve as the starting point for representation learning.

---

### Rubric Construction

The LLM analyzes a **small, diverse set of examples in-context** and produces a **rubric** describing what evidence to extract.

A rubric acts like a programmatic specification for representation construction.

Example rubric fields may include:

- comorbidities
- recent symptoms
- medication history
- diagnostic indicators
- relevant lab trends

---

### Two Types of Rubrics

#### Global Rubrics

A **single shared rubric** is learned from a subset of examples and then applied to all inputs.

Properties:

- fixed schema
- easy to audit
- deterministic parsing
- inexpensive at inference time

After construction, global rubrics can be applied via **simple parsing scripts without additional LLM calls**.

---

#### Local Rubrics

A **task-conditioned rubric is generated per example**.

These resemble structured summaries and are produced via LLM prompting.

Properties:

- more flexible
- potentially higher fidelity
- requires an LLM call per example

---

### Downstream Modeling

The rubric outputs can be used in two ways:

1. **Text embeddings**
   - rubric output passed through an embedding model
   - logistic regression classifier trained on embeddings

2. **Tabular representation**
   - rubric fields converted into structured features
   - compatible with standard ML models

---

## Experimental Evaluation

### Dataset

We evaluate on the **EHRSHOT benchmark**, which contains:

- **6,739 patients**
- longitudinal EHR data
- shared splits across tasks

EHRSHOT defines **15 clinical prediction tasks** across four categories:

1. Operational outcomes
2. Assignment of new diagnoses
3. Anticipation of laboratory results
4. Chest X-ray findings

---

### Baselines

We compare against strong baselines:

**Structured models**

- Count-GBM (gradient boosting on count features)
- CLMBR-T clinical foundation model pretrained on **2.57M patients**

**LLM pipelines**

- Naive text serialization embeddings
- Zero-shot chain-of-thought prompting

---

## Results

Across the 15 tasks:

- **Rubric representations outperform all baselines on average.**
- Gains are especially strong for:
  - diagnosis prediction
  - lab result anticipation

Rubric methods beat:

- count-feature models
- naive text embeddings
- a clinical foundation model trained on orders of magnitude more data.

Key observations:

- Global rubrics achieve strong performance even with small labeled datasets.
- Local rubrics perform similarly overall but require higher inference cost.
- Operational outcome tasks remain slightly stronger for CLMBR-T.
