# Data Science

### 忽略警告

```
import warnings
warnings.filterwarnings('ignore')
```

### 从CSV中读取数据

```python
import pandas as pd
# 加r防止路径转义
train_data = pd.read_csv(
    r'C:\Users\Jacky1\Desktop\GoPro\house-prices-advanced-regression-techniques\train.csv')
test_data = pd.read_csv(
    r'C:\Users\Jacky1\Desktop\GoPro\house-prices-advanced-regression-techniques\test.csv')
```

### 查看统计

```
# 查看列
train_data.columns
# 统计描述
train_data['SalePrice'].describe()
# 绘制直方图
sns.distplot(train_data['SalePrice'])
```

### 处理缺失数据

* 删除

* 数据填补

* 不处理

  #### 删除

1.**统计每列缺失值的个数** ```df.isnull().sum()```

2.**删除含有缺失值的列**  ``` df.dropna()```

3.**直接删除含有缺失值的列 **  ```df.dropna(axis=1)```

#### 	填补

对缺失值的插补大体可分为3种：

- 替换缺失值。替换是通过数据中非缺失数据的相似性来填补，其核心思想是发现相同群体的共同特征。
- 拟合缺失值。拟合是通过其他特征建模来填补
- 虚拟变量。虚拟变量是衍生的新变量代替缺失值。

1. **全局填充** ```df.fillna(0)```
2. **对每一列填充不同的值 ** ```df.fillna({'C':1, 'D':2})```
3. **均值填充**

```python
# 定义一个函数
# 对每一列均填充相应列的 均值、中位数、众数
def FillNan(df, col, opt):
    if opt == 1:
        # 均值填充
        df[col] = df[col].fillna(df[col].mean())
    elif opt == 2:
        # 中位数填充
        df[col] = df[col].fillna(df[col].median())
    else:
        # 众数填充
        df[col] = df[col].fillna(df[col].mode()[0])
    return df

```

#### 热卡填补

热卡填充法是在完整数据中找到一个与它最相似的对象，然后用这个相似对象的值来进行填充。通常会找到超出一个的相似对象，在所有匹配对象中没有最好的，而是从中随机的挑选一个作为填充值。这个问题关键是不同的问题可能会选用不同的标准来对相似进行判定，以及如何制定这个判定标准。该方法概念上很简单，且利用了数据间的关系来进行空值估计，但缺点在于难以定义相似标准，主观因素较多。

#### K最近距离邻法（K-means clustering）

另外一种方法就是利用无监督机器学习的聚类方法。通过K均值的聚类方法将所有样本进行聚类划分，然后再通过划分的种类的均值对各自类中的缺失值进行填补。归其本质还是通过找相似来填补缺失值。

注：缺失值填补的准确性就要看聚类结果的好坏了，而聚类结果的可变性很大，通常与初始选择点有关，并且在下图中可看到单独的每一类中特征值也有很大的差别，因此使用时要慎重。

#### 拟合缺失值

- 回归预测
- 极大似然估计
- 多重插补
- 随机森林

```python
from sklearn.ensemble import RandomForestRegressor
# 试验随机森林的插值方法，泰坦尼克数据集
def set_missing_values(df, col):
    age_df = df[['Age','Fare', 'Parch', 'SibSp', 'Pclass']]
    # 乘客分成已知年龄和未知年龄两部分 针对年龄这一列进行缺失统计
    known_age = age_df[age_df[col].notnull()].values
    unknown_age = age_df[age_df[col].isnull()].values
    # y即目标年龄 完整数据的年龄
    y = known_age[:, 0]
    # X即为完整数据的除了年龄的所有指标
    X = known_age[:, 1:]
    # 建立模型
    rf_clf = RandomForestRegressor(random_state=23, n_estimators = 500, n_jobs=-1)
    # 拟合数据
    rf_clf.fit(X, y)
    # 预测 用年龄缺失部分的非年龄变量作为X
    rf_clf_pre = rf_clf.predict(unknown_age[:,1:])
    # 用上述预测值填补原始数据的相应部分
    df.loc[(df[col].isnull()), col] = rf_clf_pre
    return df    

df2 = set_missing_values(titanic_data, 'Age')
df2.isnull().sum()
```

