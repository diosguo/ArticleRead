# 1

# 2

通过论文中的引用部分来生成综述

# Dataset

- 110,000 articles in xml format from PLOS One between 2006 and 2015
- identified citation sentences by ruls, then remove the xml labels
- extracted 4,339,217 citation sentences.

# Method

![l9NeE.png](https://s1.ax2x.com/2018/04/17/l9NeE.png)

## Retrieval module

Use **Lucene** to index and retrieve dataset, and rank the results which are used for the next step of clustering.

> Lucene 是一个开源的全文检索引擎工具。

## Clustering Module

- 使用**VSM**来表示每个引用的句子，用**TF-IDF**计算特征权重。得到的每个句子的表示为$\sum$

  > VSM : Vector Space Model

- 使用Carrot2中内置的K-means、Lingo和STC算法对引用文本进行据类。为了减轻计算负担，先使用NMF算法进行数据降维

  > NMF : Non-negative Martix Factorization 非负矩阵分解

## Clustering Label Generation

- 使用类别中的常用词作为类别的名字（Label），之后使用三种相似度计算模型（仅Word2Vec,仅WordNet,Word2Vec和WordNet一起）对相似的类别进行合并

## Automatic Summary Generation

- 所有类别按照其中包含句子的数量从多到少排序，然后每个类别都是最终Summary的一个段落，然后用两种方法来获取每个类别中的最重要的句子。
  - TF-IDF
  - MMR