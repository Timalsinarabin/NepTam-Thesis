# Nepali to Tamang NMT & In-Context Learning Evaluation

This repository evaluates Neural Machine Translation (NMT) performance across **0-Shot**, **1-Shot**, and **Few-Shot (5-Shot)** paradigms using the `ilprl-docse/NepTam20k-transformer-49.07M` base model, alongside fine-tuning experiments with `NLLB-200` and `mBART-50` on the NepTam parallel corpus.

---

## 1. Experimental Overview

* **In-Context Learning (ICL):** Evaluates `ilprl-docse/NepTam20k-transformer-49.07M` across 0-Shot, 1-Shot, and 5-Shot prompting setups.
* **Fine-Tuning:** Benchmarks `facebook/nllb-200-distilled-600M` and `ilprl-docse/NepTam20k-mBART-50-610.9M`.

---

## 2. Model Fine-Tuning & Parameter Optimization

* **Base Architectures Evaluated:** `facebook/nllb-200-distilled-600M` and `ilprl-docse/NepTam20k-mBART-50-610.9M`.
* **Hardware Footprint:** Optimized to run strictly within a **6 GB VRAM safety budget** on an NVIDIA RTX 3050 Laptop GPU.
* **LoRA Adaptation:** Full fine-tuning caused VRAM spikes during the cross-entropy loss phase. Fine-tuning via **LoRA** ($r = 16/32$, $\alpha = 16$) stabilized VRAM usage at **~2.35 GiB**.
* **Hyperparameter Setup:** Trained for **3 Epochs** with a max sequence length of **512 tokens**, **AdamW 8-bit** optimizer, and a **Linear Learning Rate Scheduler** ($2 \times 10^{-5}$).

---

## 3. Devanagari Proxy Token Strategy (mBART-50)

* **Challenge:** Tamang lacks a native pre-trained language code in mBART-50's original 50-language vocabulary.
* **Solution:** Used **`hi_IN` (Hindi - India)** as a Devanagari proxy token during tokenization and decoding.
* **Execution:** Forcing `forced_bos_token_id = tokenizer.lang_code_to_id["hi_IN"]` routes output generation through pre-existing Devanagari subword embeddings without requiring vocabulary expansion.

---

## 4. Interface Mismatch & Deployment Guidelines

* **Chat Interface Failures:** Sequence-to-Sequence (Seq2Seq) architectures (NLLB-200 / mBART-50) fail inside Causal LLM chat workbenches (e.g., Unsloth Studio, Open-WebUI) because chat template tags (`<|user|>`) scramble encoder self-attention and lack `forced_bos_token_id` support.
* **Deployment Recommendation:** Run inference directly via standard PyTorch scripts or deploy as a custom REST API using FastAPI / Flask.

---

## 5. Key Findings

* **0-Shot Inference:** Produces clean, accurate translations when passing pure source text natively to Seq2Seq architectures.
* **1-Shot & Few-Shot Prompting:** Causes output hallucination and token echoing because small Seq2Seq architectures do not support native In-Context Learning (ICL) prompt formats (`Nepali: ... \n Tamang: ...`).

---


## 6. Quick Start

1. **Clone the repository:**
   ```bash
   git clone [https://github.com/Timalsinarabin/NepTam-Thesis.git](https://github.com/Timalsinarabin/NepTam-Thesis.git)
   cd NepTam-Thesis
