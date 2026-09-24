# mental_health_research
# Depression Severity Detection from Social Media: A Comparative Analysis of BERT Models

Comparison of **MentalBERT**, **RoBERTa**, and **MentalRoBERTa** for classifying depression severity from social media posts, across two datasets.

**Author:** Aimeerim Muratbek Kyzy — Dept. of Computer Engineering, Kocaeli University

---

## Overview

Depression affects 300M+ people worldwide, yet traditional diagnosis is slow, costly, and stigma-prone — leaving ~60% undiagnosed. Social media (Reddit, Twitter) offers a low-cost signal for early detection. This project fine-tunes three transformer models on two datasets to automatically classify depression severity, and runs a preprocessing ablation study.

## Goals

1. Compare MentalBERT, RoBERTa, and MentalRoBERTa on depression severity classification across two datasets.
2. Quantify the effect of text preprocessing steps via ablation.

## Datasets

| | DS1 (Kayalvizhi & Durairaj, 2022) | DS2 (KUAS-ubicomp, 2023) |
|---|---|---|
| Source | Reddit | Reddit + Twitter |
| Size | 8,891 posts | 41,859 posts |
| Train/Test | 7,112 / 1,779 | 33,487 / 8,372 |
| Labels | 3: Not Depressed, Moderate, Severe | 4 (BDI-3): Minimal, Mild, Moderate, Severe |
| Cohen's Kappa | 0.686 | 0.68–0.75 |
| Class balance | Imbalanced (SMOTE applied) | Balanced |

## Models

| Model | Params | Notes |
|---|---|---|
| RoBERTa | ~125M | General-purpose baseline |
| MentalBERT | ~110M | BERT pretrained on mental-health subreddits |
| MentalRoBERTa | ~125M | RoBERTa + mental-health domain pretraining |

**Setup:** Fine-tuned with AdamW, lr=2e-5. MentalRoBERTa: 256 tokens/5 epochs; others: 128 tokens/3 epochs.

## Results

**DS1 (Macro F1):** RoBERTa 0.84 · MentalBERT 0.84 · **MentalRoBERTa 0.86**

**DS2 (F1-micro):** MentalBERT 0.7321 · RoBERTa 0.7326 · **MentalRoBERTa 0.7465**

MentalRoBERTa led on every metric in both datasets, confirming the benefit of domain-adaptive pretraining. RoBERTa and MentalBERT differed by only 0.0005 on DS2, showing a strong general model can nearly match a domain-specific one.

## Preprocessing Ablation (MentalRoBERTa, DS2)

Tested 10 cumulative preprocessing steps against a raw-text baseline (F1=0.7678):

- **Character-repeat normalization**: +0.0040 (best single gain)
- **Unicode normalization**: +0.0003
- Punctuation/whitespace normalization: −0.0098 each (hurt performance)
- Best config = baseline + these two steps only → **F1 = 0.7576**

**Takeaway:** Aggressive cleaning removes meaningful stylistic/emotional signal in social media text; only light, targeted preprocessing helps.

## Benchmark Comparison

All models beat the reference baseline HelaDepDet (F1=0.66):

| Model | F1 |
|---|---|
| HelaDepDet (baseline, literature) | 0.660 |
| MentalBERT | 0.7321 |
| RoBERTa | 0.7326 |
| MentalRoBERTa | 0.7465 |
| **MentalRoBERTa + Preprocess** | **0.7576** |

## Key Findings

1. MentalRoBERTa is the best model overall (DS1 F1=0.86, DS2 F1=0.76).
2. Only minimal, targeted preprocessing (character-repeat + Unicode normalization) helps; heavier cleaning hurts.
3. All models clearly outperform the literature baseline.
4. A well-optimized general model (RoBERTa) can rival a domain-specific one (MentalBERT).

## Future Work

- Longer-context models (e.g., Longformer, 512+ tokens)
- Multilingual datasets
- Comparison with LLM-based approaches
- Clinical validation studies

## Key References

- Kayalvizhi & Durairaj (2022) — DS1 dataset
- Priyadarshana et al. (2023) — DS2 dataset / HelaDepDet
- Ji et al. (2021) — MentalBERT / MentalRoBERTa
- Liu et al. (2019) — RoBERTa
- Chawla et al. (2002) — SMOTE
- Beck et al. (1996) — BDI

---
*Dept. of Computer Engineering, Kocaeli University*
