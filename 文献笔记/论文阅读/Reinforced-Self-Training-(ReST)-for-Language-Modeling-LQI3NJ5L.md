---
tags: []
parent: 'Reinforced Self-Training (ReST) for Language Modeling'
collections:
    - self-improvement
$version: 12070
$libraryID: 1
$itemKey: LQI3NJ5L

---
## Reinforced Self-Training (ReST) for Language Modeling

#### ℹ️基本信息

| <!-- --> |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **期刊:**\*\*\*\*\*\*（发表年份:**2023**）**作者:**Caglar Gulcehre; Tom Le Paine; Srivatsan Srinivasan; Ksenia Konyushkova; Lotte Weerts; Abhishek Sharma; Aditya Siddhant; Alex Ahern; Miaosen Wang; Chenjie Gu; Wolfgang Macherey; Arnaud Doucet; Orhan Firat; Nando de Freitas**机构:**deepmind                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| \*\*摘要: \*\**Reinforcement learning from human feedback (RLHF) can improve the quality of large language model's (LLM) outputs by aligning them with human preferences. We propose a simple algorithm for aligning LLMs with human preferences inspired by growing batch reinforcement learning (RL), which we call Reinforced Self-Training (ReST). Given an initial LLM policy, ReST produces a dataset by generating samples from the policy, which are then used to improve the LLM policy using offline RL algorithms. ReST is more efficient than typical online RLHF methods because the training dataset is produced offline, which allows data reuse. While ReST is a general approach applicable to all generative learning settings, we focus on its application to machine translation. Our results show that ReST can substantially improve translation quality, as measured by automated metrics and human evaluation on machine translation benchmarks in a compute and sample-efficient manner.* |
| \*\*Local Link: \*\*[Gulcehre 等 - 2023 - Reinforced Self-Training (ReST) for Language Modeling.pdf](zotero://open-pdf/0_NZ42HE6I)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |


#### 💡一、研究内容

一种简单的算法使 LLM 与人类偏好对齐 [ReST](https://zhida.zhihu.com/search?content_id=232925629\&content_type=Article\&match_order=1\&q=ReST\&zhida_source=entity)（Reinforced Self-Training），ReST 通过生成和使用离线数据进行训练，从而使得 LLM 与人类偏好保持一致。

给定一个初始 LLM 策略，ReST 能够根据该策略生成数据集，然后该数据集基于离线 RL 算法被反过来提高 LLM 策略。ReST 比典型的在线 RLHF 方法更有效，因为训练数据集是离线生成的，这允许数据重用。

ReST 算法将典型 RL pipeline 的数据集增长（Grow）和策略改进（Improve）解耦成两个单独的离线阶段。

#### 📜二、研究背景

#### 🔬三、主要方法

#### 🚩四、实验结果

#### 📌五、知识点

<https://zhuanlan.zhihu.com/p/693582342>

**online/offline** 解决的问题：**“是否可以持续交互收集新数据”**

*   **在线强化学习**：在学习过程中，智能体直接与环境实时交互，每一步选择动作、接收环境反馈（奖励和下一个状态），并立即用这些新鲜数据更新其策略或价值函数。这意味着学习策略会直接影响到实际行为，并且环境需要是可交互的。

*   **离线强化学习**：事先从一个固定的数据集（通常是之前交互产生的）中学习，不与环境实时互动。这个数据集可以是通过专家演示、历史记录或其他代理的行为收集的。离线学习的目标是从这些静态数据中最大化学习效率，而无需进一步探索环境。

#### 🔬六、思考
