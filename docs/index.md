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

!!! info

    All models were trained from 1980/10/01 to 1995/09/30 and tested from 1995/10/01 to 2010/09/30 on all 671 CAMELS basins.

| Model |  Median NSE | Median KGE | Median Absolute FLV (%) | Median Non-Absolute FLV (%) | Median low flow RMSE (mm/day) | Median peak flow RMSE (mm/day) | 
| --- | --- | --- | --- | --- | --- | --- |
| LSTM (Kratzert et al. 2019) | 0.74 | 0.75 | 32.02 (5.54) | 18.02 (-15.80) | 0.051  | 3.70  |
| LSTM-hydroDL (high-flow expert) | 0.74 | 0.76 | 31.79 (-9.08) | 16.20 (-13.42) | 0.049 | 3.28 |

#### All HydroDL Models

!!! info

    All models were trained from 1980/10/01 to 1995/09/30 and tested from 1995/10/01 to 2010/09/30 on all 671 CAMELS basins.

| Model |  Median NSE | Median KGE | Median Absolute FLV (%) | Median Non-Absolute FLV (%) | Median low flow RMSE (mm/day) | Median peak flow RMSE (mm/day) | Baseflow index spatial correlation | Median NSE of temporal ET simulation|
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| LSTM-hydroDL (low-flow expert) | 0.75 | 0.75 | 20.01 (15.39) | 17.01 (-3.47) | 0.025 | 3.10  | - | - |
| LSTM-hydroDL (high-flow expert) | 0.75 | 0.78 | 44.88 | 14.13 | 0.059 | 2.51 | - | - |
| LSTM (Kratzert et al. 2019’s code) | 0.73 | 0.77 | 40.59 (29.70) | 13.46  (-4.19) | 0.055  | 2.56  | - | - |
| SAC-SMA (Traditional) | 0.66 | 0.73 | 59.40 (46.96) | 17.55 (-9.79) | 0.081 | 3.19 | - | - |
| Unmodified $\delta$HBV | 0.69 | 0.72 | 47.58 (16.84) | 16.40 (-10.80) | 0.066  | 2.74 | 0.76 | 0.43  |
| $\delta$HBV  | 0.73 | 0.73 | 56.53 (50.93) | 15.29 (-8.89) | 0.074 | 2.56 | 0.76 | 0.59  |
| $\delta$HBV.adj (expert 1)| 0.72 | 0.75 | 43.29 (37.61) | 13.25 (-4.33) | 0.048 | 2.47 | 0.83 | 0.61 |
| $\delta$HBV.adj (expert 2)| 0.75 | 0.76 | 40.56 (32.78) | 14.09 (-7.97) | 0.045 | 2.59 | 0.87 | 0.62 |
### Citations

<div class="result" markdown>

=== "LSTM (Kratzert et al. 2019)"  

    ```
    Kratzert, Frederik, Daniel Klotz, Guy Shalev, Günter Klambauer, Sepp Hochreiter, and Grey 
    Nearing. "Benchmarking a catchment-aware long short-term memory network (LSTM) for
    large-scale hydrological modeling." Hydrol. Earth Syst. Sci. Discuss 2019 (2019): 1-32.
    ``` 

=== "LSTM-hydroDL (low-flow expert)"   

    ```
    Feng, Dapeng, Kuai Fang, and Chaopeng Shen. "Enhancing streamflow forecast and extracting 
    insights using long‐short term memory networks with data integration at continental scales.
    " Water Resources Research 56, no. 9 (2020): e2019WR026793. 
    ``` 

=== "LSTM-hydroDL (high-flow expert)"    

    ``` 
    Feng, Dapeng, Kuai Fang, and Chaopeng Shen. "Enhancing streamflow forecast and extracting 
    insights using long‐short term memory networks with data integration at continental scales.
    " Water Resources Research 56, no. 9 (2020): e2019WR026793. 
    ``` 

=== "SAC-SMA (Traditional)"

    ```
    Newman, Andrew J., Martyn P. Clark, Kevin Sampson, Andrew Wood, Lauren E. Hay, Andy Bock, 
    Roland J. Viger et al. "Development of a large-sample watershed-scale hydrometeorological 
    data set for the contiguous USA: data set characteristics and assessment of regional 
    variability in hydrologic model performance." Hydrology and Earth System Sciences 19, no. 1 
    (2015): 209-223. 
    ```

=== "Unmodified $\delta$ HBV" 

    ``` 
    Feng, D., Liu, J., Lawson, K. and Shen, C., 2022. Differentiable, learnable, regionalized 
    process‐based models with multiphysical outputs can approach state‐of‐the‐art hydrologic 
    prediction accuracy. Water Resources Research, 58(10), p.e2022WR032404. 
    ``` 

=== "$\delta$ HBV" 

    ``` 
    Feng, D., Liu, J., Lawson, K. and Shen, C., 2022. Differentiable, learnable, regionalized 
    process‐based models with multiphysical outputs can approach state‐of‐the‐art hydrologic 
    prediction accuracy. Water Resources Research, 58(10), p.e2022WR032404.
    ``` 

=== "$\delta$ HBV.adj"

    ```
    Song, Yalan, Wouter Knoben, Martyn P. Clark, Dapeng Feng, Kathryn Lawson, and Chaopeng 
    Shen, When ancient numerical demons meet physics-informed machine learning: adjoint-based 
    gradients for implicit differentiable modeling 
    Preprint link: https://hess.copernicus.org/preprints/hess-2023-258/ 
    ```

</div>

