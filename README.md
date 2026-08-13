HDTBO_DMN: An Intrusion Detection System for IoT-Enabled Healthcare and Secure Communication

Reference implementation of HDTBO_DMN, a hybrid metaheuristic-optimized Deep Maxout Network (DMN) framework for intrusion detection in IoT-enabled healthcare systems, paired with a blockchain-assisted authentication and data-protection protocol.

The pipeline covers the full manuscript workflow:

Authentication Phase — registration, key generation, login, OTP-based authentication, RHHO-keyed data protection, authorization, and data sharing/decryption (Section 4.1).
Preprocessing — missing-value handling and Min-Max normalization (Section 4.2).
Data Augmentation — SMOTE-based class-imbalance correction.
Feature Fusion — Levenshtein-distance similarity features fused with a lightweight DNN encoder.
Intrusion Detection — a Deep Maxout Network (DMN) classifier trained by the proposed HDTBO optimizer (a hybrid of HLBO exploration and DTBO exploitation).
Evaluation — Attack Detection Rate, F1-score, FPR, Precision, Accuracy, MCC, ROC-AUC, plus algorithmic, ablation, and attack-resistance comparisons against baseline optimizers (Jaya, CSO, HLBO, DTBO) and RHHO for the authentication key.
Repository Structure
├── config.py                  # All hyperparameters (dataset, DMN, HDTBO, auth phase, sweeps)
├── data_utils.py               # Dataset loading, missing-value handling, Min-Max scaling, SMOTE
├── feature_fusion.py           # Levenshtein distance + DNN feature-fusion block
├── dmn_model.py                 # Deep Maxout Network (NumPy, flattened weight vector)
├── optimizers.py                # Jaya, CSO, HLBO, DTBO, HDTBO (proposed), RHHO
├── auth_phase.py                 # Authentication protocol (registration → decryption)
├── metrics.py                    # Attack detection rate, F1, FPR, precision, accuracy, MCC, ROC-AUC
├── train_utils.py                 # Data prep + train/evaluate pipeline (single-split and k-fold)
├── plot_utils.py                   # Shared Matplotlib helpers for grouped bar/line charts
├── performance_analysis.py          # Figure 7 & 9 — metrics vs. training % / K-fold
├── algorithmic_analysis.py           # Figure 11 — optimizer comparison
├── ablation_analysis.py               # Figure 12 — component ablation study
├── resistance_analysis.py              # Figure 13 — simulated attack resistance
├── main.py                              # Runs the full demo suite, writes outputs/
├── requirements.txt
└── outputs/                              # Generated figures land here
Requirements
Python 3.9+ (tested with 3.9–3.12)
See requirements.txt for pinned versions:
numpy==1.23.5
pandas==1.5.3
scikit-learn==1.2.2
imbalanced-learn==0.10.1
matplotlib==3.7.1
python-Levenshtein==0.21.0
scipy==1.10.1

Note: on newer Python versions (3.11+) these exact pins may fail to build. If installation fails, install the latest compatible versions instead: pip install numpy pandas scikit-learn imbalanced-learn matplotlib python-Levenshtein scipy

Minimum Hardware
Component	Minimum Specification
OS	Windows 10 (64-bit) / Linux / macOS
Processor	Intel Core i3 or equivalent
RAM	8 GB recommended
GPU	Optional (not required — pure NumPy implementation)
Storage	100 GB free (for the full BoT-IoT dataset)
Installation
bash
git clone <this-repo-url>
cd HDTBO_DMN
python3 -m venv venv
source venv/bin/activate            # Windows: venv\Scripts\activate
pip install -r requirements.txt
Dataset

This project is designed around the BoT-IoT dataset (https://research.unsw.edu.au/projects/bot-iot-dataset), specifically the 5% subset recommended for IDS benchmarking (43 features, extreme class imbalance: ~38 normal samples per 1,000,000 attack samples).

Download the BoT-IoT 5% CSV subset from the link above.
Place it at data/BoT_IoT_5pct.csv, or point to it explicitly:
bash
export BOTIOT_CSV_PATH=/path/to/BoT_IoT_5pct.csv   # Windows: set BOTIOT_CSV_PATH=...
The CSV must contain a binary label column (0 = normal, 1 = attack). Update config.LABEL_COLUMN if your column is named differently.

⚠️ Known limitation: config.py documents a synthetic-data fallback (governed by N_SYNTHETIC_SAMPLES / SYNTHETIC_NORMAL_RATIO) intended to let the pipeline run end-to-end without the real dataset. As currently wired, data_utils.load_dataset() does not implement this fallback — it calls pd.read_csv() directly and will raise FileNotFoundError if no CSV exists at DATA_CSV_PATH. A real (or synthetic placeholder) CSV at that path is currently required before running main.py. See Known Issues below.

Running

Full demo suite (writes all figures to outputs/):

bash
python main.py                  # fast demo run (FAST_MODE=1, small pop/iterations)
FAST_MODE=0 python main.py      # paper-scale settings (Table 4), needs the real dataset

Each experiment can also be run standalone:

bash
python performance_analysis.py     # Figures 7 & 9 — metrics vs. training % / K-fold
python algorithmic_analysis.py     # Figure 11 — optimizer comparison
python ablation_analysis.py        # Figure 12 — component ablation
python resistance_analysis.py      # Figure 13 — simulated attack resistance (replay, MITM, KCI, Sybil)

The authentication protocol can be exercised independently:

bash
python auth_phase.py
Configuration

All tunable parameters live in config.py:

Dataset: DATA_CSV_PATH, LABEL_COLUMN, N_FEATURES, train/val/test split ratios.
FAST_MODE: 1 (default) uses small population/iteration/layer counts for a quick sanity-check run; 0 switches to paper-scale settings (Table 4) — slower and intended for the real BoT-IoT dataset.
DMN hyperparameters: layers, hidden units, Maxout pieces, learning rate, dropout, weight decay, epochs.
HDTBO / metaheuristic hyperparameters: population size, bounds, iterations, exploration/exploitation rates, mutation probability.
Sweep values: training percentages, iteration counts, K-fold values, solution sizes, attack types used by the analysis scripts.
Authentication phase: random-number range, key length/dimension, RHHO population/iterations, OTP validity window, timestamp delay threshold.
Manuscript-to-Code Mapping
Manuscript section	Module
Section 4.1 — Authentication Phase (Eqs. 1–20)	auth_phase.py
Section 4.1.5(a) — RHHO key optimizer (Eq. 11)	optimizers.py::RHHO
Section 4.2 — Preprocessing / SMOTE	data_utils.py
Section 4.2 — Levenshtein + DNN feature fusion	feature_fusion.py
Section 4.2.5(a) — Deep Maxout Network (Eq. 38)	dmn_model.py
Section 4.2.5(b) — HDTBO optimizer (Eqs. 42–60)	optimizers.py::HDTBO
Section 5.3 — Evaluation metrics (Eqs. 61–66)	metrics.py
Section 5.4 / Figure 7	performance_analysis.py::run_training_percentage_sweep
Section 5.6.2 / Figure 9	performance_analysis.py::run_kfold_sweep
Figure 11 — Algorithmic analysis	algorithmic_analysis.py
Figure 12 — Ablation analysis	ablation_analysis.py
Figure 13 — Attack-resistance analysis	resistance_ana
