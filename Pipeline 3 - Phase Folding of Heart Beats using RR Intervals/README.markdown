# Pipeline 3: Phase Folding

## Description
This pipeline aligns and averages heartbeat cycles in preprocessed Lead V6 signals using R-peak data from Pipeline 2. Phase folding normalizes cycles to 400 points via linear interpolation, aligning R-peaks at phase 0.375 to reduce noise and enable consistent PQRST detection in Pipeline 4. The representative beat is stored in HDF5 and CSV formats, with visualizations for quality assessment.

## Requirements
- **Python Libraries**:
  - `h5py`: For reading input HDF5 files and writing results.
  - `pandas`: For reading `ptbxl_database.csv` and CSV output.
  - `numpy`: For numerical operations and interpolation.
  - `scipy`: For linear interpolation (`interp1d`).
  - `matplotlib`: For generating plots.
  - `tqdm`: For progress bar during dataset processing.
  - `csv`: For CSV output.
- **Input Files**:
  - `ptbxl_database.csv`: For metadata (`filename_hr`, `ecg_id`, `scp_codes`, `age`, `sex`).
  - `preprocessed_lead_v6.h5`: Preprocessed Lead V6 signals from Pipeline 1.
  - `rpeak_detection_results.h5`: R-peak data from Pipeline 2.
- **Dataset**:
  - PTB-XL dataset (500 Hz version) from [PhysioNet](https://physionet.org/content/ptb-xl/1.0.3/).
  - Place in `/path/to/datasets/PTB_XL` and update `base_path`.
- **System**:
  - Python 3.8+.
  - Minimum: 8 GB RAM, 5 GB free storage.

## Usage
1. Install dependencies:
   ```bash
   pip install h5py pandas numpy scipy matplotlib tqdm
   ```
2. Ensure the PTB-XL dataset, `preprocessed_lead_v6.h5`, and `rpeak_detection_results.h5` are available.
3. Open `Phase Folding.ipynb` in Jupyter Notebook:
   ```bash
   jupyter notebook "Phase Folding.ipynb"
   ```
4. Update file paths in the notebook:
   ```python
   base_path = Path("/path/to/datasets/PTB_XL")
   csv_path = base_path / "ptbxl_database.csv"
   hdf5_lead_v6_path = "/path/to/preprocessed_lead_v6.h5"
   hdf5_rpeak_path = "/path/to/rpeak_detection_results.h5"
   hdf5_output_path = "/path/to/phase_folded_results.h5"
   csv_output_path = "/path/to/phase_folded_results.csv"
   ```
5. Run the first section to test phase folding on a subset of records (includes visualization for `ecg_id_47`, Figure 3.3).
6. Run the second section to process the entire dataset.
7. Ensure the output directory exists for saving plots (e.g., `phase_folding_ecg_id_47.png`).

## Outputs
- **HDF5 File**: `phase_folded_results.h5`
  - Contains representative beats (400 points) per `ecg_id`.
  - Attributes: `ecg_id`, `patient_id`, `filename_hr`, `mean_heart_rate`, `mean_rr_interval`, `rr_std`, `number_of_rpeaks`, `scp_codes`, `age`, `sex`.
- **CSV File**: `phase_folded_results.csv`
  - Stores serialized representative beats and metadata.
- **Visualization**: `Visualizations/phase_folded_ecg_id_47.png`
  - Corresponds to Figure 3.3 in the thesis, showing stacked aligned cycles (top, viridis colormap) and representative beat (bottom, blue) for `ecg_id_47` (NORM).
  - Format: Phase (0 to 1) vs. amplitude (mV), R-peak at phase 0.375 (red dashed line), DPI=300.

## Thesis Reference
- **Figure 3.3**: [phase_folded_ecg_id_47.png](https://raw.githubusercontent.com/BalkrishnaMotanavar/Single-Lead-ECG-Classifier-PhaseFolding-LightGBM/main/Visualizations/phase_folded_ecg_id_47.png)
  - Caption: "Phase folding for ECG ID 47 (NORM), showing signal, aligned beats, and averaged heartbeat."

## Notes
- Accurate R-peak data from Pipeline 2 is critical; misalignment can affect cycle normalization.
- Cycles shorter than 1 sample are skipped to avoid errors.
- The notebook processes `ecg_id_47` for Figure 3.3; ensure it’s included in the input HDF5 files.
- For troubleshooting, check the thesis (Section 3.3) or contact [your_email@example.com].
