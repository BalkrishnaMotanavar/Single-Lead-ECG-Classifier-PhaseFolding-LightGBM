# Pipeline 2: R-Peak Detection and Clinical Rhythm Interpretation

## Description
This pipeline detects R-peaks in preprocessed Lead V6 signals from Pipeline 1 using NeuroKit2's Pan-Tompkins algorithm. R-peaks mark ventricular depolarization, enabling cycle delineation and feature extraction (e.g., RR intervals, heart rate variability). It also provides clinical interpretations (e.g., bradycardia, tachycardia) based on heart rate and RR intervals. The pipeline generates visualization plots, including those for Figure 3.2 in the thesis (normal sinus rhythm and myocardial infarction).

## Requirements
- **Python Libraries**:
  - `h5py`: For reading `preprocessed_lead_v6.h5` and writing results.
  - `pandas`: For reading `ptbxl_database.csv` and CSV output.
  - `matplotlib`: For generating plots.
  - `numpy`: For numerical operations.
  - `neurokit2`: For R-peak detection.
  - `tqdm`: For progress bar during dataset processing.
  - `csv`: For CSV output.
- **Input Files**:
  - `ptbxl_database.csv`: For mapping `filename_hr` to `ecg_id` and retrieving SCP codes.
  - `preprocessed_lead_v6.h5`: Preprocessed Lead V6 signals from Pipeline 1.
- **Dataset**:
  - PTB-XL dataset (500 Hz version) from [PhysioNet](https://physionet.org/content/ptb-xl/1.0.3/).
  - Place in `/path/to/datasets/PTB_XL` and update `base_path`.
- **System**:
  - Python 3.8+.
  - Minimum: 8 GB RAM, 5 GB free storage.

## Usage
1. Install dependencies:
   ```bash
   pip install h5py pandas matplotlib numpy neurokit2 tqdm
   ```
2. Ensure the PTB-XL dataset and `preprocessed_lead_v6.h5` are available.
3. Open `R-Peak Detection_Clinical Rhythm Interpretation.ipynb` in Jupyter Notebook:
   ```bash
   jupyter notebook "R-Peak Detection_Clinical Rhythm Interpretation.ipynb"
   ```
4. Update file paths in the notebook:
   ```python
   base_path = Path("/path/to/datasets/PTB_XL")
   csv_path = base_path / "ptbxl_database.csv"
   hdf5_input_path = "/path/to/preprocessed_lead_v6.h5"
   hdf5_output_path = "/path/to/rpeak_detection_results.h5"
   csv_output_path = "/path/to/rpeak_detection_results.csv"
   ```
5. To generate Figure 3.2 plots (`rpeak_detection_ecg_id_5.png`, `rpeak_detection_ecg_id_177.png`):
   - Modify the `records_to_test` list in the first section:
     ```python
     records_to_test = [
         "records500/00000/00005_hr",  # NORM, ecg_id_5
         "records500/00000/00177_hr",  # MI, ecg_id_177
     ]
     ```
   - Run the first section to generate and save plots to `/path/to/Visualization/`.
6. Run the second section to process the entire dataset.
7. Ensure the `Visualization/` directory exists for saving plots.

## Outputs
- **HDF5 File**: `rpeak_detection_results.h5`
  - Contains R-peak data (indices, times, amplitudes, RR intervals, heart rates) per `ecg_id`.
  - Attributes: `ecg_id`, `patient_id`, `filename_hr`, `mean_heart_rate`, `mean_rr_interval`, `rr_std`, `number_of_rpeaks`, `scp_codes`, `age`, `sex`.
- **CSV File**: `rpeak_detection_results.csv`
  - Stores R-peak data and metadata in serialized form.
- **Visualization Plots**:
  - `Visualizations/rpeak_detection_ecg_id_5.png`: Normal sinus rhythm (Figure 3.2, top).
  - `Visualizations/rpeak_detection_ecg_id_177.png`: Myocardial infarction (Figure 3.2, bottom).
  - Format: Preprocessed Lead V6 signal (black) with R-peaks (red dots), legend outside plot, DPI=300.

## Thesis Reference
- **Figure 3.2**: "R-peak detection for patients with normal sinus rhythm (top) and myocardial infarction (bottom), showing detected R-peaks (red dots) on preprocessed Lead V6 signals."

## Notes
- If `ecg_id_5` or `ecg_id_177` are missing from `preprocessed_lead_v6.h5`, verify Pipeline 1 processed these records.
- To generate only Figure 3.2 plots, limit `records_to_test` to `["records500/00000/00005_hr", "records500/00000/00177_hr"]` and run the first section.
- Upload the generated plots to the `Visualizations` folder on GitHub:
  - Go to `Visualizations` in the repository.
  - Click “Add file” > “Upload files.”
  - Drag and drop `rpeak_detection_ecg_id_5.png` and `rpeak_detection_ecg_id_177.png`.
  - Commit with a message like “Added R-peak detection plots for Figure 3.2.”
- For troubleshooting, check the thesis (Section 3.2) or contact [your_email@example.com].
