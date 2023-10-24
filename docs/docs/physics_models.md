# Physics Models

HydroDL's implemented physics models are all child classes of the [Pytorch](https://pytorch.org/) `nn.Module` class. By creating your physics model as an nn.Module, you can tap into PyTorch's neural network functionality and get a lot of bonuses. 

## Basics

Our physics models are structured as follows:

```py linenums="1"
from typing import Tuple

import torch
import torch.nn as nn

class PhysicsModel(nn.Module):
    def __init__(self, cfg: DictConfig) -> None:
        super(PhysicsModel, self).__init__()
        self.cfg = cfg

    def forward(self, inputs: Tuple[..., torch.Tensor]) -> torch.Tensor:
```

Where all that is required by a physics model is it's specified configuration file. Since there are different requirements for each physics model, it is necessary for you to read into the specific configurations required by each module. 


