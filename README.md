# SRL BERT Model

[![Hugging Face Model](https://img.shields.io/badge/🤗%20Hugging%20Face-yeomtong%2Fsrl__bert__model-yellow)](https://huggingface.co/yeomtong/srl_bert_model)

BERT-based **Semantic Role Labeling (SRL)** model — this repository contains the **training and inference code**.

> **📦 Pretrained model weights are hosted on Hugging Face** (too large for GitHub):
> 👉 **[huggingface.co/yeomtong/srl_bert_model](https://huggingface.co/yeomtong/srl_bert_model)**

## Repository Contents

| File | Description |
|---|---|
| `data_prep.py` | Data preprocessing for SRL training |
| `model.py` | Model architecture definition |
| `training.py` | Finetuning script |
| `testing.py` | Evaluation script |
| `predicator.py` | Inference / prediction on new text |

## Quick Start

### 1. Use the pretrained model (recommended)

Load the finetuned weights directly from the Hugging Face Hub:

```python
from transformers import AutoTokenizer, AutoModel

tokenizer = AutoTokenizer.from_pretrained("yeomtong/srl_bert_model")
model = AutoModel.from_pretrained("yeomtong/srl_bert_model")
```

Then run inference with:

```bash
python predicator.py
```

### 2. Train from scratch

```bash
python data_prep.py    # prepare training data
python training.py     # finetune the model
python testing.py      # evaluate
```

## Model Weights

The trained model checkpoint is **not stored in this repository** due to file size limits. All weights, tokenizer files, and the model card live on the Hugging Face Hub:

- 🤗 **Model**: [yeomtong/srl_bert_model](https://huggingface.co/yeomtong/srl_bert_model)

## Related Projects

- [srl-aware-sepf](https://github.com/philz0918/srl-aware-sepf) — SRL-aware sentiment, emotion, politeness & formality analysis using this SRL model with gated fusion
