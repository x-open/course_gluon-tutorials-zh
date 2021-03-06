# 子词嵌入（fastText）

英语单词通常有其内部结构和形成方式。例如我们可以从“dog”、“dogs”和“dog-catcher”的字面上推测他们的关系，它们都有同一个词根dog，但使用不同的后缀来改变词的意思。而且这个关联有着一定的推广性，例如“dog”和“dogs”的关系如同“cat”和“cats”，“dog”和“dog-catcher”的关系如同“dish”和“dishwasher”。这一特点不是英语独有，法语和西班牙语中，很多动词根据场景不同有40多种不同的形态，而在芬兰语中，一个名词可能有15种以上的形态。事实上，语言学的一个重要分支，构词学（morphology），正是研究词单词的内部结构和其形成方式。

在word2vec中，我们并没有利用构词学中的信息。每次形态不同的单词使用不同的向量来表示，例如表示“dog”和“dogs”的向量之间没有明显的关系。fastText [1] 的提出是为了将构词信息引入到word2vec中的跳字模型中。在fastText中，每个中心词被表示成子词（subword）的集合。下面我们用单词“where”作为例子来看子词是如何生成的。首先，我们在单词的首尾分别添加特殊字符“&lt;”和“&gt;”来将作为前后缀的子词区分出来。然后将其当成一个由字符构成的序列来提取n-gram。例如当$n=3$时，我们得到所有长度为3的子词：

$$\textrm{<wh}, \ \textrm{whe}, \ \textrm{her}, \ \textrm{ere}, \ \textrm{re>}.$$

全长的子词$\textrm{<where>}$称之为特殊子词。

一般来说，对于一个词$w$，我们将它所有长度在3到6的子词和特殊子词的并集记为$\mathcal{G}_w$。那么词典则是所有词的子词集合的并集。假设词典中子词$g$的向量为$\boldsymbol{z}_g$，那么跳字模型中词$w$的作为中心词的向量$\boldsymbol{v}_w$则表示成

$$\boldsymbol{v}_w = \sum_{g\in\mathcal{G}_w} \boldsymbol{z}_g$$

fastText其余的部分同跳字模型一致，不在此复述。可以看到，同跳字模型相比，fastText中词典规模更大，造成模型参数更多，同时一个词的向量需要对所有子词向量求和，继而导致计算复杂度更高。但与此同时，出现次数较小的复杂单词，或者词典没出现过的单词，可能会从同它结构类似的其它词那里获取有用信息。

## 小结

- fastText在word2vec中的跳字模型的基础上，将中心词向量表示成单词的子词向量之和。从而利用构词上的规律来提升生僻词的表示质量。

## 练习

- 子词过多（例如，6字英文组合数约为$3\times 10^8$）会有什么问题？你有什么办法来解决它吗？提示：可参考fastText论文3.2节末尾 [1]。

## 扫码直达[讨论区](https://discuss.gluon.ai/t/topic/8057)

![](../img/qr_fasttext.svg)




## 参考文献

[1] Bojanowski, P., Grave, E., Joulin, A., & Mikolov, T. (2016). Enriching word vectors with subword information. arXiv preprint arXiv:1607.04606.
