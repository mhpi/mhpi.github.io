# Code Release

To start using 𝛿MG, clone the framework `generic_deltaModel`:

- [𝛿MG](https://github.com/mhpi/generic_deltaModel)

For [tutorials](https://github.com/mhpi/generic_deltaModel/tree/master/example) and MHPI benchmarks, additionally clone the hydrologic model package `hydroDL2` and download our CAMELS data extraction:

- [HydroDL 2.0](https://github.com/mhpi/hydroDL2)
- [CAMELS Date ](https://mhpi-spatial.s3.us-east-2.amazonaws.com/mhpi-release/camels/camels_data.zip)


<br>


# Getting Started with *HydroDL 2.0*

## System Requirements

𝛿MG uses PyTorch models requiring CUDA exclusively supported by NVIDIA GPUs. This requires using

- Windows or Linux

- NVIDIA GPU(s) (with CUDA version >12.0 recommended)



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

---

### 4. Build Models

- That's it. You should now be able to run the tutorials, train/test MHPI benchmarks, and build your own differentiable models.
