# 機械学習まとめ

---

# PyTorch

```python
import numpy as np

import torch
from torch import nn, optim, utils
from torch.nn import functional as F

from torchvision import datasets
from torchvision.models import vgg11
import torchvision.transforms as transforms

np.zeros((2, 3))
torch.zeros((2, 3))
```

`zeros()`
`ones()`
`rand()` 0~1

`array()`, `tensor()`

`.shape`
`.dtype`

| default | int   | float   |
| ------- | ----- | ------- |
| numpy   | int32 | float64 |
| pytorch | int64 | float32 |

`t.numpy()`: ndarray
`torch.from_numpy(n)`: tensor

> これらはメモリを共有する。

`.transpose(0,1)`
`reshape(shape)`

# CUDA (Compute Unified Device Architecture)

NVIDIA が開発・提供している GPU 向け汎用並列コンピューティングプラットフォームおよびプログラミングモデル。

```python
if torch.cuda.is_available():
    gpu = torch.device("cuda")  # GPUデバイスオブジェクトの作成
    cpu = torch.device("cpu")  # CPUデバイスオブジェクトの作成
    data1 = torch.zeros((2, 2), device=gpu)  # GPU上に作成
    data_cpu = data1.to(cpu)  # CPUへ転送
    data_gpu = data_cpu.to(gpu)  # GPUへ転送
    print(data_cpu)
    print(data_gpu)
```

# NN (neural network)

```python
x = torch.Tensor([1, 2, 3, 4])
y = torch.Tensor([5, 6])

# fully_connected_layer
fc = nn.Linear(in_features=4, out_features=2)
u = fc(x)

# Relu
z = F.relu(u)

criterion = nn.MSELoss()
loss = criterion(z, y)

model = vgg11()
optimizer = optim.Adam(model.parameters())
```

```python
class MLP(nn.Module):
  def __init__(self):
    super.__init__()
    self.fc1 = nn.Linear(3, 5)
    self.fc2 = nn.Linear(5, 2)
    self.criterion = nn.MSELoss()
    self.optimizer = optim.Adam(self.parameters())

  def forward(self, x):
    print("input: ", x)

    x = self.fc1(x)
    print("fc1: ", x)

    x = F.relu(x)
    print("relu: ", x)

    x = self.fc2(x)
    print("fc2: ", x)

    return x

model = MLP()
x = torch.Tensor([1, 2, 3])
output = model(x)
print("output: ", output)
```

`fc`
`relu`
`conv2d`
