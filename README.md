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
