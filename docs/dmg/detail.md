# 𝛿MG

---

A generic framework for building [differentiable models](https://www.nature.com/articles/s43017-023-00450-9). 𝛿MG enables seamless coupling of neural networks with differentiable process-based equations, leveraging PyTorch's auto-differentiation for efficient, large-scale optimization on GPU. The spiritual successor to [HydroDL](https://github.com/zhonghu17/HydroDL), 𝛿MG generalizes differentiable modeling for cross-domain application while also imposing basic standardizations for research-to-operations pipelines.

### Key Features

- 🤝 **Hybrid Modeling**: Combine neural networks with process-based equations for enhanced interpretability and generalizability. Instead of manual model parameter calibration, for instance, use neural networks to directly learn robust and interpretable parameters ([Tsai et al., 2021](https://doi.org/10.1038/s41467-021-26107-z)).
- 🔁 **PyTorch Integration**: Scale with PyTorch for efficient training and compatibility with modern ML tools and numerical solvers.
- 🧩 **Modular Plugin Architecture**: Swap in domain-specific components and configurations with ease.
- ⚡ **Benchmarking**: All in one place. 𝛿MG + hydroDL2 will enable rapid deployment and replication of key published MHPI results.
- 🌊 **NextGen-ready**: Designed for [CSDMS BMI](https://csdms.colorado.edu/wiki/BMI) compliance to support differentiable hydrological models in [NOAA-OWP](https://water.noaa.gov/about/owp)'s [NextGen National Water Modeling Framework](https://github.com/NOAA-OWP/ngen). (See the NextGen-ready [𝛿HBV2.0](https://github.com/mhpi/dHBV2.0) for an example with a 𝛿MG-supported BMI).

</br>

𝛿MG is designed to scale with modern deep learning tools (e.g., foundation models) while maintaining physical interpretability. Our peer-reviewed and published [benchmarks](https://mhpi.github.io/benchmarks/#10-year-training-comparison) show that well-tuned differentiable models can match deep networks in performance—while better extrapolating to extreme or data-scarce conditions and predicting physically meaningful variables.

Differentiable modeling introduces more modeling choices than traditional deep learning due to its physical constraints. This includes learning parameters, missing process representations, corrections, or other enhancements for physical models.

**Note**: While differentiable models are powerful and have many desirable characteristics, they come with a larger decision space than purely data-driven neural networks since physical processes are involved, and can thus feel "trickier" to work with. Hence, *we recommend* beginning with our example [notebooks](./example/hydrology/) and then systematically making changes, one at a time. Pay attention to multifaceted outputs, diverse causal analyses, and predictions of untrained variables permitted by differentiable models, rather than purely trying to outperform other models' metrics.

</br>

This work is mantained by [MHPI](http://water.engr.psu.edu/shen/) and advised by [Dr. Chaopeng Shen](https://water.engr.psu.edu/shen/). If you find it useful, please cite (dedicated citations are coming):

    Shen, C., et al. (2023). Differentiable modelling to unify machine learning and physical models for geosciences. Nature Reviews Earth & Environment, 4(8), 552–567. <https://doi.org/10.1038/s43017-023-00450-9>.

</br>

## Installation

To install 𝛿MG, clone the repo and install in developer mode with [Astral UV](https://docs.astral.sh/uv/):

```bash
git clone git@github.com:mhpi/generic_deltamodel.git
uv pip install -e ./generic_deltamodel
```

Pip and Conda are also supported, though UV is recommended. See [setup](./docs/setup.md) for further details.

</br>

## Quick Start

See [how to run](./docs/how_to_run.md).

**Example -- Differentiable Parameter Learning**: Use an LSTM to learn parameters for the [HBV](https://en.wikipedia.org/wiki/HBV_hydrology_model) hydrological model.

```python
from hydroDL2.models.hbv.hbv import HBV

from dMG.core.data.loaders import HydroLoader
from dMG.core.utils import load_nn_model, print_config, set_randomseed
from dMG.models.delta_models import DplModel
from example import load_config, take_data_sample

CONFIG_PATH = '../example/conf/config_dhbv_1_0.yaml'


# Model configuration
config = load_config(CONFIG_PATH)

# Initialize physical model and NN.
phy_model = HBV(config['delta_model']['phy_model'])
nn = load_nn_model(phy_model, config['delta_model'])

# Create differentiable model dHBV: a torch.nn.Module that describes how 
# the NN is linked to the physical model HBV.
dpl_model = DplModel(phy_model=phy_model, nn_model=nn)

# Load dataset of NN and HBV inputs.
dataset = HydroLoader(config).dataset
dataset_sample = take_data_sample(config, dataset, days=730, basins=100)

output = dpl_model(dataset_sample)
```

This exposes a key characteristic of the differentiable model `DplModel`: composition of a physical model, `phy_model`, and a neural network, `nn`. Internally, `DplModel` looks like

```python
# NN forward
parameters = self.nn_model(dataset_sample['xc_nn_norm'])        

# Physics model forward
predictions = self.phy_model(
    dataset_sample,
    parameters,
)
```

Check out [examples](https://github.com/mhpi/generic_deltamodel/tree/master/example/hydrology) to see model training/testing/simulation in detail. We recommend starting [here](./example/hydrology/example_dhbv_1_0.ipynb), which is a continuation of the above. A [Colab Notebook](https://colab.research.google.com/drive/19PRLrI-L7cGeYzkk2tOetULzQK8s_W7v?usp=sharing) for this δHBV example is also available.

</br>

## Use Cases

### 1. Lumped Hydrology

Lumped differentiable rainfall-runoff models [𝛿HBV 1.0](https://agupubs.onlinelibrary.wiley.com/doi/epdf/10.1029/2022WR032404) and improved [𝛿HBV 1.1p](https://essopenarchive.org/doi/full/10.22541/essoar.172304428.82707157).

### 2. Unseen Extreme Events Test with 𝛿HBV 1.1p

In the unseen extreme events spatial test, we used water years with a 5-year or lower return period peak flow from 1990/10/01 to 2014/09/30 for training, and held out the water years with greater than a 5-year return period peak flow for testing. The spatial test was conducted using a 5-fold cross-validation approach for basins in the [CAMELS dataset](https://gdex.ucar.edu/dataset/camels.html). This application has been benchmarked against LSTM and demonstrates better extrapolation abilities. Find more details and results in [Song, Sawadekar, et al. (2024)](https://essopenarchive.org/doi/full/10.22541/essoar.172304428.82707157).

<!-- ![Alt text](./docs/images/extreme_temporal.png) -->

### 3. National-scale Water Modeling

A national-scale water modeling study on approximately 180,000 river reaches (with a median length of 7 km) across CONUS using the high-resolution, multiscale, differentiable water model 𝛿HBV 2.0. This model is also operating at global-scales and has been used to generate high-quality, seamless simulations for the entire CONUS. Find more details and results in [Song, Bindas, et al. (2025)](https://agupubs.onlinelibrary.wiley.com/doi/pdf/10.1029/2024WR038928).

<!-- ![Alt text](./docs/images/CONUS_dataset.jpg) -->

### 4. Global-scale Photosynthesis Modeling

Currently in development. Find more details and results in [Aboelyazeed et al. (2024)](https://doi.org/10.22541/au.173101418.87755465/v1).

<!-- ![Alt text](./docs/images/Vcmax25_learnt_global_combined_2011_2020.png) -->

</br>

## Ecosystem Integration

- **HydroDL 2.0 ([`hydroDL2`](https://github.com/mhpi/hydroDL2))**: Home to MHPI's suite of process-based hydrology models and differentiable model augmentations.
<!-- - **HydroData ([`hydro_data_dev`](https://github.com/mhpi/hydro_data_dev))**: Data extraction, processing, and management tools optimized for geospatial datasets. (In development) -->
<!-- - **Config GUI ([`GUI-Config-builder`](https://mhpi-spatial.s3.us-east-2.amazonaws.com/mhpi-release/config_builder_gui/Config+Builder+GUI.zip))([Source](https://github.com/mhpi/GUI-Config-builder))**: An intuitive, user-friendly tool designed to simplify the creation and editing of configuration files for model setup and development. -->
- **Differentiable Ecosystem Modeling ([`diffEcosys (dev version only)`](https://github.com/hydroPKDN/diffEcosys/))**: A physics-informed machine learning system for ecosystem modeling, demonstrated using the photosynthesis process representation within the Functionally Assembled Terrestrial Ecosystem Simulator (FATES) model. This model is coupled to NNs that learn parameters from observations of photosynthesis rates.
- **Other Development**: Many additions are currently in the progress: (i) numerical PDE solvers on PyTorch, torchode, torchdiffeq; (ii) [adjoint](https://doi.org/10.5194/hess-28-3051-2024) sensitivity; (iii) extremely efficient and highly accurate surrogate models for process-based equations; (iv) data assimilation methods; (v) downscaled and bias-corrected climate data; (vi) mysteriously powerful neural networks, and more ...

</br>

## 𝛿MG Architecture

- **Data Loaders**: Bulk data preprocessors customized per dataset.
- **Data Samplers**: Dataset samplers for minibatching during model training and inference.
- **Trainers**: Orchestrates high-level model training, testing, and simulation.
- **ModelHandler**: Manages multimodeling, multi-GPU computation, and other high level operations. Acts as an drop-in model interface for CSDMS BMI or other R2O wrappers.
- **Delta Models**: Differentiable models; describes how NNs and physical models are coupled (e.g., `DplModel` for parameter learning).

</br>

## Repo

    ```text
    .
    ├── src/dMG/
    │   ├── __main__.py                 # Runs 𝛿MG; models, experiments
    │   ├── core/                       
    │   │   ├── calc/                   # Calculation utilities
    │   │   ├── data/                   # Data loaders and samplers
    │   │   ├── post/                   # Post-processing utilities; plotting
    │   │   └── utils/                  # Helper functions
    │   ├── models/                     
    │   │   ├── criterion               # Loss functions  
    │   │   ├── delta_models            # Differentiable model modalities
    │   │   ├── multimodels             # Multimodeling processors
    │   │   ├── neural_networks/        # Neural network architectures
    │   │   ├── phy_models/             # Physical Models
    │   │   └── model_handler.py        # High-level model manager
    │   └── trainers/                   # Model training routines
    ├── conf/
    │   ├── hydra/                      # Hydra settings
    │   ├── observations/               # Observation configuration files
    │   ├── config.py                   # Configuration validator
    │   └── default.yaml                # Default master configuration file
    ├── docs/                           
    ├── envs/                           # Python ENV configurations
    └── example/                        # Tutorials
    ```

</br>

## Contributing

We welcome contributions! Please submit changes via a fork and pull requests. For more details, refer to [docs/CONTRIBUTING.md](./docs/CONTRIBUTING.md).

---

*Please submit an [issue](https://github.com/mhpi/generic_deltamodel/issues) to report any questions, concerns, bugs, etc.*

<!-- 
# 𝛿MG

A domain-agnostic, PyTorch-based framework for developing trainable differentiable models that merge neural networks
with process-based equations. "Differentiable" means that gradient calculations can be achieved efficiently at large
scale throughout the model, so process-based equations can be trained together with NNs on big data, on GPU. 
Following as a generalization of `HydroDL`, 𝛿MG (`generic_deltaModel`) aims to expand differentiable modeling and
learning capabilities to a wide variety of domains where prior equations can bring in benefits.

<br>


## Ecosystem Integration
For differentiable hydrology models used in MHPI research, 𝛿MG seamlessly integrates with:

- **HydroDL2.0 ([`hydroDL2`](https://github.com/mhpi/hydroDL2))**: Home to MHPI's suite of physics-based hydrology models, and differentiable model augmentations (think variational data
      assimilation, model coupling, and additional physics-based hydrology tools).
- **HydroData ([`hydro_data_dev`](https://github.com/mhpi/hydro_data_dev))**: Data extraction, processing, and management tools optimized for geospatial datasets.
- **Config GUI ([`GUI-Config-builder`](https://mhpi-spatial.s3.us-east-2.amazonaws.com/mhpi-release/config_builder_gui/Config+Builder+GUI.zip))([Source](https://github.com/mhpi/GUI-Config-builder))**: An intuitive, user-friendly tool designed to simplify the creation and editing of configuration files for model setup and development.
- **Concurrent development activities**: We are working on these efforts connected to 𝛿MG: (i) numerical PDE solvers on torch; (ii) [adjoint](https://doi.org/10.5194/hess-28-3051-2024) sensitivity; (iii) extremely efficient and highly accurate surrogate models; (iv) downscaled and bias corrected climate data; (v) mysteriously powerful neural networks, and more...

<br>


## Key Features
- **Hybrid Modeling**: Combines neural networks with physical process equations for enhanced interpretability and generalizability. Skip manually tuning model parameters by using neural networks to feed robust and interpretable parameter predictions directly.

- **PyTorch Integration**: Easily scales with PyTorch, enabling efficient training and compatibility with modern deep learning tools, trained foundation models, differentiable numerical solvers.

- **Domain-agnostic and Flexible**: Extends differentiable modeling to any field where physics-guided learning can add value, with modularity to meet the diversity of needs along the way.

- **Benchmarking**: All in one place. 𝛿MG + hydroDL2 will enable rapid deployment and replication of key published MHPI results.

- **NextGen-ready**: 𝛿MG is designed to be [CSDMS BMI](https://csdms.colorado.edu/wiki/BMI)-compliant, and our differentiable hydrology models in hydroDL2 come with a prebuilt BMI allowing seamless compatibility with [NOAA-OWP](https://water.noaa.gov/about/owp)'s [NextGen National Water Modelling Framework](https://github.com/NOAA-OWP/ngen). Incidentally, this capability also lends to 𝛿MG being easily wrappable for other applications.

<br>


## Use Cases
This package powers the global- and  ([`national-scale water model`](https://doi.org/10.22541/essoar.172736277.74497104/v1)) that provide high-quality seamless hydrologic simulations over US and the world.
It also hosts ([`global-scale photosynthesis `](https://doi.org/10.22541/au.173101418.87755465/v1)) learning and simulations

<br>


## The Overall Idea
Characterized by the combination of process-based equations with neural networks (NNs), differentiable models train these components together, enabling parameter inputs for the equations to be effectively and efficiently learned at scale by the NNs. There are many possibilities for how such models are built.

In 𝛿MG, we define a differentiable model with the class *DeltaModel* that can couple one or more NNs with a process-based model (itself potentially a collection of models). This class holds `nn` and a `phy_model` objects, respectively, as attributes internally and describes how they interface with each other. The *DeltaModel* object can be trained and forwarded just as any other PyTorch model (nn.Module).

We also define *DataLoader* and *DataSampler* classes to handle datasets, a *Trainer* class for running train/test experiments, and a *ModelHandler* class for multimodel handling, multi-GPU training, data assimilation and streaming in a uniform and modular way. All model, training, and simulation settings are be collected in a configuration file that can be adapted to custom applications. 
According to this schema, we define these core classes, from bottom up:

- **nn**: PyTorch neural networks that can learn and provide either parameters, missing process representations, corrections, or other forms of enhancements to physical models.
- **phy_model**: The physical model written in PyTorch (or potentially another interoperable differentiable platform) that takes learnable outputs from the `nn` model(s) and returns a prediction of some target variable(s). This can also be a wrapper holding several physical models.
- **DeltaModel**: Holds (one or multiple) `nn` objects and a `phy_model` object, and describes how they are coupled; connection to ODE packages.
- **ModelHandler**: Manages multimodeling, multi-GPU compute, and data assimilation or streaming. Can contain its own optimizers. Acts as an interface to CSDMS BMI or other interfaces.
- **DataSampler**: Samples data according to data format and training/testing requirements.
- **Trainer**: Manages model training and testing, and connects data to models.
- **DataLoader**: Preprocesses data to be used in training, testing, and simulation.

<br>


## 𝛿MG Repository Structure:
    .
    ├── deltaModel/
    │   ├── __main__.py                 # Run the framework; model experiments
    │   ├── conf/                       # Configuration repository
    │   │   ├── config.py
    │   │   ├── config.yaml             # Main configuration file
    │   │   ├── hydra/                  
    │   │   └── observations/           # Data configuration files
    │   ├── core/                       
    │   │   ├── calc/                   # Calculation utilities
    │   │   ├── data/                   # Data Loaders and Samplers
    │   │   └── utils/                  # Helper functions
    │   ├── models/                     
    │   │   ├── differentiable_model.py # Differentiable model (dPL modality)
    │   │   ├── model_handler.py        # High-level model manager
    │   │   ├── loss_functions/         # Custom loss functions
    │   │   └── neural_networks/        # Neural network architectures
    │   └── trainers/                   # Training routines
    ├── docs/                           
    ├── envs/                           # Environment configuration files
    └── example/                        # Example and tutorial scripts

<br>


# Getting Started with *𝛿MG* + *HydroDL 2.0*
## System Requirements
𝛿MG uses PyTorch models requiring CUDA exclusively supported by NVIDIA GPUs. This requires using

- Windows or Linux

- NVIDIA GPU(s) (with CUDA version >12.0 recommended)

<br>


## Setup
### 1. Clone and Download Example Data
- Open a terminal on your system, navigate to the directory where 𝛿MG and HydroDL2 will be stored, and clone (`master` branch):
  
    ```shell
    git clone https://github.com/mhpi/generic_deltaModel.git
    git clone https://github.com/mhpi/hydroDL2.git
    ```

- Download the CAMELS data zip from the link above and extract, optionally to a `data/` folder in your working directory, which should now look something like

```
    .
    ├── data/
    │   ├── training_file           # Pickle file with training data
    │   ├── validation_file         # Pickle file with validation/testing data
    │   ├── gage_ids.npy            # Numpy array with all 671 CAMELS gage ids
    │   └── 531_subset.txt          # Text file of gage ids in 531-gage subset
    ├── generic_deltaModel/
    └── hydroDL2/ 

```

### 2. Install the ENV
- A minimal yaml list of essential packages is included in `generic_deltaModel`: `generic_deltaModel/envs/deltamodel_env.yaml`.
- To install, run the following (optionally, include `--prefix` flag to specify the env download location):

     ```shell
     conda env create --file /generic_deltaModel/envs/deltamodel_env.yaml
     ```
     or
  
     ```shell
     conda env create --prefix path/to/env --file /generic_deltaModel/envs/deltamodel_env.yaml
     ```

- Activate with `conda activate deltamodel` and open a Python instance to check that CUDA is available with PyTorch:

     ```python
     import torch
     print(torch.cuda.is_available())
     ```

- If CUDA is not available, often PyTorch is installed incorrectly. Uninstall PyTorch from the env and reinstall according to your system specifications [here](https://pytorch.org/get-started/locally/). E.g.,

     ```shell
     conda uninstall pytorch
     conda install pytorch torchvision torchaudio pytorch-cuda=12.1 -c pytorch -c nvidia
     ```

### 3. Install HydroDL2
- For `hydroDL2` to be accessible within `generic_deltaModel`, install with pip (optionally, include `-e` flag to install with developer mode):

     ```shell
     cd hydroDL2
     pip install .
     ```

     or
  
     ```shell
     cd hydroDL2
     pip install -e .
     ```

### 4. Build Models
- That's it. You should now be able to run the tutorials, train/test MHPI benchmarks, and build your own differentiable models.

<br>


## Using the Config GUI
### Setup

To use the HydroDL Config Builder from the GitHub source code, you have two options:

- Run Directly: Execute the tool using main.py.
- Build a Windows Executable: Use build.py to generate a standalone Windows executable.
  
Alternatively, you can skip the build process by downloading the precompiled executable [here](https://mhpi-spatial.s3.us-east-2.amazonaws.com/mhpi-release/config_builder_gui/Config+Builder+GUI.zip). Once downloaded, simply unzip and run the executable  `HydroDL Config Builder.exe` on Windows to start the builder and begin creating/editing your configuration files. 

Two files can potentiallly be created by this process. One contains model and experiment settings, while the other is a data config that specifies dataset specific information like data save paths.


### Where do Config Files go?

Once you have created and saved your YAML config files, they can go one of two places depending on your intentions. 

- Tutorials: `example/conf`, with data config in `example/conf/observations`.
- Development with 𝛿MG: `deltaModel/conf` with data config in `deltaModel/conf/observations`

Note. Before running 𝛿MG, ensure that 'observations' in the main config matches the name of the data config you want to use.

<br>


## Quick Start: Building a Differentiable HBV (𝛿HBV) Model
Here’s an example of how you can build a differentiable model, coupling a physical model with a neural network to intelligently learn parameters. In this instance, we use an
LSTM to learn parameters for the [HBV](https://en.wikipedia.org/wiki/HBV_hydrology_model) hydrology model.
```python
from example import load_config 
from hydroDL2.models.hbv.hbv import HBV as hbv
from deltaModel.models.neural_networks import init_nn_model
from deltaModel.models.differentiable_model import DeltaModel
from deltaModel.core.data.data_loaders.hydro_loader import HydroDataLoader
from deltaModel.core.data.data_samplers.hydro_sampler import take_sample


CONFIG_PATH = '../example/conf/config_dhbv1_1p.yaml'


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
dpl_model = DeltaModel(phy_model=phy_model, nn_model=nn)


## From here, forward or train dpl_model just as any torch.nn.Module model.

# 5. For example, to forward:
output = dpl_model.forward(dataset_sample)
```

In the above, we illustrate a critical behavior of the differentiable model object `DeltaModel`, which is the the composition of the physical model, `phy_model=hbv`, with a neural network, `nn`. 

When we forward DeltaModel, we feed scaled inputs (stored within the dataset dictionary) to the NN and forward, which returns a set of predicted parameters. These parameters then pass with the dataset dictionary to forward the phy_model and output final model predictions. Internally, these steps are represented within DeltaModel forward method as

```python
# Parameterization
parameters = self.nn_model(dataset_sample['xc_nn_norm'])        

# Physics model forward
predictions = self.phy_model(
    dataset_sample,
    parameters,
)
```

See [examples/](https://github.com/mhpi/generic_deltaModel/blob/master/example/differentiable_hydrology/dhbv_tutorial.ipynb) in the `generic_deltaModel` repository for this and other tutorials.

Note, the [Config GUI](https://mhpi-spatial.s3.us-east-2.amazonaws.com/mhpi-release/config_builder_gui/Config+Builder+GUI.zip) can be used to create/edit additional config files for use with these examples. ([Usage instructions](https://github.com/mhpi/GUI-Config-builder/blob/main/README.md))

<br>


Explore the [roadmap](https://github.com/orgs/mhpi/projects/4) for planned features and improvements. Differentiable numerical packages like torchode and torchdiffeq will be coming additions in the near future. -->
