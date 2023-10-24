# Datasets

The Datasets used in hydroDL are individual `@dataclass` classes used to create a Pytorch `torch.utils.data.Dataloader`. The classe

`Data`

:    Inputs to the neural networks

`Observations`

:    Targets used when training

## Data

Data classes are implementations of the following `ABC` class:

``` py linenums="1" title="__init__.Data.py"
from abc import ABC, abstractmethod

from omegaconf import DictConfig
import torch

class Data(ABC):
     @abstractmethod
    def __init__(self, cfg: DictConfig, dates: Dates, normalize: Normalize):
        """A function to define what inputs are required by a Data object"""
        pass

    @abstractmethod
    def _read_attributes(self) -> None:
        """
        Abstract method for reading attributes related to the data.

        """
        pass

    @abstractmethod
    def _read_forcings(self) -> None:
        """
        Abstract method for reading attributes related to the data.
        :return: None
        """
        pass

    @abstractmethod
    def _read_data(self) -> None:
        """The method to read all data"""
        pass

    @abstractmethod
    def get_data(self) -> Hydrofabric:
        """
        Abstract method for retrieving data in the form of a hydrofabric

        """
        pass
```

