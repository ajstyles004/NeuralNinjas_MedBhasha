# NeuralNinjas_MedBhasha: Clinical NLP Pipeline for Bengali Medical Text
### COMSYS Hackathon-8 — Round 2: Modeling (NER, Clustering & Grounded RAG)
**Team Name:** Neural Ninjas  
**Track:** MedBhasha Clinical NLP Track  
**Submission Archive:** `NeuralNinjas_MedBhasha.zip`  
**GitHub Repository:** [https://github.com/ajstyles004/NeuralNinjas_MedBhasha](https://github.com/ajstyles004/NeuralNinjas_MedBhasha)

---

## 📌 Executive Overview

This repository contains the complete, reproducible end-to-end clinical NLP modeling pipeline developed by team **Neural Ninjas** for **MedBhasha Round 2**. The pipeline addresses three clinical intelligence tasks built on top of a deduplicated and validated Bengali clinical corpus:

1. **Clinical Named Entity Recognition (NER)**:
   - Sequence modeling via Conditional Random Fields (CRF) trained on token morphology, affix windows, and transitional constraints.
   - Generates exact Unicode code-point character offsets (`start`, `end`) on all 1,691 test notes across the 10 official schema categories.
2. **Unsupervised Phenotype Clustering & Graph Analytics**:
   - Extraction of entity co-occurrence profiles, dimensionality reduction via Truncated SVD, and clustering into 12 unsupervised phenotypes.
   - Internal validation self-checks: **Silhouette Score: 0.1678**, **Davies-Bouldin Index: 2.1281**.
   - Frequency distribution and relational entity co-occurrence heatmaps.
3. **Corpus-Grounded Clinical Question Answering (RAG)**:
   - Dual-corpus retrieval strictly bounded to internal documents (zero external API/web reliance) on all 30 official Bengali test queries.
   - Query execution across both the **"test"** corpus (1,691 notes) and our **"own"** corpus (280 deduplicated notes).
   - Structured, evidence-grounded answer synthesis preventing medical hallucinations.

---

## 📂 Repository & Submission Structure

The repository conforms strictly to the official MedBhasha Round 2 layout:

```
NeuralNinjas_MedBhasha/
├── README.md                            ← Comprehensive repository documentation
├── notebooks/
│   └── final_notebook.ipynb             ← Single runnable notebook for all 3 stages
├── predictions/
│   ├── ner_predictions.csv              ← Predicted entity spans with character offsets
│   ├── clusters.csv                     ← Unsupervised cluster IDs for test notes
│   ├── analysis/                        ← Frequency and co-occurrence charts & tables
│   │   ├── cluster_distribution.png
│   │   ├── entity_frequency.png
│   │   ├── entity_cooccurrence_heatmap.png
│   │   ├── entity_frequencies.csv
│   │   └── entity_cooccurrence_matrix.csv
│   └── rag_outputs.jsonl                ← 60 RAG entries (30 queries × "test" & "own")
├── data/
│   └── annotated_corpus.csv             ← Deduplicated Round 1 corpus (280 notes)
├── raw_data.csv                         ← Pre-annotation raw clinical text
├── validation_report.txt                ← Official validator output (0 Errors, 0 Warnings)
├── source_declaration.md                ← Source declaration & ethical compliance
├── eda_summary.pdf                      ← Round 1 exploratory data analysis report
├── requirements.txt                     ← Exact pinned environment dependencies
├── report.pdf                           ← Official Round 2 Technical Report (PDF)
├── round2_technical_report.md           ← Technical report (Markdown source)
└── NeuralNinjas_MedBhasha.zip           ← Complete submission zip archive
```

---

## 📊 Key Evaluation Metrics & Benchmarks

| Component | Task / Metric | Obtained Value | Target / Benchmark |
|---|---|---|---|
| **Corpus Deduplication** | Unique Notes / Tokens | **280 notes / 14,631 tokens** | > 10,000 unique token threshold (Met) |
| **Validation** | `validate.py` Syntax & BIO | **PASS (0 Errors, 0 Warnings)** | 100% compliant with 10-class schema |
| **Stage 1: NER** | Holdout F1 (micro/macro) | **1.0000 / 1.0000** | Strict span match (`seqeval`) |
| **Stage 1: NER** | Test Predictions Spans | **46,362 extracted entities** | Exact Unicode offsets (`start`, `end`) |
| **Stage 2: Clustering** | Silhouette Score | **0.1678** | Unsupervised cluster cohesion self-check |
| **Stage 2: Clustering** | Davies-Bouldin Index | **2.1281** | Distinct cluster boundary self-check |
| **Stage 3: RAG** | Retrieval Groundedness | **100% Corpus-Restricted** | Evaluated on all 30 queries × 2 corpora |

---

## 🏷️ Entity Schema (10 Classes)

All annotations and predictions adhere to the MedBhasha schema:
- `DIAGNOSIS`: Confirmed or suspected conditions (e.g., ডায়াবেটিস, নিউমোনিয়া).
- `SYMPTOM`: Subjective complaints or clinical signs (e.g., জ্বর, কাশি, বুকে ব্যথা).
- `MEDICATION`: Pharmaceutical names (e.g., মেটফরমিন, প্যারাসিটামল).
- `DOSAGE`: Quantitative strengths, frequencies, routes (e.g., ৫০০ মিগ্রা, দিনে দুইবার).
- `TEST`: Named investigations or imaging (e.g., ইসিজি, রক্ত পরীক্ষা).
- `TEST_RESULT`: Quantitative or qualitative findings (e.g., ১৪০/৯০, পজিটিভ).
- `BODY_SITE`: Anatomical structures (e.g., বুক, ফুসফুস, কিডনি).
- `PROCEDURE`: Interventional or surgical acts (e.g., এন্ডোস্কপি, অস্ত্রোপচার).
- `TEMPORAL`: Durations and follow-up intervals (e.g., গত তিন দিন ধরে).
- `NEGATION_UNCERTAINTY`: Negation markers and uncertainty cues (e.g., নেই, সন্দেহ).

---

## 🚀 Quickstart & Reproducibility

### 1. Environment Setup
```bash
git clone https://github.com/ajstyles004/NeuralNinjas_MedBhasha.git
cd NeuralNinjas_MedBhasha
pip install -r requirements.txt
```

### 2. Run the Unified Notebook
Open and run `notebooks/final_notebook.ipynb`:
```bash
jupyter notebook notebooks/final_notebook.ipynb
```
The notebook executes:
1. Training the CRF NER model on `data/annotated_corpus.csv`.
2. Extracting entity features and performing unsupervised phenotype clustering.
3. Querying the hybrid retrieval engine and generating grounded responses.

### 3. Re-run Pipeline via CLI
To re-generate all test predictions, clusters, analysis plots, and RAG outputs:
```bash
python pipeline/round2/official_round2_runner.py
```

---

## 📄 Deliverables Summary

- **Notebook:** [`notebooks/final_notebook.ipynb`](./notebooks/final_notebook.ipynb)
- **NER Predictions:** [`predictions/ner_predictions.csv`](./predictions/ner_predictions.csv)
- **Clusters:** [`predictions/clusters.csv`](./predictions/clusters.csv)
- **RAG Outputs:** [`predictions/rag_outputs.jsonl`](./predictions/rag_outputs.jsonl)
- **Technical Report:** [`report.pdf`](./report.pdf)
- **Submission Archive:** [`NeuralNinjas_MedBhasha.zip`](./NeuralNinjas_MedBhasha.zip)

---

**Team Neural Ninjas**  
*COMSYS Hackathon-8 — MedBhasha Clinical NLP Track*  
