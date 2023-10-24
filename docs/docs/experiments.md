# Experiments

HydroDL experiments are designed to seamlessly be both reusableand structured. All experiments are child classes of the base `Experiment` class:

```py linenums="1" title="__init__.Experiment.py"
from abc import ABC, abstractmethod
from typing import Dict, Type

import torch
import torch.nn

class Experiment(ABC):
    @abstractmethod
    def run(
        self,
        data_loader: torch.utils.data.DataLoader,
        neural_network: nn.Module,
        physics_models: Dict[str, Type[nn.Module]],
    ) -> None:
        """a method that runs your experiment"""
        pass
```

The arguments passed into the parameters of the run function are all either class references (`physics_models`) or full instantiated classes (`data_loader`, or `neural_network`)
