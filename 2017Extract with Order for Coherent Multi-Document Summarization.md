# 1

# Introduction

## 多文档摘要之难

原文中提到

> The information overlap between the documents from the same topic makes the multi-document summarization more challenging than the task of summarizing single documents

# Method

![lIC4p.jpg](https://s1.ax2x.com/2018/04/20/lIC4p.jpg))

## Sentence Extraction

### Preprocessing

使用NLTK工具进行分词和词性标注与词形还原。

### Sentence Similarity

通过Word Embedding和词频的计算，如图中公式，得到每个句子的向量。

### Sentence Ranking

使用TextRank算法进行句子图模型的建立，并且使用Word2Vec来优化其中的语义相似的计算。

### Sentence Clustering

使用图中写明的论文中的方法，来进行聚类，得到多个Cluster

### Sentence Selection

基于ILP框架，并加入一点小修改，通过关键词（通过RAKE方法）的权重

通过选择，使其包含的信息最多

---

## Sentence Ordering

根据语义相关和相似性来对最终文档中的句子进行排序。

---

# Evalution

## DataSet

DUC 2004

## Evaluate Rule

- ROUGE
- Lapata and Barzilay,2005; Barzilay and Lapata,2008 to evalute **coherence**

## BaseLine

- LexRank
- GreedyKL
- Submodular,ICSISumm