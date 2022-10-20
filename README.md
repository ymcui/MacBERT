
<p>
<a href="https://console.tiyaro.ai/explore?q=hfl/&pub=hfl"> <img src="https://tiyaro-public-docs.s3.us-west-2.amazonaws.com/assets/try_on_tiyaro_badge.svg"></a>
</p>

[**简体中文**](https://github.com/ymcui/MacBERT) | [**English**](./README_EN.md)

<p align="center">
    <br>
    <img src="./pics/banner.png" width="500"/>
    <br>
</p>
<p align="center">
    <a href="https://github.com/ymcui/MacBERT/blob/master/LICENSE">
        <img alt="GitHub" src="https://img.shields.io/github/license/ymcui/MacBERT.svg?color=blue&style=flat-square">
    </a>
</p>
本目录包含**MacBERT预训练模型**，该模型引入了一种纠错型掩码语言模型（Mac）预训练任务，缓解了“预训练-下游任务”不一致的问题。MacBERT在多种NLP任务上取得了显著性能提升。


- **[Revisiting Pre-trained Models for Chinese Natural Language Processing](https://www.aclweb.org/anthology/2020.findings-emnlp.58)**  
- *Yiming Cui, Wanxiang Che, Ting Liu, Bing Qin, Shijin Wang, Guoping Hu*
- Published in *Findings of EMNLP 2020*

----

[中文MacBERT](https://github.com/ymcui/MacBERT) | [中文ELECTRA](https://github.com/ymcui/Chinese-ELECTRA) | [中文XLNet](https://github.com/ymcui/Chinese-XLNet) | [知识蒸馏工具TextBrewer](https://github.com/airaria/TextBrewer) | [模型裁剪工具TextPruner](https://github.com/airaria/TextPruner)

更多HFL发布的资源：https://github.com/ymcui/HFL-Anthology

## News
**2022/3/30 发布了新的预训练模型PERT：https://github.com/ymcui/PERT** 

2021/12/17 发布了模型裁剪工具TextPruner：https://github.com/airaria/TextPruner

2021/10/24 发布了首个面向少数民族语言的预训练模型CINO：https://github.com/ymcui/Chinese-Minority-PLM

2021/7/21  ["自然语言处理：基于预训练模型的方法"](https://item.jd.com/13344628.html) 一书正式出版。

2020/11/3 预训练好的中文MacBERT已发布，使用方法与BERT一致。

2020/9/15 论文["Revisiting Pre-Trained Models for Chinese Natural Language Processing"](https://arxiv.org/abs/2004.13922) 被[Findings of EMNLP](https://2020.emnlp.org) 录用为长文。


## 目录
| 章节 | 描述 |
|-|-|
| [简介](#简介) | 简要介绍MacBERT |
| [下载](#下载) | 下载MacBERT |
| [快速加载](#快速加载) | 介绍如何使用 [🤗Transformers](https://github.com/huggingface/transformers) 快速加载模型 |
| [基线效果](#基线效果) | 在中文NLP任务上的效果 |
| [FAQ](#FAQ) | 常见问题 |
| [引用](#引用) | 文章引用信息 |


## 简介
**MacBERT** 是BERT的改进版本，引入了纠错型掩码语言模型（MLM as correction，Mac）预训练任务，缓解了“预训练-下游任务”不一致的问题。

掩码语言模型（MLM）中，引入了[MASK]标记进行掩码，但[MASK]标记并不会出现在下游任务中。在MacBERT中，**我们使用相似词来取代[MASK]标记**。相似词通过[Synonyms toolkit (Wang and Hu, 2017)](https://github.com/chatopera/Synonyms)工具获取，算法基于word2vec (Mikolov et al., 2013)相似度计算。同时我们也引入了Whole Word Masking（wwm）和N-gram masking技术。当要对N-gram进行掩码时，我们会对N-gram里的每个词分别查找相似词。当没有相似词可替换时，我们将使用随机词进行替换。

以下是训练样本示例。

|  | 例子       |
| -------------- | ----------------- |
| **原始句子** | we use a language model to predict the probability of the next word. |
|  **MLM** | we use a language [M] to [M] ##di ##ct the pro [M] ##bility of the next word . |
| **Whole word masking**   | we use a language [M] to [M] [M] [M] the [M] [M] [M] of the next word . |
| **N-gram masking** | we use a [M] [M] to [M] [M] [M] the [M] [M] [M] [M] [M] next word . |
| **MLM as correction** | we use a text system to ca ##lc ##ulate the po ##si ##bility of the next word . |

**MacBERT的主要框架与BERT完全一致，可在不修改现有代码的基础上进行无缝过渡。**

更多细节请参考我们的论文：**[Revisiting Pre-trained Models for Chinese Natural Language Processing](https://www.aclweb.org/anthology/2020.findings-emnlp.58)**  


## 下载
主要提供TensorFlow 1.x版本的模型下载。

* **`MacBERT-large, Chinese`**: 24-layer, 1024-hidden, 16-heads, 324M parameters   
* **`MacBERT-base, Chinese`**：12-layer, 768-hidden, 12-heads, 102M parameters   

| 模型                                |                         Google Drive                         |                        百度盘                        | 大小 |
| :----------------------------------- | :----------------------------------------------------------: | :----------------------------------------------------------: | :--: |
| **`MacBERT-large, Chinese`**    | [TensorFlow](https://drive.google.com/file/d/1lWYxnk1EqTA2Q20_IShxBrCPc5VSDCkT/view?usp=sharing) | [TensorFlow（pw:zejf）](https://pan.baidu.com/s/1nJEjhUAnWGO_1RPki21mxA?pwd=zejf) | 1.2G |
| **`MacBERT-base, Chinese`**     | [TensorFlow](https://drive.google.com/file/d/1aV69OhYzIwj_hn-kO1RiBa-m8QAusQ5b/view?usp=sharing) | [TensorFlow（pw:61ga）](https://pan.baidu.com/s/1EAs2fmraqtvfia5Q5rXnuQ?pwd=61ga) | 383M |

### PyTorch/TensorFlow2 版本

如果需要PyTorch或者TensorFlow2版本的模型：

1. 使用  [🤗Transformers](https://github.com/huggingface/transformers) 自行转换
2. 或者从 https://huggingface.co/hfl 下载

下载步骤（也可以直接用git将整个目录克隆下来）：

1. 进入https://huggingface.co/hfl 之后选择某个MacBERT模型，例如MacBERT-base：https://huggingface.co/hfl/chinese-macbert-base
2. 选择"files and versions"选项卡
3. 点击需要下载的bin/json等文件


## 快速加载
通过  [🤗Transformers](https://github.com/huggingface/transformers)  可以快速加载MacBERT模型。

```
tokenizer = BertTokenizer.from_pretrained("MODEL_NAME")
model = BertModel.from_pretrained("MODEL_NAME")
```
**注意：请使用BertTokenizer和BertModel来加载MacBERT模型！**

对应的`MODEL_NAME` 如下所示：

| 原模型        | 模型调用名                |
| ------------- | ------------------------- |
| MacBERT-large | hfl/chinese-macbert-large |
| MacBERT-base  | hfl/chinese-macbert-base  |

## 基线效果
这里展示MacBERT在6个下游任务上的效果（更多结果请参考论文）：

- [**CMRC 2018 (Cui et al., 2019)**：抽取式阅读理解（简体中文）](https://github.com/ymcui/cmrc2018)
- [**DRCD (Shao et al., 2018)**：抽取式阅读理解（繁体中文）](https://github.com/DRCSolutionService/DRCD)
- [**XNLI (Conneau et al., 2018)**：自然语言推断](https://github.com/google-research/bert/blob/master/multilingual.md)
- [**ChnSentiCorp**：情感分类](https://github.com/pengming617/bert_classification)
- [**LCQMC (Liu et al., 2018)**：句对匹配](http://icrc.hitsz.edu.cn/info/1037/1146.htm)
- [**BQ Corpus (Chen et al., 2018)**：句对匹配](http://icrc.hitsz.edu.cn/Article/show/175.html)

为了保证结果的稳定性，我们同时给出独立运行10次的平均值（括号内）和最大值。

### CMRC 2018
[**CMRC 2018数据集**](https://github.com/ymcui/cmrc2018)是哈工大讯飞联合实验室发布的中文机器阅读理解数据。
根据给定问题，系统需要从篇章中抽取出片段作为答案，形式与SQuAD相同。
评测指标为：EM / F1

| Model                     |        Development        |           Test            |         Challenge         | #Params |
| :------------------------ | :-----------------------: | :-----------------------: | :-----------------------: | :-----: |
| BERT-base                 | 65.5 (64.4) / 84.5 (84.0) | 70.0 (68.7) / 87.0 (86.3) | 18.6 (17.0) / 43.3 (41.3) |  102M   |
| BERT-wwm                  | 66.3 (65.0) / 85.6 (84.7) | 70.5 (69.1) / 87.4 (86.7) | 21.0 (19.3) / 47.0 (43.9) |  102M   |
| BERT-wwm-ext              | 67.1 (65.6) / 85.7 (85.0) | 71.4 (70.0) / 87.7 (87.0) | 24.0 (20.0) / 47.3 (44.6) |  102M   |
| RoBERTa-wwm-ext           | 67.4 (66.5) / 87.2 (86.5) | 72.6 (71.4) / 89.4 (88.8) | 26.2 (24.6) / 51.0 (49.1) |  102M   |
| ELECTRA-base          | 68.4 (68.0) / 84.8 (84.6) | 73.1 (72.7) / 87.1 (86.9) | 22.6 (21.7) / 45.0 (43.8) |  102M   |
| **MacBERT-base** | 68.5 (67.3) / 87.9 (87.1) |73.2 (72.4) / 89.5 (89.2)|30.2 (26.4) / 54.0 (52.2)|102M|
| ELECTRA-large         |        69.1 (68.2) / 85.2 (84.5)        |        73.9 (72.8) / 87.1 (86.6)        |        23.0 (21.6) / 44.2 (43.2)        |  324M   |
| RoBERTa-wwm-ext-large | 68.5 (67.6) / 88.4 (87.9) | 74.2 (72.4) / 90.6 (90.0) | 31.5 (30.1) / 60.1 (57.5) |324M|
| **MacBERT-large** | 70.7 (68.6) / 88.9 (88.2) | 74.8 (73.2) / 90.7 (90.1) | 31.9 (29.6) / 60.2 (57.6) | 324M |


### DRCD
[**DRCD数据集**](https://github.com/DRCKnowledgeTeam/DRCD)由中国台湾台达研究院发布，其形式与SQuAD相同，是基于繁体中文的抽取式阅读理解数据集。
**由于ERNIE中去除了繁体中文字符，故不建议在繁体中文数据上使用ERNIE（或转换成简体中文后再处理）。**
评测指标为：EM / F1

| Model                     |        Development        |           Test            | #Params |
| :------------------------ | :-----------------------: | :-----------------------: | :-----: |
| BERT-base                 | 83.1 (82.7) / 89.9 (89.6) | 82.2 (81.6) / 89.2 (88.8) |  102M   |
| BERT-wwm                  | 84.3 (83.4) / 90.5 (90.2) | 82.8 (81.8) / 89.7 (89.0) |  102M   |
| BERT-wwm-ext              | 85.0 (84.5) / 91.2 (90.9) | 83.6 (83.0) / 90.4 (89.9) |  102M   |
| RoBERTa-wwm-ext           | 86.6 (85.9) / 92.5 (92.2) | 85.6 (85.2) / 92.0 (91.7) |  102M   |
| ELECTRA-base          | 87.5 (87.0) / 92.5 (92.3) | 86.9 (86.6) / 91.8 (91.7) |  102M   |
| **MacBERT-base** | 89.4 (89.2) / 94.3 (94.1) | 89.5 (88.7) / 93.8 (93.5) | 102M |
| ELECTRA-large         |        88.8 (88.7) / 93.3 (93.2)        |        88.8 (88.2) / 93.6 (93.2)        |  324M   |
| RoBERTa-wwm-ext-large | 89.6 (89.1) / 94.8 (94.4) | 89.6 (88.9) / 94.5 (94.1) |324M|
| **MacBERT-large** | 91.2 (90.8) / 95.6 (95.3) | 91.7 (90.9) / 95.6 (95.3) |324M|


### XNLI
在自然语言推断任务中，我们采用了[**XNLI**数据](https://github.com/google-research/bert/blob/master/multilingual.md)，需要将文本分成三个类别：`entailment`，`neutral`，`contradictory`。
评测指标为：Accuracy

| Model                     | Development |    Test     | #Params |
| :------------------------ | :---------: | :---------: | :-----: |
| BERT-base                 | 77.8 (77.4) | 77.8 (77.5) |  102M   |
| BERT-wwm                  | 79.0 (78.4) | 78.2 (78.0) |  102M   |
| BERT-wwm-ext              | 79.4 (78.6) | 78.7 (78.3) |  102M   |
| RoBERTa-wwm-ext           | 80.0 (79.2) | 78.8 (78.3) |  102M   |
| ELECTRA-base          | 77.9 (77.0) | 78.4 (77.8) |  102M   |
| **MacBERT-base** | 80.3 (79.7) | 79.3 (78.8) | 102M |
| ELECTRA-large         |    81.5 (80.8)    |    81.0 (80.9)    |  324M   |
| RoBERTa-wwm-ext-large | 82.1 (81.3) | 81.2 (80.6) |324M|
| **MacBERT-large** | 82.4 (81.8) | 81.3 (80.6) |324M|


### ChnSentiCorp
在情感分析任务中，二分类的情感分类数据集ChnSentiCorp。
评测指标为：Accuracy

| Model                     | Development |    Test     | #Params |
| :------------------------ | :---------: | :---------: | :-----: |
| BERT-base                 | 94.7 (94.3) | 95.0 (94.7) |  102M   |
| BERT-wwm                  | 95.1 (94.5) | 95.4 (95.0) |  102M   |
| BERT-wwm-ext              | 95.4 (94.6) | 95.3 (94.7) |  102M   |
| RoBERTa-wwm-ext           | 95.0 (94.6) | 95.6 (94.8) |  102M   |
| ELECTRA-base          | 93.8 (93.0) | 94.5 (93.5) |  102M   |
| **MacBERT-base** | 95.2 (94.8) | 95.6 (94.9) | 102M |
| ELECTRA-large         |    95.2 (94.6)    |    95.3 (94.8)    |  324M   |
| RoBERTa-wwm-ext-large | 95.8 (94.9) | 95.8 (94.9) |324M|
| **MacBERT-large** | 95.7 (95.0) | 95.9 (95.1) | 324M |


### LCQMC
[LCQMC](http://icrc.hitsz.edu.cn/info/1037/1146.htm)由哈工大深圳研究生院智能计算研究中心发布。 
评测指标为：Accuracy

| Model                     | Development |    Test     | #Params |
| :------------------------ | :---------: | :---------: | :-----: |
| BERT                      | 89.4 (88.4) | 86.9 (86.4) |  102M   |
| BERT-wwm                  | 89.4 (89.2) | 87.0 (86.8) |  102M   |
| BERT-wwm-ext              | 89.6 (89.2) | 87.1 (86.6) |  102M   |
| RoBERTa-wwm-ext           | 89.0 (88.7) | 86.4 (86.1) |  102M   |
| ELECTRA-base          | 90.2 (89.8) | 87.6 (87.3) |  102M   |
| **MacBERT-base** | 89.5 (89.3) | 87.0 (86.5) | 102M |
| ELECTRA-large         |    90.7 (90.4)    |    87.3 (87.2)    |  324M   |
| RoBERTa-wwm-ext-large | 90.4 (90.0) | 87.0 (86.8) |324M|
| **MacBERT-large** | 90.6 (90.3) | 87.6 (87.1) | 324M |

### BQ Corpus 
[BQ Corpus](http://icrc.hitsz.edu.cn/Article/show/175.html)由哈工大深圳研究生院智能计算研究中心发布，是面向银行领域的数据集。
评测指标为：Accuracy

| Model                     | Development |    Test     | #Params |
| :------------------------ | :---------: | :---------: | :-----: |
| BERT                      | 86.0 (85.5) | 84.8 (84.6) |  102M   |
| BERT-wwm                  | 86.1 (85.6) | 85.2 (84.9) |  102M   |
| BERT-wwm-ext              | 86.4 (85.5) | 85.3 (84.8) |  102M   |
| RoBERTa-wwm-ext           | 86.0 (85.4) | 85.0 (84.6) |  102M   |
| ELECTRA-base          | 84.8 (84.7) | 84.5 (84.0) |  102M   |
| **MacBERT-base** | 86.0 (85.5) | 85.2 (84.9) | 102M |
| ELECTRA-large         |    86.7 (86.2)    |    85.1 (84.8)    |  324M   |
| RoBERTa-wwm-ext-large | 86.3 (85.7) | 85.8 (84.9) |324M|
| **MacBERT-large** | 86.2 (85.7) | 85.6 (85.0) | 324M |

## FAQ
**Q1: 有英文版的MacBERT吗？**

A1: 目前没有。

**Q2: 如何使用MacBERT？**

A2: 和使用BERT一样，只需要简单替换模型文件和config就能使用了。当然，你也可以通过加载我们的模型（即初始化transformers部分）来进一步训练其他预训练模型。

**Q3: 能提供MacBERT的训练代码吗？**

A3: 暂无开源计划。

**Q4: 能开源预训练的语料吗？**

A4: 我们无法开源训练语料，因为没有相应重发布的权利。GitHub上有一些开源中文语料资源，可以多加关注利用。

**Q5: 有计划在更大的语料上训练MacBERT并开源吗？**

A5: 我们暂时没有计划。

## 引用
如果本项目中的资源对您的研究有帮助，请引用以下论文。

```
@inproceedings{cui-etal-2020-revisiting,
    title = "Revisiting Pre-Trained Models for {C}hinese Natural Language Processing",
    author = "Cui, Yiming  and
      Che, Wanxiang  and
      Liu, Ting  and
      Qin, Bing  and
      Wang, Shijin  and
      Hu, Guoping",
    booktitle = "Proceedings of the 2020 Conference on Empirical Methods in Natural Language Processing: Findings",
    month = nov,
    year = "2020",
    address = "Online",
    publisher = "Association for Computational Linguistics",
    url = "https://www.aclweb.org/anthology/2020.findings-emnlp.58",
    pages = "657--668",
}
```

或者：
```
@journal{cui-etal-2021-pretrain,
  title={Pre-Training with Whole Word Masking for Chinese BERT},
  author={Cui, Yiming and Che, Wanxiang and Liu, Ting and Qin, Bing and Yang, Ziqing},
  journal={IEEE Transactions on Audio, Speech and Language Processing},
  year={2021},
  url={https://ieeexplore.ieee.org/document/9599397},
  doi={10.1109/TASLP.2021.3124365},
 }
```

## 致谢
感谢Google [TPU Research Cloud (TFRC)](https://www.tensorflow.org/tfrc)提供计算资源支持。



## 问题反馈
如有问题，请在GitHub Issue中提交。

- 在提交问题之前，请先查看FAQ能否解决问题，同时建议查阅以往的issue是否能解决你的问题。
- 重复以及与本项目无关的issue会被[stable-bot](stale · GitHub Marketplace)处理，敬请谅解。
- 我们会尽可能的解答你的问题，但无法保证你的问题一定会被解答。
- 礼貌地提出问题，构建和谐的讨论社区。
