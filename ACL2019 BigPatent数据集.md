# ACL2019 BigPatent数据集

[TOC]

论文PDF地址：https://www.aclweb.org/anthology/P19-1212

数据集下载地址：

- [evasharma.github.io/bigpatent](https://evasharma.github.io/bigpatent/)(GoogleDrive)
- [百度网盘]()

> ACL 2019 Sharma, E., Li, C., & Wang, L. (2019). Bigpatent: a large-scale dataset for abstractive and coherent summarization.

## 1 概述

作者使用1971年以来9个领域的130万份专利文档的介绍和摘要构建的文本摘要数据集。原始文档从*Goole Patents Public Datasets*获取。相比较于之前的**CNN/DM、NYT**这些新闻类数据集，专利文档具有独到的特点。作者也在此数据集上，获得了当前主流模型的性能并与其他数据集进行比较，下面两部分分别介绍本数据集特点与模型性能



# 2 特点

### 2.1 表述结构

BigPatent数据集的摘要部分表述结构更加丰富。新闻类文章更加倾向于对事件的线性陈述，所以其表述结构很单一。专利类文档，其说明性质更强，同一个实体在摘要部分中出现的重复项更强。

<img src="ACL2019%20BigPatent%E6%95%B0%E6%8D%AE%E9%9B%86.assets/image-20190919231901332.png" width="60%" />

### 2.2 关键信息分布

BigPatent数据集的正文部分关键信息更加分散。新闻类文章由于其文体结构，重要信息更加倾向于出现在文章的开头，所以实际上训练处的摘要模型并不是能够正确地处理全局信息。而专利类文档，关键信息遍布文档各个部分，能够更好地训练摘要模型。如下图所示，**BigPatent关键信息在各个部分中分布比较均匀。**

<img src="ACL2019%20BigPatent%E6%95%B0%E6%8D%AE%E9%9B%86.assets/image-20190919230649598.png" width="60%"/>

### 2.3 摘要性

BigPatent数据集的摘要性更强。前面提到的比如CNN/DM数据集，其摘要内容很多都是直接从原文中截取的一部分。虽然训练的是摘要模型，但是数据集一定程度上有抽取的特征。如下面的统计特征，新的多元词组同时在摘要与正文出现的比例相对也高一些。 

<img src="ACL2019%20BigPatent%E6%95%B0%E6%8D%AE%E9%9B%86.assets/image-20190919230850561.png" width="60%"/>

### 3 主流模型在各数据集性能比较

直接上表，对应论文见最下方参考文献部分：

![image-20190919231222690](ACL2019%20BigPatent%E6%95%B0%E6%8D%AE%E9%9B%86.assets/image-20190919231222690.png)



## 4 总结

新的BigPatent数据集，一定程度上对文本摘要提出了更大的挑战，也让很多模型走出了LEAD-3的阴影。确实有助于很多除了新闻以外的其他领域能够有更好的模型发展。



### 参考文献

Yen-Chun Chen and Mohit Bansal. 2018. Fast abstractive summarization with reinforce-selected sentence rewriting. In Proceedings of the 56th Annual Meeting of the Association for Computational Linguistics (Volume 1: Long Papers), pages 675–686. Association for Computational Linguistics.

Gunes Erkan and Dragomir R Radev. 2004. Lexrank: ¨ Graph-based lexical centrality as salience in text summarization. Journal of artificial intelligence research, 22:457–479.

Rada Mihalcea and Paul Tarau. 2004. Textrank: Bringing order into text. In Proceedings of the 2004 Conference on Empirical Methods in Natural Language Processing.

Ani Nenkova and Lucy Vanderwende. 2005. The impact of frequency on summarization. Microsoft Research, Redmond, Washington, Tech. Rep. MSR-TR2005, 101.

Abigail See, Peter J. Liu, and Christopher D. Manning. 2017. Get to the point: Summarization with pointergenerator networks. In Proceedings of the 55th Annual Meeting of the Association for Computational Linguistics (Volume 1: Long Papers), pages 1073– 1083. Association for Computational Linguistics.

Ilya Sutskever, Oriol Vinyals, and Quoc V Le. 2014. Sequence to sequence learning with neural networks. In Advances in neural information processing systems, pages 3104–3112.