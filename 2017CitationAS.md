# 1

# 2

通过论文中的引用部分来生成综述

# Dataset

- 110,000 articles in xml format from PLOS One between 2006 and 2015
- identified citation sentences by ruls, then remove the xml labels
- extracted 4,339,217 citation sentences.

# Method

![l9NeE.png](https://s1.ax2x.com/2018/04/17/l9NeE.png)

<center>
<img src="https://s1.ax2x.com/2018/04/17/l9NeE.png" width="200">
</center>

## Retrieval module

Use **Lucene** to index and retrieve dataset, and rank the results which are used for the next step of clustering.

> Lucene 是一个开源的全文检索引擎工具。

## Clustering Module

使用**VSM**来表示每个引用的句子，用**TF-IDF**计算特征权重。

> VSM : Vector Space Model