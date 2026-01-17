---
title: 'LoRA Variants Surveys'
date: '2026-01-17T00:09:30+08:00'
# weight: 1
# aliases: ["/first"]
tags: ["Flash Card", "LoRA Variants"]
author: "PaperMoon"
# author: ["Me", "You"] # multiple authors
showToc: true
TocOpen: false
draft: false
hidemeta: false
comments: false
# description: "Desc Text."
canonicalURL: "https://canonical.url/to/page"
# disableHLJS: true # to disable highlightjs
disableShare: false
disableHLJS: false
hideSummary: false
searchHidden: false
ShowReadingTime: false
ShowBreadCrumbs: true
ShowPostNavLinks: true
ShowWordCount: true
ShowRssButtonInSectionTermList: true
UseHugoToc: true
math: true
cover:
    image: "<image path/url>" # image path/url
    alt: "<alt text>" # alt text
    caption: "<text>" # display caption under cover
    relative: false # when using page bundles set this to true
    hidden: true # only hide on current single page
editPost:
    URL: "https://github.com/<path_to_repo>/content"
    Text: "Suggest Changes" # edit text
    appendFilePath: true # to append file path to Edit link
---

## 1 Timeline Order
> Summarize the literature reviewed in chronological order.
>

+ ### 2023

📖【_**EMNLP 2023 - Main**_】- Sparse Low-rank Adaptation of Pre-trained Language Models (*Tsinghua University, The University of Chicago*)

{{< box default >}}
**Subject:** Adaptive Rank Selection

+ **Problem:** Standard LoRA uses a fixed, inflexible rank (hyperparameter $ r
 $), requiring expensive manual tuning.
+ **Core Idea:** Make the rank learnable rather than fixed.
+ **Mechanism:**
    - **Gating:** Introduces an optimizable gating unit to the low-rank matrices.
    - **Optimization:** Uses proximal gradient methods to update the gates.
    - **Dynamics:** Prunes less important ranks during training automatically.
+ **Result:** Eliminates discrete rank search; the model discovers its own optimal rank structure.
{{< /box >}}

![SoRA](1-sora.png)

+ ### 2024

🍰【_**Arxiv 2024**_】- MixLoRA: Enhancing Large Language Models Fine-Tuning with LoRA-based Mixture of Experts (*Sichuan University, Purdue University, Emory University, Nanyang Technology University*)

{{< box default >}}
对多LoRA微调的一系列方法总结的非常好，仅记录，不分析。
{{< /box >}}

![MixLoRA](2-mixlora.png)



+ ### 2025

🍰【_**Arxiv 2025**_】- ShareLoRA: Parameter Efficient and Robust Large Language Model Fine-tuning via Shared Low-Rank Adaptation (*University of California*)

{{< box default >}}
许多工作在致力于降低LoRA的可训练参数，但是显著地降低参数会导致收敛变慢，且不充分的缩减方式也会导致模型容易过拟合。并且，许多现有的PEFT方法在fine-tuning之后**难以维持跨不同域的鲁棒性**。
+ 作者发现：低秩矩阵A和B不需要在不同层被独特的配置，也可以实现最优的性能。
+ 作者方法：在所有层之间共享矩阵A或B，同时保持对应项（比如qkv）在每一层中不同即可。

探索了各种共享的策略，当然重要的发现是这样去共享也不会损失性能。
{{< /box >}}