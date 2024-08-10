## Roberta(Roberta: A Robustly Optimized BERT Pretraining Approach)作用
根据BERT**训练不足**的缺点提出了更有效的预训练方法，并发布了具有更强**鲁棒性**的BERT：RoBERTa


## 改进点：
#### 1.动态掩码
BERT中，对于每一个样本序列进行mask之后，mask的tokens都固定下来了，也就是静态mask的方式。RoBERTa的训练过程中使用了动态mask的方式：**对于每一个输入样本序列，都会复制10条，然后复制的每一个都会重新随机mask，其中每个句子被mask的token不同：即拥有不同的masked tokens。**

#### 2.移除NSP任务
实验表明，NSP任务并没有使得模型性能变好，故移除。


#### 3.更大规模训练数据
 BERT的预训练语料是 BOOKCORPUS+English WIKIPEDIA的**16GB**语料。RoBERT除了在Toronto BookCorpus和英文维基百科上进行训练，还在CC-News(Common Crawl-News)数据集、Open WebText和Stories(Common Crawl的子集)上进行训练。RoBERT模型在5个数据集上进行预训练，这5个数据集总大小为**160G**。

BERT预训练的batch size为256，训练了1M步。而RoBERTa则使用了**更大的batch size**，训练RoBERTa时采样了更大的批大小，达到了8000,训练了300000步。在同样的批大小上，也训练了一个更长训练步的版本，有500000步。**训练一个更大的批大小可以增加训练速度同时也可以优化模型的表现**。

#### 4.BBPE作为分词器
BBPE：BBPE核心思想将BPE的**从字符级别扩展到子节（Byte）级别**。**BPE的一个问题是如果遇到了unicode编码，基本字符集可能会很大**。BBPE就是以一个字节为一种“字符”，不管实际字符集用了几个字节来表示一个字符。这样的话，基础字符集的大小就锁定在了256（2^8）。采用BBPE的好处是可以**跨语言共用词表**，显著压缩词表的大小。而坏处就是，对于类似中文这样的语言，一段文字的序列长度会显著增长。因此，BBPE based模型可能比BPE based模型表现的更好。然而，BBPE sequence比起BPE来说略长，这也导致了更长的训练/推理时间。BBPE其实与BPE在实现上并无大的不同，只不过基础词表使用256的字节集。


## 学习资源
**论文解读**: https://blog.csdn.net/weixin_44750512/article/details/129320041
**代码实现**: https://github.com/pytorch/fairseq/tree/master/examples/roberta




