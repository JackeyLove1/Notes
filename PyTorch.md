# PyTorch

[TOC]



## 实用主义

## 线性回归

### 生成数据集

```python
import torch
import numpy as np
num_inputs = 2
num_examples = 1000
true_w = [2, -3.4]
true_b = 4.2
features = torch.tensor(np.random.normal(0, 1, (num_examples, num_inputs)), dtype=torch.float)
labels = true_w[0] * features[:, 0] + true_w[1] * features[:, 1] + true_b
labels += torch.tensor(np.random.normal(0, 0.01, size=labels.size()), dtype=torch.float)
```

### 读取数据

```python
import torch.utils.data as Data

batch_size = 10
# 将训练数据的特征和标签组合
dataset = Data.TensorDataset(features, labels)
# 随机读取小批量
data_iter = Data.DataLoader(dataset, batch_size, shuffle=True)
```

```python
for X, y in data_iter:
    print(X, y)
    break
```

### 定义模型

```python
from torch import nn
class LinearNet(nn.Module):
    def __init__(self, n_feature):
        super(LinearNet, self).__init__()
        self.linear = nn.Linear(n_feature, 1)
    # forward 定义前向传播
    def forward(self, x):
        y = self.linear(x)
        return y

net = LinearNet(num_inputs)
print(net) # 使用print可以打印出网络的结构
```

### 查看可学习参数

```python
for param in net.parameters():
    print(param)
```

### 初始化参数模型

```python
from torch.nn import init

init.normal_(net.linear.weight, mean=0, std=0.01)
init.constant_(net.linear.bias, val=0)  # 也可以直接修改bias的data: net[0].bias.data.fill_(0)
```

### 定义损失函数

```python
loss = nn.MSELoss()
```

### 定义优化算法

```python
import torch.optim as optim

optimizer = optim.SGD(net.parameters(), lr=0.03)
print(optimizer)
```

#### 设置不同学习率

```python
optimizer =optim.SGD([
                # 如果对某个参数不指定学习率，就使用最外层的默认学习率
                {'params': net.subnet1.parameters()}, # lr=0.03
                {'params': net.subnet2.parameters(), 'lr': 0.01}
            ], lr=0.03)
```

#### 调整学习率

```python
# 调整学习率
for param_group in optimizer.param_groups:
    param_group['lr'] *= 0.1 # 学习率为之前的0.1倍
```

### 训练模型

```python
num_epochs = 3
for epoch in range(1, num_epochs + 1):
    for X, y in data_iter:
        output = net(X)
        l = loss(output, y.view(-1, 1))
        optimizer.zero_grad() # 梯度清零，等价于net.zero_grad()
        l.backward()
        optimizer.step()
    print('epoch %d, loss: %f' % (epoch, l.item()))
```

### 查看权重

```python
dense = net[0]
print(true_w, dense.weight)
print(true_b, dense.bias)
```

## torchversion的组成部分

本节我们将使用torchvision包，它是服务于PyTorch深度学习框架的，主要用来构建计算机视觉模型。torchvision主要由以下几部分构成：

1. `torchvision.datasets`: 一些加载数据的函数及常用的数据集接口；
2. `torchvision.models`: 包含常用的模型结构（含预训练模型），例如AlexNet、VGG、ResNet等；
3. `torchvision.transforms`: 常用的图片变换，例如裁剪、旋转等；
4. `torchvision.utils`: 其他的一些有用的方法。

## Softmax

还有很多函数可以创建`Tensor`，去翻翻官方API就知道了，下表给了一些常用的作参考。

| 函数                              |                      功能 |
| --------------------------------- | ------------------------: |
| Tensor(*sizes)                    |              基础构造函数 |
| tensor(data,)                     |    类似np.array的构造函数 |
| ones(*sizes)                      |                 全1Tensor |
| zeros(*sizes)                     |                 全0Tensor |
| eye(*sizes)                       |        对角线为1，其他为0 |
| arange(s,e,step)                  |        从s到e，步长为step |
| linspace(s,e,steps)               | 从s到e，均匀切分成steps份 |
| rand/randn(*sizes)                |             均匀/标准分布 |
| normal(mean,std)/uniform(from,to) |         正态分布/均匀分布 |
| randperm(m)                       |                  随机排列 |

### torch.view可以改变张量的维度和大小，-1可以从其他维度推断

x = torch.randn(4, 4)
y = x.view(16)
z = x.view(-1, 8)  #  size -1 从其他维度推断
print(x.size(), y.size(), z.size())

### 对参数推断求导

```python
x = torch.ones(2, 2, requires_grad=True)

y = x + 2

z = y * y * 3

out = z.mean()

out.backward()
print("d(out)/dx ", x.grad)
```

#### how to denote string

* one hot
* Embedding (Word2Vec, glove ...)

