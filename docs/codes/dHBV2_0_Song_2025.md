# High-resolution differentiable model δHBV2.0 for national-scale water modeling 

## Code Release

δHBV2.0 code for streamflow is available at these links: https://doi.org/10.5281/zenodo.14827983 and https://github.com/mhpi/generic_deltamodel

## Short Summary
In this work, we developed a high-resolution, differentiable, physics-informed machine learning model (δHBV2.0) for national-scale water modeling. This model captures sub-basin heterogeneity—an aspect not addressed in previous lumped differentiable models—and enables seamless streamflow prediction across the high-resolution CONUS river network.δHBV2.0 uses neural networks to predict high-resolution physical parameters, which are then used by the HBV and Muskingum-Cunge models to perform high-resolution hydrologic simulation and river network routing. The entire system is differentiable and trained as a unified pipeline, enabling end-to-end optimization. It leverages big data by training on approximately 2,800 basins across CONUS in parallel and enables large-scale parameterization across the entire region, leading to significant performance improvements. δHBV2.0 requires approximately 125 GPU hours to train on 2,800 basins with 15 years of data using an NVIDIA A100 GPU on a server hosted by the University of Alabama. Once trained, it only takes 1 GPU hour to run δHBV2.0UH across all ~180,000 MERIT unit basins for a 40-year simulation.  Beyond the scope of this paper, we have also included an implementation of the SAC-SMA model.
## Bibtex Citation


```bibtex
@article{song2025high,
  title={High-resolution national-scale water modeling is enhanced by multiscale differentiable physics-informed machine learning},
  author={Song, Yalan and Bindas, Tadd and Shen, Chaopeng and Ji, Haoyu and Knoben, Wouter JM and Lonzarich, Leo and Clark, Martyn P and Liu, Jiangtao and van Werkhoven, Katie and Lamont, Sam and others},
  journal={Water Resources Research},
  volume={61},
  number={4},
  pages={e2024WR038928},
  year={2025},
  publisher={Wiley Online Library}
}
```
