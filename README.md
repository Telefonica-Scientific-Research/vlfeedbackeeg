# VLFEEDBACK-EEG: Neural Signals as Implicit Feedback for Vision-Language Model Alignment

> Code and data for the paper accepted at **Findings of EACL 2027**.
>
> Angela Lopez-Cardona, Sebastian Idesis, Mireia Masias Bruns, Matteo Mazzini,
> Joemon M. Jose, Sergi Abadal, Ioannis Arapakis
>
> An earlier version of this work was presented at the RLxF: Reinforcement Learning from World Feedback Workshop at ICML 2026:
> https://openreview.net/forum?id=1vYMwkm0ih

---

## Overview

**VLFEEDBACK-EEG** investigates whether non-invasive EEG recordings of human annotators can serve as implicit, continuous feedback for aligning Vision-Language Models (VLMs). Rather than relying solely on explicit preference labels, we capture the neural responses of 21 subjects while they annotate a VQA dataset and study whether these signals carry discriminative information about annotator preferences over competing VLM responses.

Our main contributions are:

- **A novel dataset:** VLFEEDBACK-EEG comprises EEG recordings from 21 participants as they read and evaluated model-generated responses from VLFeedback, yielding the first resource of its kind for cognitively-grounded multimodal alignment.
- **Preference decoding:** Via classifier training, we find that pairwise evaluation and entropy features are consistently more informative than pointwise evaluation and band power, a pattern replicated across two independent analyses. Classification does not reach significance after correction for multiple comparisons at this sample size.
- **Reward model fusion:** An exploratory fusion with pretrained reward models does not surpass RM-only baselines; differences against noise and shuffled controls are consistent in direction but inconclusive at this scale.

---

## Installation

Different parts of the pipeline use different Conda environments due to conflicting dependencies:

| Module | Environment | Notes |
|--------|-------------|-------|
| `eeg_preprocessing/` | `envs/eeg_preprocessing.yml` | EEG preprocessing deps (e.g. MNE) |
| `eeg_classifier/` | `envs/eeg_classifier.yml` | Classifier deps (e.g. scikit-learn) |
| `preference_reward_*/` | `envs/reward_fusion.yml` | Fusion/reward model deps (PyTorch, LLaVA) |

All scripts assume the relevant environment is active and commands are run from the **repository root** (the directory containing `config.json`):

```bash
conda activate <env>
cd /path/to/vlfeedback_eeg
```

Shell pipelines use `${PYTHON_BIN:-python}`; after activation that resolves to the correct interpreter. To pin it explicitly:

```bash
export PYTHON_BIN="$(which python)"
```

---

## Repository Structure

Each module has its own README with usage instructions.

| Module | Contents |
|--------|----------|
| [`data/`](./data/README.md) | Raw EEG recordings and processed features |
| [`eeg_preprocessing/`](./eeg_preprocessing) | ICA artifact removal, band-pass filtering, feature extraction (entropy, band power, PSD, PLV, alpha asymmetry) |
| [`eeg_classifier/`](./eeg_classifier) | Subject-level cross-validated classifiers (LR, SVM, RF, GB, KNN, NB) on tabular EEG features and per-channel entropy |
| [`preference_reward_features/`](./preference_reward_features) | M2: MLP over EEG features → GatedFusion with frozen RM embeddings & scores |
| [`preference_reward_raweeg/`](./preference_reward_raweeg) | M1: 1D CNN encoder on raw EEG segments → GatedFusion with frozen RM embeddings |

Shared infrastructure:

```
├── preferece_reward_utils/      # Evaluation, ablation & table-printing utils
├── data_loaders/                # PairSegDataset and fusion data loaders
├── models/                      # GatedFusion, EEGPreferenceEncoder
├── assets/                      # Figures used in the paper
├── config.json                  # Data & results path configuration
└── requirements.txt
```

---

## License

© 2025 Telefónica Innovación Digital.

Released under the GNU Lesser General Public License v3.0 (LGPLv3).
See [LICENSE](./LICENSE) for details.

---

## Citation

```bibtex
@inproceedings{lopezcardona2027vlfeedbackeeg,
  title     = {{VLFEEDBACK-EEG}: Neural Signals as Implicit Feedback for Vision-Language Model Alignment},
  author    = {Lopez-Cardona, Angela and Idesis, Sebastian and Masias Bruns, Mireia and Mazzini, Matteo and Jose, Joemon M. and Abadal, Sergi and Arapakis, Ioannis},
  booktitle = {Findings of the Association for Computational Linguistics: EACL 2027},
  year      = {2027},
  publisher = {Association for Computational Linguistics}
}
```
