---
tags: []
parent: ""
collections:
    - 'Reasoning enhancement'
$version: 646
$libraryID: 1
$itemKey: L64WTLN3

---
## Paper List

#### 1、**Detecting hallucinations in large language models using semantic entropy**

[nature链接](https://www.nature.com/articles/s41586-024-07421-0)

[本地链接](C:%5CUsers%5Cdell%5CDesktop%5Cllm-paper%5C%E7%9F%A5%E8%AF%86%E5%B9%BB%E8%A7%89%5Cs41586-024-07421-0.pdf)

***（1） 文章信息***： 牛津大学 2024 Nature

***（2） 研究内容***：大型语言模型（LLMs）在生成文本时出现的“幻觉”现象，即模型生成不合理或与给定信息不符的内容。研究如何利用语义熵作为一种工具来检测知识幻觉和提高LLM生成文本的可靠性。

***（3） 摘要逻辑***：

大模型取得了令人印象深刻的效果，但依然存在知识幻觉----->

回答不可靠严重限制了LLMs在高严谨领域的应用--------->

监督学习、强化学习取得了一定的成功----------->

依然需要一种有效的方法检测幻觉，即使对于人类没见过或不知道答案的问题------->

为此提出了基于语义熵的不确定检测器，用于检测幻觉------------>

该方法相比之前克服了语义多样性的问题，且不需要先验知识。

> \[!CAUTION]
>
> 大模型幻觉定义：大模型生成无意义或不忠于源内容的输出，LLMs generating “content that is nonsensical or unfaithful to the provided source content”
>
> 本篇文章针对模型幻觉中的虚构问题，提出了基于语义熵的检测方法（熵高意味着不确定性高，相比于传统的基于token的熵计算，语义熵能够克服语义多样性的问题）

***（4） 主要步骤：***

*   设置模型的推理生成参数（temperature，top-k），采样模型对问题的多个输出回答

*   将输出根据语义进行分类（根据是否相互语义蕴含分为一类，使用某种NLI模型完成）

*   根据语义类别计算语义不确定性（语义熵）![\<img alt="" data-attachment-key="27X4U97Q" width="888" height="273" src="attachments/27X4U97Q.png" ztype="zimage">](attachments/27X4U97Q.png)**具体实例：**![\<img alt="image-20240701002355627" data-attachment-key="ZLSAIAEQ" width="866" height="548" src="attachments/ZLSAIAEQ.png" ztype="zimage">](attachments/ZLSAIAEQ.png)

#### 2、Task Oriented In-Domain Data Augmentation

[arxiv链接](https://arxiv.org/pdf/2406.16694)

[本地链接](C:%5CUsers%5Cdell%5CDesktop%5Cllm-paper%5C%E6%95%B0%E6%8D%AE%E5%90%88%E6%88%90%5CTPAIT.pdf)

***（1） 文章信息*** ： 清华大学 2024

***（2） 研究内容***：本篇文章针对大模型训练专业领域数据不足的问题，提出了一种名为TRAIT的任务导向域内数据增强框架，用于大型语言模型的持续预训练。

***（3） 摘要逻辑***：（研究动机）

LLMs在多个领域展现出优越的性能——>

为了实现在特殊领域更好的性能，LLMs通常在领域数据上进行continual pre-trained——>

然而，面临两个问题：首先领域数据稀缺；其次CP数据缺乏任务数据，对下游任务没有帮助——>

因此提出了TRAIT，该框架包含两部分：领域数据选择和任务导向的数据合成——>

将TRAIT进行实验，取得了提升

> \[!CAUTION]
>
> 相关引用文章：
>
> [Best Practices and Lessons Learned on Synthetic Data for Language Models](https://arxiv.org/pdf/2404.07503)

***（4） 主要步骤：***

1.  从general corpas里选择相关领域的数据语料，为此训练了一个FastText classifier，去判断语料库中的数据是否是目标领域的数据。（需要保证这些数据是否是具有教育价值的高质量数据）> \[!TIP]
    >
    > 1.  DSIR（[Data selection for language models via importance resampling. ](https://arxiv.org/abs/2302.03169)）, an importance sampling strategy for selecting in-domain data from general corpora, such that the selected data distibutionally similar with the in-domain data.
    >
    > 2.  replay（[Simple and scalable strategies to continually pre-train large language models.](https://arxiv.org/abs/2403.08763)） happens when the continual pre-training data contain a certain amount of pre-training data, which is crucial to keep LLM’s generality (e.g., instruction following) after training.

2.  任务导向的passage data合成，合成的主要过程为：首先每个passage包含几个来自下游任务的问题，其次对于每个问题会生成一段推荐答案的文本，最后还会生成一个enlightenment paragraph用于强调上述问题之间的关系![\<img alt="image-20240715155042233" data-attachment-key="GUIG2NX2" width="1438" height="523" src="attachments/GUIG2NX2.png" ztype="zimage">](attachments/GUIG2NX2.png)

***（5） 实验细节***

1.  训练的模型及参数![\<img alt="image-20240715160034168" data-attachment-key="VZMWUSTC" width="1426" height="402" src="attachments/VZMWUSTC.png" ztype="zimage">](attachments/VZMWUSTC.png)

2.  训练时使用的数据量ads领域选择了15B的数据，math领域选择了5.5B的数据

#### 3、 A Comprehensive Study of Knowledge Editing for Large Language Models

<https://arxiv.org/pdf/2401.01286>

*   文章信息： 浙江大学 2024

<https://www.53ai.com/news/qianyanjishu/2124.html>

*   知识编辑前沿技术调研

#### 4、Buffer of Thoughts: Thought-Augmented Reasoning with Large Language Models

<https://arxiv.org/pdf/2406.04271>

***（1） 文章信息：*** 北京大学 2024

***（2） 研究内容：***

#### 5、 OntoFact: Unveiling Fantastic Fact-Skeleton of LLMs via Ontology-Driven Reinforcement Learning

[本地链接](file:///C:/Users/dell/Desktop/llm-paper/%E6%95%B0%E6%8D%AE%E5%90%88%E6%88%90/ontofact.pdf)

***（1） 文章信息***： 东南大学 2024 AAAI

***（2） 研究内容：*** 大模型存在严重的知识幻觉，为了进行模型的幻觉检测，本文提出了一种知识图谱驱动的幻觉检测框架，旨在更加全面合理的评估模型的知识薄弱分布。

***（3） 摘要逻辑***： （研究动机）

大模型在信息检索方面展现出优越的性能，但依然存在内在幻觉，容易产生与事实不符的错误响应---->

关键在于大模型训练在海量数据上，事实分布不清晰、不可靠---->

主流的事实检测任务采用QA范式，检测模型是否回答正确----->

但研究主要集中特定领域获取测试案例，限制了对缺失知识的全面观察和分析---->

为此提出了基于知识图的自适应框架OntoFact，帮助检测大模型的未知知识---->

并提出了ORL（本体驱动的强化学习策略）指导生成模型容易出错的test case------>

此外提出了HFD帮助模型对yes和no的偏好问题------>

使用了五种图谱，对32个LLMs进行评估

> \[!CAUTION]
>
> 了解大模型内在的事实分布非常重要，现有的事实检测方法主要分为文本驱动和KG驱动两种，文本驱动会包含很多冗余信息，使用三元组简洁精准。此外，之前的方法量级都比较小。现有的数据中本身也有很多的脏数据

***（4） 主要方法***：

> \[!CAUTION]
>
> 在方法介绍之前，需要先声明两个概念，ontology-level graph（本体图）和instance-level graph（实例图）
>
> ![\<img alt="image-20240716211002596" data-attachment-key="DXIEAVXB" width="501" height="267" src="attachments/DXIEAVXB.png" ztype="zimage">](attachments/DXIEAVXB.png)

1.  测试实例初始化： 从ontology-view kg出发，得到一个本体子图，并对本体子图中的每个triples生成一个文本template，在将instance对应到ontology中，得到初始的问题集$Q_I$

2.  测试实例更新：使用了两个等级的强化学习智能体，来帮助模型在图谱上进行游走。

3.  测试实例执行:相比于直接评估，该方式先对一个三元组构造反例，然后看模型是否对反例回答正确，如果一直no，则最终使用原三元组进行评估。

***（5） 缺点和不足***：本篇文章仅停留在事实检测，且针对的是单跳的简单逻辑，问题形式单一。

#### 6、 Complex Reasoning over Logical Queries on Commonsense Knowledge Graphs

[本地链接](%22C:%5CUsers%5Cdell%5CDesktop%5Cllm-paper%5C%E6%95%B0%E6%8D%AE%E5%90%88%E6%88%90%5CCOM2.pdf%22)

> \[!IMPORTANT]
>
> 值得注意的是，这篇文章为三种等级的图提供了指导思想：instance-level，ontology-level，structure-level。
>
> 此外，对于数据合成的文章，很多都设置了相应的验证集，以显示自己方法的优越性能。

***（1） 文章信息***： CSE 2024 ACL

***（2） 研究内容***： 事件常识推理（Event commonsense reasoning） 需要能力去推理多跳复杂关系，为了解决这方面的数据稀缺问题，提出了COM2，通过在KG上采样问题

***（3） 摘要逻辑***：（研究动机）

事件常识推理需要强大的关系推理能力，以及推断这种关系背后的隐含上下文---->

然而数据稀缺导致模型面临很大的挑战----->

为此提出了COM2，通过在CSKG中采样多跳的逻辑查询生成数据(结合人工设计的规则)---->

实验展示了COM2对模型逻辑推理能力的提升

> \[!CAUTION]
>
> 1.  图结构的稀疏性限制了编码多个推理跳跃的潜在问题的数量
> 2.  KG中的三元组转化为文本形式时存在错误
> 3.  在多跳设置中，多个推理跳跃的实体必须以连贯的方式一起表达

***（4） 主要方法***：

![\<img alt="image-20240718165534393" data-attachment-key="G2NYEJGB" width="810" height="755" src="attachments/G2NYEJGB.png" ztype="zimage">](attachments/G2NYEJGB.png)

1.  预处理过程：对CSKG进行合并节点和三元组合理性过滤，已解决图稀疏、含有错误的问题
2.  Query采样：按下列图中的几中特定的图结构进行生成。对于一个给定的图结构，从答案实体开始先序遍历free variables 和 anchor entities ； 最后再进行一个后续遍历从anchor entity开始搜索可能的全部答案实体。
3.  形成描述语言（文本）： 对anchor entity文本描述（基于规则，基于gpt）；对关系文本描述（基于规则、模板）

![\<img alt="image-20240718165554065" data-attachment-key="LYLXEQIF" width="810" height="694" src="attachments/LYLXEQIF.png" ztype="zimage">](attachments/LYLXEQIF.png)

***（5） 缺点和不足***：该文章意图基于初始规则图指导图搜索，从而指导合成数据，但它所依赖的KG比较特殊，并且为了将结构数据转化为文本数据，人工设计了很多复杂的规则。

#### 7、 MuSR: Testing the Limits of Chain-of-thought with Multistep Soft Reasoning

[本地链接](C:%5CUsers%5Cdell%5CDesktop%5Cllm-paper%5C%E6%95%B0%E6%8D%AE%E5%90%88%E6%88%90%5CMuSR.pdf)

***（1） 文章信息***： 德克萨斯大学奥斯汀分校 2024 ICLR Spotlight

***（2） 研究内容***： 模型的推理能力得到了不断的成长提高，但现有的评测集显得不够充分，为此本篇文章研究如何控制合成合理的多跳推理数据。

***（3） 摘要逻辑***：（研究动机）

大模型（使用COT等）在多种任务上须取得优越的能力，但依然在复杂的推理中表现不足----->

然而，模型能力不断提升的同时评测推理数据集没有太大的提升，需要新的评测数据集-------->

为此，提出了MuSR，一个用于评测模型多步推理的新数据集----->

该数据的两个主要特点：1. 一种新颖的神经符号合成到自然生成的合成算法；2. 数据集实例是与现实世界推理领域相对应的自由文本叙述； ----->

在这个数据集上评估了一系列 LLM 和提示技术，并描述了思路链等技术在执行稳健推理方面仍然存在的差距。

> \[!IMPORTANT]
>
> 核心的动机在于：
>
> 1.  What is lacking is a benchmark involving both ***sophisticated natural language and sophisticated reasoning***
>
> 2.  对于模型本身无法解决的问题，仅靠prompt无法让模型生成类似的复杂问题
>
> 3.  现有的推理数据集，一部分没有自然语言文本，另一部分没有结合常识和多步推理，还有些对于LLMs没有挑战（通过rule-based方法能够轻松解决）

***（4） 主要方法***：

![\<img alt="image-20240722170436255" data-attachment-key="8J7PW22P" width="1415" height="989" src="attachments/8J7PW22P.png" ztype="zimage">](attachments/8J7PW22P.png)

选择了三个领域进行合成，以谋杀故事为例，我们的构建方法包括三部分

1.  树状模板的构建：对于每个领域都包含一个上层的领域知识集合，对于一个问题，首先利用LLM提示模型生成具体的gold facts，包含解决该问题的核心fact（会对原始的facts集合进行补充，以提高多样性和叙述性）

2.  推理树构建：对于每个fact——$f_i$，都会以其为root node，不断递归的生成（调用LLM）新的facts

    1.  S(T) 场景特定的
    2.  C(T) commonsense，人们普遍认可的

    这些facts能够递归的推导出原始的$f_i$

3.  根据S（T） 利用LLM生成合适的描述性文本（最终问题）

***（5） 缺点和不足***：

该方法优势在于利用了结构化数据进行引导，以控制推理长度，但整个过程多次调用了LLMs，代价很高，生成的数据集规模较小（几百），不能用于生成训练数据。且整个推理树的构建也调用了LLM，这对于最终生成的数据具有不可控性。

#### 8、Scaling Synthetic Data Creation with 1,000,000,000 Personas

***（1） 文章信息***： Tencent AI lab

***（2） 研究内容***： 合成数据在训练大型语言模型 (LLM) 方面越来越受到重视，然而大规模创建合成数据并非易事：虽然我们可以轻松扩大合成数据的数量，但很难确保其多样性也能扩大。为此，本文提出了一种以人为本的高效、多样的大规模数据合成方法。

***（3） 研究动机***：

相关工作总结，现有的数据合成方法主要可以分为两种范式：

1.  实例驱动（instance-driven）:利用种子语料库（即基于种子语料库中的实例创建新的实例）来使数据合成提示多样化。

    *   代表工作：self-instruct，MetaMath
    *   不足：合成数据的多样性主要来自于种子实例，很难真正超越种子语料。考虑到大多数实际场景中种子语料的规模有限，很难扩大合成数据的规模。

2.  关键点驱动（key-point-driven）：这种方法通过精心挑选的综合关键点（或概念）列表使数据合成提示多样化，这些关键点（或概念）可以是主题、目标或我们期望合成数据包含的任何知识。

    *   代表工作：[GLAN](https://arxiv.org/abs/2402.13064)，[KPDDS](https://arxiv.org/abs/2403.02333)

    *   不足：除非局限于一个狭窄而特定的领域，否则通过列举不同粒度级别的所有关键点来整理一份综合列表实际上是不现实的。

> \[!CAUTION]
>
> 除了上述的按合成方法进行分类，还可以按数据类型和数据用途进行分类。
>
> 1.  数据类型：
>
>     *   通用指令数据合成（指令遵循能力）
>     *   领域指令数据合成（特定任务能力展示了一种使用 LLM 而不是人工来创建大量具有不同复杂程度的指令数据的方法。）
>
> 2.  数据用途：
>
>     *   预训练数据
>     *   SFT数据
>     *   评测数据

相比于上述的两类工作，受到以下观察的启发：只需在数据合成提示中添加一个角色，就可以引导 LLM 走向相应的视角，以创建独特的合成数据。

![\<img alt="image-20240805164032861" data-attachment-key="SDDFSJCW" width="1658" height="561" src="attachments/SDDFSJCW.png" ztype="zimage">](attachments/SDDFSJCW.png)

这篇文章可以利用 LLM 中封装的几乎所有视角来大规模创建多样化的合成数据，而不受种子语料库大小的限制。此外，人物角色可以与几乎任何类型的数据合成提示相结合，使它们普遍适用于各种数据合成场景。

***（4） 主要方法：***

*   首先创建合适的person hub：基于两种方法，一种是text to person，另一种是person to person。
*   其次利用三种prompt格式进行person-driven的数据合成。

![\<img alt="image-20240805163826523" data-attachment-key="YI4SH8L6" width="1652" height="1011" src="attachments/YI4SH8L6.png" ztype="zimage">](attachments/YI4SH8L6.png)

#### 9、 Key-Point-Driven Data Synthesis with its Enhancement on Mathematical Reasoning

***（1） 文章信息***： 微软 2024

***（2） 研究内容***:

#### 10、WizardLM: Empowering Large Language Models to Follow Complex Instructions（Evol-instruct）

<https://mingchao.wang/LO8uep9D/#wizardlm-evol-instruct>

***（1） 文章信息***： 微软，北京大学 ICLR 2024

***（2） 研究内容***：展示了一种使用 LLM 而不是人工来创建大量具有不同复杂程度的指令数据的方法，本文的指令进化分为深度进化和广度进化。

***（3） 研究动机***：

使用开放指令训练的大模型取得了巨大的成功，但这样的指令数据是稀缺的，人工标注耗费大量的时间和精力。并且，**人工以及self-instruct很难生成高复杂度的指令**。

为此，本文展示了一种使用 LLM 而不是人工来创建大量具有不同复杂程度的指令数据的方法，本文的指令进化分为深度进化和广度进化。

通过使用混合所有生成的指令数据来微调 LLaMA，取得了匹配90%GPT的能力。

***（4） 主要方法***：

选取一个种子数据集，然后对其进行多轮的迭代演化，每次演化只使用前一次的结果数据，且先用LLM演化指令，然后再用LLM生成回复。

![\<img alt="image-20240813203028589" data-attachment-key="ANII7NMW" width="902" height="787" src="attachments/ANII7NMW.png" ztype="zimage">](attachments/ANII7NMW.png)

1.  指令进化

    *   进化后的指令必须是合理的、人能够理解、并且人能够对该指令写出response。
    *   指令的难度应该逐步增加，保证整个指令池中简单、中等、困难的指令数量是相对均衡的。
    *   对于深度进化，为了限制难度的增加，限制每次进化指令最多增加10到20个单词。
    *   对于广度进化，会要求其生成的指令的难度与原始指令是基本相同的。

    <!---->

    *   深度演化：add constraints, deepening, concretizing, increase reasoning steps, complicate input。
    *   广度演化：mutation

2.  生成response在此调用LLM

3.  进化消除四种方式进行进化消除：

    *   使用 ChatGPT 判断新生成的指令与原指令相比是否有新的信息
    *   对进化后的指令使用 LLM 很难生成 response

***（5） 实验设置***：

与Alpaca and Vicuna 进行对比，Alpaca 使用的数据（使用 self-instruct \[32] 生成）和 Vicuna 使用的 70k ShareGPT（由真实用户共享）。

***（6） 方法缺陷***：

1.  难以生成大模型无法解决的问题（先生成问题，再生成答案，对于模型解决不了的问题没办法解决）
2.  对于生成的数据的演化不可控，生成啥样的数据全靠大模型
3.  每个指令需要调用2-3次的大模型
