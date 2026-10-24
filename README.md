# PRUF-SST

This repository provides the code for PRUF-SST: Probabilistic Reconstruction of Cloud-Obscured Sea Surface Temperature for Uncertainty-Guided Forecasting.

All code was developed and executed in the Google Colab environment. The repository is organized by module, with the corresponding code and input data provided for each component of the proposed framework.

### 1. PEBNF Reconstruction

The PEBNF module implements probabilistic sea surface temperature (SST) reconstruction using Bayesian neural fields, with sea level anomaly (SLA) incorporated as an auxiliary ocean-dynamical variable.

The `code.ipynb` notebook contains the core PEBNF implementation, including neural-field fitting, ensemble-based probabilistic prediction, predictive-quantile generation, and spatial grid mapping.

The subsequent AOQS-based quantile selection and relative uncertainty (RU) computation are performed separately.

### 2. RUGM Forecasting

The RUGM module implements uncertainty-guided SST forecasting using an encoder-decoder ConvLSTM architecture.

The `code.ipynb` notebook contains the core implementation of the Relative Uncertainty-Guided Modulation (RUGM) mechanism, including RU normalization, high-uncertainty region selection, learnable feature gating, and autoregressive SST forecasting.

The model uses 10 days of reconstructed SST and corresponding RU fields as historical inputs to predict SST for the following 3 days.

For the South China Sea experiment, the RUGM module applies a Top-20% uncertainty selection strategy with a learnable attenuation coefficient to modulate the input-derived convolutional features.

### 3. Code Structure

The PEBNF and RUGM modules are organized separately, with their corresponding example data.

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


PRUF-SST_RUGM/
│
├── code.ipynb
│
└── data/
    └── example/
        ├── mask.bin
        ├── Train_Best_RU_10to3_GT.mat
        ├── Val_Best_RU_10to3_GT.mat
        └── Test_Best_RU_10to3_GT.mat
```

The module packages, including their code and accompanying input data, are distributed through the [Releases](https://github.com/xuluochuan340-hub/PRUF-SST/releases) section when published.

### 4. Third-party Dependency: BayesNF

The PEBNF implementation uses the open-source [BayesNF](https://github.com/google/bayesnf) package developed by Saad et al. (2024).

BayesNF is a third-party dependency and is not included in this repository. Before running the PEBNF notebook in Google Colab, install the package using:

```python
!pip install bayesnf
```

Reference:

Saad et al. (2024). Scalable spatiotemporal prediction with Bayesian neural fields. Nature Communications, 15, 7942.

https://doi.org/10.1038/s41467-024-51477-5

The RUGM forecasting module is implemented using PyTorch.

### 5. Input Data

The `data/example/` directories contain the input files provided for the corresponding modules.

PEBNF Reconstruction:

- `cmems_meshgrid.mat`: Longitude and latitude grids of the study region.
- `cmems_train_sla_sst.mat`: Input observations used for PEBNF model fitting.
- `cmems_validation_sla_sst_gt.mat`: Validation data used for probabilistic prediction and reconstruction evaluation.

RUGM Forecasting:

- `mask.bin`: Land-ocean mask used to identify valid ocean pixels.
- `Train_Best_RU_10to3_GT.mat`: Training data containing reconstructed SST, RU fields, and future SST reference values.
- `Val_Best_RU_10to3_GT.mat`: Validation data used for model selection and performance evaluation.
- `Test_Best_RU_10to3_GT.mat`: Testing data used to evaluate the forecasting performance.

The RUGM input data are derived from the reconstructed SST and corresponding uncertainty information produced by the reconstruction stage.

### 6. Execution

Both notebooks are designed to run in Google Colab. Users should prepare the corresponding input files, install the required dependencies, and configure the file paths before execution.

When the module folders are placed directly under the MyDrive directory in Google Drive, the expected file organization is:

```text
/content/drive/MyDrive/
│
├── PRUF-SST_PE-BNF/
│   ├── code.ipynb
│   └── data/example/
│       ├── cmems_meshgrid.mat
│       ├── cmems_train_sla_sst.mat
│       └── cmems_validation_sla_sst_gt.mat
│
└── PRUF-SST_RUGM/
    ├── code.ipynb
    └── data/example/
        ├── mask.bin
        ├── Train_Best_RU_10to3_GT.mat
        ├── Val_Best_RU_10to3_GT.mat
        └── Test_Best_RU_10to3_GT.mat
```

6.1. PEBNF Reconstruction

The PEBNF notebook performs probabilistic SST reconstruction using 10-day sliding windows, with independent model fitting for each window.

An ensemble of eight neural-field models is used to generate predictive quantiles from 0.01 to 0.99 at intervals of 0.01.

Users can adjust the starting window index to process selected windows or the complete dataset. To begin from the first window, set:

```python
start_window = 0
```

The reconstruction outputs are saved in MATLAB-compatible `.mat` format.

6.2. RUGM Forecasting

The RUGM notebook performs SST forecasting using an encoder-decoder ConvLSTM with uncertainty-guided feature modulation.

The historical input consists of 10 days of reconstructed SST and RU fields, while the forecasting target consists of the following 3 days of SST.

For the South China Sea configuration, the high-uncertainty selection proportion is set to:

```python
TOP_RU_PERCENT = 0.20
```

The model uses a learnable attenuation coefficient to modulate the input-derived convolutional features in high-uncertainty regions.

Training, validation, and testing are performed using the corresponding input datasets. Model checkpoints and prediction results are saved to the configured output directories.

### 7. Notes

The PEBNF notebook provides the core neural-field fitting and probabilistic prediction procedures, while the RUGM notebook provides the uncertainty-guided ConvLSTM forecasting implementation.

The provided modules do not include all intermediate data-processing procedures or the complete set of experiments reported in the paper.

Users should verify their input data structures, software dependencies, and file paths before execution. The paths specified in the notebooks may differ from the example directory structure shown above and should be adjusted accordingly.

The example datasets are intended to demonstrate the implementation and are not necessarily sufficient to reproduce all quantitative results reported in the paper.
