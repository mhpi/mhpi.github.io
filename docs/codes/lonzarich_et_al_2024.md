# 𝛿MG + HydroDL 2.0

## Code Release

Code releases and extracted CAMELS data are linked:

- [𝛿MG](https://github.com/mhpi/generic_deltaModel)
- [HydroDL 2.0](https://github.com/mhpi/hydroDL2)
- [Extracted Data](https://onedrive.live.com/login)

### Summary

𝛿MG: A generic, scalable differentiable modeling framework for integrating neural networks and physical models in PyTorch. 

hydroDL2 couples with this framework as a repository for hydrology models to enable MHPI-specific hydrologic modeling capabilities. The combination serve as both benchmarks for published results (including hydroDL) and an exploratory platform for future hydrology research in MHPI.

### Key Novelties
- Hybrid Modeling: Skip manually tuning physical model parameters by using neural networks to feed robust and interpretable parameter predictions directly to the model.
  
- Domain-agnostic and Flexible: Extends beyond hydrology to any field or model benefiting from parameter learning, with modularity to meet the diversity of needs along the way.
- Benchmarks: All in one place. 𝛿MG + hydroDL2 will enable rapid deployment and replication of key published MHPI results.
- NextGen-ready: 𝛿MG is designed to be CSDMS BMI-compliant, and our differentiable hydrology models in hydroDL2 come with a prebuilt BMI allowing seamless compatibility with NOAA-OWP's Next Generation National Water Model. Incidentally, the design of 𝛿MG also lends to it being easily wrapped for other applications.
