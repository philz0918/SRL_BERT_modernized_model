# SRL BERT Model — Predicate-Aware Semantic Role Labeling

[![Hugging Face Model](https://img.shields.io/badge/🤗%20Hugging%20Face-yeomtong%2Fsrl__bert__model-yellow)](https://huggingface.co/yeomtong/srl_bert_model)

BERT-based **Predicate-Aware Semantic Role Labeling (SRL)** model.

> **⚠️ Note:** The scripts in this repository are **reference code / example snippets** showing how the model was built and trained.
> For actual use, load the **working modules and pretrained checkpoint from the Hugging Face Hub** (checkpoint is too large for GitHub):
> 👉 **[huggingface.co/yeomtong/srl_bert_model](https://huggingface.co/yeomtong/srl_bert_model)**

## Why This Model?

In practice, **[AllenNLP](https://allenai.org/allennlp)** has long been the go-to library for ready-to-use SRL, but it is no longer actively maintained and its inference is relatively slow.

**This model achieves comparable (or better) performance while running significantly faster** — and it is designed for **ease of use**: download from Hugging Face and predict in a few lines, no additional preprocessing or setup required (see Quick Start below).

### Performance Comparison

| Model | P | R | F1 | Time (m) |
|---|---|---|---|---|
| Our Model (BERT) | 85.98 | 86.32 | 86.15 | **1.36** |
| Our Model (RoBERTa) | 86.19 | 87.97 | 87.07 | 5.36 |
| Our Model (DeBERTa) | 86.78 | **88.22** | **87.49** | 7.57 |
| AllenNLP | **86.81** | 86.63 | 86.72 | 13.18 |
| Zhang et al. (2022) | 84.53 | 85.41 | 85.45 | 1.46 |

- **Our Model (DeBERTa)** achieves the best F1 (87.49) and recall, outperforming AllenNLP while running ~1.7× faster.
- **Our Model (BERT)** offers the best speed–usability balance: ~10× faster than AllenNLP with only a small F1 trade-off, and ready to use out of the box.

## Installation

```bash
pip install torch transformers huggingface_hub
```

## Quick Start — Load the Pretrained Model from Hugging Face

Download the checkpoint and supporting modules directly from the Hub, then run predictions:

```python
import sys
from huggingface_hub import hf_hub_download, snapshot_download

# Download the trained checkpoint
ckpt_path = hf_hub_download(
    repo_id="yeomtong/srl_bert_model",
    filename="best_srl_Sep_29.ckpt")

# Download the full repo (predictor / model / visualizer modules)
repo_dir = snapshot_download("yeomtong/srl_bert_model")
sys.path.append(repo_dir)

from predictor import srl_init
from model import PredicateAwareSRL
from visualizer import prediction, prediction_formatted

# Load model
srl_init(ckpt_path, bert_name="bert-base-cased")

# Run SRL prediction
test_sentence = "I want to go home"
prediction(test_sentence)
```

Output:

```
Sentence: I want to go home
————————————————————————————————————————————————————————————
[ARG0: I] [V: want] [ARG1: to go home]
TOKEN: I      want to     go     home
LABEL: B-ARG0 B-V  B-ARG1 I-ARG1 I-ARG1
————————————————————————————————————————————————————————————
[ARG0: I] want to [V: go] [ARG4: home]
TOKEN: I      want to go  home
LABEL: B-ARG0 .    .  B-V B-ARG4
```

For structured (AllenNLP-style) output:

```python
prediction_formatted(test_sentence)
```

```python
{'verbs': [{'verb': 'want',
   'description': '[ARG0: I] [V: want] [ARG1: to go home]',
   'tags': ['B-ARG0', 'B-V', 'B-ARG1', 'I-ARG1', 'I-ARG1']},
  {'verb': 'go',
   'description': '[ARG0: I] want to [V: go] [ARG4: home]',
   'tags': ['B-ARG0', 'O', 'O', 'B-V', 'B-ARG4']}],
 'words': ['I', 'want', 'to', 'go', 'home']}
```

## Repository Contents (Reference / Example Code)

The files below are **example snippets** illustrating the training and inference pipeline. They are for reference only — the runnable, up-to-date versions of these modules are distributed with the model on Hugging Face (and are fetched automatically by `snapshot_download` in the Quick Start above).

| File | Description |
|---|---|
| `data_prep.py` | Example: data preprocessing for SRL training |
| `model.py` | Example: `PredicateAwareSRL` model architecture (BERT backbone) |
| `training.py` | Example: finetuning script |
| `testing.py` | Example: evaluation script |
| `predicator.py` | Example: inference / prediction on new text |

## Model Weights & Working Code

The trained checkpoint (`best_srl_Sep_29.ckpt`) and the working inference modules (`predictor.py`, `model.py`, `visualizer.py`) are **not stored in this repository** — they live on the Hugging Face Hub:

- 🤗 **Model**: [yeomtong/srl_bert_model](https://huggingface.co/yeomtong/srl_bert_model)

## Related Projects

- [SRL-gated-fusion-sepf](https://github.com/philz0918/srl-aware-sepf) — SRL-aware sentiment, emotion, politeness & formality analysis using this SRL model with gated fusion
