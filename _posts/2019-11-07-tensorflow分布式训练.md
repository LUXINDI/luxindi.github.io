
Tensorflow分布式训练(结构篇)
https://dataxujing.github.io/TensorFlow%E5%88%86%E5%B8%83%E5%BC%8F%E5%B9%B6%E8%A1%8C-1/


简单粗暴Tensorflow 2.0
https://tf.wiki/zh/appendix/distributed.html

tensorflow官方文档
https://www.tensorflow.org/guide/distributed_training








周三:

模型评价:
1. ESMM model和LR model在33619973实验流量上评估:
   ESMM model在自己的实验流量上严重高估, 而LR model在该部分流量上比较明显的低估:
   目前ESMM model是在原有的LR model投出来的流量上训练的, 而在abtest时, 使用ESMM model最终投放出来的流量可能与LR投放出来的流量完全不一样, 这就导致了ESMM model实际的测试集与其训练集分布不一致的问题
   解决方案: 加大实验流量, 积攒数据

2. ESMM model在实验流量上评估的COPC与abtest的成本比值不一致
   问题: 部分广告主在实验流量上的copc接近1, 但是abtest的成本比值接近2
   证明:


$$成本比值=\frac{CPA_{true}}{CPA_{target}}, \quad COPC=\frac{order\_number}{\sum_{i=1}^{n}cvr_i}$$
$$CPA_{true}=\frac{fee}{order\_number}\leq\frac{CPA_{target}\times\sum_{i=1}^{n}cvr_i}{order\_number}$$
$$\Longrightarrow 成本比值\leq \frac{\sum_{i=1}^{n}cvr_i\times CPA_{target}}{order\_number\times CPA_{target}}=\frac{1}{COPC}$$
$$\Longrightarrow 成本比值\times COPC \leq 1$$



实验:
