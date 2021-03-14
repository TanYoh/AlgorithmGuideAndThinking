# Word2vec原理，有哪两种模型，各自有什么优势？

原理：word2vec通过学习文本然后用词向量表示语义信息，然后使得相似语义的单词在嵌入空间中距离很近。

（word embedding，similarity）

两种模型：

**skip-gram**：给定单词求上下文

优点：处理少量词效果很好，可以很好的表示低频词

缺点：速度慢 O(cv)

**cbow**：给定上下文求单词

优点：学习快，高频词表达好





# Word2vec的优化

核心思想：限制输出单元的数量（Limit）

两种方法：

**分层softmax**：Huffman Tree

(HLD)

a.构造Huffman树

b.最大化对数似然函数.

E = 从根节点到单词w的路径上，第j+1个节点是第j个节点的左子节点则为1否则为-1 X 从根节点到单词w的路径上第j个节点的向量表达 X 隐藏层h

即沿着Huffman Tree 找到对应的词，每到一个节点就取log sigmoid(E)，然后连续相加

c.对每层每个变量求偏导

时间复杂度 O(V)  -> O(logV)

**负采样**：利用负采样后的输出分布来模拟真实分布

可以从所有负向单元中随机采样一批负向单元，仅仅利用这批负向单元来更新。这称作负采样。

a.统计每个词的出现频率，丢弃低频词（Low Frequency）

b.每次选择softmax负采样时，从丢弃的词库里进行选择





# Word2Vec的缺点

a.没有考虑词序(Sequence)

b.对于中文依赖于分词的好坏 (Chinese TAG)

c.新生词无法友好的处理(New)

d.没有正则化处理(Regular)



# Word2Vec超参数

size = 300   *# 词向量取300维*
min_count = 40   *# 词频小于40个单词就去掉*
workers = 4     *# 并行运行的线程数*
window = 10      *# 上下文滑动窗口的大小*
model_ = 0       *# 使用CBOW模型进行训练*



# Word2Vec结果评估

- 通过 `kmeans` 聚类，查看聚类的簇分布。
- 通过词向量计算单词之间的相似度，查看相似词。
- 通过类比来查看类比词：`a` 之于 `b`，等价于 `c` 之于 `d` 。
- 使用 `tsne` 降维可视化查看词的分布





# Word2Vec 6 种计算句子相似度的方法

- 无监督方法：

  - 对句子中所有的词的词向量求平均，获得`sentence embedding` 。

  - 对句子中所有的词的词向量加权平均，每个词的权重为 `tf-idf` ，获得`sentence embedding` 。

  - 对句子中所有的词的词向量加权平均，每个词的权重为 `smooth inverse frequency:SIF` ；然后考虑所有的句子，并执行主成分分析；最后对每个句子的词向量加权平均减去`first principal componet`，获得`sentence embedding` 。

    `SIF` 定义为： ，其中 是一个超参数（通常取值为 0.001）， 为数据集中单词 的词频。

  - 通过 `Word Mover's Distance:WMD` ，直接度量句子之间的相似度。

    `WMD` 使用两个句子中单词的词向量来衡量一个句子中的单词需要在语义空间中`移动`到另一个句子中的单词的最小距离。

- 有监督方法：

  - 通过分类任务来训练一个文本分类器，取最后一个 `hidden layer` 的输出作为 `sentence embedding`。

    其实这就是使用文本分类器的前几层作为 `encoder` 。

  - 直接训练一对句子的相似性，其优点是可以直接得到 `sentence embeding` 。

最终结论是：简单加权的词向量平均已经可以作为一个较好的 `baseline` 。



#  Word2vec和glove区别

- word2vec是基于邻近词共现，glove是基于全文共现
  - word2vec利用了负采样或者层次softmax加速，相对更快
  - glove用了全局共现矩阵，更占内存资源
- word2vec是“predictive”的模型，而GloVe是“count-based”的模型



# Item2vec原理

受到word2vec的启发，可以将用户在一个session内浏览的商品集合作为一个句子，每个商品作为一个word，出现在同一个集合内的商品对视为正类

类似 `word2vec`，采用负采样和降采样，使用 `SGD` 学习模型则得到每个商品的`embedding` 。一旦得到了商品的 `embedding`，两两计算 `cosine` 即可得到 `item` 之间的相似度。



#  怎么衡量学到的embedding的好坏

a.人工判别：从item2vec得到的词向量中随机抽出一部分进行人工判别可靠性。即人工判断各维度item与标签item的相关程度，判断是否合理，序列是否相关

b.聚类：对词向量进行聚类或者可视化(t-SNE)
