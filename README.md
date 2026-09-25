# Depression Severity Detection from Social Media: A Comparative Analysis of BERT Models

## Project Purpose

This project compares **MentalBERT**, **RoBERTa**, and **MentalRoBERTa** for automatically classifying depression severity from social media posts. It has two goals: (1) systematically compare the three models' classification performance across two datasets, and (2) measure how different text preprocessing steps affect model performance through an ablation study.

## Datasets

### DS1 — Kayalvizhi & Durairaj (2022), Reddit
- **Size:** 8,891 posts (7,112 train / 1,779 test)
- **Labels:** 3 classes — Not Depressed, Moderate, Severe
- **Annotation:** 2 experts, manual labeling, Cohen's Kappa = 0.686 (Substantial Agreement)
- **Balance:** Imbalanced (Moderate 63%, Not Depressed 28%, Severe 9%) → balanced with SMOTE
- **Link:** https://arxiv.org/abs/2202.03047

### DS2 — KUAS-ubicomp (Priyadarshana et al., 2023), Reddit + Twitter
- **Size:** 41,859 posts (33,487 train / 8,372 test)
- **Labels:** 4 classes based on the BDI-3 clinical scale — Minimal, Mild, Moderate, Severe
- **Annotation:** Existing labels re-evaluated by 3 annotators, Cohen's Kappa = 0.68–0.75
- **Balance:** Balanced
- **Reference:** Priyadarshana, Y. H. P. P., Liang, Z., & Piumarta, I. (2023). *HelaDepDet: A novel multi-class classification model for detecting the severity of human depression.* LNCS, 14199, 3–18.

*Both datasets are also aggregated in the community repository: https://github.com/bucuram/depression-datasets-nlp*

## Results

**DS1 (Macro F1):** RoBERTa 0.84 · MentalBERT 0.84 · **MentalRoBERTa 0.86**

**DS2 (F1-micro):** MentalBERT 0.7321 · RoBERTa 0.7326 · **MentalRoBERTa 0.7465**

MentalRoBERTa achieved the best score on every metric in both datasets, confirming the benefit of domain-adaptive pretraining. RoBERTa and MentalBERT differed by only 0.0005 on DS2 — a strong general-purpose model can nearly match a domain-specific one. All models clearly beat the literature baseline HelaDepDet (F1 = 0.66).

## Preprocessing Ablation (MentalRoBERTa, DS2)

10 cumulative preprocessing steps were tested against the raw-text baseline (F1 = 0.7678):

| Step | F1-micro | ΔF1 |
|---|---|---|
| Baseline (raw text) | 0.7678 | — |
| + Character-repeat normalization ★ | 0.7718 | +0.0040 |
| + Unicode normalization ★ | 0.7681 | +0.0003 |
| + Punctuation normalization | 0.7580 | −0.0098 |
| + Whitespace normalization | 0.7580 | −0.0098 |
| Full pipeline (all steps) | 0.7650 | −0.0028 |

Only character-repeat and Unicode normalization improved results. Aggressive cleaning (punctuation, whitespace) hurt performance, showing that social media writing style carries meaningful emotional signal. The best configuration (baseline + these two steps only) reached **F1 = 0.7576**, the highest score overall.

---
*Dept. of Computer Engineering, Kocaeli University — Aimeerim Muratbek Kyzy*