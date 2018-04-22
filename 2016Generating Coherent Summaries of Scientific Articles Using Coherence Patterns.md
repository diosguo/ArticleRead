# Generating Coherent Summaries of Scientific Articles Using Coherence Patterns

# 使用相关模式生成科学论文的相关综述

论文作者：Daraksha Parveen、Mohshe Mesgar 、Michael Strube

[TOC]

## 1 摘要 Abstract

使用基于图的方法对论文进行综述，利用相似模式来保证生成的综述是相关的。另外还提出了一个方法来整合相关性、重要性和非冗余性。同时还使用混合整数规划方法来优化参数。

They use a graph-based approach to summarize articles, and employ coherence patterns to ensure that the generated summaries are coherent. They also propose a method to combine the importance, coherence and non-redundancy. To optimize these factories, they use  Mixed Integer Programming.

## 2 背景概念和知识 Introduction

### 2.1 综述的三个需要考虑的属性 Three properties summarize should consider

- 重要性 Importance
  - 综述应该包含输入的文档中的重要信息。
  - The summary should contain the important information of the input document.
- 非冗余性 Non-Redundancy
  - 综述应该包含非冗余的信息，这些信息都应该是不相同的。
  - The summary should contain non-redundant information. The information should be diverse in the summary.
- 相关性 Coherence
  - 尽管综述应该包含输入文档的不同的重要信息，他的句子还应该衔接连贯而且易于阅读。
  - Though the summary should comprise diverse and important information of the input document, its sentences should be connected to one another such that it becomes coherent and easy to read.

### 2.2 评估标准 ROUGE SCORE

- ROUGH，是用来评估自然语言处理中自动总结和机器翻译软件的一系列度量和软件包。这个度量工作比较自动处理的总结或者翻译和人工的总结与翻译来得到。


- **ROUGE**, or **Recall-Oriented Understudy for Gisting Evaluation**,is a set of metrics and a software package used for evaluating [automatic summarization](https://en.wikipedia.org/wiki/Automatic_summarization) and [machine translation](https://en.wikipedia.org/wiki/Machine_translation) software in [natural language processing](https://en.wikipedia.org/wiki/Natural_language_processing). The metrics compare an automatically produced summary or translation against a reference or a set of references (human-produced) summary or translation.

### 2.3 二分图 Bipartite Graph （Bigraph）

- 把图形的理论放在数学中考虑，一个二分图的含义就是他里面的节点都能分成不相交而且不互相依赖的两类。
- In the [mathematical](https://en.wikipedia.org/wiki/Mathematics) field of [graph theory](https://en.wikipedia.org/wiki/Graph_theory), a **bipartite graph** (or **bigraph**) is a [graph](https://en.wikipedia.org/wiki/Graph_(discrete_mathematics)) whose [vertices](https://en.wikipedia.org/wiki/Vertex_(graph_theory)) can be divided into two [disjoint](https://en.wikipedia.org/wiki/Disjoint_sets) and [independent sets](https://en.wikipedia.org/wiki/Independent_set_(graph_theory))![U](https://wikimedia.org/api/rest_v1/media/math/render/svg/458a728f53b9a0274f059cd695e067c430956025) and ![V](https://wikimedia.org/api/rest_v1/media/math/render/svg/af0f6064540e84211d0ffe4dac72098adfa52845) such that every [edge](https://en.wikipedia.org/wiki/Edge_(graph_theory)) connects a vertex in ![U](https://wikimedia.org/api/rest_v1/media/math/render/svg/458a728f53b9a0274f059cd695e067c430956025) to one in ![V](https://wikimedia.org/api/rest_v1/media/math/render/svg/af0f6064540e84211d0ffe4dac72098adfa52845). Vertex sets ![U](https://wikimedia.org/api/rest_v1/media/math/render/svg/458a728f53b9a0274f059cd695e067c430956025) and ![V](https://wikimedia.org/api/rest_v1/media/math/render/svg/af0f6064540e84211d0ffe4dac72098adfa52845) are usually called the *parts* of the graph. Equivalently, a bipartite graph is a graph that does not contain any odd-length [cycles](https://en.wikipedia.org/wiki/Cycle_(graph_theory)).

## 3 方法 Method

![89dpY.png](https://s1.ax2x.com/2018/04/22/89dpY.png)

### 3.0 预处理

1. Use Stanford parser to  determine sentence boundaries.

2. Use Brown coherence toolkit to convert the articles into entity grids.

3. Use gSpan to extract all subgraphs from the projection graphs of the abstracts of the PubMed corpus.

### 3.1 文本表示 Document Representation

- 用一个实体图来表示这些科技论文，这个实体图是一个二分图，这个图里面**实体**和**句子**这两组不相交的节点构成。其中，实体节点只能和句子节点相连（仅当实体出现在句子中的时候），而不能和其他实体节点相连。
- They use the entity graph to present scientific articles. The entity graph is a bipartite graph which consists of entities and sentences as two disjoint sets of nodes. Entity nodes are connected only with sentence nodes and not among each other. An entity node is connected with a sentence node only if the entity is present in the sentence.
- 得到论文的二分图后，对其中的句子节点进行单模式的映射，来创建一个有向的单模式投影图。当两个句子有相同的实体的时候，他们会有连接，边的方向和在文中出现的顺序一致。
- They perform a one-mode projection on sentence nodes to create a directed one-mode projection graph. Two sentence nodes in the one-mode projection graph are connected if they share at least one entity in the entity graph.

### 3.2 挖掘相关模式 Mining Coherence Patterns

- 使用**PubMed corpus**的摘要的单模式投影图来挖掘相关模式。

- Use one-mode projection graphs of abstracts in the PubMed corpus to mine coherence patterns.

- 有一个相似模式的权重的计算方法，其中`q`是语料库中我们得到的图的数量。*$g_k$*表示第$k$个摘要的图。

  ​							$weight(pat_u)=\frac{\sum_{k=1}^qfreq(pat_u,g_k)}{max_{k=1}^qfreq(pat_u),g_k}$

- There is a method to calculate the weight of patterns, where q is the number of graphs associated with abstracts in the corpus, and $g_k$ represents the graph of the $k^{th}$ abstract in the PubMed corpus.

-  我们可以判断，权重的值，并不是在同一个范围内，所以我们用一个sigmoid函数来对$weight$进行正则化，把它的范围缩放到[0,1].

- The weights of the coherence patterns are not on the same scale. We use a sigmoid function scales weights to the interval [0,1].

### 3.3 生成综述 Summary Generation

#### 3.3.1 重要性 Importance

- 通过Rank函数计算而来，算法是由Kleinberg的**HITS**算法得到的
- Importance is calculated by considering the ranks of selected sentences for the summary.

#### 3.3.2 非冗余性 Non-Redundancy

​				$f_R(E)=\sum_{j=1}^me_j$

- Where *m* is the number of entities and $e_j$ is a binary variable for each entity.

#### 3.3.3 相关性 Coherence

​			$f_C(P)=\sum_{u=1}^Uweight(pat_u)\times p_u$

- Where $p_u$ is a boolean variable associated with coherence pattern $pat_u$

## 数据集 Dataset

### 1 PubMed corpus ( Get Coherence Pattern)

- 分析美国国立医学图书馆中的论文的摘要，来获取这里的相关性模式。
- obtain coherence patterns by analyzing a corpus of abstracts of articles from biomedicine (PubMed corpus).

### 2 PLOS Medicine dataset ( Test the system) 公共科学图书馆医学

- 将其中的论文输入系统，并和人工书写的综述进行比较来评估他们的系统。
- Input the documents to their system, and evaluate them by comparing with summaries written by a PLOS Medicine editor.

### 3 DUC 2002 Document Understanding Conference 文本理解会议

- 用来评估综述结果
- Used to evaluate the summaries.

## 相关工作 Related Work

## 总结 Conclusion