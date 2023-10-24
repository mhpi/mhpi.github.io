# Neural Networks

Neural Networks are configured similar to how they are instantiated in other PyTorch packages. 

```py linenums="1"
from functools import partial

from omegaconf import DictConfig
import torch
import torch.nn as nn

from hydroRoute.neural_networks import Initialization

class NN(nn.Module):
    def __init__(self, cfg: DictConfig):
        super(MLP, self).__init__()
        self.cfg = cfg
        self.Initialization = Initialization(self.cfg)

    def initialize_weights(self) -> None:
        """
        The partial function used to 
        """
        func = self.Initialization.get()
        init_func = partial(self._initialize_weights, func=func)
        self.apply(init_func)

    @staticmethod
    def _initialize_weights(m, func) -> None:
        """
        An internal class used to intialize weights based
        on a provided initialization function
        """
        if isinstance(m, nn.Linear):
            func(m.weight)

    def forward(self, inputs: torch.Tensor) -> None:
        pass
```

