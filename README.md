# Multilingual Transliteration Model (Asm–Hin–Mar)

This project implements a **multilingual transliteration system** that converts **romanized text into native Indic scripts**.

Supported languages:

- Assamese (asm)
- Hindi (hin)
- Marathi (mar)

The system is trained using the **Aksharantar dataset** and the **mT5-small transformer model**, and optimized using **CTranslate2** to significantly improve inference speed.

---

# Project Overview

Pipeline:

Aksharantar Dataset  
↓  
Data Preprocessing  
↓  
Language Token Injection  
↓  
mT5 Model Training  
↓  
Model Evaluation (CER)  
↓  
Model Optimization (CTranslate2)  
↓  
Deployment using Gradio / HuggingFace Spaces  

---

# Development Environment

This project was developed and trained using **Google Colab**.

Because of this:

- Training code exists inside **Jupyter Notebook (.ipynb)** files
- Directory structure may differ from a typical production repository
- Model artifacts are generated dynamically during notebook execution

---

# Setup Instructions

## 1 Install Dependencies

Run the following in Colab or local environment:

```bash
pip install datasets transformers accelerate sentencepiece evaluate jiwer sacrebleu
```

For model optimization:

```bash
pip install ctranslate2==4.4.0
```

---

# Dataset

Dataset used:

AI4Bharat Aksharantar Dataset

HuggingFace dataset:

ai4bharat/Aksharantar

Languages used:

| Language | Code |
|--------|------|
| Assamese | asm |
| Hindi | hin |
| Marathi | mar |


# Training Process

Model used:

google/mt5-small

The model is trained as a **sequence-to-sequence multilingual transformer**.

Each input contains a **language token** indicating the target language.

Example:

Input: `<2hin> saurav`  
Output: `सौरव`

Language tokens:

| Language | Token |
|--------|------|
| Assamese | `<2asm>` |
| Hindi | `<2hin>` |
| Marathi | `<2mar>` |

---

# Training Hyperparameters

| Parameter | Value |
|---|---|
| Model | mT5-small |
| Batch Size | 32 |
| Learning Rate | 8e-4 |
| Max Training Steps | 4500 |
| Warmup Steps | 100 |
| Weight Decay | 0.01 |
| Max Source Length | 32 |
| Max Target Length | 32 |
| Mixed Precision | fp16 |

Training framework:

HuggingFace Seq2SeqTrainer

Training environment:

Google Colab (NVIDIA T4 GPU)

---

# Evaluation Metric

The model is evaluated using **Character Error Rate (CER)**.

Formula:

CER = Edit Distance / Reference Length

Lower CER indicates better transliteration performance.

---

# Model Optimization with CTranslate2

To improve inference speed, the trained model was converted to **CTranslate2 format**.

Benefits:

- Smaller model size
- Faster inference
- Optimized runtime for GPU

---

# Benchmark Results

Performance comparison between PyTorch and optimized CTranslate2 model.

| Model | Size | Inference Speed |
|------|------|----------------|
| PyTorch mT5 | ~1.2 GB | ~20–25 req/sec |
| CTranslate2 | ~420 MB | ~70–80 req/sec |

Speed improvement:

≈ **3× faster inference with CTranslate2**

---

# Sample Outputs

## Hindi (hin)

| Input | Output |
|------|-------|
| saurav | सौरव |

---

## Marathi (mar)

| Input | Output |
|------|-------|
| saurav | सौरव |
---

## Assamese (asm)

| Input | Output |
|------|-------|
| saurav | চাৰিৰ |

---

# Repository Structure

Since the project was developed in **Google Colab**, the repository structure may differ from a typical software project.

Typical files include:

project/

├── Multilingual_Transliteration_Colab.ipynb  
└── README.md  

---


# Future Improvements

Potential improvements for this project include:

- Integrating **NVIDIA NeMo toolkit** for training and fine-tuning multilingual sequence-to-sequence models.
- Leveraging **NeMo Megatron models** to scale transliteration to larger multilingual architectures.
- Training on **larger Indic transliteration datasets** to improve accuracy and generalization.
- Supporting **additional Indic languages** such as Bengali, Tamil, Telugu, and Gujarati.
- Using **NeMo + TensorRT-LLM or Triton Inference Server or Riva** for optimized production deployment.
- Exploring **beam search and advanced decoding strategies** for higher transliteration accuracy.
