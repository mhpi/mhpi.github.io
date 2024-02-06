# $\delta$ differentiable stream temperature (LSTM + SNTEMP)
![Alt text](../assets/project-figures/Fig5_Rahmani2023.png)

## Code, data, models, and results Release

[See here for the USGS release](https://doi.org/10.1029/2023WR034420)

## Summary

We integrated a process-based stream water temperature model (SNTEMP) with LSTM module in a differentiable platform (PyTorch) to predict dily stream water temperature. The main difference between the differentiable model and many other previous physics-guided frameworks is that the process-based model is written in a differentiable platform like those used for NNs. This approach makes it possible for the framework to seamlessly integrate both machine learning and process-based model parts and be trained as a unified model (Shen et al., 2023). The training process is “end-to-end,” with any parameters within the model being learnable by gradient descent, just like when a pure LSTM model is trained. Thus, there is no need to calibrate each component independently. In other words, the process-based model becomes a part of the NN with the added advantage of process transparency..
All the hybrid models were trained on daily water temperature observations for 415 stations across the conterminous United States (CONUS). We evaluated the model with two metrics: the daily temperature simulation accuracy against observed data, and the correlation between the baseflow estimation of our model with a published alternative estimate based on baseflow recession analysis. 

## Bibtex Citation


```bibtex
@article{Rahmani2021lstm,
  title={Identifying Structural Priors in a Hybrid Differentiable Model for Stream Water Temperature Modeling},
  author={Rahmani, Farshid and Appling, Alison and Feng, Dapeng and Lawson, Kathryn and Shen, Chaopeng},
  journal={Water Resources Research},
  year={2023},
  publisher={AGU}
}
```
