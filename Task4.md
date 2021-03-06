# Task 4 建模调参

## 学习目标

- 了解常用的机器学习模型
- 掌握机器学习的建模与调参流程

## 线性回归

### 定义

本质上它是一系列特征的线性组合，在二维空间中，你可以把它视作一条直线，在三维空间中可以视作是一个平面。

### 普遍形式

$f(x)=w^{\prime} x+b$

### 损失函数

为了获得w和b我们需要制定一定的策略，而这个策略在机器学习的领域中，往往描述为真实值与回归值的偏差。

loss$=(f(x)-y)^{2}$

### 优化方法

#### 最小二乘

如果我们想要让loss取到最小，只需要对这个式子进行求导，导数为0的地方就是极值点，也就是使loss最大或者最小的点。

$$\frac{d\left(\left(w^{\prime} X+b\right)-y\right)^{2}}{d w}=0$$

#### 梯度下降

梯度下降的策略与最小二乘优化不同，它采用的不是用数学方法一步求出解析解，而是一步一步的往让loss变到最小值的方向走，直到走到那个点。

梯度方向就是增长最快的方向，如果我们想要函数值减小，只需要沿着负梯度方向走就行了。

$g r a d=2\left(w^{\prime} X-y\right) X^{T}$

$w \leftarrow w+0.1 *$grad

## 模型验证

### 五折交叉验证

> 在使用训练集对参数进行训练的时候，经常会发现人们通常会将一整个训练集分为三个部分（比如mnist手写训练集）。一般分为：训练集（train_set），评估集（valid_set），测试集（test_set）这三个部分。这其实是为了保证训练效果而特意设置的。其中测试集很好理解，其实就是完全不参与训练的数据，仅仅用来观测测试效果的数据。而训练集和评估集则牵涉到下面的知识了。

> 因为在实际的训练中，训练的结果对于训练集的拟合程度通常还是挺好的（初始条件敏感），但是对于训练集之外的数据的拟合程度通常就不那么令人满意了。因此我们通常并不会把所有的数据集都拿来训练，而是分出一部分来（这一部分不参加训练）对训练集生成的参数进行测试，相对客观的判断这些参数对训练集之外的数据的符合程度。这种思想就称为交叉验证（Cross Validation）

## 模型调参

1. 贪心策略

> 所谓贪心算法是指，在对问题求解时，总是**做出在当前看来是最好的选择**。也就是说，不从整体最优上加以考虑，它所做出的仅仅是在某种意义上的**局部最优解**。
>  贪心算法没有固定的算法框架，算法设计的关键是贪心策略的选择。必须注意的是，贪心算法不是对所有问题都能得到整体最优解，选择的贪心策略必须具备无后效性（即某个状态以后的过程不会影响以前的状态，只与当前状态有关。）
>  **所以，对所采用的贪心策略一定要仔细分析其是否满足无后效性。**

**贪心策略适用的前提是：局部最优策略能导致产生全局最优解。**

2. 网格搜索 调参数

GridSearchCV：一种调参的方法，当你算法模型效果不是很好时，可以通过该方法来调整参数，通过循环遍历，尝试每一种参数组合，返回最好的得分值的参数组合
比如支持向量机中的参数 C 和 gamma ，当我们不知道哪个参数效果更好时，可以通过该方法来选择参数，我们把C 和gamma 的选择范围定位[0.001,0.01,0.1,1,10,100]
每个参数都能组合在一起，循环过程就像是在网格中遍历，所以叫网格搜索

需要较大的算力

3. 自动贝叶斯调参

贝叶斯优化通过基于目标函数的过去评估结果建立替代函数（概率模型），来找到最小化目标函数的值。贝叶斯方法与随机或网格搜索的不同之处在于，它在尝试下一组超参数时，会参考之前的评估结果，因此可以省去很多无用功。

超参数的评估代价很大，因为它要求使用待评估的超参数训练一遍模型，而许多深度学习模型动则几个小时几天才能完成训练，并评估模型，因此耗费巨大。贝叶斯调参发使用不断更新的概率模型，通过推断过去的结果来“集中”有希望的超参数。