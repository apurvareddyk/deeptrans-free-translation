# DeepTrans: Reinforcement Learning for Free Translation

### Think First, Then Translate

![image](https://github.com/user-attachments/assets/6beaca04-63ea-45bd-b317-6fa9d3299398)


This repository is inspired by the paper [Deep Reasoning Translation via Reinforcement Learning](https://arxiv.org/abs/2504.10187), which proposes **DeepTrans**, a novel LLM-based framework for high-quality free translation using explicit reasoning and reinforcement learning. The model uses structured thought generation, reward shaping, and an LLM-based judge to train without parallel data.

---

## Overview

**DeepTrans** redefines how machine translation is approached. Instead of relying on paired translations or fine-tuning on synthetic data, it teaches an LLM to "think" through the task and refine its translation based on qualitative feedback. This reward-driven reasoning approach mirrors how humans improve translation through feedback and internal reflection.

---

## Table of Contents

* [What is Free Translation?](#what-is-free-translation)
* [Architecture Overview](#architecture-overview)
* [Training Pipeline](#training-pipeline)
* [Reward System](#reward-system)
* [Key Results](#key-results)
* [Ablation Insights](#ablation-insights)
* [Links](#links)
* [Conclusion](#conclusion)
* [Acknowledgment](#acknowledgment)

---

## What is Free Translation?

Free translation focuses on conveying the **meaning, tone, and cultural context** of the original text, rather than sticking to word-for-word fidelity. This is crucial for:

* Literary works
* Marketing copy
* Cultural idioms and metaphors

Traditional machine translation models often fail to capture this depth, especially when trained with limited or synthetic data.

---

## Architecture Overview

DeepTrans uses the [Qwen2.5–7B-Instruct](https://github.com/QwenLM/Qwen) model as a base. It generates structured output:

```
<think> Here's my reasoning... </think>
Here’s the translated sentence.
```

This explicit reasoning process helps guide the model to more nuanced and high-quality translations.

---

## Training Pipeline

Training consists of two key phases:

### Phase 1: Supervised Fine-Tuning (SFT)

* Purpose: Teach the model the expected output format.
* Data: Small dataset generated with DeepSeek-R1.
* Time: \~1 GPU hour

### Phase 2: Reinforcement Learning (GRPO)

* Uses **Generalized PPO (GRPO)** with a reward signal from **DeepSeek-v3**.
* Feedback focuses on structure, reasoning, and translation quality.
* Time: \~2000 GPU hours

![image](https://github.com/user-attachments/assets/ca4f5d30-9fb6-4b14-8c8e-2735e128d667)


---

## Reward System

The reward model (DeepSeek-v3) acts as a **LLM judge**, providing feedback on:

* `rformat`: Proper structure and cleanliness of output
* `rthought`: Depth and helpfulness of the reasoning section
* `rtrans`: Fluency, fidelity, and appropriateness of the final translation

The final reward is calculated as:

```
rall = rtrans + α * rthought  (if rformat is correct)
```

![image](https://github.com/user-attachments/assets/6ac0a78b-68eb-4784-b6a1-9c165b529529)


---

## Key Results

* **+16.3% improvement** over Qwen2.5–7B on the MetaphorTrans dataset using GEA5.
* Outperforms SFT-based baselines (like DRT-7B).
* Competitive with larger models like GPT-40 and QwQ-32B.

> 📊 GEA5 is a GPT-4 based metric that assesses fluency and literary quality, making it more aligned with human evaluations than BLEU or ROUGE.

---

## Ablation Insights

* **No `rthought` → Poor reasoning or missing thoughts**
* **Coarse reward scale (3-point) → Stylization bias + poor exploration**
* **100-point fine-grained reward → Best learning and generalization**
* **Penalizing long thoughts → Slows performance (bad idea)**

These ablations reveal how critical **reward engineering** is to guide language model behavior.

![image](https://github.com/user-attachments/assets/bd615979-3a1f-45d3-bbbf-0be3e26e1602)


---

## Links

*  Medium Article: [DeepTrans: AI That Learns to Think Before It Translates](https://medium.com/@apurva.karne/deeptrans-ai-that-learns-to-think-before-it-translates-cd6ebe3c0793)
*  YouTube Presentation: [Link](https://youtu.be/6He5grucJTQ)
*  Slideshare Slide Deck: [Link](https://www.slideshare.net/slideshow/deeptrans-ai-that-learns-to-think-before-it-translates/278856888)

---

## Conclusion

**DeepTrans** is a step toward AI systems that can reason and reflect—offering not just translations, but true understanding. Its ability to learn from qualitative feedback instead of paired data has implications far beyond translation, including:

* Code generation
* Long-form writing
* Policy interpretation

---

## Acknowledgment

This repository and article are inspired by the paper:
[Deep Reasoning Translation via Reinforcement Learning](https://arxiv.org/abs/2504.10187) by Jiaan Wang, Fandong Meng, and Jie Zhou.
