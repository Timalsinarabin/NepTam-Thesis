# Nepali to Tamang NMT: In Context Learning

This repository evaluates the performance of the **ilprl-docse/NepTam20k-transformer-49.07M** model across **0-Shot**, **1-Shot**, and **Few-Shot (5-Shot)** translation paradigms on the NepTam parallel corpus.

## Key Findings
- **0-Shot Inference:** Produces clean, accurate translations when passing pure source text natively.
- **1-Shot & Few-Shot Prompting:** Causes output hallucination and token echoing because small Seq2Seq architectures do not support native In-Context Learning (ICL) prompt formats (`Nepali: ... \n Tamang: ...`).

## Repository Structure
- `Nepali_Tamang_InContext_Learning_Analysis.ipynb` — Main experimental notebook containing pipeline setup, model loading, and comparative evaluation.
- `EVALUATION_REPORT.md` — Detailed analysis of findings and architectural causes.

## Quick Start
1. Clone the repository:
   ```bash
   git clone [https://github.com//Timalsinarabin/NepTam-Thesis.git](https://github.com//Timalsinarabin/NepTam-Thesis.git)
   cd NepTam-Thesis
