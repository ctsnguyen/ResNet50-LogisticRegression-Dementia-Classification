# Processing the data

### How the Notebook works
This notebook handles the extraction and preprocessing of MRI data from the OASIS dataset. Before running the pipeline, you must download the disc1.tar.gz, disc2.tar.gz, etc. files from the OASIS website and extract them locally.

### Required Manual Upload Step

Inside each patient’s folder, locate the FSL_SEG directory. For every patient, you will find three required files: *.img, *.hdr, *.txt

These contain the segmented MRI volume, header metadata, and volumetric anatomical features (GM/WM/CSF), respectively. When running the notebook, you will encounter a cell that prompts you to upload these files manually. You must:

Open the upload widget.
Select the .img, .hdr, and .txt files for a single patient’s FSL_SEG folder.
Upload them.


### Tabular Data Processing
- Removed patients with missing metadata, reducing usable tabular cases from **438 → 216**.  
- Engineered additional ratio features:
  - `gm_ratio`, `wm_ratio`, `csf_ratio`, `gm_wm_ratio`
- Converted cleaned metadata into JSON and XLSX for modeling.
- Excluded irrelevant metadata (`patient_id`, `dominant_hand`).

### MRI Image Processing
- Extracted central brain slices where AD-related atrophy is most likely to appear.
- Selected two slice configurations per patient:
  - **12 slices**
  - **40 slices**
- Applied preprocessing steps:
  - Resize to **224 × 224**
  - Normalize grayscale intensities
  - Data augmentation: rotation, horizontal flip, scaling, translation

### Train/Validation/Test Split
To avoid data leakage, we split **patient-wise** (not slice-wise):

- **75%** Train  
- **10%** Validation  
- **15%** Test  


This ensured no slices from the same individual appear across different splits.
