# PyTorch2

### 类型检测

isinstance(data)

### 生成张量

torch.tensor([])

### 元素总个数

a.numel()

### 生成随机矩阵

torch.rand([])

torch.randint(w, b, min, max)

torch.randn([])

在indx处增加维度unsqueeze(indx)

### 扩展维度

b.shape(1,32,1,1)

b.expand(4,32,14,14)

-1 表示维度不变

### 矩阵转置

a.t()

a.transpose(index1, index2).contiguous()

b.permute(index1, index2, ... )

### 比较a和b是否全部相同

torch.all(torch.eq(a,b))

### 矩阵乘法

torch.matmal(a, b)

### 次方运算

a.pow(n)

torch.exp()

torch.log()

### 裁剪

torch.floor()

torch.ceil()

torch.trunc()整数部分

torch.frac()小数部分

### 范数

a.norm(p, dim = )

### 索引

a.argmax(), b.argmin()

a.top(n, dim = m)

a.kthvalue(n, dim = m)

### 常用

from  torch.nn import functional as F

### 使用优化器

![image-20201020143613387](C:\Users\Jacky1\AppData\Roaming\Typora\typora-user-images\image-20201020143613387.png)

