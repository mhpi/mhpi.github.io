# 𝛿MG

## Generic, Scalable Differentiable Modeling Framework on PyTorch

[![image](https://img.shields.io/github/license/saltstack/salt)](https://github.com/mhpi/generic_deltaModel/blob/master/LICENSE)
[![Python](https://img.shields.io/badge/python-3.9%20%7C%203.10%20%7C%203.11-blue)]()


- [𝛿MG](https://github.com/mhpi/generic_deltaModel)
- [HydroDL 2.0](https://github.com/mhpi/hydroDL2)
- [Example Data](https://onedrive.live.com/login)


A domain-agnostic, PyTorch-based framework for developing trainable differentiable models that merge neural networks with process-based equations. "Differentiable" means that gradient calculations can be achieved efficiently at large scale throughout the model, so process-based equations can be trained together with NNs on big data, on GPU. 
Following as a generalization of *HydroDL*, `generic_deltaModel` (or *DeltaModel*) aims
to expand differentiable modeling and learning capabilities to a wide variety of domains where prior equations can bring in benefits.

Closely synergizes with deep learning tools and the scale advantage of PyTorch. Maintained by ([`MHPI group`](https://mhpi.info/)) advised by Dr. Chaopeng Shen.

<br>

### Key Features
- **Hybrid Modeling**: Combines neural networks with physical process equations for enhanced interpretability and generalizability.

- **PyTorch Integration**: Easily scales with PyTorch, enabling efficient training and compatibility with modern deep learning tools, trained foundation models, differentiable numerical solvers.

- **Domain-agnostic Flexibility**: While originally designed for hydrology, DeltaModel extends to other fields where physics-guided learning can add value.

<br>

### Ecosystem Integration
For differentiable hydrology models used in MHPI research, DeltaModel seamlessly integrates with:

- **HydroDL2.0 ([`hydroDL2`](https://github.com/mhpi/hydroDL2))**: Home to MHPI's suite of physics-based hydrology models, and differentiable model augmentations (think variational data
      assimilation, model coupling, and additional physics-based hydrology tools).
- **HydroData ([`hydro_data_dev`](https://github.com/mhpi/hydro_data_dev))**: Data extraction, processing, and management tools optimized for hydrology applications. [*In development*]


Explore the project's [roadmap](https://github.com/orgs/mhpi/projects/4) for planned features and future improvements. It is in our roadmap to interface with differentiable numerical packages like torchode and torchdiffeq.


<br>

### The overall idea
We define a "differentiable model" (dModel) class which describes how neural networks and the process-based model are coupled. dModel holds NNs and process-based models as attributes and can be trained and forwarded just as any other PyTorch model (nn.Module). We define classes to handle datasets (dataset class), various train/test experiments (trainer), multimodel handling and multi-GPU training (model handler), data assimilation and streaming in a uniform and modular way. All training and simulations can be specified by a config file to be adapted to custom applications. 
According to the schema, we define these core classes, from bottom up:

- **NN**: Neural networks that can provide either parameters, missing process representations, corrections or other forms of enhancements to process-based models.
- **phy_model**: The physical model written in PyTorch (or potentially other interoperable differentiable platform).
- **dModel**: Holds (one or multiple) NNs and (one or multiple) phy_model and describe how they are coupled; connection to ODE packages.
- **model_handler**: Manages ensemble model, multi-GPU compute, and data assimilation or streaming. Can contain its own optimizers. Interface to BMI or other interfaces.
- **Trainer**: Manages the train and test of models and connects data to model.
- **dataset**: Manages data ingestion in a unified format; support multiple file formats.


<br>

### Quick Start: Building a Differentiable HBV ($\delta$ HBV) Model

Here’s an example of how you can build a differentiable model, coupling a physics-based model with a neural network to intelligently learn model parameters. In this instance, we use an
LSTM with the [HBV](https://en.wikipedia.org/wiki/HBV_hydrology_model) hydrology model.
```python
# 1. Load configuration dictionary of model parameters and options.
config = load_config(CONFIG_PATH)

# 2. Setup a dataset dictionary of NN and physics model inputs.
dataset = HydroDataLoader(config, test_split=True).eval_dataset
dataset_sample = take_sample(config, dataset, days=730, basins=100)

# 3. Initialize physical model and NN.
phy_model = hbv(config['dpl_model']['phy_model'])
nn = init_nn_model(phy_model, config['dpl_model'])

# 4. Create the differentiable model dHBV: a torch.nn.Module that describes how 
# the NN is linked to the physical model HBV.
dpl_model = dHBV(phy_model=phy_model, nn_model=nn)


## From here, forward or train dpl_model just as any torch.nn.Module model.

# 5. For example, to forward:
output = dpl_model.forward(dataset_sample)

```

### Use cases
This package powers the global- and  ([`national-scale water model`](https://doi.org/10.22541/essoar.172736277.74497104/v1)) that provide high-quality seamless hydrologic simulations over US and the world.
It also hosts ([`global-scale photosynthesis `](https://doi.org/10.22541/au.173101418.87755465/v1)) learning and simulations
