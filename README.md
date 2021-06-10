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

This repository contains the resources in our paper **"Revisiting Pre-trained Models for Chinese Natural Language Processing"**, which will be published in "[Findings of EMNLP](https://2020.emnlp.org)". You can read our camera-ready paper through [ACL Anthology](https://www.aclweb.org/anthology/2020.findings-emnlp.58/) or [arXiv pre-print](https://arxiv.org/abs/2004.13922).


**[Revisiting Pre-trained Models for Chinese Natural Language Processing](https://arxiv.org/abs/2004.13922)**  
*Yiming Cui, Wanxiang Che, Ting Liu, Bing Qin, Shijin Wang, Guoping Hu*


For resources other than MacBERT, please visit the following repositories:
- Chinese BERT-wwm series: https://github.com/ymcui/Chinese-BERT-wwm
- Chinese ELECTRA: https://github.com/ymcui/Chinese-ELECTRA
- Chinese XLNet: https://github.com/ymcui/Chinese-XLNet

More resources by HFL: https://github.com/ymcui/HFL-Anthology


## News
**[Nov 3, 2020] Pre-trained MacBERT models are available through direct [Download](#Download) or [Quick Load](#Quick-Load). Use it as if you are using original BERT (except for it cannot perform the original MLM).**

[Sep 15, 2020] Our paper ["Revisiting Pre-Trained Models for Chinese Natural Language Processing"](https://arxiv.org/abs/2004.13922) is accepted to [Findings of EMNLP](https://2020.emnlp.org) as a long paper.


## Guide
| Section | Description |
|-|-|
| [Introduction](#Introduction) | Introduction to MacBERT |
| [Download](#Download) | Download links for MacBERT |
| [Quick Load](#Quick-Load) | Learn how to quickly load our models through [🤗Transformers](https://github.com/huggingface/transformers) |
| [Results](#Results) | Results on several Chinese NLP datasets |
| [FAQ](#FAQ) | Frequently Asked Questions |
| [Citation](#Citation) | Citation |


## Introduction
**MacBERT** is an improved BERT with novel **M**LM **a**s **c**orrection pre-training task, which mitigates the discrepancy of pre-training and fine-tuning.

Instead of masking with [MASK] token, which never appears in the ﬁne-tuning stage, **we propose to use similar words for the masking purpose**. A similar word is obtained by using [Synonyms toolkit (Wang and Hu, 2017)](https://github.com/chatopera/Synonyms), which is based on word2vec (Mikolov et al., 2013) similarity calculations. If an N-gram is selected to mask, we will ﬁnd similar words individually. In rare cases, when there is no similar word, we will degrade to use random word replacement.

Here is an example of our pre-training task.
|  | Example       |
| -------------- | ----------------- |
| **Original Sentence**  | we use a language model to predict the probability of the next word. |
|  **MLM** | we use a language [M] to [M] ##di ##ct the pro [M] ##bility of the next word . |
| **Whole word masking**   | we use a language [M] to [M] [M] [M] the [M] [M] [M] of the next word . |
| **N-gram masking** | we use a [M] [M] to [M] [M] [M] the [M] [M] [M] [M] [M] next word . |
| **MLM as correction** | we use a text system to ca ##lc ##ulate the po ##si ##bility of the next word . |

Except for the new pre-training task, we also incorporate the following techniques.

- Whole Word Masking (WWM)
- N-gram masking
- Sentence-Order Prediction (SOP)

**Note that our MacBERT can be directly replaced with the original BERT as there is no differences in the main neural architecture.**

For more technical details, please check our paper: [Revisiting Pre-trained Models for Chinese Natural Language Processing](https://arxiv.org/abs/2004.13922)


## Download
We mainly provide pre-trained MacBERT models in TensorFlow 1.x.

* **`MacBERT-large, Chinese`**: 24-layer, 1024-hidden, 16-heads, 324M parameters   
* **`MacBERT-base, Chinese`**：12-layer, 768-hidden, 12-heads, 102M parameters   

| Model                                |                         Google Drive                         |                        iFLYTEK Cloud                         | Size |
| :----------------------------------- | :----------------------------------------------------------: | :----------------------------------------------------------: | :--: |
| **`MacBERT-large, Chinese`**    | [TensorFlow](https://drive.google.com/file/d/1lWYxnk1EqTA2Q20_IShxBrCPc5VSDCkT/view?usp=sharing) | [TensorFlow（pw:3Yg3）](http://pan.iflytek.com:80/link/805D743F3826EC4F4EB5C774D34432AE) | 1.2G |
| **`MacBERT-base, Chinese`**     | [TensorFlow](https://drive.google.com/file/d/1aV69OhYzIwj_hn-kO1RiBa-m8QAusQ5b/view?usp=sharing) | [TensorFlow（pw:E2cP）](http://pan.iflytek.com:80/link/CF2A1F9AEBF859650E8956854A994C1B) | 383M |

### PyTorch/TensorFlow2 Version

If you need these models in PyTorch/TensorFlow2,

1) Convert TensorFlow checkpoint into PyTorch/TensorFlow2, using [🤗Transformers](https://github.com/huggingface/transformers)

2) Download from https://huggingface.co/hfl

Steps: select one of the model in the page above → click "list all files in model" at the end of the model page → download bin/json files from the pop-up window.


## Quick Load
With [Huggingface-Transformers](https://github.com/huggingface/transformers), the models above could be easily accessed and loaded through the following codes.
```
tokenizer = BertTokenizer.from_pretrained("MODEL_NAME")
model = BertModel.from_pretrained("MODEL_NAME")
```
**Notice: Please use BertTokenizer and BertModel for loading MacBERT models.  **

The actual model and its `MODEL_NAME` are listed below.

| Original Model | MODEL_NAME                |
| -------------- | ------------------------- |
| MacBERT-large  | hfl/chinese-macbert-large |
| MacBERT-base   | hfl/chinese-macbert-base  |

## Results
We present the results of MacBERT on the following six tasks (please read our paper for other results).
- [**CMRC 2018 (Cui et al., 2019)**：Span-Extraction Machine Reading Comprehension (Simplified Chinese)](https://github.com/ymcui/cmrc2018)
- [**DRCD (Shao et al., 2018)**：Span-Extraction Machine Reading Comprehension (Traditional Chinese)](https://github.com/DRCSolutionService/DRCD)
- [**XNLI (Conneau et al., 2018)**：Natural Langauge Inference](https://github.com/google-research/bert/blob/master/multilingual.md)
- [**ChnSentiCorp**：Sentiment Analysis](https://github.com/pengming617/bert_classification)
- [**LCQMC (Liu et al., 2018)**：Sentence Pair Matching](http://icrc.hitsz.edu.cn/info/1037/1146.htm)
- [**BQ Corpus (Chen et al., 2018)**：Sentence Pair Matching](http://icrc.hitsz.edu.cn/Article/show/175.html)

To ensure the stability of the results, we run 10 times for each experiment and report the maximum and average scores (in brackets).

### CMRC 2018
[CMRC 2018 dataset](https://github.com/ymcui/cmrc2018) is released by the Joint Laboratory of HIT and iFLYTEK Research. The model should answer the questions based on the given passage, which is identical to [SQuAD](http://arxiv.org/abs/1606.05250). Evaluation metrics: EM / F1

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
[DRCD](https://github.com/DRCKnowledgeTeam/DRCD) is also a span-extraction machine reading comprehension dataset, released by Delta Research Center. The text is written in Traditional Chinese. Evaluation metrics: EM / F1

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
We use [XNLI](https://github.com/google-research/bert/blob/master/multilingual.md) data for testing the NLI task. Evaluation metrics: Accuracy

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
We use [ChnSentiCorp](https://github.com/pengming617/bert_classification) data for testing sentiment analysis. Evaluation metrics: Accuracy

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
[**LCQMC**](http://icrc.hitsz.edu.cn/info/1037/1146.htm) is a sentence pair matching dataset, which could be seen as a binary classification task. Evaluation metrics: Accuracy

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
[**BQ Corpus**](http://icrc.hitsz.edu.cn/Article/show/175.html) is a sentence pair matching dataset, which could be seen as a binary classification task. Evaluation metrics: Accuracy

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
**Question 1: Do you have an English version of MacBERT?**

A1: Sorry, we do not have English version of pre-trained MacBERT. 

**Question 2: How to use MacBERT?**

A2: Use it as if you are using original BERT in the fine-tuning stage (just replace the checkpoint and config files). Also, you can perform further pre-training on our checkpoint with MLM/NSP/SOP objectives. 

**Question 3: Could you provide pre-training code for MacBERT?**

A3: Sorry, we cannot provide source code at the moment, and maybe we'll release them in the future, but there is no guarantee.

**Question 4: How about releasing the pre-training data?**

A4: We have no right to redistribute these data, which will have potential legal violations.

**Question 5: Will you release pre-trained MacBERT on a larger data?**

A5: Currently, we have no plans on this.

## Citation
If you find our resource or paper is useful, please consider including the following citation in your paper.
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

## Acknowledgment
The first author would like to thank [Google TensorFlow Research Cloud (TFRC) Program](https://www.tensorflow.org/tfrc).

## Issues
Before you submit an issue:

- **You are advised to read [FAQ](#FAQ) first before you submit an issue.** 
- Repetitive and irrelevant issues will be ignored and closed by [stable-bot](stale · GitHub Marketplace). Thank you for your understanding and support.
- We cannot acommodate EVERY request, and thus please bare in mind that there is no guarantee that your request will be met.
- Always be polite when you submit an issue.

