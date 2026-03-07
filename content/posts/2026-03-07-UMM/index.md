---
title: 'The Evolution of Unified Multimodal Models'
date: '2026-03-07T14:51:21+08:00'
# weight: 1
# aliases: ["/first"]
tags: ["Flash Card", "Unified Multimodal Models"]
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
    URL: "https://github.com/MilknoCandy/milknocandy.github.io/tree/main/content"
    Text: "Suggest Changes" # edit text
    appendFilePath: true # to append file path to Edit link
---

## 1 Timeline Order
> Summarize the literature reviewed in chronological order.

+ ### 2026

🍰【_**Arxiv 2026**_】WeMMU: Enhanced Bridging of Vision-Language Models and Diffusion Models via Noisy Query Tokens (*<ins>MoE Key Laboratory of Brain-inspired Intelligent Perception and Cognition - University of Science and Technology of China</ins>* *, ZheJiang University, The Hong Kong University of Science and Technology*)

{{< box default >}}
**Subject:** Bridging Pre-trained VLMs and Diffusion Models for UMMs
+ **Problem:** Existing methods (MetaQuery) <mark>performs alignment via learnable queries</mark>, but suffer from poor task generalization. They require retraining in the early stage for significantly different task types.
+ **Core Idea:** Probabilistic Expert Bridge (from Bagel) samples Noisy Query Tokens.
+ **Mechanism:**
    - **Noisy Query Tokens:** Sample tokens from the standard normal distribution $N(0, I)$ at each training step to learn a robust distributed intermediate representation space instead of task-specific features.
    - **Probabilistic Expert Bridge:** Freeze VLM core parameters, add a parallel generative pathway, follow the division of labor (VLM for understanding, Diffusion Model for generation), and use Position MLP for feature alignment and spatial cue injection.
    - **VAE Branch:** Inject VAE fine-grained features into VLM via a linear projection layer to fuse high-level semantics ans low-level visual details, reducing the Diffusion Models's burden.
    - **Progressive Training:** Adopt a four-stage curriculum training strategy, flexibly switch between contrastive/conditional flow matching loss, and gradually upgrade resolution and task complexity.
+ **Results:** Though the performance is not SOTA, it alleviates task generalization collapse of UMMs, facilitates stable cross-task continual learning and retains fine-grained image details.
{{< /box >}}

![WeMMU](1_WeMMU.png)