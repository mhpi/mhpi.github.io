# Examples

Several examples related to the above papers are presented here. **Click the title link** to see each example.

## [1.Train a LSTM data integration model to make streamflow forecast](https://github.com/mhpi/hydroDL/blob/release/example/StreamflowExample-DI.py)

The dataset used is NCAR CAMELS dataset. Download CAMELS following [this link](https://ral.ucar.edu/solutions/products/camels). 
Please download both forcing, observation data `CAMELS time series meteorology, observed flow, meta data (.zip)` and basin attributes `CAMELS Attributes (.zip)`. 
Put two unzipped folders under the same directory, like `your/path/to/Camels/basin_timeseries_v1p2_metForcing_obsFlow`, and `your/path/to/Camels/camels_attributes_v2.0`. Set the directory path `your/path/to/Camels`
as the variable `rootDatabase` inside the code later.

Computational benchmark: training of CAMELS data (w/ or w/o data integration) with 671 basins, 10 years, 300 epochs, in ~1 hour with GPU.
