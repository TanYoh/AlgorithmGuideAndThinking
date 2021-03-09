# LSTM和GRU的原理

LSTM由**输入门，输出门，遗忘门**和一个cell构成，第一步是决定从cell中丢弃什么信息，第二步是决定有多少信息进入cell状态中，最终基于目前的状态输出什么样的信息。

![img](https://uploadfiles.nowcoder.com/images/20190318/311436_1552883932369_C100C0499F22CFB35E51EEE95A8DA9BD)

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9qdWx5ZWR1LWltZy5vc3MtY24tYmVpamluZy5hbGl5dW5jcy5jb20vcXVlc2Jhc2U2NDE1NTY5ODE1MTk4MzgyMDI3NC5qcGc?x-oss-process=image/format,png)

GRU由**重置门**和**更新门**构成，其输入为前一时刻隐藏层的输出和当前的输入，输出为下一时刻隐藏层的信息。重置门用来计算**候选隐藏层**的输出，其作用是控制多少当前隐藏层的信息，而更新门的作用是控制多少候选隐藏门的输出信息，从而得到当前隐藏层的输出





# LSTM和GRU的区别

1）GRU和LSTM的性能在很多任务上不分伯仲
2）GRU 参数更少因此更容易收敛，但是数据集很大的情况下，LSTM表达性能更好
3）从结构上来说，GRU只有两个门（update和reset），LSTM有三个门（forget，input，output），GRU直接将hidden state 传给下一个单元，而LSTM则用memory cell 把hidden state 包装起来





# LSTM和RNN的区别

RNN在处理长期依赖（时间序列上距离较远的节点）时会遇到巨大的困难，因为计算距离较远的节点之间的联系时会涉及雅可比矩阵的多次相乘，这会带来梯度消失（经常发生）或者梯度膨胀（较少发生）的问题。

1.LSTM是门限RNN，通过增加输入门限，遗忘门限和输出门限，使得自循环的权重是变化的。参数固定的情况下，不同时刻的积分尺度可以动态改变，从而避免了梯度消失或者梯度膨胀的问题。

2.LSTM结构更为复杂，在RNN中，将过去的输出和当前的输入concatenate到一起，通过tanh来控制两者的输出，它只考虑最近时刻的状态。在RNN中有两个输入和一个输出。

3.LSTM有细胞状态，LSTM为了能记住长期的状态，在RNN的基础上增加了一路输入和一路输出，增加的这一路就是细胞状态。



# LSTM是如何解决长程依赖问题的

与简单的RNN网络模型比，LSTMs不是仅仅依靠快速变化的hidden state的信息去产生预测，而是还去考虑记忆细胞C中的信息。



# LSTM如何解决梯度消失

![img](https://img2020.cnblogs.com/blog/1244340/202007/1244340-20200716001949682-1128828675.png)

损失函数对每个时间步的梯度都是一样的，然后根据链式求导发展，每个时间步继续对输入门里的参数求梯度，结果可想而知，每个时间步下的输入门的梯度都比较大，所以就没有简单RNN梯度消失的问题。遗忘门和输出门同理。





# 遗忘门对梯度消失的影响

在上一节我们假设了遗忘门的输出为1。如果遗忘门的输出为1，也就是不对记忆细胞C进行遗忘。
如果遗忘门的输出小于1，那么，越早的时间步的梯度就会越小，可能出现梯度消失。



# 遗忘门初始化技巧

一般而言为了避免训练LSTM因梯度消失而训练缓慢的问题，可以将输入门的偏置初始化大一些，这样就可以保证遗忘门的输出接近于1，就可以高效地训练网络了。





参考 https://www.nowcoder.com/ta/review-ml/review?tpId=96&tqId=32602&query=LSTM&asc=true&order=&page=4

https://blog.csdn.net/see_you_yu/article/details/106585496

https://www.cnblogs.com/itmorn/p/13303155.html

https://www.cnblogs.com/itmorn/p/13311342.html#ct1
