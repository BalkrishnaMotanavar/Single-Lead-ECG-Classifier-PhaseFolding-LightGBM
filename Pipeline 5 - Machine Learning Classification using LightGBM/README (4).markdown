# Pipeline 5: Superclass Mapping, LightGBM Classification, and ECGdeli Correlation

## Description
This pipeline performs three tasks:
1. **Superclass Mapping** (`1_Superclass_mapping.ipynb`): Maps PTB-XL SCP codes to five diagnostic superclasses (NORM, CD, STTC, MI, HYP) using `scp_statements.csv`, creating `ptbxl_database_with_superclass.csv` for classification.
2. **LightGBM Classification** (`2_ML_LightGBM.ipynb`): Trains a LightGBM model on PQRST features from Pipeline 4, using SMOTE for class imbalance and 10-fold cross-validation, achieving 96.9% accuracy.
3. **ECGdeli Correlation** (`3_Correlation_with_ECGDeli.ipynb`): Validates extracted PQRST features against ECGdeli features, computing Pearson correlations for clinical metrics (e.g., PR interval, R amplitude).

The pipeline generates performance visualizations (confusion matrix, feature importance, correlation bar plot) for the thesis.

## Requirements
- **Python Libraries**:
  - `pandas`: For data manipulation and CSV handling.
  - `numpy`: For numerical operations.
  - `wfdb`: For reading PTB-XL WFDB files (Superclass Mapping).
  - `ast`: For parsing SCP codes (Superclass Mapping).
  - `lightgbm`: For classification (LightGBM Classification).
  - `scikit-learn`: For feature engineering, SMOTE, and cross-validation (LightGBM Classification).
  - `imbalanced-learn`: For SMOTE and Tomek Links (LightGBM Classification).
  - `matplotlib`, `seaborn`: For visualization plots (LightGBM Classification, ECGdeli Correlation).
- **Input Files**:
  - `ptbxl_database.csv`: PTB-XL metadata.
  - `scp_statements.csv`: Diagnostic code mappings (Superclass Mapping).
  - `PQRST_Complexes_and_Features_final.csv`: PQRST features from Pipeline 4 (LightGBM Classification, ECGdeli Correlation).
  - `ecgdeli_selected_clinical_features.csv`: ECGdeli features for validation (ECGdeli Correlation).
- **Dataset**:
  - PTB-XL dataset (100 Hz or 500 Hz) from [PhysioNet](https://physionet.org/content/ptb-xl/1.0.3/).
  - Place in `/path/to/datasets/PTB_XL` and update paths.
- **System**:
  - Python 3.8+.
  - Recommended: 16 GB RAM for LightGBM training, 5 GB free storage.

## Usage
1. Install dependencies:
   ```bash
   pip install pandas numpy wfdb ast lightgbm scikit-learn imbalanced-learn matplotlib seaborn
   ```
2. Ensure the PTB-XL dataset and input files are available.
3. Run each notebook in sequence:

   **Superclass Mapping**:
   - Open `1_Superclass_mapping.ipynb`:
     ```bash
     jupyter notebook 1_Superclass_mapping.ipynb
     ```
   - Update paths:
     ```python
     path = "/path/to/datasets/PTB_XL/"
     ```
   - Run all cells to generate `ptbxl_database_with_superclass.csv`.

   **LightGBM Classification**:
   - Open `2_ML_LightGBM.ipynb`:
     ```bash
     jupyter notebook 2_ML_LightGBM.ipynb
     ```
   - Update paths (based on `lightgbm_classification.py`):
     ```python
     ptbxl_csv_path = "/path/to/datasets/PTB_XL/ptbxl_database.csv"
     superclass_csv_path = "/path/to/ptbxl_database_with_superclass.csv"
     pqrst_csv_path = "/path/to/PQRST_Complexes_and_Features_final.csv"
     output_dir = "/path/to/Visualization/training_results/"
     ```
   - Run all cells to train the model and generate visualizations.
   - Ensure `output_dir` exists.

   **ECGdeli Correlation**:
   - Open `3_Correlation_with_ECGDeli.ipynb`:
     ```bash
     jupyter notebook 3_Correlation_with_ECGDeli.ipynb
     ```
   - Update paths:
     ```python
     your_csv_path = "/path/to/PQRST_Complexes_and_Features_final.csv"
     ecgdeli_csv_path = "/path/to/ecgdeli_selected_clinical_features.csv"
     ```
   - Run all cells to compute correlations and generate the bar plot.

## Outputs
- **Superclass Mapping**:
  - `ptbxl_database_with_superclass.csv`: PTB-XL metadata with `diagnostic_superclass` column.
- **LightGBM Classification**:
  - **Visualization Plots** (in `Visualizations/training_results/`):
    - `percentage_confusion_matrix.png`: Figure 4.1, showing classification performance.
    - `feature_importance_fold_1.png`: Figure 4.2, showing key features (e.g., T-wave duration).
  - **Metrics**: Console output or file with accuracy (96.9%), F1-scores (e.g., 0.995 for NORM, 0.821 for HYP).
- **ECGdeli Correlation**:
  - **Visualization**: `ecg_feature_correlation_barplot.png`
    - Corresponds to Figure 4.3, showing Pearson correlations between ECGdeli and extracted features.
  - **CSV File**: `ecg_feature_correlation_barplot.csv`
    - Full correlation matrix for all features.

## Thesis Reference
- **Figure 4.1**: [percentage_confusion_matrix.png](https://raw.githubusercontent.com/BalkrishnaMotanavar/Single-Lead-ECG-Classifier-PhaseFolding-LightGBM/main/Visualizations/training_results/percentage_confusion_matrix.png)
  - Caption: "Heatmap of percentage confusion matrix for ECG classification."
- **Figure 4.2**: [feature_importance_fold_1.png](https://raw.githubusercontent.com/BalkrishnaMotanavar/Single-Lead-ECG-Classifier-PhaseFolding-LightGBM/main/Visualizations/training_results/feature_importance_fold_1.png)
  - Caption: "Feature importance for ECG classification (Fold 1)."
- **Figure 4.3**: [correlation_with_ecgdeli.png](https://raw.githubusercontent.com/BalkrishnaMotanavar/Single-Lead-ECG-Classifier-PhaseFolding-LightGBM/main/Visualizations/training_results/correlation_with_ecgdeli.png)
  - Caption: "Correlation of phase-folded Lead V6 features with ECGdeli features."

## Notes
- **Superclass Mapping**: Ensure `scp_statements.csv` is in the PTB-XL dataset directory.
- **LightGBM Classification**: Adjust SMOTE parameters if class imbalance persists. High RAM is recommended for training on 21,799 samples.
- **ECGdeli Correlation**: The notebook drops rows with missing values, reducing the sample size (e.g., from 21,799 to 9,911). Verify `ecgdeli_selected_clinical_features.csv` for completeness.
- Upload visualization plots to `Visualizations/training_results/` on GitHub:
  - Go to `Visualizations/training_results/`.
  - Click “Add file” > “Upload files.”
  - Drag and drop `percentage_confusion_matrix.png`, `feature_importance_fold_1.png`, `correlation_with_ecgdeli.png`.
  - Commit with a message like “Added Pipeline 5 visualizations for Figures 4.1–4.3.”
- For troubleshooting, check the thesis (Section 3.5) or contact [your_email@example.com].