# HydroDL

[![PyPI](https://img.shields.io/pypi/v/hydroDL?color=blue)](https://pypi.org/project/hydroDL/)  [![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.3993880.svg)](https://doi.org/10.5281/zenodo.3993880) [![CodeStyle](https://img.shields.io/badge/code%20style-Black-black)]()

HydroDL is a coding library designed to model hydrologic systems using two classes of models: (1) purely data-driven neural networks and (2) hybrid, physics-informed differentiable ($\delta$) models. Applications range from soil moisture to streamflow to water quality and ecosystem. Through this portal, we collect all of our published models so you can navigate through them easily.

These classes each has its own advantages. Purely data-driven models are great because they are:
(i) easy to train and run at scale.  
(ii) automatically adaptable to predictable errors in forcings  
(iii) highly efficient in computation.  
(iv) not heavily prone to human bias in model selection.  

Differentiable models mix process-based equations (called priors) and neural networks (NNs) at the fundamental level so they can be trained together in one stage (called "end-to-end"). This way, the network components can be supervised indirectly by outputs of the combined system, and do not necessarily need supervising data for its direct outputs. Differentiability can be supported by automatic differentiation, adjoint (Song et al., 2023, https://hess.copernicus.org/preprints/hess-2023-258/), or any other method that can produce the gradients of loss with respect to large amounts of parameters efficiently. Such models can train one neural network using big data, improving the generalizability, robustness, and complexity of the learned relationships.   

While being able to match purely data-driven models in performance in even data-dense regions, differentiable models have the following advantages:  
(1) Interpretable: they represent the full physical processes and output intermediate physical variables not used in training, e.g., ET, recharge, snow water equivalent, which help to provide an explainable narrative.  
(2) Generalizability: because of the use of priors, differentiable models often generalize better under data-sparse regions like in global hydrologic simulations. (Feng et al., 2023a, https://doi.org/10.5194/hess-27-2357-2023; 2023b, https://gmd.copernicus.org/preprints/gmd-2023-190/)  
(3) (Under investigation) Extremes: forced by physical priors, the differentiable models may be better at representing unseen extreme scenarios and future trends.  
(4) Knowledge Discovery: we can use NNs as question marks in the combined system to learn unknown/uncertain relationships from data. In this sense, the priors serve to constrain or narrow down the scope of learning so we can understand the learned relationships.  
(5) Respect physical laws and sensitivities: we can force the model to respect laws such as mass balance, energy-driven evapotranspiration, etc. so our model has the correct sensitivities.
