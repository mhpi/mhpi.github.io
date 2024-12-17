# 𝛿MG

## The Generic, Scalable Differentiable Modeling Framework
<!-- ![Alt text](../assets/project-figures/dMG.png) -->

[Code Release][dmg_code] 

[`𝛿MG`](https://github.com/mhpi/generic_deltaModel) is a domain-agnostic, PyTorch-based framework for developing trainable differentiable models that merge neural networks with process-based equations. 𝛿MG is not a partcularly model. Rather, it is a generic framework that support many models across various domains (some are from HydroDL2.0) in a uniform way, while integrating ecosystem tools. Although the packages contains some basic examples for learners' convenience, the deployment models are supposed to exit in separate repositories and couple to the 𝛿MG framework. 

For example, the [`hydroDL2`](https://github.com/mhpi/hydroDL2) repository for hydrology models couples with the 𝛿MG framework to enable MHPI-specific hydrologic modeling capabilities. The combination serves both as a benchmark capability for published results (including those that used hydroDL) and an exploratory platform for future hydrology research in MHPI. 

We include an optional [GUI](https://mhpi-spatial.s3.us-east-2.amazonaws.com/mhpi-release/config_builder_gui/Config+Builder+GUI.zip) ([source code](https://github.com/mhpi/GUI-Config-builder)) for constructing/editing 𝛿MG YAML configuration files with a user-friendly interface:

Closely synergizes with deep learning tools and the scale advantage of PyTorch. Maintained by the [MHPI group](http://water.engr.psu.edu/shen/) advised by Dr. Chaopeng Shen.

<br>

## Differentiable Models
 
Characterized by the combination of process-based equations with neural networks (NNs), differentiable models train these components together, enabling parameter inputs for the equations to be effectively and efficiently learned at scale by the NNs. There are many possibilities for how such models are built.

In 𝛿MG, we define a differentiable model with the class *DeltaModel* that can couple one or more NNs with a process-based model (itself potentially a collection of models). This class holds `nn` and a `phy_model` objects, respectively, as attributes internally and describes how they interface with each other:

- **nn**: PyTorch neural networks that can learn and provide either parameters, missing process representations, corrections, or other forms of enhancements to physical models.
- **phy_model**: The physical model written in PyTorch (or potentially another interoperable differentiable platform) that takes learnable outputs from the `nn` model(s) and returns a prediction of some target variable(s). This can also be a wrapper holding several physical models.

The *DeltaModel* object can be trained and forwarded just as any other PyTorch model (nn.Module).

  [dmg_code]: https://github.com/mhpi/generic_deltaModel
