
Tensorflow分布式训练(结构篇)
https://dataxujing.github.io/TensorFlow%E5%88%86%E5%B8%83%E5%BC%8F%E5%B9%B6%E8%A1%8C-1/


简单粗暴Tensorflow 2.0
https://tf.wiki/zh/appendix/distributed.html

tensorflow官方文档
https://www.tensorflow.org/guide/distributed_training

周报: 2019-12-02 - 2019-12-06

一. 数据
   问题: ESMM model在实验流量上评估的COPC与abtest的成本比值不一致
   部分广告主在实验流量上的copc接近1, 但是abtest的成本比值接近2 (可证明:成本比值*COPC<=1)

   原因:
   投放机端抽取的DisplayType包含文字链类型: 26, 而从session-log抽取的训练集没有26这个特征, 被默认赋值为0(UNKNOWN).
   线上测试的时候, 由于在模型中找不到DisplayType=26特征的权重, 默认为0, 而实际上DisplayType=0的权重为负值(-1.098), 这就导致了部分投放文字链较多的广告主高估比较严重.

   处理:
    通知投放机端修改session-log的thrift字段后, 训练数据会同时混入DisplayType=0和DisplayType=26两种特征, 需将0统一替换为26


二. 模型评价
   ESMM model和LR model在33619973实验流量上评估:
   ESMM model在自己的实验流量上严重高估, 而LR model在该部分流量上比较明显的低估:

   分析:
   目前ESMM model是在原有的LR model投出来的流量上训练的, 而在abtest时, 使用ESMM model最终投放出来的流量可能与LR投放出来的流量完全不一样, 这就导致了ESMM model实际的测试集与其训练集分布不一致的问题

   解决方案:
   加大实验流量, 积攒数据



三. 实验
1. feature occurrence filter实验
对于出现频率小于一定值的feature进行过滤, 可以一定程度上避免稀疏特征对于数据量较小的广告主的影响.

以12-03.30的训练数据为例, 未筛选前不同feature值的个数是85834, 若仅保留feature occurrence>=10的特征值, feature值个数降为50356

实验结果:
实验数据: 2019-12-03.30  测试数据: 2019-12-04.1(全部流量/实验流量)
在全部流量上, 经筛选后的模型表现和筛选前相比并未出现明显的提升或者下降; 在实验流量上, 由于数据量较少, 也未能体现明显的改善或者下降.

考虑到按频率筛选特征的合理性, 2019-12-07更新的模型将使用feature occurrence>5的条件过滤


2. loss权重实验
考虑调整cvr, ctr 以及ctcvr loss的权重, 使得cvr学得更好.
实验结果评估中.

3. 模型结构
考虑到联合训练的目标是使得feature embedding能够学得更好, ctr端也使用和cvr端完全相同的结构
实验结果评估中, 似乎是使用相同的结构模型每天的表现要更加稳定一些.



工程:
1. 在target-cpa任务下的target-cpa-model-evaluation job下添加各model在33619973实验流量上的评价, 并且增加company name字段以方便分析
2. 去掉esmm代码项目中废弃的raw-sample数据生成部分








周三:

模型评价:
ESMM model和LR model在33619973实验流量上评估:
   ESMM model在自己的实验流量上严重高估, 而LR model在该部分流量上比较明显的低估:
   目前ESMM model是在原有的LR model投出来的流量上训练的, 而在abtest时, 使用ESMM model最终投放出来的流量可能与LR投放出来的流量完全不一样, 这就导致了ESMM model实际的测试集与其训练集分布不一致的问题
   解决方案: 加大实验流量, 积攒数据

数据
   ESMM model在实验流量上评估的COPC与abtest的成本比值不一致
   问题: 部分广告主在实验流量上的copc接近1, 但是abtest的成本比值接近2

   $$证明:成本比值\times COPC \leq 1$$


$$成本比值=\frac{CPA_{true}}{CPA_{target}}, \quad COPC=\frac{order\_number}{\sum_{i=1}^{n}cvr_i}$$
$$CPA_{true}=\frac{fee}{order\_number}\leq\frac{CPA_{target}\times\sum_{i=1}^{n}cvr_i}{order\_number}$$
$$\Longrightarrow 成本比值\leq \frac{\sum_{i=1}^{n}cvr_i\times CPA_{target}}{order\_number\times CPA_{target}}=\frac{1}{COPC}$$
$$\Longrightarrow 成本比值\times COPC \leq 1$$

原因: 投放机端抽取的DisplayType包含文字链类型: 26, 而从session-log抽取的训练集没有26这个特征, 文字链被划分为0(UNKNOWN). 在线上测试的时候, 由于在模型中找不到DisplayType=26特征的权重, 默认为0, 而实际上DisplayType=0的权重为负值(-1.098), 这就导致了部分投放文字链较多的广告主高估比较严重.

处理: 通知投放机端修改session-log的thrift字段后, 训练数据会同时混入DisplayType=0和DisplayType=26两种特征, 需将0统一替换为26






实验:
1. feature occurrence filter实验
对于出现频率小于一定值的feature进行过滤, 可以一定程度上避免稀疏特征对于数据量较小的广告主的影响.

以12-03.30的训练数据为例, 未筛选前不同feature值的个数是85834, 若仅保留feature occurrence>=10的特征值, feature值个数降为50356,





工程:
1. 在target-cpa任务下的target-cpa-model-evaluation job下添加各model在33619973实验流量上的评价, 并且增加company name字段以方便分析
2. 去掉esmm代码项目中废弃的raw-sample数据生成部分



2019年度总结

个人业绩:
1. Tree-based Deep-learning Model的模型优化、线上测试和模型评估：
    业绩指标: tdm召回 50%上线
    具体包括:
     1.1 模型特征分析(特征增减实验)、新特征的测试(CookieFreq, mvId, banner memo等);
     1.2 权重添加及优化： 为解决正负例不均衡问题, 添加loss权重, 并且实现依据fee为正例增添相应的点击权重， 提升tdm的线上rpm等收入指标;
     1.3 模型评价:  添加模型隔天评价任务; 探究TOP K 的K对于模型指标的影响; leaf embedding可视化评估聚类效果等.

2. ESMM 模型线下测试、线上流程搭建、模型评估等：
    业绩指标: Esmm Model 10% 上线, 实验流量上花费覆盖提升10%左右
    具体包括:
    2.1  ESMM 模型线上流程搭建:  包括数据生成等
    2.2  ESMM模型线下优化测试:  包括训练模式(分公司/分广告主), 超参调整、正则优化、特征分析、结构调整、添加duration预测任务等
    2.3  ESMM模型日常评估

个人成长:

   1. 知识储备:
      通过相关书籍的阅读以及日常一些交流学习会, 对于计算广告, 推荐系统有了更加深入的理解.

   2. 工程能力:
      在师傅王华呈的指导下, 通过参与tdm项目, 较为深入地学习了深度学习模型从线下测试到线上workflow搭建的完整流程, 为后期的ESMM model从数据生成, 深度学习模型代码实现, 到azkaban workflow搭建等打下坚实基础.

  2. 思维模式:
      Tdm项目的参与以及ESMM model的优化与上线让自己深刻认识到以下几点:
      2.1 算法要与业务情境相结合: 很长一段时间自己完全抛开业务只考虑模型本身的优化, 后来发现脱离业务后很多工作都容易跑偏. 模型固然重要, 更重要的是要结合具体的业务情境去分析
      2.2 注重细节, 不放过任何一个可疑点: 优化点往往就存在于这些容易被忽略的小问题中, 不要想当然.
      2.3 数据极其重要, 可以说数据决定了模型的上限.

   3. 沟通合作能力:
      刚开始的时候习惯自己闷头工作, 工位离其他同事也比较远, 有问题就喜欢憋着, 但是慢慢发觉这样会导致工作效率低下, 自己也憋得难受. 后期就有意识的鼓励自己多说话, 及时反馈, 但是在加强交流这一点上自己仍然做的不够好, 有待继续改善.

自我评价:
    整体来说工作较为认真,  但应加强交流, 及时反馈并且加深对于业务的理解, 注重细节.


下一阶段目标：
1. ESMM模型后续优化以及维护，提升各广告主ocpc投放计划成本比值的稳定性
2. 参与tdm用于ocpc流量召回的项目
