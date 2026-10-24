# PRUF-SST

This repository provides the code for PRUF-SST: Probabilistic Reconstruction of Cloud-Obscured Sea Surface Temperature for Uncertainty-Guided Forecasting.

All code was developed and executed in the Google Colab environment. The repository is organized by module, with the corresponding code and input data provided for each component of the proposed framework.

### 1. PEBNF Reconstruction

The PEBNF module implements probabilistic sea surface temperature (SST) reconstruction using Bayesian neural fields, with sea level anomaly (SLA) incorporated as an auxiliary ocean-dynamical variable.

The `code.ipynb` notebook contains the core PEBNF implementation, including neural-field fitting, ensemble-based probabilistic prediction, predictive-quantile generation, and spatial grid mapping.

### 2. Code Structure

```text
PRUF-SST_PE-BNF/
│
├── code.ipynb
│
└── data/
    └── example/
        ├── cmems_meshgrid.mat
        ├── cmems_train_sla_sst.mat
        └── cmems_validation_sla_sst_gt.mat
```

### 3. Third-party Dependency: BayesNF

The PEBNF implementation uses the open-source [BayesNF](https://github.com/google/bayesnf) package developed by Saad et al. (2024).

BayesNF is a third-party dependency and is not included in this repository. Before running the notebook in Google Colab, install the package using:

```python
!pip install bayesnf
```

Reference:

Saad et al. (2024). Scalable spatiotemporal prediction with Bayesian neural fields. Nature Communications, 15, 7942.

https://doi.org/10.1038/s41467-024-51477-5

### 4. Input Data

The `data/example/` directory contains the input files required by the PEBNF notebook.

- `cmems_meshgrid.mat`: Longitude and latitude grids of the study region.
- `cmems_train_sla_sst.mat`: Input observations used for PEBNF model fitting.
- `cmems_validation_sla_sst_gt.mat`: Validation data used for probabilistic prediction and reconstruction evaluation.

### 5. Execution

The notebook is designed to run in Google Colab. Users should first install BayesNF, prepare the input data, and configure the corresponding file paths.

When the project folder is placed directly under the MyDrive directory in Google Drive, the expected file locations are:

```text
/content/drive/MyDrive/PRUF-SST_PE-BNF/
    code.ipynb
    data/example/cmems_meshgrid.mat
    data/example/cmems_train_sla_sst.mat
    data/example/cmems_validation_sla_sst_gt.mat
```

The notebook performs PEBNF reconstruction using 10-day sliding windows, with independent model fitting for each window. An ensemble of eight neural-field models is used to generate predictive quantiles from 0.01 to 0.99 at intervals of 0.01.

Users can adjust the starting window index to process selected windows or the complete dataset. For execution from the first window, set:

```python
start_window = 0
```

Model outputs are saved in MATLAB-compatible `.mat` format.

### 6. Notes

The notebook provides the core PEBNF fitting and probabilistic prediction procedures. It does not include the complete PRUF-SST forecasting framework.

Users should verify their input data structure, file paths, and software dependencies before execution. The file paths specified in the notebook must be updated to match the actual location of the input data.
