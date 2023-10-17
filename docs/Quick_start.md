# Quick Start:
_The detailed code for quick start can be found in [tutorial_quick_start.py](./example/tutorial_quick_start.py)_

See below for a brief explanation of the major components you need to run a hydroDL model:

``` py linenums="1"
# imports
from hydroDL.model.crit import RmseLoss
from hydroDL.model.rnn import CudnnLstmModel as LSTM
from hydroDL.model.train import trainModel
from hydroDL.model.test import testModel

# load your training and testing data 
# x: forcing data (pixels, time, features)
# c: attribute data (pixels, features)
# y: observed values (pixels, time, 1)
x_train, c_train, y_train, x_val, c_val, y_val = load_data(...)

# define your model and loss function
model = LSTM(nx=num_variables, ny=1)
loss_fn = RmseLoss()

# train your model
model = trainModel(model,
    x_train,
    y_train,
    c_train,
    loss_fn,
)

# validate your model
pred = testModel(model,
             x_val,
             c_val,
)

```
