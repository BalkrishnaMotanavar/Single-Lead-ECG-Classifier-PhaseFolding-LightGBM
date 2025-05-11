# Pipeline 1: Signal Selection and Preprocessing

## Description
This pipeline extracts and preprocesses Lead V6 signals from the PTB-XL dataset, removing baseline drift and high-frequency noise using median and Butterworth filters. Lead V6 is selected for its sensitivity to left ventricular abnormalities, critical for detecting conditions like myocardial infarction (MI) and hypertrophy (HYP). The preprocessed signals are stored in HDF5 and CSV formats for use in subsequent pipelines.

## Requirements
- **Python Libraries**:
  - `wfdb`: For reading PTB-XL WFDB files.
  - `numpy`: For numerical operations.
  - `scipy`: For signal processing (median filter, Butterworth filter).
  - `matplotlib`: For visualization plots.
  - `h5py`: For HDF5 file operations.
  - `pandas`: For reading `ptbxl_database.csv` and CSV output.
  - `tqdm`: For progress bar during dataset processing.
- **Dataset**:
  - PTB-XL dataset (500 Hz version) from [PhysioNet](https://physionet.org/content/ptb-xl/1.0.3/).
  - Place in `/path/to/datasets/PTB_XL` and update `base_path` in the notebook.
  - Requires `ptbxl_database.csv` for metadata.
- **System**:
  - Python 3.8+.
  - Minimum: 8 GB RAM, 5 GB free storage for dataset and outputs.

## Usage
1. Install dependencies:
   ```bash
   pip install wfdb numpy scipy matplotlib h5py pandas tqdm
   ```
2. Download the PTB-XL dataset and place it in `/path/to/datasets/PTB_XL`.
3. Open `signal_selection_preprocessing.ipynb` in Jupyter Notebook:
   ```bash
   jupyter notebook signal_selection_preprocessing.ipynb
   ```
4. Update file paths in the notebook:
   ```python
   base_path = Path("/path/to/datasets/PTB_XL")
   csv_path = base_path / "ptbxl_database.csv"
   output_hdf5_path = "/path/to/preprocessed_lead_v6.h5"
   output_csv_path = "/path/to/preprocessed_lead_v6.csv"
   ```
5. Run all cells to:
   - Test Lead V6 extraction on a subset of records (first section).
   - Process the entire dataset (second section).
6. Ensure the `Visualization/` directory exists for saving plots.

## Outputs
- **HDF5 File**: `preprocessed_lead_v6.h5`
  - Contains preprocessed Lead V6 signals, keyed by `ecg_id`.
  - Attributes: `patient_id`, `filename_hr`, `record_path`.
- **CSV File**: `preprocessed_lead_v6.csv`
  - Stores preprocessed signals with metadata (`ecg_id`, `patient_id`, `filename_hr`, `sample_0` to `sample_4999`).
- **Visualization**: `Visualizations/preprocessed_signal_00008_hr.png`
  - Corresponds to Figure 3.1 in the thesis, showing original (red) and filtered (black) Lead V6 signals for `00008_hr`.

## Thesis Reference
- **Figure 3.1**: [preprocessed_signal_00008_hr.png](https://raw.githubusercontent.com/BalkrishnaMotanavar/Single-Lead-ECG-Classifier-PhaseFolding-LightGBM/main/Visualizations/00008_hr_lead_v6.png.png)
  - Caption: "Original (top, red) and filtered (bottom, black) Lead V6 signals for patient 00008_hr, demonstrating preprocessing efficacy."

## Notes
- Ensure the PTB-XL dataset is the 500 Hz version, as the notebook expects `fs=500`.
- If `ptbxl_database.csv` is missing, download it from PhysioNet.
- The HDF5 file is required for Pipelines 2–5; verify its generation before proceeding.
- For troubleshooting, check the thesis (Section 3.1) or contact [your_email@example.com].
