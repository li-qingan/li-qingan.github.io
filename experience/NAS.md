Neural Architecture Search

# Background
深度学习在图像识别、语音识别、机器翻译等领域取得了很大的进展，部分原因在于各种创新的神经网络结构。当前的网络结构主要是通过人类专家手工设计，费时费力又容易犯错（就像编程一样）。因此，研究人员提出自动化的神经网络结构搜索方法（neural architecture search, NAS）。NAS本质上是在给定空间里搜索满足特定目标（比如性能）的神经网络结构，可以从三个角度来回顾现有的NAS研究：搜索空间、搜索策略和性能估计策略。

# 搜索空间
搜索空间实际上限定了能够表示的网络结构的范围。利用适合特定任务的网络结构的典型属性的先验知识，可以简化搜索空间和搜索策略，但是同时也引入了人类偏见，从而很难脱离人类现有知识的限制找到更创新的结构。

<font color=blue>搜索空间的选择在很大程度上决定了优化问题的难度。通过不同的抽象级别，可以实现不同程度的搜索空间压缩。</font>

## 简单的链式神经网络
其参数空间包括：
- 层数
- 每一层的操作类别，比如pooling, convolution, 或者depthwise separable convolution, dilated convolution
- 跟操作类别相关的超参，比如过滤器的数目、卷积层的核的大小和步长，全联接网络中结点数目。

## 多分支网络
将输入层i建模成前面网络层输出的函数，可以支持链式网络、residual networks, denseNets等。

## 分块优化
通过提取网络中的块模式，然后以块为粒度来建立搜索空间，从而压缩搜索空间；通过提高抽象级别，还有可以提高在不同数据集之间的可迁移性。

# 搜索策略
搜索策略即搜索方法。因为通常需要搜索指数级空间甚至是无界的空间，所以一方面希望尽快找到高性能的解，另一方面需要避免过早收敛到局部最优解。常用的搜索策略包括随机搜索、贝叶斯优化、演化算法、强化学习（Reinforcement Learning, RL）、基于梯度的方法等。


# 性能估计策略
NAS的目标通常是找到在新数据集上预测效果好的网络结构。最简单的性能估计是在数据集上进行标准的训练-测试评估，但是计算成本极为昂贵，从而大大限制了可以估计的网络结构数目。所以目标是提高估计性能。

## lower fidelity方法
通过使用更少的epoch，更少的数据集、简化的网络模型等，来降低训练时间。
## learning curve extrapolation
只跑一部分训练，然后根据学习曲线的趋势通过外插值得到性能估计。
## Weight inheritance/Network Morphism
对新的模型，继承已训练好的别的相关模型参数，开始训练，而不是从零开始。
## One-Shot MOdels/Weight sharing
将所有的网络结构视为同一个超图（one-shot model）的子图；训练该超图，然后在子图训练时共享权重。

# 下一步方向

- NAS的应用范围从目前的图像分类，推广到更多领域，同时也能提供更多的可行解。
- <font color=blue>多目标NAS。除了性能目标，还有资源受限目标等。该领域跟网络压缩（network compression）紧密相关。</font>
- 不同的NAS方法之间的比较和可重现面临很大挑战。一方面时间成本高，另一方面性能评估受网络结构以外的太多因素影响。
- NAS仍然不能针对为什么某个网络结构表现优秀提供很好的解释。

# Reference
Elsken, Thomas, Jan Hendrik Metzen, and Frank Hutter. "Neural Architecture Search: A Survey." Journal of Machine Learning Research 20.55 (2019): 1-21.