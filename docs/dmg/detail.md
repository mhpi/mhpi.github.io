# 𝛿MG

A domain-agnostic, PyTorch-based framework for developing trainable differentiable models that merge neural networks
with process-based equations. "Differentiable" means that gradient calculations can be achieved efficiently at large
scale throughout the model, so process-based equations can be trained together with NNs on big data, on GPU. 
Following as a generalization of `HydroDL`, 𝛿MG (`generic_deltaModel`) aims to expand differentiable modeling and
learning capabilities to a wide variety of domains where prior equations can bring in benefits.

<br>

### Ecosystem Integration
For differentiable hydrology models used in MHPI research, 𝛿MG seamlessly integrates with:

- **HydroDL2.0 ([`hydroDL2`](https://github.com/mhpi/hydroDL2))**: Home to MHPI's suite of physics-based hydrology models, and differentiable model augmentations (think variational data
      assimilation, model coupling, and additional physics-based hydrology tools).
- **HydroData ([`hydro_data_dev`](https://github.com/mhpi/hydro_data_dev))**: Data extraction, processing, and management tools optimized for geospatial datasets.
- **Config GUI ([`GUI-Config-builder`](https://mhpi-spatial.s3.us-east-2.amazonaws.com/mhpi-release/config_builder_gui/Config+Builder+GUI.zip))([Source](https://github.com/mhpi/GUI-Config-builder))**: An intuitive, user-friendly tool designed to simplify the creation and editing of configuration files for model setup and development.
- **Concurrent development activities**: We are working on these efforts connected to 𝛿MG: (i) numerical PDE solvers on torch; (ii) [adjoint](https://doi.org/10.5194/hess-28-3051-2024) sensitivity; (iii) extremely efficient and highly accurate surrogate models; (iv) downscaled and bias corrected climate data; (v) mysteriously powerful neural networks.

<br>

### Key Features
- **Hybrid Modeling**: Combines neural networks with physical process equations for enhanced interpretability and generalizability. Skip manually tuning model parameters by using neural networks to feed robust and interpretable parameter predictions directly.

- **PyTorch Integration**: Easily scales with PyTorch, enabling efficient training and compatibility with modern deep learning tools, trained foundation models, differentiable numerical solvers.

- **Domain-agnostic and Flexible**: Extends differentiable modeling to any field where physics-guided learning can add value, with modularity to meet the diversity of needs along the way.

- **Benchmarking**: All in one place. 𝛿MG + hydroDL2 will enable rapid deployment and replication of key published MHPI results.

- **NextGen-ready**: 𝛿MG is designed to be [CSDMS BMI](https://csdms.colorado.edu/wiki/BMI)-compliant, and our differentiable hydrology models in hydroDL2 come with a prebuilt BMI allowing seamless compatibility with [NOAA-OWP](https://water.noaa.gov/about/owp)'s [NextGen National Water Modelling Framework](https://github.com/NOAA-OWP/ngen). Incidentally, this capability also lends to 𝛿MG being easily wrappable for other applications.

<br>

### Use Cases
This package powers the global- and  ([`national-scale water model`](https://doi.org/10.22541/essoar.172736277.74497104/v1)) that provide high-quality seamless hydrologic simulations over US and the world.
It also hosts ([`global-scale photosynthesis `](https://doi.org/10.22541/au.173101418.87755465/v1)) learning and simulations

<br>

### The Overall Idea
Characterized by the combination of process-based equations with neural networks (NNs), differentiable models train these components together, enabling parameter inputs for the equations to be effectively and efficiently learned at scale by the NNs. There are many possibilities for how such models are built.

In 𝛿MG, we define a differentiable model with the class *DeltaModel* that can couple one or more NNs with a process-based model (itself potentially a collection of models). This class holds `nn` and a `phy_model` objects, respectively, as attributes internally and describes how they interface with each other. The *DeltaModel* object can be trained and forwarded just as any other PyTorch model (nn.Module). We also define *DataLoader* and *DataSampler* classes to handle datasets, a *Trainer* class for running train/test experiments, and a *ModelHandler* class for multimodel handling, multi-GPU training, data assimilation and streaming in a uniform and modular way. All model, training, and simulation settings are be collected in a configuration file that can be adapted to custom applications. 
According to this schema, we define these core classes, from bottom up:

- **nn**: PyTorch neural networks that can learn and provide either parameters, missing process representations, corrections, or other forms of enhancements to physical models.
- **phy_model**: The physical model written in PyTorch (or potentially another interoperable differentiable platform) that takes learnable outputs from the `nn` model(s) and returns a prediction of some target variable(s). This can also be a wrapper holding several physical models.
- **DeltaModel**: Holds (one or multiple) `nn` objects and a `phy_model` object, and describes how they are coupled; connection to ODE packages.
- **ModelHandler**: Manages multimodeling, multi-GPU compute, and data assimilation or streaming. Can contain its own optimizers. Acts as an interface to CSDMS BMI or other interfaces.
- **DataSampler:**: Samples data according to data format and training/testing requirements.
- **Trainer**: Manages model training and testing, and connects data to models.
- **DataLoader**: Preprocesses data to be used in training, testing, and simulation.

<br>

### 𝛿MG Repository Structure:

    .
    ├── deltaModel/
    │   ├── __main__.py                 # Main entry point
    │   ├── conf/                       # Configuration files
    │   │   ├── config.py
    │   │   ├── config.yaml             # Main configuration file
    │   │   ├── hydra/                  
    │   │   └── observations/           # Observation data config
    │   ├── core/                       
    │   │   ├── calc/                   # Calculation utilities
    │   │   ├── data/                   # Data processing
    │   │   └── utils/                  # Helper functions
    │   ├── models/                     
    │   │   ├── differentiable_model.py # Differentiable model definition DeltaModel
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
For a functioning 𝛿MG + HydroDL2 setup...

### 1. Clone and Download Data
- Open a terminal on your system, navigate to the directory where 𝛿MG and HydroDL2 will be stored, and clone (`master` branch):
  
    ```shell
    git clone https://github.com/mhpi/generic_deltaModel.git
    git clone https://github.com/mhpi/hydroDL2.git
    ```

- Download the CAMELS data zip from the link above and extract, optionally to a `Data/` folder in your working directory, which should now look something like

```
    .
    ├── Data/
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


### Where do the Config Files go?

Once you have created and saved your YAML config files, they can go one of two places depending on your intentions. 

- Tutorials: `example/conf`, with data config in `example/conf/observations`.
- Development with 𝛿MG: `deltaModel/conf` with data config in `deltaModel/conf/observations`

Note. Before running 𝛿MG, ensure that 'observations' in the main config matches the name of the data config you want to use.

<br>

## Quick Start: Building a Differentiable HBV (𝛿HBV) Model

Here’s an example of how you can build a differentiable model, coupling a physics-based model with a neural network to intelligently learn model parameters. In this instance, we use an
LSTM with the [HBV](https://en.wikipedia.org/wiki/HBV_hydrology_model) hydrology model. The [Config GUI](https://mhpi-spatial.s3.us-east-2.amazonaws.com/mhpi-release/config_builder_gui/Config+Builder+GUI.zip) can be used to create/edit additional config files for use with these examples. (See [here](https://github.com/mhpi/GUI-Config-builder/blob/main/README.md) for usage instructions.)
```python
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
dpl_model = dHBV(phy_model=phy_model, nn_model=nn)


## From here, forward or train dpl_model just as any torch.nn.Module model.

# 5. For example, to forward:
output = dpl_model.forward(dataset_sample)
```

See [here](https://github.com/mhpi/generic_deltaModel/blob/master/example/differentiable_hydrology/dhbv_tutorial.ipynb) in the `generic_deltaModel` repository for this and other tutorials.

<br>

Explore the [roadmap](https://github.com/orgs/mhpi/projects/4) for planned features and improvements. It is in our roadmap to interface with differentiable numerical packages like torchode and torchdiffeq.
