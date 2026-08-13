# HDTBO_DMN: An Intrusion Detection System for IoT-Enabled Healthcare and Secure Communication

Reference implementation of **HDTBO_DMN**, a hybrid metaheuristic-optimized Deep Maxout Network (DMN) framework for intrusion detection in IoT-enabled healthcare systems, paired with a blockchain-assisted authentication and data-protection protocol.

The pipeline covers the full manuscript workflow:

1. **Authentication Phase** — registration, key generation, login, OTP-based authentication, RHHO-keyed data protection, authorization, and data sharing/decryption.
2. **Preprocessing** — missing-value handling and Min-Max normalization.
3. **Data Augmentation** — SMOTE-based class-imbalance correction.
4. **Feature Fusion** — Levenshtein-distance similarity features fused with a lightweight DNN encoder.
5. **Intrusion Detection** — a Deep Maxout Network (DMN) classifier trained by the proposed **HDTBO** optimizer (a hybrid of HLBO exploration and DTBO exploitation).
6. **Evaluation** — Attack Detection Rate, F1-score, FPR, Precision, Accuracy, MCC, and ROC-AUC, along with algorithmic, ablation, and attack-resistance comparisons against baseline optimizers (Jaya, CSO, HLBO, DTBO) and RHHO for the authentication key.

---

## Repository Structure

├── config.py # All hyperparameters (dataset, DMN, HDTBO, auth phase, sweeps)
├── data_utils.py # Dataset loading, missing-value handling, Min-Max scaling, SMOTE
├── feature_fusion.py # Levenshtein distance + DNN feature-fusion block
├── dmn_model.py # Deep Maxout Network (NumPy, flattened weight vector)
├── optimizers.py # Jaya, CSO, HLBO, DTBO, HDTBO (proposed), RHHO
├── auth_phase.py # Authentication protocol (registration → decryption)
├── metrics.py # Attack detection rate, F1, FPR, precision, accuracy, MCC, ROC-AUC
├── train_utils.py # Data prep + train/evaluate pipeline (single-split and k-fold)
├── plot_utils.py # Shared Matplotlib helpers for grouped bar/line charts
├── performance_analysis.py # Metrics vs. training percentage / K-fold
├── algorithmic_analysis.py # Optimizer comparison
├── ablation_analysis.py # Component ablation study
├── resistance_analysis.py # Simulated attack resistance
├── main.py # Runs the full demo suite, writes outputs/
├── requirements.txt
└── outputs/ # Generated figures land here


---

## Requirements

- Python 3.9+

numpy==1.23.5
pandas==1.5.3
scikit-learn==1.2.2
imbalanced-learn==0.10.1
matplotlib==3.7.1
python-Levenshtein==0.21.0
scipy==1.10.1


### Minimum Hardware

| Component | Minimum Specification |
|---|---|
| OS | Windows 10 (64-bit) / Linux / macOS |
| Processor | Intel Core i3 or equivalent |
| RAM | 8 GB recommended |
| GPU | Optional (pure NumPy implementation) |
| Storage | 100 GB free (for the full BoT-IoT dataset) |

---

## Installation

```bash
git clone <this-repo-url>
cd HDTBO_DMN
python3 -m venv venv
source venv/bin/activate            # Windows: venv\Scripts\activate
pip install -r requirements.txt
```

---

## Dataset

This project uses the **BoT-IoT dataset** (https://research.unsw.edu.au/projects/bot-iot-dataset), specifically the 5% subset recommended for IDS benchmarking (43 features, with the extreme class imbalance characteristic of real-world intrusion traffic).

1. Download the BoT-IoT 5% CSV subset from the link above.
2. Place it at `data/BoT_IoT_5pct.csv`, or point to it directly:

```bash
export BOTIOT_CSV_PATH=/path/to/BoT_IoT_5pct.csv   # Windows: set BOTIOT_CSV_PATH=...
```

3. The CSV should contain a binary `label` column (0 = normal, 1 = attack). Update `config.LABEL_COLUMN` if your column is named differently.

---

## Running

Run the full demo suite (writes all figures to `outputs/`):

```bash
python main.py                  # fast demo run (FAST_MODE=1, small pop/iterations)
FAST_MODE=0 python main.py      # paper-scale settings (Table 4)
```

Each experiment can also be run standalone:

```bash
python performance_analysis.py     # Metrics vs. training % / K-fold
python algorithmic_analysis.py     # Optimizer comparison
python ablation_analysis.py        # Component ablation
python resistance_analysis.py      # Simulated attack resistance (replay, MITM, KCI, Sybil)
```

The authentication protocol can be exercised independently:

```bash
python auth_phase.py
```

---

## Configuration

All tunable parameters live in `config.py`:

- **Dataset**: `DATA_CSV_PATH`, `LABEL_COLUMN`, `N_FEATURES`, train/val/test split ratios.
- **`FAST_MODE`**: toggles between a quick sanity-check run (small population/iteration/layer counts) and paper-scale settings for full reproduction.
- **DMN hyperparameters**: layers, hidden units, Maxout pieces, learning rate, dropout, weight decay, epochs.
- **HDTBO / metaheuristic hyperparameters**: population size, bounds, iterations, exploration/exploitation rates, mutation probability.
- **Sweep values**: training percentages, iteration counts, K-fold values, solution sizes, attack types used by the analysis scripts.
- **Authentication phase**: random-number range, key length/dimension, RHHO population/iterations, OTP validity window, timestamp delay threshold.

---

## Manuscript-to-Code Mapping

| Component | Module |
|---|---|
| Authentication Phase | `auth_phase.py` |
| RHHO key optimizer | `optimizers.py::RHHO` |
| Preprocessing / SMOTE | `data_utils.py` |
| Levenshtein + DNN feature fusion | `feature_fusion.py` |
| Deep Maxout Network | `dmn_model.py` |
| HDTBO optimizer (proposed) | `optimizers.py::HDTBO` |
| Evaluation metrics | `metrics.py` |
| Performance vs. training % / K-fold | `performance_analysis.py` |
| Algorithmic analysis | `algorithmic_analysis.py` |
| Ablation analysis | `ablation_analysis.py` |
| Attack-resistance analysis | `resistance_analysis.py` |

---

## Citation

If you use this code in your research, please cite the associated manuscript:

> *HDTBO_DMN: A Secure IoT-Enabled Healthcare Intrusion Detection System for Enhanced Communication*

---

## License

Add a license file appropriate for your submission/venue requirements.

## Contributing

Issues and pull requests are welcome.


