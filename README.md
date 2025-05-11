# 🩺 Automated Single-Lead ECG Classification using Phase Folding & LightGBM

## 📘 Project Summary

This project presents a single-lead (Lead V6) ECG classification system to identify five cardiovascular diagnostic superclasses:

- **NORM** – Normal Rhythm  
- **CD** – Conduction Disturbance  
- **STTC** – ST/T Changes  
- **MI** – Myocardial Infarction  
- **HYP** – Hypertrophy

Using the PTB-XL dataset (21,837 records), we implemented a five-stage processing and classification pipeline combining clinical signal processing with interpretable machine learning.

> 🧠 Achieved **96.9% accuracy** with macro F1-score of 0.943 using 10-fold cross-validation.

---

## 🔬 Methodology Overview

**Pipeline Structure:**

1. **Signal Preprocessing**  
   - Median filtering (baseline drift correction)  
   - 2nd-order Butterworth low-pass filter (noise removal)

2. **R-Peak Detection**  
   - Pan-Tompkins algorithm via [NeuroKit2](https://neuropsychology.github.io/NeuroKit/)

3. **Phase Folding**  
   - Heartbeat alignment using RR intervals  
   - Average beat generation with 400-point interpolation

4. **PQRST Feature Extraction**  
   - Detection of wave peaks, onsets, offsets using Hilbert envelope and peak analysis  
   - Extraction of clinical features: PR/QT/QRS durations, wave amplitudes, ST-elevation, etc.

5. **Machine Learning Classification**  
   - Model: LightGBM  
   - Feature count: 22  
   - Handling imbalance: SMOTE, Tomek Links, RandomUnderSampler  
   - Evaluation: 10-fold cross-validation

---

## 📊 Dataset

- **Source:** [PTB-XL](https://physionet.org/content/ptb-xl/1.0.1/)  
- **Leads:** 12 (only **Lead V6** used)  
- **Sampling Rate:** 500 Hz  
- **Records:** 21,837  
- **Classes:** 5 superclasses from SCP codes  
- **Metadata:** age, sex, SCP labels, diagnostic confidence

---

## 💡 Key Results

| Class | F1-score |
|-------|----------|
| NORM | 0.995 |
| CD | 0.967 |
| STTC | 0.986 |
| MI | 0.947 |
| HYP | 0.821 |

- 🔍 **Top Feature:** T-wave duration (crucial for MI/STTC detection)  
- ⚡ **Strength:** Efficient, interpretable, low-resource compatible

---

## 📄 Citation

> Balkrishna Mallikarjun Mottannavar,  
> *Automated Single Lead ECG Classification for Cardiovascular Diagnosis Using Phase Folding and LightGBM*,  
> MSc Data Science Thesis, University of Aberdeen, 2025.

---

## 📌 Future Work

- Multi-lead integration (e.g., ICBEB2018)
- Edge computing for real-time wearable deployment
- P-wave enhancement using wavelet or CWT methods
- Real-world reliability testing using ADASYN

---

## 📖 Full Thesis

📄 **[Read Full Thesis](./Automated Single Lead ECG Classification for Cardiovascular Diagnosis Using Phase Folding and LightGBM.pdf)**  
For full pipeline details, validation experiments, literature survey, and future scope, please read the MSc Data Science thesis submitted to the University of Aberdeen in May 2025.

---

## 📬 Contact

For questions or collaborations, please reach out to:  
📧 **balkrishnamotanavar@gmail.com**

