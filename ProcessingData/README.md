# Processing the data


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

This ensures no slices from the same individual appear across different splits.