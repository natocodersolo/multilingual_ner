# Multilingual Named Entity Recognition

[![Live Demo](https://img.shields.io/badge/Live%20Demo-Streamlit-FF4B4B?style=flat-square&logo=streamlit&logoColor=white)](https://6hdjukgmcud7jdh3rtxihf.streamlit.app/)
[![Model](https://img.shields.io/badge/Model-Hugging%20Face-FFD21E?style=flat-square&logo=huggingface&logoColor=black)](https://huggingface.co/SoloCode/multilingual-ner)

> A transformer-based multilingual Named Entity Recognition system for extracting identity-related information from KYC-oriented text.

---

## Table of Contents

- [Origins and My Contributions](#origins-and-my-contributions)
- [Project Context](#project-context)
- [Live Demo](#live-demo)
- [Trained Model](#trained-model)
- [Overview](#overview)
- [Problem Statement](#problem-statement)
- [Objectives](#objectives)
- [Dataset](#dataset)
- [Why Dataset Merging Matters](#why-dataset-merging-matters)
- [BIO Labeling](#bio-labeling)
- [Methodology](#methodology)
- [Model](#model)
- [Application](#application)
- [System Architecture](#system-architecture)
- [Technology Stack](#technology-stack)
- [Repository Structure](#repository-structure)
- [Installation](#installation)
- [Usage](#usage)
- [Loading the Model Directly](#loading-the-model-directly)
- [Deployment](#deployment)
- [Limitations and Constraints](#limitations-and-constraints)
- [Potential Improvements](#potential-improvements)
- [Responsible Use](#responsible-use)
- [Acknowledgment](#acknowledgment)

---

## Origins and My Contributions

This project began as a group project at the **Ethiopian Information Network Security Agency (INSA) Summer Camp**, built together with Solomon Tsega, Esayas Adugna, Samuel Tafere, Nathan Solomon, Eba Leta, and Melat Endalamaw. The original group repository is [soltsega/multilingual_ner](https://github.com/soltsega/multilingual_ner).

This repository is my own copy of that group work, submitted as a personal project. The idea, dataset pipeline, model, and application come from the team effort.

**What I did in this version:**

- Rewrote and reorganized the README to document the project's methodology, architecture, limitations, and responsible-use considerations.
- Corrected the repository structure section to match the actual folders.

---

## Project Context

This project originated in the **INSA Summer Camp**.

It explores the application of **Natural Language Processing (NLP), multilingual machine learning, and transformer-based Named Entity Recognition (NER)** to a practical KYC-oriented information extraction problem.

The work covers the complete machine-learning workflow:

```
Dataset Preparation
       ↓
Dataset Standardization
       ↓
Dataset Merging
       ↓
Tokenization & BIO Labeling
       ↓
Model Fine-Tuning
       ↓
Evaluation
       ↓
Model Publishing
       ↓
Streamlit Deployment
```

The project therefore goes beyond building a user interface. It investigates the data, modeling, and deployment decisions required to turn annotated multilingual text into an interactive information-extraction system.

---

## Live Demo

The trained model is integrated into a deployed Streamlit application.

[**Open the Multilingual KYC NER Application**](https://6hdjukgmcud7jdh3rtxihf.streamlit.app/)

The application provides:

- Text input for NER inference
- Entity extraction
- Entity labels
- Confidence scores
- Annotated text visualization
- Methodology documentation
- Dataset documentation
- System limitations
- Future improvement directions
- System architecture

---

## Trained Model

The fine-tuned model is hosted on the Hugging Face Model Hub:

### [SoloCode/multilingual-ner](https://huggingface.co/SoloCode/multilingual-ner)

The model was published by the original team. It is not stored in this GitHub repository because the trained model files are large. The application retrieves the model from Hugging Face when the inference pipeline is initialized.

```python
MODEL_ID = "SoloCode/multilingual-ner"
```

---

## Overview

Know Your Customer (KYC) processes frequently involve information that is initially available as unstructured or semi-structured text.

A single text sequence may contain several pieces of identity-related information, such as:

- Personal names
- Identification information
- Dates
- Locations
- Organizations
- Other relevant entities

Named Entity Recognition provides an information-extraction layer capable of identifying relevant spans of text and assigning semantic labels to them.

The system uses a **multilingual transformer model fine-tuned for token classification**.

```
KYC Text
   ↓
Tokenization
   ↓
Multilingual Transformer
   ↓
Token-Level Predictions
   ↓
Entity Aggregation
   ↓
Structured Entity Output
```

---

## Problem Statement

Extracting structured information from unstructured KYC text is challenging because real-world text can vary considerably in:

- Language
- Writing style
- Spelling
- Formatting
- Entity representation
- Sentence structure
- Document quality

A rule-based system can address predictable patterns, but such systems become increasingly difficult to maintain when multiple languages and diverse textual forms are involved.

This project investigates whether a **multilingual transformer-based NER system** can learn useful entity representations from annotated data and provide a practical extraction interface.

---

## Objectives

1. Develop a multilingual NER system for KYC-oriented text.
2. Prepare and standardize annotated datasets for supervised NER.
3. Combine compatible datasets to increase training-data diversity.
4. Resolve differences in label representation between datasets.
5. Convert annotations into a consistent BIO-based representation.
6. Fine-tune a multilingual transformer model for token classification.
7. Evaluate the resulting model.
8. Publish the trained model on Hugging Face.
9. Build an interactive Streamlit application.
10. Document the methodology, limitations, and future development opportunities.

---

## Dataset

The quality of a Named Entity Recognition system is strongly dependent on the quality and coverage of its training data.

Multiple annotated datasets were investigated and combined to provide broader training coverage. Before combining them, their entity definitions, label schemas, annotation conventions, language distributions, data formats, and annotation boundaries were considered.

### Dataset Standardization

Different datasets may represent similar entities using different label names or annotation conventions. For example, one dataset might use `PER` while another uses `PERSON`. If these labels represent the same semantic category, they must be mapped into a consistent target schema before training, so the model receives coherent supervision.

---

## Why Dataset Merging Matters

A single dataset may provide limited coverage of languages, entity types, writing styles, entity variations, and contextual patterns. Combining compatible datasets can expose the model to a broader range of linguistic patterns.

However:

> More data does not automatically mean a better model.

Potential problems include:

- Different label definitions
- Different annotation boundaries
- Inconsistent annotation quality
- Class imbalance
- Language imbalance
- Duplicate examples
- Domain differences

Dataset merging must therefore be accompanied by **label alignment and data-quality analysis**.

---

## BIO Labeling

The training data uses the BIO-style representation for token-level entity labeling.

| Tag   | Meaning                          |
|-------|----------------------------------|
| `B-X` | Beginning of an entity of type X |
| `I-X` | Inside an entity of type X       |
| `O`   | Outside any entity               |

Example:

```
Solomon      B-PERSON
Tsega        I-PERSON
lives        O
in           O
Addis        B-LOCATION
Ababa        I-LOCATION
```

This lets the model learn both which tokens belong to entities and where each entity begins and ends.

---

## Methodology

1. **Dataset selection:** chosen for task relevance, language coverage, entity coverage, and availability of annotations.
2. **Dataset analysis:** differences in labels, definitions, formats, language distribution, and boundaries were examined to decide whether sources could be safely combined.
3. **Label alignment:** equivalent labels were mapped into a common schema to avoid contradictory supervision.
4. **Dataset merging:** standardized datasets were combined into a unified training corpus, aiming for diversity rather than raw record count.
5. **BIO conversion:** entity annotations were converted into token-level BIO labels.
6. **Tokenization:** text was processed with the tokenizer of the selected multilingual transformer. Because words may split into several subword tokens, labels must stay aligned with the resulting sequence.
7. **Model fine-tuning:** a pretrained multilingual transformer was fine-tuned for token classification.
8. **Evaluation:** precision, recall, and F1-score, ideally broken down by language, entity type, dataset source, and error category.
9. **Model publication:** the trained model was published to Hugging Face.
10. **Application development:** a Streamlit application exposes the model through an interactive interface.

---

## Model

The project uses a **multilingual transformer-based token classification model** fine-tuned for NER.

```
Input Text
    ↓
Tokenizer
    ↓
Transformer Encoder
    ↓
Token Classification Head
    ↓
Entity Predictions
```

The application aggregates token-level predictions into readable entity spans.

---

## Application

The Streamlit application provides an interactive interface around the trained model.

- **NER Analysis:** enter text and view detected entities, entity types, confidence scores, and annotated text.
- **Methodology:** dataset preparation, merging, label alignment, BIO labeling, tokenization, fine-tuning, and evaluation.
- **Dataset:** why the datasets were selected and why merging matters.
- **Limitations:** the constraints of the current system.
- **Future Improvements:** organized around data, model architecture, document processing, evaluation, and deployment.
- **Architecture:** how the components interact.

---

## System Architecture

```
                    User
                     |
                     v
          +---------------------+
          |    Streamlit UI     |
          +----------+----------+
                     |
                     v
          +---------------------+
          |   Input Processing  |
          +----------+----------+
                     |
                     v
          +---------------------+
          |      Tokenizer      |
          +----------+----------+
                     |
                     v
          +---------------------+
          | Multilingual NER    |
          | Transformer Model   |
          +----------+----------+
                     |
                     v
          +---------------------+
          | Entity Aggregation  |
          +----------+----------+
                     |
                     v
          +---------------------+
          | Results & Annotation|
          +---------------------+
```

Application code and model are deliberately separated:

```
GitHub
   |
   └── Streamlit Application
            |
            v
     Hugging Face Hub
            |
            └── Fine-tuned Model
```

---

## Technology Stack

- **Machine learning:** Python, PyTorch, Hugging Face Transformers and Hub
- **Data processing:** Python preprocessing, dataset standardization, BIO labeling, tokenization, label alignment
- **Application:** Streamlit
- **Version control:** Git, GitHub
- **Model hosting:** Hugging Face Model Hub
- **Deployment:** Streamlit Community Cloud

---

## Repository Structure

```
multilingual_ner/
├── .devcontainer/
├── .github/workflows/
├── doc/
├── notebooks/
├── src/
│   └── app.py
├── .gitignore
├── requirements.txt
└── README.md
```

Large trained model files are not stored in this repository.

---

## Installation

### 1. Clone the repository

```bash
git clone <your-repository-url>
cd multilingual_ner
```

### 2. Create a virtual environment

Python 3.12 is recommended.

**Windows / Git Bash**

```bash
python -m venv .venv
source .venv/Scripts/activate
```

**Linux / macOS**

```bash
python3.12 -m venv .venv
source .venv/bin/activate
```

### 3. Install dependencies

```bash
pip install -r requirements.txt
```

---

## Usage

Run the Streamlit application:

```bash
streamlit run src/app.py
```

Enter a text sample in the NER Analysis section and select **Analyze Text**.

```
Input → Tokenization → Model Inference → Entity Aggregation → Confidence Estimation → Visualization
```

---

## Loading the Model Directly

```python
from transformers import (
    AutoTokenizer,
    AutoModelForTokenClassification,
    pipeline,
)

MODEL_ID = "SoloCode/multilingual-ner"

tokenizer = AutoTokenizer.from_pretrained(MODEL_ID)
model = AutoModelForTokenClassification.from_pretrained(MODEL_ID)

ner = pipeline(
    "token-classification",
    model=model,
    tokenizer=tokenizer,
    aggregation_strategy="simple",
)

text = "Your text goes here."
print(ner(text))
```

---

## Deployment

The application is deployed on **Streamlit Community Cloud**.

```
GitHub Repository → Streamlit Community Cloud → Streamlit Application → Hugging Face Model
```

Live deployment: [https://6hdjukgmcud7jdh3rtxihf.streamlit.app/](https://6hdjukgmcud7jdh3rtxihf.streamlit.app/)

The model is loaded as a cached application resource so that normal Streamlit reruns do not reconstruct it.

---

## Limitations and Constraints

- **Dataset coverage:** performance depends on the languages, entity types, styles, and domains in the training data. Underrepresented languages or entity types may receive weaker supervision.
- **Dataset compatibility:** merged datasets may have inconsistent annotation policies or boundaries, which can affect learning.
- **Domain shift:** real KYC text can contain abbreviations, formatting variations, mixed languages, OCR errors, unusual names, incomplete information, and structured fields embedded in free text.
- **OCR:** the model operates on text. If text comes from scanned documents, OCR errors propagate into the extracted entities.
- **Confidence scores:** high confidence reflects the model's certainty, not the factual correctness or authenticity of the information.
- **KYC verification:** NER is an information-extraction component. It does not perform identity verification, document authentication, fraud detection, biometric verification, or database verification.
- **Compute:** transformer models can require significant memory and compute, which is why the model is hosted on Hugging Face.

---

## Potential Improvements

**Data**
- Expand KYC-specific multilingual datasets and add underrepresented languages
- Add more entity types and improve annotation consistency
- Detect duplicates and near-duplicates, and address class imbalance

**Model**
- Try alternative multilingual architectures and domain-adaptive pretraining
- Parameter-efficient fine-tuning, distillation, and quantization

**Document understanding**

```
Document → OCR → Layout Understanding → Text Extraction → Multilingual NER → Entity Normalization → Structured KYC Information
```

**Evaluation**
- Overall and per-language / per-entity precision, recall, and F1
- Cross-domain evaluation, confusion analysis, and error categorization

**Production**

```
Frontend → Backend API → NER Model Service → Validation / Verification Services
```

---

## Responsible Use

This project is for **research, educational, and demonstration purposes**.

NER predictions should not be treated as authoritative identity information. In a real KYC environment, predictions should be combined with appropriate validation, identity verification, security controls, privacy protections, human review, and audit mechanisms.

The model should not be the sole basis for high-impact decisions about an individual's identity.

---

## Acknowledgment

I would like to thank the **Ethiopian Information Network Security Agency (INSA)** for the Summer Camp environment and the opportunity to work on an applied machine-learning project, and my original teammates (Solomon Tsega, Esayas Adugna, Samuel Tafere, Nathan Solomon, Eba Leta, and Melat Endalamaw) for the collaboration this work grew out of.
