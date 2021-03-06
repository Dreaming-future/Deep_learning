# PRML - Chapter 01: Introduction

对于第一章来说，都是一些简单的介绍，是一些机器学习的基础知识，如：训练集、测试集、泛化、有监督学习、无监督学习、特征抽取等基本概念。并且，这章会介绍三个重要工具：概率论、决策论、信息论。虽然这些东西听起来让⼈感觉害怕，但是实际上它们非常直观。并且，在实际应⽤中，如果想让机器学习技术发挥最⼤作⽤的话，清楚地理解它们是必须的。

- [PRML - Chapter 01: Introduction](#PRML--Chapter-01-Introduction)
	- [基本知识点](#基本知识点)
	- [1.1. 例子 : 多项式曲线拟合](#11-例子--多项式曲线拟合)
	- [1.2. 概率论](#12-概率论)
		- [1.2.1. 概率密度](#121-概率密度)
			- [离散随机变量](#离散随机变量)
			- [连续随机变量](#连续随机变量)
		- [1.2.2. 期望 与 协方差](#122-期望-与-协方差)
		- [1.2.3. 经典概率论 与 贝叶斯概率论](#123-经典概率论-与-贝叶斯概率论)
		- [1.2.4. 高斯分布](#124-高斯分布)
		- [1.2.5. 概率建模 : 多项式曲线拟合](#125-概率建模--多项式曲线拟合)
		- [1.2.6. 贝叶斯曲线拟合 ( Sec 3.3. )](#126-贝叶斯曲线拟合--Sec-33-)
	- [1.3. 模型选择](#13-模型选择)
	- [1.4. 维度灾难](#14-维度灾难)
	- [1.5. 决策论 ( 分类问题、模式识别 )](#15-决策论--分类问题、模式识别-)
		- [1.5.1. 最小化错误分类率](#151-最小化错误分类率)
		- [1.5.2. 最小化期望损失 ( 加先验 )](#152-最小化期望损失--加先验-)
		- [1.5.3. 拒绝选项](#153-拒绝选项)
		- [1.5.4. 推断与决策](#154-推断与决策)
		- [1.5.5. 回归问题的损失函数](#155-回归问题的损失函数)
	- [1.6. 信息论](#16-信息论)
		- [1.6.1. 相对熵 和 互信息](#161-相对熵-和-互信息)
	- [小结](#小结)

## 基本知识点

-   训练集 ( training set ) : 用来通过训练来调节模型的参数。
    -   输入变量 $\text{x}$ 的 $N$ 次观测组成，记作 $\text{X}\equiv\{\text{x}_1,\cdots,\text{x}_N\}$
    -   目标变量 $t$ 的 $N$ 次观测组成，记作 $\mathbf{t}\equiv\{t_1,\cdots,t_N\}$（通常是被独立考察，人工标注的）
-   学习的结果 : 表示为一个函数 $y ( x )$，它以新的 $x$ 为输入，产生的 $y$ 为输出，结果与 $t$ 的形式相同。
    -   $y$ 的具体形式 ( 参数 ) 是在训练 ( training ) 阶段被确定的，也被称为学习 ( learning ) 阶段。
    -   当训练阶段完成后，可以使用新的数据集去检验训练的结果，这种数据集称为测试集 ( test set )。
    -   泛化 ( generalization ) : 正确分类与训练集不同的新样本的能力。
-   原始输入向量需要被预处理 ( pre-processed )，变换到新的变量空间，也称为特征抽取 ( feature extraction )，使问题变得更加容易解决，加快计算速度。
-   有监督学习 ( supervised learning )
    -   离散输出学习称为分类 ( classification ) 问题
    -   连续输出学习称为回归 ( regression ) 问题
-   无监督学习 ( unsupervised learning )
    -   离散输出学习称为聚类 ( clustering ) 问题
    -   连续输出学习称为密度估计 ( density estimation )
        -   高维空间投影到二维或者三维空间，为了数据可视化 ( visualization ) 或者降维
-   反馈学习 ( 强化学习 ) ( reinforcement learning ) : 在给定的条件下，找到合适的动作，使得奖励达到最⼤值。

## 1.1. 例子 : 多项式曲线拟合

理论基础

-   概率论提供了数学框架，用来描述不确定性
-   决策论提供了合适的标准，用来进行最优的预测。

前提条件

-   训练集 : 输入数据 : 由 $x$ 的 $N$ 次观察组成 $\mathbf{x}\equiv ( x_1,\cdots,x_N )^T$
-   训练集 : 目标数据 : 由 $t$ 的 $N$ 次观察组成 $\mathbf{t}\equiv ( t_1,\cdots,t_N )^T$

多项式函数是线性模型，应用于 线性回归 ( Ch 03 ) 和 线性分类 ( Ch 04 )

$$
y ( x,\text{w} ) = w_0 + w_1 x + w_2 x^2 + \cdots + w_M x^M = \sum_{j=0}^M w_j x^j
$$

最小化误差函数 ( error function ) 可以调整多项式函数的参数

-   平方误差函数 ( square error function ) : 最常用

$$
E ( \text{w} ) =\frac12\sum_{n=1}^N [y ( x_n,\text{w} ) - t_n]^2
$$

-   根均方 ( root-mean-square, RMS ) 误差函数 : 更方便

$$
E_{RMS}=\sqrt{2E ( \text{w}^* ) /N}
$$

多项式的阶数 $M$ 的选择，属于 模型对比 ( model comparison ) 问题 或者 模型选择 ( model selection ) 问题。

拟合问题 : 模型容量 与 实际问题 不匹配

-   欠拟合 ( Under-fitting ) : 模型过于简单，模型容量低，不能充分描述问题
-   过拟合 ( Over-fitting ) : 模型过于复杂，模型容量高，可能描述数据噪声

正则化 ( regularization ) : 解决过拟合问题，即给误差函数增加惩罚项，使得系数不会达到很大的值

-   正则项的 $\lambda$ 系数控制过拟合的影响
-   统计学 : 叫做收缩 ( shrinkage ) 方法
-   二次正则项 : 称为岭回归 ( ridge regression )
-   神经网络 : 称为权值衰减 ( weight decay )

确定模型复杂度 : 验证集 ( validation set )，也被称为拿出集 ( hold-out set )，缺点是不能充分利用数据，太浪费有价值的训练数据了。

数据集规模 : 训练数据的数量应该是模型可调节参数的数量的 `5~10` 倍。

最大似然 ( maximum likelihood, ML )

-   最小二乘法 是 最大似然法 的特例
-   过拟合问题 是 ML 的一种通用属性
-   使用 Bayesian 方法解决过拟合问题，等价于正则化

## 1.2. 概率论

理解 离散随机变量 与 连续随机变量 之间的关系

离散随机变量

-   联合概率 : $X$ 取值 $x_i$，$Y$ 取值 $y_j$，的联合概率是

$$
p ( X=x_i,Y=y_j ) =\frac{n_{ij}}{N}
$$

-   边缘概率 : $X$ 取值 $x_i$( 与 $Y$ 取值无关 ) 的边缘概率是

$$
p ( X=x_i ) = \frac{c_i}N
$$

-   加和规则推导

$$
p ( X=x_i ) = \sum_{j=1}^L p ( X=x_i,Y=y_j )
$$

-   条件概率 : 在$X$ 取值 $x_i$ 中 $Y$ 取值 $y_j$ 的条件概率是

$$
p ( Y=y_j|X=x_i ) =\frac{n_{ij}}{c_i}
$$

-   乘积规则推导

$$
p ( X=x_i,Y=y_j ) =\frac{n_{ij}}N=\frac{n_{ij}}{c_i}\cdot\frac{c_i}N = p ( Y=y_j|X=x_i ) p ( X=x_i )
$$

-   贝叶斯定理

$$
p ( Y|X ) =\frac{p ( X|Y ) p ( Y )}{p ( X )}=\frac{p ( X|Y ) p ( Y )}{\sum_Y p ( X|Y ) p ( Y )}
$$

-   $X$ 和 $Y$ 相互独立

$$
p ( X,Y ) =p ( X ) p ( Y )
$$

**概率论的两种基本规则** ( 后面会有大量的应用，需要充分理解才能正确的使用 )

-   加和规则 ( sum rule ) : $p ( X ) =\sum_Y p ( X,Y )$
-   乘积规则 ( product rule ) : $p ( X,Y ) =p ( Y|X ) p ( X )$

这⾥$p(X,Y)$是联合概率，可以表述为“X且Y 的概率”。类似地，$p(Y | X)$是条件概率，可以表述为“给定X的条件下Y 的概率”，p(X)是边缘概率，可以简单地表述为"X的概率”。这两个简单的规则组成了我们在全书中使用的全部概率推导的基础。

### 1.2.1. 概率密度

#### 离散随机变量

概率质量函数 ( probability mass function )
$$
p ( X=x_i ) = \sum_j n_{ij} /N
$$
条件概率 ( conditional probability )
$$
p ( Y=y_j|X=x_i ) = n_{ij}/ \sum_j n_{ij}
$$

-   利用乘积规则得到联合概率 ( conditional probability )

$$
p ( X=x_i,Y=y_j ) =p ( Y=y_j|X=x_i ) p ( X=x_i )
$$

联合概率 ( joint probability )
$$
p ( X=x_i,Y=y_j ) =n_{ij}/N
$$

-   利用加和规则得到边缘概率 ( marginal probability )

$$
p ( X=x_i ) =\sum_{j=1}^L p ( X=x_i,Y=y_j )
$$

#### 连续随机变量

概率密度函数 ( probability density function ) : $p ( x\in ( a,b )) =\int_a^b p ( x ) dx$

累积分布函数 ( cumulative distribution function ) : $p ( z ) = \int_{-\infty}^z p ( x ) dx$



由于概率是非负的，并且x的值一定位于实数轴上的某个位置，因此概率密度一定满足下面两个条件

- $p(X) \ge 0$
- $\int_{-\infty}^{+\infty} p( x ) dx=1$

### 1.2.2. 期望 与 协方差

期望 ( expectation ) : 函数的平均值

-   离散随机变量 的 期望 : $\mathbb{E}[f] = \sum_x p ( x ) f ( x )$
-   连续随机变量 的 期望 : $\mathbb{E}[f]=\int p ( x ) f ( x ) dx$
-   有限数量的数据集，数据集中的点满足某个 概率分布函数 或者 概率密度函数，那么 **期望** 可以用 **求和** 的方式来估计

$$
\mathbb{E}[f]\simeq \frac1N\sum_{n=1}^N f ( x_n )
$$

-   多变量函数的期望，使用下标 $\mathbb{E}_x [f ( x,y )]$ 表明关于 $x$ 的分布的平均，是 $y$ 的一个函数

$$
\mathbb{E}_x [f ( x,y )] = \sum_x p ( x ) f ( x,y )
$$

-   条件分布的条件期望 ( conditional expectation )

$$
\mathbb{E}_x [f|y]=\sum_x p ( x|y ) f ( x )
$$

方差 ( variance ) : 度量了函数 $f ( x )$ 在均值 $\mathbb{E}[f ( x )]$ 附近的变化性
$$
\begin{aligned}
\text{var}[f] &= \mathbb{E}[( f ( x ) -\mathbb{E}[f ( x )] )^2] =\mathbb{E}[f ( x )^2]-\mathbb{E}[f ( x )]^2\\
\text{var}[x] &=\mathbb{E}[x^2]-\mathbb{E}[x]^2
\end{aligned}
$$

协方差 ( covariance ) : 度量两个随机变量之间的关系，表示多大程度上$x$和$y$会共同变化。

-   两个值随机变量的协方差

$$
\text{cov}[x,y] = \mathbb{E}_{x,y} [( x - \mathbb{E}[x] ) ( y - \mathbb{E}[y] )] = \mathbb{E}_{x,y}[xy] - \mathbb{E}[x]\mathbb{E}[y]
$$

-   两个向量随机变量协方差，协方差是一个矩阵

$$
\text{cov}[\text{x},\text{y}] = \mathbb{E}_{\text{x},\text{y}} [( \text{x}-\mathbb{E}[\text{x}] ) ( \text{y}^T-\mathbb{E}[\text{y}^T] )] = \mathbb{E}_{\text{x},\text{y}} [\text{xy}^T]-\mathbb{E}[\text{x}]\mathbb{E}[\text{y}^T]
$$

-   注：如果两个随机变量相互独立，则它们的协方差为 0

-   一个向量随机变量 : $\text{cov}[x] \equiv \text{cov}[x,x]$

### 1.2.3. 经典概率论 与 贝叶斯概率论

经典概率论

-   概率 : 随机重复事件发生的频率。
-   似然函数
    -   频率学派：参数 $w$ 是固定的值，可以通过某种形式的"估计"来确定
        -   估计的误差通过考察可能的数据集 $\mathcal{D}$ 获得
        -   确定误差的方法 : （自助发）Bootstrap[^Geof,2009]。通过从原始数据集中随机抽取来创建多个数据集，再对多个数据集的估计值求得方差确定参数误差
    -   贝叶斯派：只有⼀个数据集$D$（即实际观测到的数据集），参数的不确定性通过$w$的概率分布来表达
    -   似然函数的负对数被叫做误差函数 ( error function )，**最大化似然函数** 等价于 **最小化误差函数**
    -   广泛使用最大似然估计 ( maximum likelihood estimator, MLE ) [^Duda,2003] ( P 68 )，
    -   无信息先验概率 就是 最大似然估计

注意：似然函数不是$w$的分布，并且它关于$w$的积分不（一定）等于1

Bayesian 概率

-   概率 : 定量描述不确定性的工具。
-   Bayesian 定理 : $p ( Y|X ) =\frac{p ( X|Y ) p ( Y )}{p ( X )}=\frac{p ( X|Y ) p ( Y )}{\sum_Y p ( X|Y ) p ( Y )}$
    -   先验概率 : $p ( w )$ 表达参数 $w$ 的假设
        -   先验概率可以方便地将先验知识包含在模型中
    -   似然函数 : 使用条件概率 $p ( \mathcal{D}|w )$ 表达观测数据的效果
        -   由观测数据集 $D$ 来估计，可以被看成参数向量 $w$ 的函数。
            -   参数的不确定性通过概率分布来表达。
        -   不是 $w$ 的概率分布，因为它关于 $w$ 的积分并不 ( 一定 ) 等于 1。因此它只是 $w$ 的似然函数，不是概率。
    -   后验概率 : $p ( w|\mathcal{D} )$ 表达观测到 $\mathcal{D}$ 之后估计参数 $w$ 的不确定性。
        -   $p ( w|mathcal{D} ) =\frac{p ( \mathcal{D}|w ) p ( w )}{p ( \mathcal{D} )}$
    -   后验概率 $\propto$似然函数 $\text{x}$ 先验概率 
    -   取样方法 : MCMC ( Ch 11 ) [^Geof,2009]
    -   高效判别方法 : 变分贝叶斯 ( Variational Bayes ) 和期望传播 ( Expectation Propagation )

### 1.2.4. 高斯分布

 ( 主要概率分布 Ch 02 )

高斯分布 ( Gaussian distribution )，也叫正态分布 ( normal distribution )

一元实值随机变量 $x$ 的高斯分布

-   定义

$$
\mathcal{N} ( \text{x}|\mu,\sigma^2 ) =
  \frac{1}{( 2\pi\sigma^2 )^{1/2}}
  \exp[-\frac{1}{2\sigma^2} ( \text{x}-\mu )^2]
$$

-   控制参数
    -   均值 $\mu$
    -   方差 $\sigma^2$，标准差 $\sigma$
    -   精度 ( precision ) $\beta = 1/{\sigma^2}$
-   性质
    -   归一化 : $\int_{-\infty}^{+\infty} \mathcal{N} ( x|\mu,\sigma^2 ) dx =1$
    -   期望 : $\mathbb{E}[x] = \int_{-\infty}^{+\infty} \mathcal{N} ( x|\mu,\sigma^2 ) x dx =\mu$
    -   二阶矩 : $\mathbb{E}[x^2] = \int_{-\infty}^{+\infty} \mathcal{N} ( x|\mu,\sigma^2 ) x^2 dx =\mu^2 + \sigma^2$
    -   方差 : $\text{var}[x] = \mathbb{E}[x^2] - \mathbb{E}[x]^2 = \sigma^2$
    -   分布的最大值叫众数，与均值恰好相等。

D 维随机向量 $\text{x}$ 的高斯分布

-   定义

$$
\mathcal{N} ( \text{x}|\boldsymbol{\mu},\Sigma ) =
  \frac1{( 2\pi )^{D/2}}\frac1{|\Sigma|^{1/2}}
  \exp\{-\frac12 ( \text{x}-\boldsymbol{\mu} )^T\Sigma^{-1} ( \text{x}-\boldsymbol{\mu} ) \}
$$

-   控制参数
    -   均值向量 : $\boldsymbol{\mu}\in\mathcal{R}^D$
    -   协方差矩阵 : ${\Sigma}\in\mathcal{R}^{D \times D}$

一元随机实值变量的例子

-   观测数据集 $\mathbf{x}= ( x_1,\cdots,x_N )^T$ 表示实值变量 $x$ 的 $N$ 次观测
    -   观测数据集独立地从高斯分布 $\mathcal{N} ( \mu,\sigma^2 )$ 中抽取出来
        -   $\mu$ 和 $\sigma^2$ 未知
        -   独立同分布 ( independent and identically distributed, i.i.d. ) : 独立地从相同数据点集合中抽取的数据
-   观测数据集的概率

$$
p ( \mathbf{x}|\mu,\sigma^2 ) = \prod_{n=1}^N \mathcal{N} ( x_n|\mu,\sigma^2 )
$$

-   对数似然函数 :

$$
\ln p ( \mathbb{x}|\mu,\sigma^2 )
  = -\frac{1}{2\sigma^2} \sum_{n=1}^N ( x_n-\mu )^2
    -\frac{N}{2}\ln\sigma^2 -\frac{N}{2}\ln ( 2\pi )
$$

-   基于 最大似然函数 估计参数
    -   均值 的 最大似然解 等于 样本均值
        -   $\mu_{ML}=\frac{1}{N}\sum_{n=1}^N x_n$
    -   方差 的 最大似然解 等于 样本均值 的 样本方差
        -   $\sigma_{ML}^2=\frac{1}{N}\sum_{n=1}^N ( x_n-\mu_{ML} )^2$
    -   理论上需要同时关于 $\mu$ 和 $\sigma^2$ 最大化函数，实际上 $\mu$ 的解与 $\sigma^2$ 的无关，因此可以先求解 $\mu$，再求解 $\sigma^2$
-   最大似然的偏移问题 是 多项式曲线拟合问题 中遇到的 过拟合问题 的核心。
    -   最大似然估计 的 均值 等于 模型 中 输入 的 真实均值
        -   ( Eq 1.55 ) : $\mathbb{E}[\mu_{ML}]=\mu$
    -   最大似然估计 的 方差 小于 模型 中 输入 的 真实方差
        -   ( Eq 1.56 ) : $\mathbb{E}[\sigma_{ML}^2]=\frac{N-1}{N}\sigma^2$
    -   无偏的方差估计
        -   ( Eq 1.59 ) : $\tilde{\sigma}^2 = \frac{N}{N-1}\sigma_{ML}^2 = \frac1{N-1}\sum_{n=1}^N ( x_n-\mu_{ML} )^2$
        -   在 $N\rightarrow\infty$ 时，最大似然解的偏移影响不大，方差的最大似然解与真实方差相等
-   最大似然估计 vs 最大后验估计
    -   最大似然估计 : 在给定的"数据集"下最大化"参数"的概率
    -   最大后验估计 : 在给定"参数"下最大化"数据集"的概率

### 1.2.5. 概率建模 : 多项式曲线拟合

概率建模
-   一对数据点 $( x,y )$ 的模型

$$
p ( t|x,\text{w},\beta ) = \mathcal{N} ( t|y ( x,\text{w} ) ,\beta^{-1} )
$$

-   $N$对数据点 ( 数据集 ) 的模型

$$
p ( \mathbf{t}|\mathbf{x},\text{w},\beta ) = \prod_{n=1}^N \mathcal{N} ( t_n|y ( x_n,\text{w} ) ,\beta^{-1} )
$$

-   对数似然函数

$$
\ln p ( \mathbf{t}|\mathbf{x},\text{w},\beta ) = -\frac{\beta}{2} \sum_{n=1}^N ( y ( x_n,\text{w} ) -t_n )^2 +\frac{N}{2}\ln\beta -\frac{N}{2}\ln ( 2\pi )
$$

-   模型参数
    -   $\beta=1/\sigma^2$
    -   $y ( x,\text{w} ) =\sum_{j=0}^M w_j x^j$

最大似然解 ( 最大化似然函数 等价于 最小化平方和误差函数 )

-   模型参数 : $\text{w}_{ML}$
-   精度参数 : $\frac{1}{\beta_{ML}}=\frac1N\sum_{n=1}^N [y ( x_n,\text{w}_{ML} ) -t_n]^2$
-   预测分布 ( Eq 1.64 ) : $p ( t|x,\text{w}_{ML},\beta_{ML} ) = \mathcal{N} ( t|y ( x,\text{w}_{ML},\beta_{ML}^{-1} ))$

最大后验解 ( 最大化后验概率 等价于 最小化正则化的平方和误差函数 )

-   先验概率

$$
p ( \text{w}|\alpha ) = \mathcal{N} ( \text{w}|\boldsymbol{0},\alpha^{-1}\mathbf{I} ) = ( \frac{\alpha}{2\pi} )^{( M+1 ) /2} \exp{( -\frac{\alpha}{2}\text{w}^T\text{w} )}
$$

-   后验概率

$$
p ( \text{w}|\mathbf{x},\mathbf{t},\alpha,\beta ) \propto p ( \mathbf{t}|\mathbf{x},\text{w},\beta ) p ( \text{w}|\alpha )
$$

-   与 最小化正则化的平方和误差函数 的等价公式

$$
\frac\beta2\sum_{n=1}^N \{y ( x_n,\text{w} ) -t_n\}^2+\frac\alpha2\text{w}^T\text{w}
$$

### 1.2.6. 贝叶斯曲线拟合 ( Sec 3.3. )

 ( 依然用于理解贝叶斯观点在模型中的应用，无法理解可以暂时放弃 )

使用最大后验估计，依然属于点估计，不属于贝叶斯的观点。在模式识别中，对所有的模型参数 $\text{w}$ 进行积分才是贝叶斯方法的核心。

贝叶斯模型

-   前提条件
    -   输入数据 $\mathbf{x}$
    -   目标数据 $\mathbf{t}$
    -   新的测试点 $x$
    -   新的预测目标 $t$
-   预测概率 ( Eq 1.69 )

$$
p ( t|x,\mathbf{x},\mathbf{t} ) = \int p ( t|x,\text{w},\beta ) p ( \text{w}|\mathbf{x},\mathbf{t},\alpha,\beta ) d\text{w} = \mathcal{N} ( t|m ( x ) ,s^2 ( x ))
$$

-   模型参数
    -   $m ( x ) =\beta\phi ( x )^T \text{S}\sum_{n=1}^N \phi ( x_n ) t_n$：表示预测值 $t$ 的不确定性，受目标变量上的噪声影响，在最大似然的预测分布 ( 1.64 ) 中通过 $\beta_{ML}^{-1}$ 表达
    -   $s^2 ( x ) =\beta^{-1} +\phi ( x )^T\text{S}\phi ( x )$：对参数 $\text{w}$ 的不确定性有影响
    -   $\text{S}^{-1} =\alpha\mathbf{I} + \beta\sum_{n=1}^N\phi ( x_n ) \phi ( x_n )^T$
    -   $\phi ( x ) \equiv \phi_i ( x ) = x^i, ( i=0,\cdots,M )$

## 1.3. 模型选择

-   模型复杂度的控制 :
    -   多项式的阶数控制了模型的自由参数的个数，即控制了模型的复杂度
    -   正则化系数 $\lambda$ 影响着模型的自由参数的取值，也控制了模型的复杂度
-   交叉验证 ( cross validation )
    -   可以解决验证模型时面临的数据不足问题
    -   会增加训练成本
    -   留一法 ( leave-one-out ) 是特例
-   信息准则 : 度量模型的质量，避免交叉验证的训练成本，使模型选择完全依赖于训练数据
    -   修正最大似然的偏差的方法 : 增加惩罚项来补偿过于复杂的模型造成的过拟合
    -   赤池信息准则 ( Akaike information criterion, AIC ) : $\ln p ( \mathcal{D}|\text{w}_{ML} ) -M$
    -   贝叶斯信息准则 ( Bayesian information criterion, BIC ) : ( Sec 4.4.1. )

## 1.4. 维度灾难

我们在三维空间中建立的集合直觉会在考虑高维空间时不起作用。例如，在高维空间中，一个球体的大部分的体积在哪里？( 可以推导公式理解 )

-   $D$ 维空间的 $r=1$ 的球体的体积 : $V_D ( r ) = K_D r^D$
-   $r=1-\epsilon$ 和 $r = 1$ 之间的球壳体积与 $r=1$ 的球体的体积比 :
    -   $( V_D ( 1 ) - V_D ( 1-\epsilon )) /V_D ( 1 ) =1- ( 1-\epsilon )^D$
    -   $D$ 的维数越大，体积比趋近于 1
    -   在高维空间中，一个球体的大部分体积都聚焦在表面附近的薄球壳上。

维度灾难 ( curse of dimensionality ) : 在高维空间中，大部分的数据都集中在薄球壳上导致数据无法有效区分

-   维度灾难的解决方案
    -   真实数据经常被限制在有着较低的有效维度的空间区域中；( 通过特征选择降维 )
    -   真实数据通常比较光滑 ( 至少局部上比较光滑 ) ( 通过局部流形降维 )

## 1.5. 决策论 ( 分类问题、模式识别 )

问题原型：输入向量 $\text{x}$ 和 目标向量 $\text{t}$

-   回归问题：目标向量是连续性变量组成
-   分类问题：目标向量是离散性变量组成，即类别标签你

决策论 : 保证在不确定性的情况下做出最优的决策。

-   联合概率分布 $p ( \text{x},\text{t} )$ 总结了所有的不确定性
-   从训练数据集中确定输入向量 $\text{x}$ 和目标向量 $\text{t}$ 的联合分布 $p ( \text{x,t} )$ 是推断 ( inference ) 问题
-   从测试数据集中确定输入向量 $\text{x}$ 和目标分类 $\mathcal{C}_k$ 的条件概率 $p ( \mathcal{C}_k |\text{x} )$ 是决策 ( inference ) 问题

### 1.5.1. 最小化错误分类率

基本概念

-   输入空间根据规则切分成不同的区域，这些区域被称为决策区域。
-   每个类别都有一个决策区域，区域 $\mathcal{R}_k$ 中的点都被分到对应类别 $\mathcal{C}_k$ 中
-   决策区域间的边界叫做决策边界 ( decision boundary ) 或者决策面 ( decision surface )。

决策 : $p ( \mathcal{C}_k|\text{x} ) =\frac {p ( \text{x}|\mathcal{C}_k ) p ( \mathcal{C}_k )}{p ( \text{x} )}$

-   $p ( \mathcal{C}_k )$ 称为类 $\mathcal{C}_k$ 的先验概率
-   $p ( \mathcal{C}_k|\text{x} )$ 称为类 $\mathcal{C}_k$ 的后验概率
-   ( Fig 1.24 ) 最小化错误分类率，将 $\text{x}$ 分配到具有最大后验概率的类别中

最小化错误分类率 : 将 $\text{x}$ 分配到后验概率 $p ( \mathcal{C}_k|\text{x} )$ 最大的类别中，分类错误的概率最小

$$
\begin{aligned}
p ( \text{mistake} )
  &=p ( \text{x}\in\mathcal{R}_1,\mathcal{C}_2 ) +p ( \text{x}\in\mathcal{R}_2,\mathcal{C}_1 ) \\
  &=\int_{\mathcal{R}_1} p ( \text{x},\mathcal{C}_2 ) \text{dx}+\int_{\mathcal{R}_2} p ( \text{x},\mathcal{C}_1 ) \text{dx}
\end{aligned}
$$

最大化正确分类率 : 将 $\text{x}$ 分配到联合概率 $p ( \mathcal{C}_k,\text{x} )$ 最大的类别中，分类正确的概率最大

$$
p ( \text{correct} )
  =\sum_{k=1}^Kp ( \text{x}\in\mathcal{R}_k,\mathcal{C}_k )
  =\sum_{k=1}^K\int_{\mathcal{R}_k} p ( \text{x},\mathcal{C}_k ) \text{dx}
$$

### 1.5.2. 最小化期望损失 ( 加先验 )

损失函数 ( loss function ) 也称为代价函数 ( cost function )，是对所有可能的决策产生的损失的一种整体度量，目标是最小化整体的损失。

效用函数 ( utility function )，目标是最大化整体的效用，如果效用函数等于损失函数的相反数，则两者等价。

最小化损失的期望 : $\mathbb{E}[L]=\sum_k\sum_j\int_{\mathcal{R}_j} L_{kj} p ( \text{x},\mathcal{C}_k ) dx$

-   $L_{kj}$ 表示损失矩阵的第 $k,j$ 个元素，即把类别 $\mathcal{C}_k$ 的数据 $\text{x}$ 错分为 $C_j$ 的损失

最小化损失的期望的决策规则 : 对于每个新的 $\text{x}$，分到的类别 $C_j$ 保证 $\sum_k L_{kj}p ( \mathcal{C}_k|\text{x} )$ 最小化

### 1.5.3. 拒绝选项

拒绝选项 ( reject option ) : 为了避免做出错误的决策，系统拒绝从做出类别选择，从而使模型的分类错误率降低。

### 1.5.4. 推断与决策

分类问题的两个阶段

-   推断 ( inference ) : 使用训练数据学习后验概率 $p ( \mathcal{C}_k|x )$ 的模型
-   决策 ( decision ) : 使用后验概率模型进行最优的分类

解决分类问题的三种方法 :

-   生成式模型 ( generative model ) : 显式或者隐式地对输入和输出进行建模的方法
    -   推断阶段 :
        -   首先基于每个类别 $\mathcal{C}_k$ 推断类条件密度 $p ( |\mathcal{C}_k )$
        -   再推断类别的先验概率密度 $p ( \mathcal{C}_k )$
        -   再基于贝叶斯定理 $p ( \mathcal{C}_k|\text{x} ) = \frac {p ( \text{x}|\mathcal{C}_k ) p ( \mathcal{C}_k )}{p ( \text{x} )} = \frac {p ( \text{x}|\mathcal{C}_k ) p ( \mathcal{C}_k )} {\sum_k p ( \text{x}||\mathcal{C}_k ) p ( \mathcal{C}_k )}$ 学习类别的后验概率密度
    -   决策阶段 : 使用决策论来确定每个新的输入 $\text{x}$ 的类别。
    -   优点：能够计算数据的边缘概率密度 $p ( \text{x} )$，方便计算具有低概率的新数据点，即离群点检测。
    -   缺点：需要计算的工作量最大
-   判别式模型 ( Discriminative Models ) : 直接对后验概率密度建模的方法
    -   推断阶段 : 首先推断类别的后验概率密度 $p ( \mathcal{C}_k|\text{x} )$
    -   决策阶段 : 使用决策论来确定每个新的输入$\text{x}$的类别。
-   同时解决两个阶段的问题，即把输入直接映射为决策的函数称为判别函数 ( discriminant function )

使用后验概率进行决策的理由

-   最小化风险 : 修改最小风险准则就可以适应损失矩阵中元素改变
-   拒绝选项 :
    -   确定最小化误分类率的拒绝标准
    -   确定最小化期望损失的拒绝标准
-   补偿类别先验概率 : 解决不平衡的数据集存在的问题
-   组合模型 ( Ch 14 ) : 通过特征的独立性假设，将大问题分解成小问题，例如 : 朴素贝叶斯模型

$$
\begin{aligned}
p ( \text{x}_I,\text{x}_B|\mathcal{C}_k )
    &=p ( \text{x}_I|\mathcal{C}_k ) p ( \text{x}_B|\mathcal{C}_k ) \\
p ( \mathcal{C}_k|\text{x}_I,\text{x}_B )
    &\propto p ( \text{x}_I,\text{x}_B|\mathcal{C}_k ) p ( \mathcal{C}_k ) \\
    &\propto p ( \text{x}_I|\mathcal{C}_k ) p ( \text{x}_B|\mathcal{C}_k ) p ( \mathcal{C}_k ) \\
    &\propto \frac{p ( \mathcal{C}_k|\text{x}_I ) p ( \mathcal{C}_k|\text{x}_B )}{p ( \mathcal{C}_k )}
\end{aligned}
$$

### 1.5.5. 回归问题的损失函数

回归问题的决策阶段：对于每个输入向量 $\text{x}$，输出目标变量 $t$ 的特定估计 $y ( \text{x} )$，造成损失 $L ( t,y ( \text{x} ))$，则整个问题的平均损失，即损失函数的期望
$$
\mathbb{E}[L]=\int\int L ( t,y ( \text{x} )) p ( \text{x},t ) \text{dx d} t
$$

-   变分法最小化 $\mathbb{E}[L]$

$$
\frac{\delta\mathbb{E}[L]}{\delta y ( \text{x} )}= 2\int [y ( \text{x} ) - t] p ( \text{x},t ) dt =0
$$

-   求解 $y ( \text{x} )$ 的最优解

$$
y ( \text{x} ) = \frac{\int t p ( \text{x},t ) \text{d}t}{p ( \text{x} )}= \int t p ( t|\text{x} ) \text{d}t= \mathbb{E}_t [t|\text{x}]
$$

损失函数的具体选择

-   平方损失函数 : $L ( t,y ( \text{x} )) =\{y ( \text{x} ) -t\}^2$
-   损失函数的期望

$$
\begin{aligned}
\mathbb{E}[L]
    & =\int\int\{y ( \text{x} ) -t\}^2 p ( \text{x},t ) \text{dx d} t\\
    & =\int\int \{y ( \text{x} ) - \mathbb{E}_t [t|\text{x}] + \mathbb{E}_t [t|\text{x}] -t\}^2 p ( \text{x},t ) \text{dx d}t\\
    & = \int\int [\{y ( \text{x} ) - \mathbb{E}_t [t|\text{x}]\}^2 + 2\{y ( \text{x} ) - \mathbb{E}_t [t|\text{x}]\}\{\mathbb{E}_t [t|\text{x}] -t\} + \{\mathbb{E}_t [t|\text{x}] -t\}^2] p ( \text{x},t ) \text{dx d}t\\
    & = \int \{y ( \text{x} ) - \mathbb{E}_t [t|\text{x}]\}^2 p ( \text{x} ) \text{dx}+ \int\text{var}[t|\text{x}] p ( \text{x} ) \text{dx}
\end{aligned}
$$

-   第一项：当 $y ( \text{x} ) = \mathbb{E}_t [t|\text{x}]$ 时取得最小值，即 $\mathbb{E}[L]$ 最小化。
    -   说明最小平方预测由条件均值给出。
-   第二项：是 $t$ 的分布的方差在 $\text{x}$ 上进行了平均，表示目标数据内在的变化性，即噪声。
    -   与 $y ( \text{x} )$ 无关，表示损失函数的不可减小的最小值。
-   闵可夫斯基 ( Minkowski ) 损失函数 ( 平方损失函数的一种推广 ) 

$$
\begin{aligned}
L_q ( t,y ( \text{x} )) &=|y ( \text{x} ) -t|^q\\
\mathbb{E}[L_q] &=\int\int|y ( \text{x} ) -t|^q p ( \text{x},t ) \text{dx d} t
\end{aligned}
$$

解决回归问题的三种思路 ( 建议从机器学习参考书中熟悉这三种方法 )

-   函数模型 : 直接从训练数据中寻找一个回归函数
-   概率建模 : 最大似然 : 先解决条件概率密度的推断问题，再求条件均值；
-   概率建模 : 最大后验 : 先求联合概率密度，再求条件概率密度，最后求条件均值；

## 1.6. 信息论

离散随机变量 $x$

-   信息量 : 学习 $x$ 的值的时候的「惊讶」程度，
    -   $h ( x ) =-\log_2 p ( x )$
    -   负号确保信息一定是正数或者为零
    -   对数确保低概率事件 $x$ 对应高的信息量
        -   使用 2 作为对数的底，$h ( x )$ 的单位是比特 ( bit, binary digit )
        -   使用 e 作为对数的底，$h ( x )$ 的单位是奈特 ( nat, Napierian digit )
-   平均信息量 : 发送者想传输一个随机变量的值给接收者，在这个传输过程中，他们传输的平均信息量就是随机变量 $x$ 的 **熵**。
    -   $H [x]=-\sum_x p ( x ) \log_2 p ( x )$
    -   无噪声编码定理 ( noiseless coding theorem ) 表明：**熵** 是传输一个随机变量状态值所需要的比特位的下界。
    -   在统计力学中，熵用来描述无序程度的度量。

离散随机变量的熵 ( entropy )

-   熵：$H [p ( x )] = -\sum_i p ( x_i ) \ln p ( x_i )$
-   离散随机变量服从均匀分布时，其熵值最大

连续随机变量的熵

-   $H_{\Delta}$ 的定义
    -   $\sum_i p ( x_i ) \Delta = 1$
    -   因为 $\Delta\rightarrow0$ 时，熵的连续形式与离散形式的差 $\ln\Delta\rightarrow\infty$，即具体化一个连续变量需要大量的比特位，因此省略 $-\ln\Delta$ 得连续随机变量的熵 $\lim_{\Delta\rightarrow0}\{-\sum_i p ( x_i ) \Delta\ln p ( x_i ) \} = -\int p ( \text{x} ) \ln p ( \text{x} ) \text{dx}$

$$
\begin{aligned}
H_{\Delta}
&=-\sum_i p ( x_i ) \Delta\ln ( p ( x_i ) \Delta ) \\
&=-\sum_i p ( x_i ) \Delta\ln p ( x_i ) - \sum_i p ( x_i ) \Delta \ln\Delta \\
&= -\sum_i p ( x_i ) \Delta\ln p ( x_i ) - \ln\Delta
\end{aligned}
$$

-   微分熵：$H[\text{x}] = -\int p ( \text{x} ) \ln p ( \text{x} ) \text{dx}$
    -   微分熵可以为负
-   最大化微分熵需要遵循的三个限制
    -   $\int_{-\infty}^{+\infty} p ( x ) \text{d}x=1$
    -   $\int_{-\infty}^{+\infty} xp ( x ) \text{d}x=\mu$
    -   $\int_{-\infty}^{+\infty} ( x-\mu )^2 p ( x ) \text{d}x=\sigma^2$
-   使用 拉格朗日乘数法求解带有限制条件的最大化问题
    -   使用变分法求解这个函数，令其导数等于零，得 $p ( x ) =\exp\{-1+\lambda_1+\lambda_2 x+\lambda_3 ( x-\mu )^2\}$
    -   将结果带入限制方程，得

$$
\begin{aligned}
p ( x ) &= \frac1{( 2\pi\sigma^2 )^{1/2}} \exp\{-\frac{( x-\mu )^2}{2\sigma^2}\}\\
-\int_{-\infty}^{+\infty} p ( x ) \ln p ( x ) \text{d}x
  &+ \lambda_1 ( \int_{-\infty}^{+\infty} p ( x ) \text{d}x -1 ) \\
  &+ \lambda_2 ( \int_{-\infty}^{+\infty} xp ( x ) \text{d}x - \mu )\\
  &+ \lambda_3 ( \int_{-\infty}^{+\infty} ( x-\mu )^2 p ( x ) \text{d}x - \sigma^2 )
\end{aligned}
$$

-   连续随机变量服从高斯分布时，其熵值最大。
    -   高斯分布的微分熵 $H [x]=\frac12\{1+\ln ( 2\pi\sigma^2 ) \}$
    -   熵随着分布宽度 ( $\sigma^2$ ) 的增加而增加
    -   当 $\sigma^2<\frac1{2\pi e}$ 时，$H [x]<0$
-   在给定 x 的情况下，y 的条件熵
    -   $H[\text{y|x}]=-\int\int p ( \text{y,x} ) \ln p ( \text{y|x} ) \text{ dy dx}$

联合熵

-   $H[\text{x,y}] = H[\text{y|x}] + H[\text{x}]$
-   $H[\text{x,y}]$ 是 $p ( \text{x,y} )$ 的微分熵
-   $H[\text{y|x}]$ 是 $p ( \text{y|x} )$ 的微分熵
-   $H[\text{x}]$ 是 $p ( \text{x} )$ 的微分熵

### 1.6.1. 相对熵 和 互信息

凸函数 : 每条弦都位于函数的弧或者弧的上面。$f ( \lambda a + ( 1-\lambda ) b ) \leq \lambda f ( a ) + ( 1-\lambda ) f ( b )$

-   凸函数的二阶导数处处为正。
-   严格凸函数 ( strictly convex function ) : 等号只在 $\lambda=0$ 和 $\lambda=1$ 处取得。

凹函数 : 如果 $f ( x )$ 是凸函数，则 $-f ( x )$ 是凹函数

-   严格凹函数 ( strictly concave function )

Jensen 不等式

-   $f ( \sum_{i=1}^M \lambda_i x_i \leq \sum_{i=1}^M \lambda_i f ( x_i )$
    -   将 $\lambda_i$ 看作取值为 $\{x_i\}$ 的离散变量 $x$ 的概率分布，则公式为 $f ( \mathbb{E}[x] ) \leq \mathbb{E}[f ( x )]$
-   $f ( \int\text{x}p\text{( x ) dx} ) \leq\int f ( \text{x} ) p ( \text{x} ) \text{dx}$

KL 散度 : 是两个分布 $p ( \text{x} )$ 和 $q ( \text{x} )$ 之间不相似程度的度量。

-   $-\ln x$ 是严格凸函数
-   归一化条件：$\int q ( \text{x} ) \text{dx}=1$

$$
\begin{aligned}
\text{KL} ( p||q )
  &= -\int p ( \text{x} ) \ln q\text{( x ) dx} - ( -\int p ( \text{x} ) \ln p\text{( x ) dx} ) \\
\text{KL} ( p||q )
  &= -\int p ( \text{x} ) \ln ( \frac{q ( \text{x} )}{p ( \text{x} )} ) \text{dx}
  \geq -\ln\int q\text{( x ) dx} = 0 \\
\end{aligned}
$$

Laplace 近似 : 是用方便计算的分布 去逼近 不方便计算的分布，解决 KL 散度不易计算的问题 ( Sec 4.4 )

-   假设数据通过未知分布 $p ( \text{x} )$ 生成，为了对 $p ( \text{x} )$ 建模可以使用  $q ( \text{x}|\theta )$ 来近似，$q ( \text{x}|\theta )$ 可以是一个已知分布 ( 例如： $\theta$ 是控制多元高斯分布的参数 )，通过最小化  $p ( \text{x} )$ 和 $q ( \text{x}|\theta )$ 之间关于 $\theta$ 的 KL 散度，就可以得到 $p ( \text{x} )$ 的近似分布。
    -   $\text{KL} ( p||q ) \simeq \frac1N \sum_{n=1}^N [-\ln q ( \text{x}_n|\theta ) + \ln p ( \text{x}_n )]$
        -   $-\ln q ( \text{x}_n|\theta )$ 是使用训练集估计的分布 $q ( \text{x}|\theta )$ 下的 $\theta$ 的负对数似然函数
        -   $\ln p ( \text{x}_n )$ 与 $\theta$ 无关
        -   最小化 KL 散度 等价于 最大化似然函数

-   互信息 ( mutual information ) : 表示一个新的观测 $\text{y}$ 造成的 $\text{x}$ 的不确定性的减小 ( 反之亦然 )
    -   两个随机变量 $p ( \text{x} )$ 和 $p ( \text{y} )$ 组成的数据集由 $p ( \text{x,y} )$ 给出
        -   如果两个变量相互独立，则联合分布 $p ( \text{x,y} ) =p ( \text{x} ) p ( \text{y} )$，互信息 $I[\text{x,y}]=0$
        -   如果两个变量没有相互独立，可以通过 KL 散度判断它们之间的「独立」程度，也称为随机变量 $p ( \text{x} )$ 和 $p ( \text{y} )$ 之间的互信息 $I[\text{x,y}]>0$

$$
\begin{aligned}
I[\text{x,y}]
  &\equiv\text{KL} ( p ( \text{x,y} ) ||p ( \text{x} ) p ( \text{y} )) \\
  &= -\int\int p ( \text{x,y} ) \ln ( \frac{p ( \text{x} ) p ( \text{y} )}{p ( \text{x,y} )} ) \text{ dx dy} \\
I[\text{x,y}]
  &= H[\text{x}] - H[\text{x|y}]\\
  &= H[\text{y}] - H[\text{y|x}]
\end{aligned}
$$

相对熵 ( relative entropy ) 或者 KL 散度 ( Kullback-Leibler divergence ) ( 推导过程建议理解 )

## 小结

-   本章对于后面需要的重要概念都进行了说明和推导，方便深入理解后面提及的各种算法。
-   如果看完本章后感觉充斥着许多新的概念，那么建议先放下，找本更加基础的模式识别与机器学习的书，例如 : 李航的《统计学习方法》和周志华的《机器学习》等

