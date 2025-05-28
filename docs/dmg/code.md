# Code Release

## Getting Started

To start using the 𝛿MG framework, clone `generic_deltaModel` from GitHub:

- [𝛿MG](https://github.com/mhpi/generic_deltamodel)

For [differentiable hydrology tutorials](https://github.com/mhpi/generic_deltamodel/tree/master/example/hydrology) and MHPI benchmarks, also clone the hydrologic model package `hydroDL2` and download our CAMELS data extraction:

- [HydroDL 2.0](https://github.com/mhpi/hydroDL2)
- [CAMELS Data](https://mhpi-spatial.s3.us-east-2.amazonaws.com/mhpi-release/camels/camels_data.zip)

## NextGen

Code supporting the high-resolution, national-scale differentiable water modeling product, 𝛿HBV2.0 (Song et al., 2025), in NOAA's NextGen water modeling framework can be found here:

- [𝛿HBV2.0](https://github.com/mhpi/dHBV2.0): Physical HBV2.0 model with BMI, configurations, and realization files supporting 𝛿HBV2.0 operation in NextGen.
- [NextGen](https://github.com/leoglonz/ngen): NextGen framework including the 𝛿HBV2.0 module above.
- [NGIAB](https://github.com/leoglonz/NGIAB-DM): NextGen In A Box using 𝛿HBV2.0 and the NextGen version above.

Instructions for setup in NextGen can be found in the [𝛿HBV2.0 repo](https://github.com/mhpi/dHBV2.0), and for instructions for NGIAB can be found in the repo or the official [DocuHub](https://docs.ciroh.org/docs/products/Community%20Hydrologic%20Modeling%20Framework/nextgeninaboxDocker/workflow/).

<!-- We include an optional GUI for constructing/editing 𝛿MG YAML configuration files with a user-friendly interface:

- [GUI Config builder (Zip)](https://mhpi-spatial.s3.us-east-2.amazonaws.com/mhpi-release/config_builder_gui/Config+Builder+GUI.zip)
- ([Source Code](https://github.com/mhpi/GUI-Config-builder)) -->
