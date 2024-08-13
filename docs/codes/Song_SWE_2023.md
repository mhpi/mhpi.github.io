# LSTM for snow water equivalent

## Code Release

LSTM code for SWE is available at this link: https://github.com/mhpi/hydroDL/tree/release/example/snow_water_equivalent

## Short Summary
We tested an LSTM network with data integration (DI) for snow water equivalent (SWE) in the western US to integrate 30-day-lagged or 7-day-lagged observations of either SWE or satellite-observed snow cover fraction (SCF) to improve future predictions. SCF proved beneficial only for shallow-snow sites during snowmelt, while lagged SWE integration significantly improved prediction accuracy for both shallow-and deep-snow sites. The median Nash-Sutcliffe model efficiency coefficient (NSE) in temporal testing improved from 0.92 to 0.97 with 30-day-lagged SWE integration, and root-mean-square error (RMSE) and the difference between estimated and observed peak SWE values (dmax ) were reduced by 41% and 57%, respectively (https://journals.ametsoc.org/view/journals/hydr/25/1/JHM-D-22-0220.1.xml).

## Bibtex Citation


```bibtex
@article{song2024lstm,
  title={LSTM-based data integration to improve snow water equivalent prediction and diagnose error sources},
  author={Song, Yalan and Tsai, Wen-Ping and Gluck, Jonah and Rhoades, Alan and Zarzycki, Colin and McCrary, Rachel and Lawson, Kathryn and Shen, Chaopeng},
  journal={Journal of Hydrometeorology},
  volume={25},
  number={1},
  pages={223--237},
  year={2024},
  publisher={American Meteorological Society}
}
```
