# $\delta$ MC-Juniata-hydroDL2

!!! info

    This paper is currently in preprint and is under review

## Code Release

Below are Zenodo releases for:

- [Code](https://zenodo.org/records/10183449)
- [Dataset](https://zenodo.org/records/10183449)

The code is released on Github [here](https://github.com/mhpi/dMC-Juniata-hydroDL2)

## Summary

A novel differentiable modeling framework to perform routing and to learn a “parameterization scheme” (a systematic way of inferring parameters from more rudimentary information) for routing flows on the river network.

Below are the main points from our paper:

- A novel differentiable routing model can learn effective river routing parameterization, recovering channel roughness in synthetic runs.
- With short periods of real training data, we can improve streamflow in large rivers compared to models not considering routing.
- For basins >2,000 km2, our framework outperformed deep learning models that assume homogeneity, despite bias in the runoff forcings.

## Bibtex Citation


```bibtex
@article{bindas2023improving,
  title={Improving large-basin river routing using a differentiable Muskingum-Cunge model and physics-informed machine learning},
  author={Bindas, Tadd and Tsai, Wen-Ping and Liu, Jiangtao and Rahmani, Farshid and Feng, Dapeng and Bian, Yuchen and Lawson, Kathryn and Shen, Chaopeng},
  journal={Authorea Preprints},
  year={2023},
  publisher={Authorea}
}
```
