[toc]



# 一、Transformer

**关键词:** **encoder, decoder, self-attention(multi-head self-attention), position encoding, Residuals, layer normalization, feed-forward**

## 1.1 传统递归网络的问题

​		处理Seq2seq最常用的就是RNN。RNN的计算限制为是顺序的, 无法Parallel(并行处理)。

**问题:**

1. 时间片 t的计算依赖 t-1时刻的计算结果，这样限制了模型的并行能力。
2. 顺序计算的过程中信息会丢失，尽管LSTM等门机制的结构一定程度上缓解了长期依赖的问题，但是对于特别长期的依赖现象,LSTM依旧无能为力。

**解决方案:**

​		解决难以RNN的难以平行化计算问题的办法是利用Transformer(Self-attention)替换RNN。

1. 首先它使用了Attention机制，将序列中的任意两个位置之间的距离是缩小为一个常量。

2. 其次它不是类似RNN的顺序结构，因此具有更好的并行性，符合现有的GPU框架。

​		论文中给出Transformer的定义是：Transformer is the first transduction model relying entirely on self-attention to compute representations of its input and output without using sequence aligned RNNs or convolution。

**缺点:**

1. 实践上：有些RNN轻易可以解决的问题transformer没做到，比如**复制string**，或者推理时碰到的sequence长度比训练时更长（因为碰到了没见过的position embedding）。

2. 理论上：transformers不是computationally universal(图灵完备)，这种非RNN式的模型是非图灵完备的的，**无法单独完成NLP中推理、决策等计算问题**（包括使用transformer的bert模型等等）。

## 1.2 Transformer整体结构

​		编码组件是一系列编码器的堆叠（文章中是6个编码器的堆叠）, 解码部分也是同样的堆叠数。

<img src="/Users/zhengxinzhi/typora_img/BERT及ERNIE学习/The_transformer_encoder_decoder_stack.png" alt="img" style="zoom:45%;" />



​		编码器在结构上都是一样的（但是它们不共享权重）。每个都可以分解成两个子模块：

<img src="/Users/zhengxinzhi/typora_img/BERT及ERNIE学习/Transformer_decoder.png" alt="img" style="zoom:67%;" />

​		编码器的输入首先流经self-attention层，该层有助于编码器对特定单词编码时查看输入序列的其他单词。

​		Self-attention层的输出被送入前馈神经网络。完全相同的前馈神经网络独立应用在每个位置。

​		解码器也具有这两层，但是这两层中间还插入了attention层，能帮助解码器注意输入句子的相关部分（和[seq2seq模型](https://link.zhihu.com/?target=https%3A//jalammar.github.io/visualizing-neural-machine-translation-mechanics-of-seq2seq-models-with-attention/)的attention相同）

## 1.3 Transformer数据流动

​	

<img src="/Users/zhengxinzhi/typora_img/BERT及ERNIE学习/17.png" alt="Transformer网络结构示意图"  />



**(1) 构建输入**

​		输入单词转成word embedding和position encoding相加作为输入, 维度512。

**(2) Encoder部分**

经过6次encoder, 每个encoder输入输出维度512, 全连接隐层维度2048

- multi-head self-attention层, 
- feed-forward层, 对输出做 residual connect + layer normalization

**(3) Decoder部分**

经过6次decoder, 每个decoder输入输出维度512, 全连接隐层维度2048

- masked multi-head self-attention层(masked代表输入来源于上一时间的输出), 对输出做 residual connect + layer normalization
- encoder-decoder attention, decoder的V与之前encoder部分的K和V去做attention, 之后对输出做 residual connect + layer normalization
- feed-forward层, 对输出做 residual connect + layer normalization



​		**区别 **Decoder与Encoder中的self-attention层有区别, decoder中仅允许 self-attention层关注输出序列中较早的位置。这是通过在计算self-attention中softmax步骤前屏蔽未来位置（将它们设置为-inf）实现的。

​		Transformer一个重要特性: 每个位置的单词在经过编码器时流经自己的路径。self-attention层中这些路径之间有依赖关系。然而前馈层并不具有这些依赖关系，所以各种路径在流经前馈层时可以并行执行。

## 1.4 Self-Attention细节

**(1) 需要从每个输入词向量中创建三个向量, Query, Key, Value**

- `q`用于match其它输出
- `k`用于被match
- `v`是抽取出来的信息。

​		**注意**这些新创建的向量的维度小于词嵌入向量(embedding vector)。它们（新创建的向量）的维度是64，而词嵌入和编码器的输入输出向量的维度是512。它们不必更小，这是一种架构选择，可以使多头注意力(multiheaded attention)计算不变。

<img src="/Users/zhengxinzhi/typora_img/BERT及ERNIE学习/3.png" alt="q、k、v生成示意图" style="zoom:67%;" />

**(2) 计算得分(score 权重)**

​		当我们在某个位置编码单词时，分数决定了对输入句子的其他部分放置多少的焦点(注意力)。

<img src="/Users/zhengxinzhi/typora_img/BERT及ERNIE学习/4.png" alt="Scaled Dot-Product Attention示意图" style="zoom:67%;" />

<img src="/Users/zhengxinzhi/typora_img/BERT及ERNIE学习/5.png" alt="归一化" style="zoom:67%;" />

**(3) 对加权值向量求和, 这样就产生了在这个位置的self-attention的输出**

<img src="/Users/zhengxinzhi/typora_img/BERT及ERNIE学习/6.png" alt="得到最终结果" style="zoom:67%;" />

**总结**

​		这就是self-attention计算。得到的向量可以送往前馈神经网络。

​		然而在真正的实现中，计算过程通过矩阵计算来进行，以便加快计算。

![img](/Users/zhengxinzhi/typora_img/BERT及ERNIE学习/self-attention-matrix-calculation-2.png)




## 1.5 Multi-head Self-attention

<img src="/Users/zhengxinzhi/typora_img/BERT及ERNIE学习/13.png" alt="Multi-head Self-attention-1" style="zoom: 67%;" />



<img src="/Users/zhengxinzhi/typora_img/BERT及ERNIE学习/v2-a20a0fbea8eb2777d44f87b89e7fee63_hd.jpg" alt="img" style="zoom: 75%;" />

**作用**

- 它扩展了模型关注不同位置的能力。
- 它给予attention层多个“表达子空间”。

## 1.6 Positional Encoding

  Self-Attention未解决输入序列中单词顺序的问题。Transformer为每个输入的词嵌入增加了一个位置向量。这些向量遵循模型学习到的特定模式，这有助于确定每个单词的位置，或者学习到不同单词之间的距离。在原始的论文中，作者加入设定的$e^i$(不是学习出来的)来解决这个问题。相当于提供位置资讯。

​		论文中的$e^i$被直接加到了$a^i$上, 为什么不把$e^i$和$a^i$做concat呢, 由下图可知本质上就是做了位置的onthot和原始输入做了concat做一个矩阵映射得到的就是$e^i$与$a^i$的累加结果

<img src="/Users/zhengxinzhi/typora_img/BERT及ERNIE学习/15.png" alt="位置编码" style="zoom:67%;" />

​		下图中，每一行对应一个向量的位置编码, 每行包含512个值—每个值介于-1到1之间。这里我们进行了涂色，使模式可见。该例子中共20个词（行），词嵌入向量维度为512维(列)。你可以看到中心区域分成两半。这是因为左边的值是由一个函数(正弦)产生的，右边的值是由另一个函数(余弦)产生的。然后将它们连接起来形成每个位置编码向量。

<img src="/Users/zhengxinzhi/typora_img/BERT及ERNIE学习/v2-2842c0b22637515f19dc2c6b8802303a_hd.jpg" alt="img" style="zoom:33%;" />

​		位置编码的公式在文章(3.5节)有描述。你可以在 `get_timing_signal_1d()` 函数中看到用于生成位置编码的代码。这并不是生成位置编码的唯一方式。然而，它的优点在于可以扩展到看不见的序列长度（eg. 如果要翻译的句子的长度远长于训练集中最长的句子）。

## 1.7 训练过程

​		损失函数

​		beam search



## 1.8 Transformer 源码分析

### 1.8.1 Transformer源码列表

- [attention-is-all-you-need-pytorch](https://link.zhihu.com/?target=https%3A//github.com/jadore801120/attention-is-all-you-need-pytorch) （强烈推荐，读源码再结合文章，理解更全面和深入）

- [The Annotated Transformer](https://link.zhihu.com/?target=https%3A//nlp.seas.harvard.edu/2018/04/03/attention.html) (harvardnlp的代码解读)及其[修改版](https://github.com/zingp/NLP/blob/master/P006TheAnnotatedTransformer/model_transformer.ipynb) [思路](https://www.cnblogs.com/zingp/p/11696111.html) (推荐)

- [tensorflow/tensor2tensor](https://link.zhihu.com/?target=https%3A//github.com/tensorflow/tensor2tensor) (官方实现)

- [tensorflow/models/official/transformer](https://link.zhihu.com/?target=https%3A//github.com/tensorflow/models/tree/master/official/transformer) (tf-models实现)

- [Pytorch-transformer](https://github.com/huggingface/transformers) (常用工具)

  ​	我选取harvardnlp修改版来进行源码分析。



## 1.9 Universal Transformers [TODO]

[Universal Transformers](https://arxiv.org/abs/1807.03819)

[Universal Transformer](https://ai.googleblog.com/2018/08/moving-beyond-translation-with.html)

<img src="/Users/zhengxinzhi/typora_img/BERT及ERNIE学习/18.png" alt="Universal Transformer"  />

## 1.10 进一步学习

如果想更深理解的话，我建议：

- 阅读文章 [Attention Is All You Need](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/1706.03762) ，和Transformer的官方博文： ([Transformer: A Novel Neural Network Architecture for Language Understanding](https://link.zhihu.com/?target=https%3A//ai.googleblog.com/2017/08/transformer-novel-neural-network.html)), 和 [Tensor2Tensor announcement](https://link.zhihu.com/?target=https%3A//ai.googleblog.com/2017/06/accelerating-deep-learning-research.html)。
- 看 [Łukasz Kaiser’s 的讲解视频](https://link.zhihu.com/?target=https%3A//www.youtube.com/watch%3Fv%3DrBCqOTEfxvg) 深入模型细节。
- 打开 [Tensor2Tensor的Jupyter notebook](https://link.zhihu.com/?target=https%3A//colab.research.google.com/github/tensorflow/tensor2tensor/blob/master/tensor2tensor/notebooks/hello_t2t.ipynb) 来详细了解。
- 探索 [Tensor2Tensor 代码仓库](https://link.zhihu.com/?target=https%3A//github.com/tensorflow/tensor2tensor)。

以及相关工作：

- [Depthwise Separable Convolutions for Neural Machine Translation](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/1706.03059)
- [One Model To Learn Them All](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/1706.05137)
- [Discrete Autoencoders for Sequence Models](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/1801.09797)
- [Generating Wikipedia by Summarizing Long Sequences](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/1801.10198)
- [Image Transformer](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/1802.05751)
- [Training Tips for the Transformer Model](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/1804.00247)
- [Self-Attention with Relative Position Representations](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/1803.02155)
- [Fast Decoding in Sequence Models using Discrete Latent Variables](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/1803.03382)
- [Adafactor: Adaptive Learning Rates with Sublinear Memory Cost](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/1804.04235)

# 二、预训练语言模型

![图1 NLP Pre-training and Fine-tuning新范式及相关扩展工作](/Users/zhengxinzhi/typora_img/BERT及ERNIE学习/a67aa6b0b1fb153de0051e7eb3e8c632130233.png)

![image-20191207142614102](/Users/zhengxinzhi/typora_img/BERT及ERNIE学习/image-20191207142614102.png)



## 2.1 Feature-based方法

​		广泛采用的**单词表征学习**，已经是数十年的活跃研究领域，包括非神经网络和神经网络的算法, 例如ELMo。

## 2.2 Fine-tuning方法

​		一种源于语言模型(LMs)的迁移学习新趋势，是微调前预训练一些LM目标上的模型架构, 例如GPT, BERT。

# 三、ELMO [TODO]

## 3.1 ELMO思想起源

<img src="/Users/zhengxinzhi/typora_img/BERT及ERNIE学习/image-20191208214521340.png" alt="image-20191208214521340" style="zoom: 33%;" />

​		尽管有不同的意思，但使用传统的word embedding的方法，相同的单词都会对应同样的embedding。但我们希望针对不同意思的bank，可以给出不同的embedding表示。

<img src="/Users/zhengxinzhi/typora_img/BERT及ERNIE学习/image-20191208214536891.png" alt="image-20191208214536891" style="zoom:33%;" />

​		根据上下文语境的不同，同一个单词bank我们希望能够得到不同的embedding，如果bank的意思是银行，我们期望它们之间的embedding能够相近，同时能够与河堤意思的bank相距较远。

​		基于这个思想，首先有了ELMO。

## 3.2 ELMO介绍(Embeddings from Language Model)

​		ELMO是Embeddings from Language Model的简称，ELMO是《芝麻街》中的一个角色。它是一个RNN-based的语言模型，其任务是学习句子中的下一个单词或者前一个单词是什么。

<img src="/Users/zhengxinzhi/typora_img/BERT及ERNIE学习/image-20191208214756647.png" alt="image-20191208214756647" style="zoom:33%;" />

​		它是一个双向的RNN网络，这样每一个单词都对应两个hidden state，进行拼接便可以得到单词的Embedding表示。当同一个单词上下文不一样，得到的embedding就不同。

​		当然，我们也可以搞更多层：

<img src="/Users/zhengxinzhi/typora_img/BERT及ERNIE学习/image-20191208214842994.png" alt="image-20191208214842994" style="zoom: 33%;" />

​		这么多层的RNN，内部每一层输出都是单词的一个表示，那我们取哪一层的输出来代表单词的embedding呢？在ELMO中，一个单词会得到多个embedding，对不同的embedding进行加权求和，可以得到最后的embedding用于下游任务。要说明一个这里的embedding个数，下图中只画了两层RNN输出的hidden state，其实输入到RNN的原始embedding也是需要的，所以你会看到说右下角的图片中，包含了三个embedding。

​		但不同的权重是基于下游任务学习出来的，上图中右下角给了5个不同的任务，其得到的embedding权重各不相同。

<img src="/Users/zhengxinzhi/typora_img/BERT及ERNIE学习/image-20191208214923200.png" alt="image-20191208214923200" style="zoom:33%;" />

# 四、GPT [TODO]

## 4.1 OpenAI GPT

## 4.2 GPT-2

# 五、BERT

## 5.1 BERT介绍

​		Bert是Bidirectional Encoder Representations from Transformers的缩写，它也是芝麻街的人物之一。即原理是Transformer的双向编码表示来改进基于架构微调的方法。Transformer中的Encoder就是Bert预训练的架构。如果是中文的话，可以把字作为单位，而不是词。

​		不同于最近的预训练模型，BERT旨在基于所有层的左、右语境来预训练深度双向表征。因此，预训练的BERT表征可以仅用一个额外的输出层进行微调，进而为很多任务(如问答和语言推理)创建当前最优模型，无需对任务特定架构做出大量修改。

​		BERT的刷新了11个NLP任务的当前最优结果，包括将GLUE基准提升至80.4%(7.6%的绝对改进)、将MultiNLI的准确率提高到86.7%(5.6%的绝对改进)，以及将SQuADv1.1问答测试F1的得分提高至93.2分(1.5分绝对提高)——比人类性能还高出2.0分。

​		BERT论文贡献:

- 我们证明了双向预训练对**语言表征量**的重要性

- 我们展示了预训练表征量能消除许多重型工程**任务特定架构**的需求。BERT是第一个基于微调的表征模型，它在大量的句子级和词块级任务上实现了最先进的性能，优于许多具有任务特定架构的系统。(fine-tuning)

- BERT推进了11项NLP任务的最高水平。

## 5.2 BERT架构

<img src="/Users/zhengxinzhi/typora_img/BERT及ERNIE学习/image-20191209153550527.png" alt="image-20191209153550527" style="zoom:50%;" />

​		BERT框架有两步, pre-training和fine-tuning, 除输出层外, 这两个步骤架构是统一的。

<img src="/Users/zhengxinzhi/typora_img/BERT及ERNIE学习/890083d95e1dc89888296e0991437dc7646898.png" alt="图2 BERT及Transformer网络结构示意图" style="zoom: 25%;" />

​		BERT模型基于多层双向Transformer Encoder实现。

<img src="/Users/zhengxinzhi/typora_img/BERT及ERNIE学习/image-20191209153611754.png" alt="image-20191209153611754" style="zoom:50%;" />

​		BERT的input embedding有token embedding + segment embedding + position embedding

- 对于英文模型，使用了Wordpiece模型来产生Subword从而减小词表规模；对于中文模型，直接训练基于字的模型。
- 模型输入需要附加一个起始Token，记为[CLS]，对应最终的Hidden State（即Transformer的输出）可以用来表征整个句子，用于下游的分类任务。
- 模型能够处理句间关系。为区别两个句子，用一个特殊标记符[SEP]进行分隔，另外针对不同的句子，将学习到的Segment Embeddings 加到每个Token的Embedding上。
- 对于单句输入，只有一种Segment Embedding；对于句对输入，会有两种Segment Embedding。

<img src="/Users/zhengxinzhi/typora_img/BERT及ERNIE学习/image-20191209180912464.png" alt="image-20191209180912464" style="zoom:50%;" />

​		$BERT_{large}$的参数是层数(L=24)隐层结点数(H=1024)和self-attention头的个数(A=16), 总参数340M。(深而窄)

## 5.3 Pre-training

​		BERT预训练过程包含两个不同的预训练任务，分别是Masked Language Model和Next Sentence Prediction任务。

​		Masked LM，通过随机掩盖一些词（替换为统一标记符[MASK]），然后预测这些被遮盖的词来训练双向语言模型，并且使每个词的表征参考上下文信息。NSP是预测下一个句子，这里，先把两句话连起来，中间加一个[SEP]作为两个句子的分隔符。而在两个句子的开头，放一个[CLS]标志符，将其得到的embedding输入到二分类的模型，输出两个句子是不是接在一起的。

​		实际中，同时使用两种方法往往得到的结果最好。

​		如果是分类任务[CLS]，在句子前面加一个标志，将其经过Bert得到的embedding输出到二分类模型中，得到分类结果。二分类模型从头开始学，而Bert在预训练的基础上进行微调（fine-tuning）。

### Task 1: Masked LM

<img src="/Users/zhengxinzhi/typora_img/BERT及ERNIE学习/image-20191208215140618.png" alt="image-20191208215140618" style="zoom:33%;" />

​		从直觉上看，研究团队有理由相信，深度双向模型比left-to-right 模型或left-to-right and right-to-left模型的浅层连接更强大。遗憾的是，标准条件语言模型只能从左到右或从右到左进行训练，因为双向条件作用将允许每个单词在多层上下文中间接地“see itself”。

​		为了训练一个深度双向表示（deep bidirectional representation），研究团队采用了一种简单的方法，即随机屏蔽（masking）部分输入token，然后只预测那些被屏蔽的token。

​		在这个例子中，与masked token对应的最终隐藏向量被输入到词汇表上的输出softmax中，就像在标准LM中一样。在团队所有实验中，随机地屏蔽了每个序列中15%的WordPiece token。

​		虽然这确实能让团队获得双向预训练模型，但这种方法有两个缺点。首先，预训练和finetuning之间不匹配，因为在finetuning期间从未看到[MASK]token。为了解决这个问题，团队并不总是用实际的[MASK]token替换被“masked”的词汇。

​		数据生成器将执行以下操作，而不是始终用[MASK]替换所选单词：

- 80％的时间：用[MASK]标记替换单词，例如，my dog is hairy → my dog is [MASK]
- 10％的时间：用一个随机的单词替换该单词，例如，my dog is hairy → my dog is apple
- 10％的时间：保持单词不变，例如，my dog is hairy → my dog is hairy. 这样做的目的是将表示偏向于实际观察到的单词。

问题1: 会造成预训练和微调时的不一致，因为在微调时[MASK]总是不可见的

​		答:把80%需要被替换成[MASK]的词进行替换，10%的随机替换为其他词，10%保留原词。由于Transformer Encoder并不知道哪个词需要被预测，哪个词是被随机替换的，这样就强迫每个词的表达需要参照上下文信息。

问题2: 由于每个Batch中只有15%的词会被预测，因此模型的收敛速度比起单向的语言模型会慢，训练花费的时间会更长。

​		答: 目前没有有效的解决办法，但是从提升收益的角度来看，付出的代价是值得的。

### Task2: Next Sentence Prediction

<img src="/Users/zhengxinzhi/typora_img/BERT及ERNIE学习/image-20191208215158836.png" alt="image-20191208215158836" style="zoom:33%;" />

​		许多重要的下游任务，如问答（QA）和自然语言推理（NLI）都是基于理解两个句子之间的关系，这并没有通过语言建模直接获得。

​		在为了训练一个理解句子的模型关系，预先训练一个二进制化的下一句测任务，这一任务可以从任何单语语料库中生成。具体地说，当选择句子A和B作为预训练样本时，B有50％的可能是A的下一个句子，也有50％的可能是来自语料库的随机句子(单一预料)。

​		团队完全随机地选择了NotNext语句，最终的预训练模型在此任务上实现了97％-98％的准确率。

### 预训练过程

​		BERT预训练过程主要遵循现有的语言模型预训练文献。对于预训练语料库,我们使用 Books Corpus(800M单词)和英语维基百科(2,500M单词)的串联。对于维基百科, 我们只提取文本段落并忽略列表、表格和题头。至关重要的是, 使用文档级语料库而不是洗牌式(乱词序)句子级语料库, 例如 Bilion word Benchmark, 以便提取长的连续序列。
​		为了生成每个训练输入序列, 我们从语料库中采样两个文本跨度, 我们将其称为句子", 即使它们通常比单个句子长得多(但也可以更短)。第一个句子接收A嵌入, 第二个句子接收B嵌入。B有50%可能刚好是A嵌入后的下一个句子,亦有50%可能是个随机句子, 此乃为下一句预测任务而做。对它们采样, 使其组合长度≤512个词块。该LM遮蔽应用于具有15%统一掩蔽率的 Wordpiece词块化之后, 并且不特别考虑部分字块。
​		我们训练批量大小为256个序列(256个序列*512个词块=128,000个词块/批次), 持续1,000,000个step, 相比于33亿个单词语料库, 有大约40个周期。我们使用Adam(学习程序), 设其learning rate=1e4, β1=0.9, β2=0.999, L2权重衰减为0.01, 学习率预热超过前10,000步以上以及线性衰减该学习率。我们在所有层上使用0.1的丢失概率。在 OpenAIGPT之后,我们使用gelu激活而不是标准reu。训练损失是平均的遮蔽LM可能性和平均的下一句子预测可能性的总和。

​		在Pod配置的4个云TPU上进行了$BERT_{BASE}$训练(总共16个TPU芯片)。在16个云TPU(总共64个TPU芯片)进行了$BERT_{LARGE}$训练。每次预训练需4天完成。

## 5.4 Fine-tuning

​		对于序列级分类任务，BERT微调很简单。为了获得输入序列的固定维度池化表征，我们对该输入第一个词块[CLS]词嵌入C的隐藏状态做处理。微调期间添加的唯一新参数是分类层向量W。该标签概率P用标准softmax函数，P=softmax(CWT)计算。BERT和W的所有参数都经过联动地微调，以最大化正确标签的对数概率。对于跨度级和词块级预测任务，必须以任务特定方式稍微修改上述过程。

　　对于微调，大多数模型超参数与预训练相同，但批量大小、学习率和训练周期数量除外。丢失概率始终保持在0.1。最佳超参数值是特定于任务的，但我们发现以下范围的可能值可以在所有任务中很好地工作：

- **批量大小**：16,32
- **学习率**(Adam)：5e-5,3e-5,2e-5
- **周期数量**：3,4

　　我们还观察到，大数据集(如100k+词块的训练样例)对超参数选择的敏感性远小于小数据集。微调通常非常快，因此需合理简单地对上述参数进行详尽搜索，并选择开发集上性能最佳的模型。

## 5.5 WordPiece原理(不适用中文)

​		现在基本性能好一些的NLP模型，例如OpenAI GPT，google的BERT，在数据预处理的时候都会有WordPiece的过程。WordPiece字面理解是把word拆成piece一片一片，其实就是这个意思。

​		WordPiece的一种主要的实现方式叫做BPE（Byte-Pair Encoding）双字节编码。

​		BPE的过程可以理解为把一个单词再拆分，使得我们的此表会变得精简，并且寓意更加清晰。

​		比如"loved","loving","loves"这三个单词。其实本身的语义都是“爱”的意思，但是如果我们以单词为单位，那它们就算不一样的词，在英语中不同后缀的词非常的多，就会使得词表变的很大，训练速度变慢，训练的效果也不是太好。

​		BPE算法通过训练，能够把上面的3个单词拆分成"lov","ed","ing","es"几部分，这样可以把词的本身的意思和时态分开，有效的减少了词表的数量。

​		BPE的大概训练过程：首先将词分成一个一个的字符，然后在词的范围内统计字符对出现的次数，每次将次数最多的字符对保存起来，直到循环次数结束。

​		我们模拟一下BPE算法。我们原始词表如下：{'l o w e r ': 2, 'n e w e s t ': 6, 'w i d e s t ': 3, 'l o w ': 5}, 其中的key是词表的单词拆分层字母，再加代表结尾，value代表词出现的频率。

​	下面我们每一步在整张词表中找出频率最高相邻序列，并把它合并，依次循环。

```
原始词表 {'l o w e r </w>': 2, 'n e w e s t </w>': 6, 'w i d e s t </w>': 3, 'l o w </w>': 5}
出现最频繁的序列 ('s', 't') 9
合并最频繁的序列后的词表 {'n e w e st </w>': 6, 'l o w e r </w>': 2, 'w i d e st </w>': 3, 'l o w </w>': 5}
出现最频繁的序列 ('e', 'st') 9
合并最频繁的序列后的词表 {'l o w e r </w>': 2, 'l o w </w>': 5, 'w i d est </w>': 3, 'n e w est </w>': 6}
出现最频繁的序列 ('est', '</w>') 9
合并最频繁的序列后的词表 {'w i d est</w>': 3, 'l o w e r </w>': 2, 'n e w est</w>': 6, 'l o w </w>': 5}
出现最频繁的序列 ('l', 'o') 7
合并最频繁的序列后的词表 {'w i d est</w>': 3, 'lo w e r </w>': 2, 'n e w est</w>': 6, 'lo w </w>': 5}
出现最频繁的序列 ('lo', 'w') 7
合并最频繁的序列后的词表 {'w i d est</w>': 3, 'low e r </w>': 2, 'n e w est</w>': 6, 'low </w>': 5}
出现最频繁的序列 ('n', 'e') 6
合并最频繁的序列后的词表 {'w i d est</w>': 3, 'low e r </w>': 2, 'ne w est</w>': 6, 'low </w>': 5}
出现最频繁的序列 ('w', 'est</w>') 6
合并最频繁的序列后的词表 {'w i d est</w>': 3, 'low e r </w>': 2, 'ne west</w>': 6, 'low </w>': 5}
出现最频繁的序列 ('ne', 'west</w>') 6
合并最频繁的序列后的词表 {'w i d est</w>': 3, 'low e r </w>': 2, 'newest</w>': 6, 'low </w>': 5}
出现最频繁的序列 ('low', '</w>') 5
合并最频繁的序列后的词表 {'w i d est</w>': 3, 'low e r </w>': 2, 'newest</w>': 6, 'low</w>': 5}
出现最频繁的序列 ('i', 'd') 3
合并最频繁的序列后的词表 {'w id est</w>': 3, 'newest</w>': 6, 'low</w>': 5, 'low e r </w>': 2}
```

​		这样我们通过BPE得到了更加合适的词表了，这个词表可能会出现一些不是单词的组合，但是这个本身是有意义的一种形式，加速NLP的学习，提升不同词之间的语义的区分度。



## 5.6 源码分析 [TODO]

源码 https://github.com/huggingface/transformers

https://huggingface.co/transformers/index.html

## 5.7 BERT应用场景

<img src="/Users/zhengxinzhi/typora_img/BERT及ERNIE学习/image-20191208215346001.png" alt="image-20191208215346001" style="zoom:33%;" />

<img src="/Users/zhengxinzhi/typora_img/BERT及ERNIE学习/image-20191208215304715.png" alt="image-20191208215304715" style="zoom:33%;" />

自然语言推理任务，给定一个前提／假设，得到推论是否正确：

<img src="/Users/zhengxinzhi/typora_img/BERT及ERNIE学习/image-20191208215400671.png" alt="image-20191208215400671" style="zoom:33%;" />

最后一个例子是抽取式QA，抽取式的意思是输入一个原文和问题，输出两个整数start和end，代表答案在原文中的起始位置和结束位置，两个位置中间的结果就是答案。

<img src="/Users/zhengxinzhi/typora_img/BERT及ERNIE学习/image-20191208215416913.png" alt="image-20191208215416913" style="zoom:33%;" />

具体怎么解决刚才的QA问题呢？把问题 - 分隔符 - 原文输入到BERT中，每一个单词输出一个黄颜色的embedding，这里还需要学习两个（一个橙色一个蓝色）的向量，这两个向量分别与原文中每个单词对应的embedding进行点乘，经过softmax之后得到输出最高的位置。正常情况下start <= end，但如果start > end的话，说明是矛盾的case，此题无解。

<img src="/Users/zhengxinzhi/typora_img/BERT及ERNIE学习/image-20191208215445383.png" alt="image-20191208215445383" style="zoom:33%;" />

<img src="/Users/zhengxinzhi/typora_img/BERT及ERNIE学习/image-20191208215502952.png" alt="image-20191208215502952" style="zoom:33%;" />

## 5.8 实验部分

## 5.9 Bert学到什么

Bert学到了什么呢？可以看下下面两个文献（给大伙贴出来：[https://arxiv.org/abs/1905.05950](https://links.jianshu.com/go?to=https%3A%2F%2Farxiv.org%2Fabs%2F1905.05950) 和[https://openreview.net/pdf?id=SJzSgnRcKX](https://links.jianshu.com/go?to=https%3A%2F%2Fopenreview.net%2Fpdf%3Fid%3DSJzSgnRcKX)）：

<img src="/Users/zhengxinzhi/typora_img/BERT及ERNIE学习/image-20191208215617289.png" alt="image-20191208215617289" style="zoom:50%;" />

# 六、ERNIE

## 6.1 ERNIE 1.0

### 6.1.1 基本思想

​		ERINE是Enhanced Representation through Knowledge Integration缩写。

​		ERINE是在BERT基础上对中文语境做的改进, 相比于BERT其改进的地方在于对Masked的改进，百度的ERNIE通过MLM掩盖的不只是字，还有词以及实体。相较于 BERT 学习原始语言信号，ERNIE 直接对先验语义知识单元进行建模，增强了模型语义表示能力。

​		ERNIE 模型本身保持基于字特征输入建模，使得模型在应用时不需要依赖其他信息，具备更强的通用性和可扩展性。

​		ERNIE 的训练语料引入了多源数据知识。除了百科类文章建模，还对新闻资讯类、论坛对话类数据进行学习。

<img src="/Users/zhengxinzhi/typora_img/BERT及ERNIE学习/image-20191209192417190.png" alt="image-20191209192417190" style="zoom: 50%;" />

### 6.1.2 训练过程改进(Knowledge Integration)

![image-20191209192829131](/Users/zhengxinzhi/typora_img/BERT及ERNIE学习/image-20191209192829131.png)

​		论文提出multi-stage knowledge masking strategy, 按顺序训练Basic-Level Masking, Phrase-Level, Entity-level Masking等过程。

### 6.1.3 Dialogue Language Model

<img src="http://fancyerii.github.io/img/bert-imp/13.png" alt="img" style="zoom: 33%;" />

​		原始对话为3个句子：”How old are you?”、”8.”和”Where is your hometown?”。模型的输入是3个句子(而不是BERT里的两个)，中间用SEP分开，而且分别用Dialogue Embedding Q和R分别表示Query和Response的Embedding，这个Embedding类似于BERT的Segment Embedding，但是它有3个句子，因此可能出现QRQ、QRR、QQR等组合。

### 6.1.3 源码分析 [TODO]

https://zhuanlan.zhihu.com/p/76757794

Pytorch版本源码 https://github.com/thunlp/ERNIE

### 6.1.4 效果验证 [TODO]

## 6.2 ERNIE 2.0

### 6.2.1 基本思想

​		名字修改为A Continual Pre-training framework for Language Understanding。

​		作者认为之前的模型，比如BERT，只是利用词的共现这个统计信息通过训练语言模型来学习上下文相关的Word Embedding。ERNIE 2.0希望能够利用多种无监督(弱监督)的任务来学习词法的(lexical)、句法(syntactic)和语义(semantic)的信息，而不仅仅是词的共现。

​		因为引入了很多新的任务，所以作为multi-task来一起训练是非常自然的想法。但是一下就把所有的任务同时来训练可能比较难以训练(这只是我的猜测)，因此使用增量的方式会更加简单：首先训练一个task；然后增加一个新的Task一起来multi-task Learning；然后再增加一个变成3个task的multi-task Learning……

### 6.2.2 ERNIE Framework

<img src="/Users/zhengxinzhi/typora_img/BERT及ERNIE学习/image-20191209194936017.png" alt="image-20191209194936017" style="zoom: 33%;" />

​		持续的(continual)pretraining过程包括两个步骤。第一步我们通过大数据和先验知识来持续的构造无监督任务。第二步我们增量的通过multi-task learning来更新ERNIE模型。

​		对于pre-training任务，我们会构造不同类型的任务，包括词相关的(word-aware)、结构相关的(structure-aware)和语义相关的(semantic-aware)任务，分别来学习词法的、句法的和语义的信息。所有这些任务都只是依赖自监督的或者弱监督的信号，这些都可以在没有人工标注的条件下从大量数据获得。对于multi-task pre-training，ERNIE 2.0使用增量的持续学习的方式来训练。具体来说，我们首先用一个简单的任务训练一个初始的模型，然后引入新的任务来更新模型。当增加一个新的任务时，使用之前的模型参数来初始化当前模型。引入新的任务后，并不是只使用新的任务来训练，而是通过multi-task learning同时学习之前的任务和新增加的任务，这样它就既要学习新的信息同时也不能忘记老的信息。通过这种方式，ERNIE 2.0可以持续学习并且累积这个过程中学到的所有知识，从而在新的下游任务上能够得到更好的效果。

​		如上图所示，持续pre-training时不同的task都使用的是完全相同的网络结构来编码上下文的文本信息，这样就可以共享学习到的知识。我们可以使用RNN或者深层的Transformer模型(具体是用Transformer还是RNN都可以，当然更加BERT等的经验，使用Transformer会更好一些)，这些参数在所有的pre-training任务是都会更新。

![image-20191209202306245](/Users/zhengxinzhi/typora_img/BERT及ERNIE学习/image-20191209202306245.png)

​		如上图所示，我们的框架有两种损失函数。一种是序列级别的损失，它使用CLS的输出来计算；而另一种是token级别的损失，每一个token都有一个期望的输出，这样就可以用模型预测的和期望的值来计算loss。不同的pre-training task有它自己的损失函数，多个任务的损失函数会组合起来作为本次multi-task pre-training的los

### 6.2.3 Model Structure

​		模型采样和BERT类似的Transformer Encoder模型。为了让模型学习到任务特定的信息，ERNIE 2.0还引入了Task Embedding。每个Task都有一个ID，每个Task都编码成一个可以学习的向量，这样模型可以学习到与某个特定Task相关的信息。

​		网络结果如下图所示：

![image-20191209202817581](/Users/zhengxinzhi/typora_img/BERT及ERNIE学习/image-20191209202817581.png)

### 6.2.4 Pre-training Tasks

#### Word-aware Tasks

**Knowledge Masking Task**

​		这其实就是ERNIE 1.0版本的任务，包括word、phrase和entity级别的mask得到的任务。

**Capitalization Prediction Task**

​		预测一个词是否首字母大小的任务。对于英文来说，首字符大小的词往往是命名实体，所以这个任务可以学习到一些entity的知识。

**Token-Document Relation Task**

​		预测当前词是否出现在其它的Document里，一个词如果出现在多个Document里，要么它是常见的词，要么它是这两个Document共享的主题的词。这个任务能够让它学习多个Document的共同主题。

#### Structure-aware Tasks

**Sentence Reordering Task**

​		给定一个段落(paragraph)，首先把它随机的切分成1到m个segment。然后把segment随机打散(segment内部的词并不打散)，让模型来恢复。那怎么恢复呢？这里使用了一种最简单粗暴的分类的方法，总共有$k=\sum_1^m k!$种分类。这就是一个分类任务，它可以让模型学习段落的篇章结构信息。

**Sentence Distance Task**

​		两个句子的”距离”的任务，对于两个句子有3种关系(3分类任务)：它们是前后相邻的句子；它们不相邻但是属于同一个Document；它们属于不同的Document。

#### Semantic-aware Tasks

**Discourse Relation Task**

​		这个任务会让模型来预测两个句子的语义或者修辞(rhetorical)关系。

**IR Relevance Task**

​		这是利用搜索引擎(百度的优势)的数据，给定Query和搜索结果(可以认为是相关网页的摘要)，可以分为3类：强相关、弱相关和完全不相关。

### 6.2.5 源码分析 [TODO]

https://github.com/PaddlePaddle/ERNIE

### 6.2.6 效果验证 [TODO]

# 七、XLNet [TODO]

# 八、BERT vs. ERNIE [TODO]

## 3.1 原理对比

## 3.2 适用范围

## 3.3 实验比较

# 五、主要参考资料

[Attention Is All You Need](https://arxiv.org/abs/1706.03762) 

[The Illustrated Transformer](https://zhuanlan.zhihu.com/p/59629215)

[Universal Transformers](https://arxiv.org/abs/1807.03819)

[李宏毅Transformer视频及笔记](https://zhiqiangho.github.io/2019/08/06/li-hong-yi-transformer-bi-ji-fu-dai-ma/)

[美团BERT的探索和实践](https://tech.meituan.com/2019/11/14/nlp-bert-practice.html)

[Pre-Training with Whole Word Masking for Chinese BERT](https://arxiv.org/pdf/1906.08101.pdf)

[ERNIE: Enhanced Representation through Knowledge Integration](https://arxiv.org/pdf/1904.09223)

[ERNIE 2.0: A Continual Pre-training Framework for Language Understanding](https://arxiv.org/pdf/1907.12412v1)。

[中文任务全面超越BERT：百度正式发布NLP预训练模型ERNIE](https://zhuanlan.zhihu.com/p/59436589)

...

