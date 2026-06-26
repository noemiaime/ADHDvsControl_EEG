# ADHDvsControl_EEG

This repository contains a computational neuroscience project focused on the analysis and classification of EEG signals from ADHD and Control subjects.

The aim of the project is to extract interpretable EEG features from time-series recordings and evaluate whether these features can distinguish ADHD subjects from Control subjects using statistical analysis and machine learning.

## Project overview

The project follows the workflow:

1. dataset exploration;
2. EEG spectral feature extraction;
3. feature exploration and statistical analysis;
4. feature correlation analysis;
5. Random Forest classification and feature importance.

The analysis is performed at subject level. This means that each subject is represented by one row in the final feature table.

## Dataset

The dataset contains EEG recordings from ADHD and Control subjects during a visual attention task.

The dataset includes:

* 121 subjects: 61 ADHD subjects + 60 Control subjects;
* 19 EEG channels;
* sampling frequency of 128 Hz.

The EEG channels are:

```text
Fp1, Fp2, F3, F4, C3, C4, P3, P4, O1, O2,
F7, F8, T7, T8, P7, P8, Fz, Cz, Pz
```

The channel names and the application of electrodes followed the international 10--20 EEG system.

## Repository structure

```text
ADHDvsControl_EEG/
│
├── data/
│   └── archive.zip
│
├── documentation/
|   └── paper/
│
├── outputs/
│   ├── figures/
│   ├── tabs/
|   ├── report/
|   └── eeg_extracted_features.csv
│
├── 0a_dataset_exploration.ipynb
├── 1a_features_extraction.ipynb
├── 1b_features_exploration.ipynb
├── 2a_random_forest.ipynb
├── .gitignore
└── README.md
```

## Data handling

The raw CSV dataset is not stored in the repository it is a large file.

The repository contains the compressed dataset file:

```text
data/archive.zip
```

The notebook `0a_dataset_exploration.ipynb` extracts the CSV file locally when needed.

The `.gitignore` file ignores CSV files inside the `data/` folder:

```text
data/*.csv
```
## Notebooks

### `0a_dataset_exploration.ipynb`

This notebook explores the raw dataset.

### `1a_features_extraction.ipynb`

This notebook extracts EEG features from the raw time series.

For each subject and each EEG channel, the Power Spectral Density is estimated using Welch's method.

The following EEG frequency bands are considered:

* delta: 1--4 Hz;
* theta: 4--8 Hz;
* alpha: 8--12 Hz;
* beta: 12--30 Hz.

For each channel, band powers are computed by integrating the PSD within each frequency band.

The theta/beta ratio is also computed.

In addition to channel-level features, regional features are extracted by grouping channels into approximate scalp regions:

* frontal;
* central;
* temporal;
* parietal;
* occipital.

The final output is a subject-level feature table 'eeg_extracted_features.csv' saved in:

```text
outputs
```

### `1b_features_exploration.ipynb`

This notebook explores and compares the extracted EEG features between ADHD and Control subjects.

The analysis includes:

* descriptive statistics;
* Mann-Whitney U tests;
* Cohen's d effect size;
* False Discovery Rate correction;
* Spearman correlation matrix across EEG features.

### `2a_random_forest.ipynb`

This notebook trains and evaluates Random Forest classifiers for ADHD vs Control classification.

The dataset is split into:

* training/validation set;
* independent test set.

The test set is kept untouched during model selection and is used only for the final evaluation.

Three feature sets are compared using stratified cross-validation:

1. all EEG features;
2. channel-level features only;
3. regional mean features only.

The best-performing feature set is selected using balanced accuracy and then tuning on the hyperparameters is execuded.

The final model is then trained on the full training/validation set and evaluated on the independent test set.
Features importance is computed. 

The best-performing feature set is then reduced using correlation matrix. 

It has been evaluated a regional theta-beta ratio features set and channel level theta-beta ratio, to understand if theta
beta ratio is sufficient to classify ADHD.

Every final model's hyperparameters has been selected through tuning based on randomized search and grid search.

### `3a_logistic_regressiom.ipynb`

The selected features sets from the Random Forest analysis were also evaluated using Logistic Regression. 
This analysis was performed to compare the Random Forest model with a simpler linear classifier and to assess whether the two group could be separated using a linear decision boundary. 

## Notes

This project is developed for the Computational Applications to Neuroscience course and it is still working progress.
