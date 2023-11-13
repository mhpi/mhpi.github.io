# HydroDL

[![PyPI](https://img.shields.io/pypi/v/hydroDL?color=blue)](https://pypi.org/project/hydroDL/)  [![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.3993880.svg)](https://doi.org/10.5281/zenodo.3993880) [![CodeStyle](https://img.shields.io/badge/code%20style-Black-black)]()

HydroDL is a coding library designed to model hydrologic systems using neural networks and differentiable ($\delta$) models. Applications range from soil moisture to streamflow. 


## Benchmarks

### CDF

<figure markdown>
  ![CDF](assets/images/CDF_NSE_adjoint.png){width="750"}
  <figcaption>Camels NSE of popular streamflow models</figcaption>
</figure>

### Metric Tables

#### HydroDL versus Neural Hydrology

| Model |  Median NSE | Median KGE | Median Absolute FLV (%) | Median Non-Absolute FLV (%) | Median low flow RMSE (mm/day) | Median peak flow RMSE (mm/day) | 
| --- | --- | --- | --- | --- | --- | --- |
| LSTM (Kratzert et al. 2019) | 0.74 | 0.75 | 32.02 (5.54) | 18.02 (-15.80) | 0.051  | 3.70  |
| LSTM-hydroDL (high-flow expert) | 0.74 | 0.76 | 31.79 (-9.08) | 16.20 (-13.42) | 0.049 | 3.28 |

#### All HydroDL Models

| Model |  Median NSE | Median KGE | Median Absolute FLV (%) | Median Non-Absolute FLV (%) | Median low flow RMSE (mm/day) | Median peak flow RMSE (mm/day) | Baseflow index spatial correlation | Median NSE of temporal ET simulation|
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| LSTM-hydroDL (low-flow expert) | 0.75 | 0.75 | 20.01 (15.39) | 17.01 (-3.47) | 0.025 | 3.10  | - | - |
| LSTM-hydroDL (high-flow expert) | 0.75 | 0.78 | 44.88 | 14.13 | 0.059 | 2.51 | - | - |
| LSTM (Kratzert et al. 2019’s code) | 0.73 | 0.77 | 40.59 (29.70) | 13.46  (-4.19) | 0.055  | 2.56  | - | - |
| SAC-SMA (Traditional) | 0.66 | 0.73 | 59.40 (46.96) | 17.55 (-9.79) | 0.081 | 3.19 | - | - |
| Unmodified $\delta$HBV | 0.69 | 0.72 | 47.58 (16.84) | 16.40 (-10.80) | 0.066  | 2.74 | 0.76 | 0.43  |
| $\delta$HBV  | 0.73 | 0.73 | 56.53 (50.93) | 15.29 (-8.89) | 0.074 | 2.56 | 0.76 | 0.59  |
| $\delta$HBV.adj | 0.75 | 0.76 | 40.56 (32.78) | 14.09 (-7.97) | 0.045 | 2.59 | 0.87 | 0.62 |

<div id="nse-cdf-plot"></div>

