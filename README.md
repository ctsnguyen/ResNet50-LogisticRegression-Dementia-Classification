<a id="readme-top"></a>

<br />
<div align="center">
  <a href="https://github.com/your-team-repo/ECS171-Alzheimer-MRI">
  </a>

  <h2 align="center">Alzheimer's Disease Classification from MRI using ResNet</h2>
  <p align="center">
    <strong>ECS171 Machine Learning Project • UC Davis</strong>
    <br />
    <br />
    <br />
    <br />
  </p>
</div>

## About The Project

This project focuses on building machine-learning models to detect Dementia using structural MRI data from the OASIS-1 dataset. Our goal is to create a non-invasive, cost-effective screening tool capable of distinguish between demented and non-demented patients based on structural brain patterns. 

We implement and compare three different modeling approaches:

1. **ResNet-50 CNN**  
   A fine-tuned convolutional neural network trained on FAST-segmented MRI slices.

2. **Logistic Regression on Clinical Features**  
   A baseline model trained on metadata such as MMSE, brain volumes, and demographic
   variables.

3. **Fusion Model (MRI + Metadata)**  
   A multimodal architecture combining CNN-generated embeddings with tabular patient data.

This comparative framework allows us to evaluate the predictive value of MRI features versus
clinical information, and highlights challenges in integrating multimodal medical data.

## Dataset

We use the **OASIS-1** Cross-Sectional MRI dataset, containing:

- **438 subjects** aged 18–96  
- Non-demented and demented individuals (CDR 0–2)  
- Raw, processed, and FAST-segmented MRI scans  
- Rich clinical metadata including:
  - Mini-Mental State Examination (MMSE)
  - Socioeconomic Status (SES)
  - Education
  - Estimated Total Intracranial Volume (eTIV)
  - Normalized Whole Brain Volume (nWBV)
  - Gray/White/CSF volumetric measurements

For CNN training, we use the **FAST-segmented, skull-stripped MRI slices**, which minimize
artifacts and highlight brain tissue differences relevant to AD classification.


## Data Processing

We performed essential preprocessing on both MRI images and patient metadata to prepare the
OASIS-1 dataset for modeling. This included cleaning the clinical data, selecting relevant MRI
slices, and applying standard transformations such as resizing, normalization, and data
augmentation. All preprocessing steps were designed to ensure consistency across subjects and to
prevent data leakage during training.

A full, detailed explanation of the preprocessing workflow—including slice selection, FAST
segmentation handling, feature engineering, and dataset organization—is available in the
**`ProcessingData/`** folder of this repository.

## Results Summary
We evaluated three model families: CNN, Logistic Regression, and Fusion Model.

### ResNet-50 CNN (MRI-Only)
- **Test Accuracy: 77%**
- Able to extract structural MRI features associated with dementia.
- Key challenges:
  - Learning of ResNet logits was difficult, had trouble managing the bias/variance tradeoff.
  - Overfitting occurred when too many backbone layers were unfrozen.

### Logistic Regression (Tabular-Only)
- **Test Accuracy: 83%**
- Highest performance across all models.
- **MMSE** was the most influential feature:
  - Removing MMSE reduced accuracy from ~83% to ~72%.
- Other strong features included:
  - `gm_ml`, `csf_ml`, `brain_ml`, and `nWBV`

### Fusion Model (MRI + Tabular)
- **Test Accuracy: 67%**
- Underperformed due to:
  - Repeated tabular features across many slices → model over-relied on metadata  
  - Missing MMSE in early experiments  
  - Poor fusion alignment between high-dimensional CNN features and low-dimensional metadata

### Key Takeaways
- Clinical data, especially **MMSE**, provides strong predictive power in OASIS-1.
- CNNs are promising but require larger datasets or improved slice selection.
- Multimodal fusion is challenging and requires careful feature balancing to avoid overfitting.


### Built With

- Python
- PyTorch
- scikit-learn
- NiBabel
- Matplotlib
- Google Colab

<p align="right">(<a href="#readme-top">back to top</a>)</p>

## Getting Started


<p align="right">(<a href="#readme-top">back to top</a>)</p>

### Prerequisites

- Python 3.8+
- pip

<p align="right">(<a href="#readme-top">back to top</a>)</p>

### Installation & Quick Start

- Fork the repository
- Clone the forked repository
```bash
git clone https://github.com/[your-GitHub-username]/ResNet50-LogisticRegression-Dementia-Classification.git
```

Since our build uses Google Colab's library, you need to import your forked and cloned repository to a shared Google Drive. 
- Open Google Drive and right click on "Shared drives". Select "Create a new shared drive...", name the new drive, and click "Create".
- Select "New" and "Upload Folder" within the new shared drive. Then select the cloned repository in your file directory.
- Open the "data" folder and open "Dataset_Import.ipynb" with Google Colab and run all in the notebook, ensuring that the destination includes the name of your shared drive and also paths to any folder you would like to download it to if not the root. This will directly download all the data from OASIS-1 and OASIS-2 into the shared drive.
- For the Preprocessing notebook, there is a cell that prompts you to upload the .img, .hdr, and .txt files from each other patient's FSL_SEG folders once you have downloaded the OASIS disc.tar.gz files. You need to manually upload each patient's files until you have the zip files for each of the extracted features (brain image slices and volumetric data) of each of the patients.
- After you mount the drive paths and the dataset path according to where you downloaded the OASIS-1 and OASIS-2 dataset, you should be able to Run All in colab for the ResNet50 and LogisticRegression notebooks.
