# eeg-erp-adhd-ml-pipeline

End-to-end EEG/ERP pipeline for ADHD detection: band decomposition, feature engineering, exploratory data analysis (EDA), and model comparison.

**Goal:** identify signal features that best separate **ADHD vs Normal** and evaluate baseline machine-learning models on those features.

---

## Dataset (summary, not redistributed)

- **Participants:** 60 children (ages 4–15); 30 ADHD and 30 controls (DSM-IV clinical criteria). ADHD participants on methylphenidate withheld medication 24 h before testing. All had normal vision/hearing, no other neurological disorders, and IQ > 80.
- **Paradigm:** Oddball (auditory and visual). 200 stimuli per run (~80% non-target, 20% target). Task: press a button on rare/target stimuli.
- **Acquisition:** Midline electrodes **Fz, Cz, Pz** (10–20 system), sampling rate **640 Hz**, recorded with a NicoletOne EEG system. Each lead is treated independently; with two modalities this yields **6 trials per subject**.
- **Data sharing:** The original dataset is **not redistributed** in this repository due to licensing. See **Data access** below for local reproduction.

---

## Repository structure

```
notebooks/
  phase-1-band-decomposition.ipynb
  phase-2-eda-and-feature-ranking.ipynb
  phase-3-modeling-and-comparison.ipynb
src/
  extract_alpha_features.py
  train_models.py
figures/              # optional: saved plots for documentation
data/                 # local only (git-ignored)
outputs/              # local only (git-ignored)
README.md
LICENSE               # MIT
requirements.txt
```

---

## Methods (three phases)

### Phase 1 — Band decomposition & feature extraction
- Transform raw EEG/ERP to the frequency domain and isolate canonical bands (delta, theta, **alpha**, beta, gamma).
- Focus on the **alpha** band in this study and extract per-trial features:
  - **Statistics:** mean, median, variance, amplitude, positive/negative peaks  
  - **Wavelet (db4):** means of cA/cD; entropies of cA/cD  
  - **Entropy:** histogram-based signal entropy  
  - **Time-series:** AR(1) coefficient, Hurst exponent  
  - **Power:** mean-squared amplitude (band power)
- Output artifact: **`alpha_features.csv`** (columns: ID, Label, features).

### Phase 2 — EDA & univariate feature ranking
- Dataset overview (preview, shape, info, summary statistics).
- Class-wise distributions (KDE overlays) and correlation heatmaps.
- Per-feature separability metrics:
  - ROC AUC, Cohen’s d, Welch t-test, Mann–Whitney U,  
  - ANOVA F, Mutual Information (MI), with FDR correction.
- Combined ranking (average rank of AUC, |d|, MI, F).
- Output artifact: **`alpha_feature_ranking.csv`** (sorted by combined rank).

### Phase 3 — Modeling & evaluation
- Pipelines with imputation, scaling (when required), and **SelectKBest**.
- Models: **Logistic Regression**, **SVM (RBF)**, **Random Forest**.
- Hyperparameter tuning with **GridSearchCV** using **Repeated Stratified 5×2 CV** (scoring = ROC AUC).
- Test-set metrics: ROC AUC, Accuracy, Balanced Accuracy, F1, Precision, Recall.
- Visualizations: CV vs Test AUC, metric bar charts, combined ROC curves, confusion matrix.
- Output artifact: **`alpha_model_comparison.csv`** (per-model CV/Test metrics and best parameters).

---

## Setup

```bash
# Create and activate a virtual environment
python -m venv .venv
# Windows
.venv\Scripts\activate
# macOS/Linux
# source .venv/bin/activate

# Install dependencies
pip install -r requirements.txt
```

**requirements.txt (minimum):**
```
numpy
pandas
scipy
scikit-learn
matplotlib
seaborn
pywt
statsmodels
```

---

## Quickstart

1. Place your local EEG/ERP files under the `data/` directory (this folder is git-ignored).  
2. Run `notebooks/phase-1-band-decomposition.ipynb` to generate band-limited CSVs (e.g., `alpha.csv`) and `alpha_features.csv`.  
3. Run `notebooks/phase-2-eda-and-feature-ranking.ipynb` to produce EDA outputs and `alpha_feature_ranking.csv`.  
4. Run `notebooks/phase-3-modeling-and-comparison.ipynb` to train/evaluate models and save `alpha_model_comparison.csv`.

> Use relative paths like `data/...` in notebooks (avoid absolute local paths).

---

## Data access

The dataset used in this project is not included in the repository. Obtain the EEG/ERP data from its original provider under appropriate terms and place it under `data/` on your machine. This repository operates solely on locally available data.

---

## License

Released under the **MIT License** — see the [LICENSE](./LICENSE) file for details. The license applies to the **code** in this repository. Original data remains subject to its own terms.
