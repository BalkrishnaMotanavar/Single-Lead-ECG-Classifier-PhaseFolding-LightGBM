# Pipeline 1: Lead V6 Extraction

## Description
This pipeline extracts and preprocesses Lead V6 signals from the PTB-XL dataset, removing baseline drift and high-frequency noise. Lead V6 is chosen for its sensitivity to left ventricular abnormalities, making it ideal for detecting conditions like myocardial infarction (MI) and hypertrophy (HYP). The preprocessed signals are stored in an HDF5 file for use in subsequent pipelines.

## Requirements
- **Python Libraries**:
  - `h5py`: For HDF5 file operations.
  - `pandas`: For reading `ptbxl_database.csv`.
  - `numpy`: For numerical operations.
  - `scipy`: For signal processing (median filter, Butterworth filter).
  - `wfdb`: For reading PTB-XL WFDB files.
- **Dataset**:
  - PTB-XL dataset (500 Hz version) from [PhysioNet](https://physionet.org/content/ptb-xl/1.0.3/).
  - Place in `/Users/your_username/datasets/PTB_XL` or update `base_path` in the script.
  - Requires `ptbxl_database.csv` for metadata.
- **System**:
  - Python 3.8+.
  - 8 GB RAM, 5 GB free storage.

## Usage
1. Ensure the PTB-XL dataset is downloaded and placed in `/Users/your_username/datasets/PTB_XL`.
2. Update the file paths in `lead_v6_extraction.py`:
   ```python
   base_path = Path("/Users/your_username/datasets/PTB_XL")
   csv_path = base_path / "ptbxl_database.csv"
   output_hdf5_path = "/Users/your_username/Dissertation ECG Parameterization/Pipeline1_LeadV6_extraction/preprocessed_lead_v6.h5"
   ```
3. Run the script:
   ```bash
   python lead_v6_extraction.py
   ```
4. The script extracts Lead V6 signals, applies preprocessing (median filter for baseline drift, Butterworth low-pass filter for noise), and saves the results to `preprocessed_lead_v6.h5`.

## Outputs
- **HDF5 File**: `preprocessed_lead_v6.h5`
  - Contains preprocessed Lead V6 signals, keyed by `ecg_id`.
  - Attributes: `patient_id`, `filename_hr`.
- **Visualization**: `Visualizations/preprocessed_signal_00008_hr.png`
  - Corresponds to Figure 3.1 in the thesis, showing original (red) and filtered (black) signals for patient `00008_hr`.

## Thesis Reference
- **Figure 3.1**: [preprocessed_signal_00008_hr.png](https://raw.githubusercontent.com/BalkrishnaMotanavar/Single-Lead-ECG-Classifier-PhaseFolding-LightGBM/main/Visualizations/preprocessed_signal_00008_hr.png)
  - Caption: "Original (top, red) and filtered (bottom, black) Lead V6 signals for patient 00008_hr, demonstrating preprocessing efficacy."

## Notes
- Ensure the `output_hdf5_path` directory exists or is created by the script.
- If `ptbxl_database.csv` is missing, download it from the PTB-XL dataset.
- For troubleshooting, check the thesis (Section 3.1) or contact [your_email@example.com].