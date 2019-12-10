
#### 介绍
用户行为可以抽象为一个序列模式(sequential pattern):
impression -> click -> conversion

sample selection bias: CVR model是在被点击数据上训练的，但是要在曝光数据上inference

data sparsity: 相对于CTR任务，数据小很多


Entire Space Multi-task Model:

pCTR: post-view click-through rate
pCVR: post-click conversion rate
pCTCVR: post-view click-through & conversion rate

pCTCVR = pCTR * pCVR


#### CVR任务的挑战
Sample selection bias:
传统的CVR任务：
$$
p(z=1|y=1,x) \approx q(z=1|x_c)
$$
$X_c$: 被点击过的样本空间

inference:
$$
p(z=1|y=1,x) \approx q(z=1|x)
$$
假设：任意的点击数据 $(x,y_x=1)$, $x\in X \rightarrow x\in X_c$

Data sparsity:
Delayed feedback:


#### Entire Space Multi-Task Model
两个sub-networks:
CVR network
CTR network

##### 损失函数
$$
L(\theta_{cvr},\theta_{ctr}) =\sum_{i=1}^N l(y_i, f(x_i; \theta_{ctr})) + \sum_{i=1}^N l(y_i\&z_i, f(x_i; \theta_{ctr}) \times f(x_i;\theta_{cvr}))
$$
其中，$\theta_{ctr}$ 和 $\theta_{cvr}$ 分别是CTR网络和CVR网络的参数，$l(\cdot)$ 是交叉熵损失函数。在CTR任务中，有点击行为的展现事件构成的样本标记为正样本，没有点击行为发生的展现事件标记为负样本；在CTCVR任务中，同时有点击和购买行为的展现事件标记为正样本，否则标记为负样本。

##### 整个空间上的建模
$$
pCVR = pCTCVR/pCTR
$$
$$
p(z=1|y=1,x) = \frac{p(y=1,z=1|x)}{p(y=1|x)}
$$
在ESMM中，pCVR是由上式约束的中间变量，pCTR和pCTCVR是ESMM预测的主要变量。


##### 特征表达的迁移
在ESMM中，CVR和CTR网络的embedding dictionary是共享的

### 阿里巴巴点击与转化预估数据集
https://tianchi.aliyun.com/dataset/dataDetail?dataId=408&userId=1

数据空间：
$$S=\{(x_i,y_i \rightarrow z_i)\} |^N_{i=1}$$


## 后续优化
[电商多目标优化小结](https://zhuanlan.zhihu.com/p/76413089)
