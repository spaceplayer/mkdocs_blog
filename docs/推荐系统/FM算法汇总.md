## FM算法汇总

### one-hot 局限

category 特征需要进行 one-hot 处理，引起两个问题：

- 稀疏
- 特征空间变得巨大

### FM

- 因式分解机(fm)结合了SVM和因式分解模型二者的优势

- 相比SVM的二阶多项式核而言，FM在样本稀疏的情况下是有优势的；而且，FM的训练/预测复杂度是线性的，而二项多项式核SVM需要计算核矩阵，核矩阵复杂度就是N平方。
- FM可以像SVM一样处理real value 特征，二者FM还可以处理稀疏的数据，而这里SVM却 是失败了的，FM处理稀疏数据主要机制是：因式分解参数可以对任何两个特征直接做组合。
- FFM二次项并不能够化简，其预测复杂度是 O(kn2)O(kn2)。

- 组合特征数量 $\frac{n(n-1)}{2}$，由于one-hot导致的高稀疏导致同时两个特征为1的数量极低，难以通过训练学习$w_{ij}$

- 为了求出$ω_{ij}$，我们对每一个特征分量$x_{i}$引入辅助向量Vi=(vi1,vi2,⋯,vik)。然后，利用$v_iv_j^T$对$ω_{ij}$进行求解

![image-20181114164847617](http://spaceplayer.oss-cn-beijing.aliyuncs.com/spaceplayer/typora_img/Library/Application Support/typora-user-images/image-20181114164847617.png)

- 要求出$<v_i,v_j>$，主要是采用了如公式$((a+b+c)^2−a^2−b^2−c^2$求出交叉项。具体过程如下

$\hat{y} = w_0 + \sum_1^nw_ix_i + \sum_{i=1}^n\sum_{j=i+1}^nw_{ij}x_ix_j$

$\hat{y} = w_0 + \sum_1^nw_ix_i + \sum_{i=1}^n\sum_{j=i+1}^n<v_i, v_j>x_ix_j$

$\hat{y} = w_0 + \sum_1^nw_ix_i + \sum_i^n\sum_{j=i+1}^n<v_i, v_j>x_ix_jI\{x_ix_j \in{keep\_set}\}$



n代表样本总数量

$w_0 $和$w_i$均为该公式线性部分（前两项）的权重因子

$<v_i, v_j>$代表的组合特征的权重因子，由$v_i$和$v_j$的点积求得

$\hat{y}$代表预测的结果

$x_i$代表输入样本的各个特征

$x_ix_j $代表组合特征

$I\{x_ix_j \in{keep\_set}\}$是示性函数，当大括号内部为''真''时，返回结果为1，否则返回0

keep_set 代表通过逐步回归筛选过后的组合特征子集





![image-20181114171908492](http://spaceplayer.oss-cn-beijing.aliyuncs.com/spaceplayer/typora_img/Library/Application Support/typora-user-images/image-20181114171908492.png)![image-20181114174110542](http://spaceplayer.oss-cn-beijing.aliyuncs.com/spaceplayer/typora_img/Library/Application Support/typora-user-images/image-20181114174110542.png)

![image-20181114180045862](http://spaceplayer.oss-cn-beijing.aliyuncs.com/spaceplayer/typora_img/Library/Application Support/typora-user-images/image-20181114180045862.png)

![image-20181114180543011](http://spaceplayer.oss-cn-beijing.aliyuncs.com/spaceplayer/typora_img/Library/Application Support/typora-user-images/image-20181114180543011.png)

### FFM

乍一看FFM的embedding方法似乎速度要比FM的embedding方法慢一个量级，但是在gpu的计算环境下，利用并行化的思想，可以将FFM的embedding方法的计算时间优化到与FM的embedding方法几乎一样。只是embedding矩阵的空间复杂度是没有办法再优化了

### DeepFM

https://zhuanlan.zhihu.com/p/38443751

### DeepFFM

### NFFM（腾讯算法大赛rank6）

https://zhuanlan.zhihu.com/p/38443751

deepffm是浅层部分为ffm，深度部分是embedding后的向量拼接成一个向量作为全连接层的输入。而nffm则是浅层部分是LR，深度部分是特征交叉后的向量作为全连接层的输入。

### FM 实现

#### csr稀疏表示

![image-20181114181903396](http://spaceplayer.oss-cn-beijing.aliyuncs.com/spaceplayer/typora_img/FM算法汇总/20181114181903396.png)

### Deep FM

- FM 实际上因为计算复杂度的原因一般都只用到了二阶特征组合 高阶组合使用DNN

- DeepFM是一个集成了FM和DNN的神经网络框架，思路和Google的Wide&Deep相似，都包括wide和deep两部分。W&D模型的wide部分是广义线性模型，DeepFM的wide部分则是FM模型，两者的deep部分都是深度神经网络。DeepFM神经网络部分，隐含层的激活函数用ReLu和Tanh做信号非线性映射，Sigmoid函数做CTR预估的输出函数。



  ![image-20181116145552340](http://spaceplayer.oss-cn-beijing.aliyuncs.com/spaceplayer/typora_img/FM算法汇总/image-20181116145552340.png)

  #### **相关工作**

  **FTRL算法**（McMahan et al. 2013），generalized linear model虽然简单，但实践中很有效果。但这类线性模型难以学习组合特征，一般需要手动构建特征向量，难以处理高阶组合或者没有出现在训练数据中的组合。

  **Factorization Machine模型**（Rendle
  2010），对特征之间进行向量内积，实现特征们的逐对组合pairwise interactions。理论上FM可以对高阶特征组合建模，但实践中只用order-2特征因为其高复杂度。

  **DNN**在特征表示学习中很有效果，可以用来学习组合特征。（Liu et al., 2015）和（Zhang et al., 2014）扩展CNN和RNN用于CTR预估，但CNN-based模型对领域特征有偏biased，RNN-based模型适合用在有时序依赖的点击数据。Factorization-machine supported Neural Network（**FNN**）（Zhang et al., 2016），在使用DNN前预训练FM，因此限制了FM的能力。Product-based Neural
  Network（**PNN**）（Qu et al., 2016）在DNN的embedding层和全连接层之间引入product层，来研究feature interactions。

  **Wide & Deep模型**(Cheng
  et al., 2016)认为，PNN/FNN和其他Deep模型提取很少low-order的feature interactions，提出的W&D模型可以同时对low-order和high-order建模，但要对wide和deep部分模型分别输入，其中wide部分还需要人工特征工程。

![image-20181116150521120](http://spaceplayer.oss-cn-beijing.aliyuncs.com/spaceplayer/typora_img/FM算法汇总/image-20181116150521120.png)

![image-20181116150728529](http://spaceplayer.oss-cn-beijing.aliyuncs.com/spaceplayer/typora_img/FM算法汇总/image-20181116150728529.png)

![image-20181116151041665](http://spaceplayer.oss-cn-beijing.aliyuncs.com/spaceplayer/typora_img/FM算法汇总/image-20181116151041665.png)

![image-20181116151132106](http://spaceplayer.oss-cn-beijing.aliyuncs.com/spaceplayer/typora_img/FM算法汇总/image-20181116151132106.png)

![image-20181116153003697](http://spaceplayer.oss-cn-beijing.aliyuncs.com/spaceplayer/typora_img/FM算法汇总/image-20181116153003697.png)

### NFM



$\hat{y} = sigmoid(y_{FM} + y_{DNN})$



![image-20190116232146228](../http://spaceplayer.oss-cn-beijing.aliyuncs.com/spaceplayer/typora_img/FM算法汇总/image-20190116232146228.png)

$y = -1.509x_1 + 0.132x_2 + 4.807x_4 + 654.584$









