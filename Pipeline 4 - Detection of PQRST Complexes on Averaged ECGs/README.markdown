# Pipeline 4: PQRST Detection

## Description
This pipeline detects PQRST complexes in phase-folded Lead V6 signals from Pipeline 3 using ECGdeli standards. It applies bandpass filtering, Savitzky-Golay smoothing, and Hilbert transform to identify wave onsets, peaks, and offsets. Clinical features (e.g., PR interval, ST-elevation, T-wave duration) are extracted for LightGBM classification in Pipeline 5. Visualizations validate the detection accuracy.

## Requirements
- **Python Libraries**:
  - `h5py`: For reading `phase_folded_results.h5` and writing results.
  - `pandas`: For reading CSV files (`ptbxl_database.csv`, `rpeak_detection_results.csv`, `ptbxl_database_with_superclass.csv`) and CSV output.
  - `numpy`: For numerical operations.
  - `scipy`: For signal processing (peak detection, filtering, Hilbert transform).
  - `matplotlib`: For generating plots.
  - `tqdm`: For progress bar during dataset processing.
- **Input Files**:
  - `ptbxl_database.csv`: PTB-XL metadata.
  - `rpeak_detection_results.csv`: R-peak metadata from Pipeline 2.
  - `ptbxl_database_with_superclass.csv`: Superclass labels from Pipeline 5 (first notebook).
  - `phase_folded_results.h5`: Phase-folded representative beats from Pipeline 3.
- **Dataset**:
  - PTB-XL dataset (500 Hz version) from [PhysioNet](https://physionet.org/content/ptb-xl/1.0.3/).
  - Place in `/path/to/datasets/PTB_XL` and update paths.
- **System**:
  - Python 3.8+.
  - Minimum: 8 GB RAM, 5 GB free storage.

## Usage
1. Install dependencies:
   ```bash
   pip install h5py pandas numpy scipy matplotlib tqdm
   ```
2. Ensure the PTB-XL dataset and input files are available.
3. Open `PQRST_Detection.ipynb` in Jupyter Notebook:
   ```bash
   jupyter notebook PQRST_Detection.ipynb
   ```
4. Update file paths in the notebook:
   ```python
   phase_folded_path = Path("/path/to/phase_folded_results.h5")
   ptbxl_csv_path = Path("/path/to/datasets/PTB_XL/ptbxl_database.csv")
   rpeak_csv_path = Path("/path/to/rpeak_detection_results.csv")
   superclass_csv_path = Path("/path/to/ptbxl_database_with_superclass.csv")
   output_h5_path = Path("/path/to/PQRST_Complexes_and_Features_final.h5")
   output_csv_path = Path("/path/to/PQRST_Complexes_and_Features_final.csv")
   ```
5. Run the first section to process the entire dataset and generate PQRST features.
6. Run the second section to generate visualization plots for sample `ecg_ids` (e.g., `ecg_plot_21.png`, includes `ecg_id_47` for Figure 3.4).
7. Ensure the output directory exists for saving plots.

## Outputs
- **HDF5 File**: `PQRST_Complexes_and_Features_final.h5`
  - Contains representative beats and PQRST point indices (onsets, peaks, offsets) per `ecg_id`.
  - Attributes: `ecg_id`, `patient_id`, `age`, `sex`, `filename_hr`, `scp_codes`, `strat_fold`, `diagnostic_superclass`, `diagnostic_single_5_superclass`, clinical features (e.g., `pq_interval_ms`, `r_amplitude_mv`).
- **CSV File**: `PQRST_Complexes_and_Features_final.csv`
  - Stores PQRST features and metadata.
- **Visualization**: `Visualizations/pqrst_detection_ecg_id_47.png`
  - Corresponds to Figure 3.4 in the thesis, showing phase-folded ECG (black) with PQRST points (colored markers: P-green, Q-blue, R-red, S-purple, T-orange).
  - Format: Phase (0 to 1) vs. amplitude (mV), DPI=200.

## Thesis Reference
- **Figure 3.4**: "PQRST detection for ECG ID 47 (NORM), showing wave onsets, peaks, and offsets."

## Notes
- Noise in phase-folded signals can affect P-wave detection; verify Pipeline 3 outputs.
- The notebook assumes `ecg_id_47` is processed; check input HDF5 files.
- Features are validated against ECGdeli in Pipeline 5 (see Figure 4.3).
- For troubleshooting, check the thesis (Section 3.4) or contact [your_email@example.com].
