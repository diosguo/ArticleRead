# ACL2019 GOLC:Global Optimization under Length Constraint for Neural Text Summarization

[TOC]

论文PDF地址:https://www.aclweb.org/anthology/P19-1099

## 1 概述

按照论文中的观点，现有大部分模型都无法控制模型输出的摘要长度，所以很多时候都会输出超长的限制。因为在模型中都没有考虑到文本长度的信息，或者已有的一些考虑长度的模型都没有取得很好的分数。故此，论文作者提出了一个考虑到文本长度的优化目标函数（损失函数）。

## 2 GOLC

### 2.1 前情提要

之前的生成类模型，大部分都是用的MLE(最大对数似然估计），并且是强制学习（Decoder中每一步的输入使用真实摘要而不是上一步的输入）所以单纯的MLE无法解决文本过长的问题（因为训练过程中，生成到原始摘要长度就停了）

### 2.2 MRT

论文中使用了改进的MRT（Minimum Risk Training），MRT方法可以在训练过程中如同预测过程相同，下一个词的生成是根据上一个词的，直到生成结束再根据损失进行优化模型。其基本损失定义如下所示
$$
L_{MRT}(\theta)=\sum_{(x,y)\in D}\sum_{y'\in\tilde{S}(x)}Q_\theta(y'|x)\Delta(y,y')
$$
其中

- $Q_\theta\varpropto p_\theta^\lambda$，$p_\theta$是给定原文产生该摘要的概率，参数$\lambda$是平滑因子
- $\Delta$是两个文档的负ROUGE分数
- $\tilde{S}(x)=S(x)\cup{y}$也就是模型可能输出的摘要与原始摘要的并集

在这种基本情况下，使用ROUGE的Recall、Precision、F1能粗略地控制输出长度，但还是不够理想。

### 2.3 GOLC

作者提出的改进方法，是修改上面的$\Delta$方法，将文本长度加入其中：
$$
L_{MRT}(\theta)=\sum_{(x,y)\in D}\sum_{y'\in\tilde{S}(x)}Q_\theta(y'|x)\tilde\Delta(y,y')
$$

$$
\tilde\Delta(y,y')=-ROUGE(y,trim(y', c_*(y)))+max(0,c_*(y')-c_*(y))
$$

其中：

- $c_*(y)$是代表文本y的长度
- trim(y,c)是代表将文本y从头开始截取c个词的子文本

这样，如果生成文本超长，第一项ROUGE分数不会再变小（因为被截取了），那么$\tilde\Delta$的第二项就不为0，损失就会变大，在优化模型的过程中，会逐渐令模型的输出长度趋近于实际长度。

## 3.效果

作者对比了两个主要模型，一个是基于LSTM（PG)，一个基于CNN（LC)，效果如下所示，可以看到，超长的情况得到了很大的缓解，而平均生成时间也缩短不少（可能因为短了）

![image-20190921094647909](ACL2019%20GOLCGlobal%20Optimization%20under%20Length%20Constraint%20for%20Neural%20Text%20Summarization.assets/image-20190921094647909.png)

## 4 总结

可以从模型效果看出，带有普通MRT的模型取得了较好的ROUGE成绩，虽然加入GOLC模型有些许损失，但是却从某方面提升了模型效果，也是不错的思路。