# Normalization的作用及各种方法的不同

**参考文献**

详解深度学习中的Normalization，BN/LN/WN https://zhuanlan.zhihu.com/p/33173246

-----------------------------分割线------------------------------------

Transformer中为何不使用Batch Norm而使用Layer Norm？每个Batch中存在不同的句子，这些句子不一定在同一段中，也不一定在同一篇文章中，
直观上思考，这些句子本就处于不同的分布上，强行拉在同一分布中则会损失不同句子之前的差异信息。在我们之前的讨论中，Batch Norm依赖于Mini-Batch的一阶和二阶统计量（期望和方差）
故适合用于每个Mini-Batch与整体数据处于相同分布的情况下。
而已有文章[Rethinking Batch Normalization in Transformers](https://arxiv.org/pdf/2003.07845.pdf)证明NLP中使用Batch Norm的均值和方差一直振荡，
导致其提供的梯度也具有不稳定性，模型难以收敛。
同样还有一篇文章[Understanding and Improving Layer Normalization](https://papers.nips.cc/paper/2019/file/2f4fe03d77724a7217006e5d16728874-Paper.pdf)发现不做Rescale反而会有效果提升，
而Layer Norm起作用的原因在于既使得输入的分布更加稳定，同时使后向的梯度也更加稳定，二者共同产生Layer Norm的优化效果
