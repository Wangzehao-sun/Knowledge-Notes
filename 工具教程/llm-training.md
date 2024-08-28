## Trainer

全面复现**大模型训练**过程

[TOC]



#### 一、数据准备过程

* **Dataset**

模型加载的数据集类，主要样式为：

```python
DatasetDict({
    test: Dataset({
        features: ['input', 'output'],
        num_rows: 1273
    }),
    eval: Dataset({
        ...
    })
})
```

可以使用`load_dataset`函数加载目标数据集，支持：文件名，文件夹名，huggingface远程等

* **load_dataset**

  使用样例：

  ```
  raw_datasets = load_dataset(
              "json",
              data_files=data_files,
              cache_dir=model_args.cache_dir,
              **dataset_args,
          )
  ```

  主要参数：

  1. `path`: 声明文件路径，或文件类型，[json,text...]
  2. `data_files`:源数据文件的路径
  3. `split` : 要加载数据的哪个拆分。如果为None，将返回DatasetDict。如果给定，将返回单个数据集Dataset。
  4. `streaming` : 如果设置为True，则不会下载数据文件。相反，它在数据集上进行迭代时逐渐流式传输数据。在这种情况下，将返回一个IterableDataset或IterableDatasetDict
  5. `use_auth_token`
  6. `cache_dir`
  7. `num_proc` : 进行数据加载处理的进程个数 

* **Tokenizer**

  加载后的数据需要进一步的预处理，包括分词、映射，截断、填充等。不同的模型有不同的tokenizer方法，一般会在模型路径中自带。encoding 主要包含了两个步骤： - 1. tokenization: 对文本进行分词 - 2. convert_tokens_to_ids：将分词后的token 映射为数字。

  * 加载方式：

  ```python
  tokenizer = AutoTokenizer.from_pretrained(model_args.model_name_or_path,
                                            **tokenizer_kwargs)
  #声明tokenize处理函数，然后再用map方法统一快速应用到整个数据集上
  def tokenize_function(example):
      return tokenizer(example["sentence1"], example["sentence2"], truncation=True)
  
  tokenized_datasets = raw_datasets.map(tokenize_function, batched=True)
  ```

  

  * 返回类型：

  ```python
  'input_ids',tokens映射为的对应数字
  'attention_mask',是否进行注意力掩码，1 代表了需要attention，0代表不需要attention。
  'labels',用于计算loss的真实输出，当进行sft时往往将不需要计算loss的token的label设置为-100
  'token_type_ids':用于指示是否为同一个句子
  ```

  

  * tokenizer() 方法的主要参数：

    1. `sequences` : 需要处理的字符串或列表对象

    2. `padding` : 是否需要填充，以及填充长度 ，[“max_length”, ”longest”, “True”]

       > `model_inputs = tokenizer(sequences, padding="max_length", max_length=8)`

    3. `truncation` : 是否将输入进行截断，会根据模型的最大输入长度，以及自定义的最大长度进行截断

    4. `return_tensors` : tokenizer 返回结果的类型，["pt", ”tf”, ”np”]

    5. `add_special_tokens` : 指示是否需要在编码后的序列中添加特殊标记

    6. `padding_side` : 进行填充的方向[“left”, “right”]

    7. `pad_token(pad_token_id)` : 一般的tokenizer中不会声明padtoken是什么，需要预先设定好

       ```python
       tokenizer.pad_token = tokenizer.eos_token
       tokenizer.pad_token_id = tokenizer.eos_token_id
       ```

  * map() 方法的主要参数：

    ```python
    lm_datasets = raw_datasets.map(
                    encode_function,
                    batched=batched,
                    num_proc=data_args.preprocessing_num_workers,
                    load_from_cache_file=not data_args.overwrite_cache,
                    desc="Tokenizing and reformatting instruction data",
                )
    ```

    1. `encode_function`: 声明好的处理函数
    2. `batched` : 是否进行批处理，如果为true，则处理函数处理的example是多个，否则为单个的例子

* **DataCollatorForSeq2Seq**

  用于对原始数据集进行前处理，得到做特定任务的数据形式。

  ![](C:\Users\dell\AppData\Roaming\Typora\typora-user-images\image-20240326184820302.png)

  - 根据第一序列预测第二序列，第二序列即为`label`，一个batch内多条第二序列长度不同时，用`-100`进行补齐。在计算交叉熵时，`-100`位置的损失为0不用考虑

#### 二、模型参数

1. `use_auto_token` : 要用作远程文件的HTTP承载授权令牌。 如果为True，则将使用在运行huggingface-cli登录时生成的令牌。

2. `trust_remote_code `: 这个选项只能设置为“True”，以便你信任的存储库并在其中已经阅读了代码，因为它将在本地执行 Hub 上的代码。

3. `torch_dtype` : ["auto", "bfloat16", "float16", "float32"]。

4. `cache_dir` : 是否保存在cache中。

5. `use_flash_attn` : 是否在训练过程中使用flash-attention

   > Flash Attention的主要目的是**加速和节省内存**，主要贡献包括：
   >
   > 1. 计算softmax时候不需要全量input数据，可以分段计算；
   > 2. 反向传播的时候，不存储attention matrix (N^2的矩阵)，而是只存储softmax归一化的系数。

#### 三、训练参数

1. `gradient_checkpointing` : 部分保留（或者不保留）中间网络的feature_map，而是在反向传播的时候重新计算，用时间换空间的一种优化现存的方式方式。这种技术的核心思想是在前向传播过程中保存梯度，然后在反向传播时仅计算和更新必要的梯度，而不是更新模型中所有参数的梯度。

2. `learning_rate` :  学习率

3. `gradient_clipping` : 设置阈值防止梯度爆炸

4. `weight_decay` :  0.01 

5. `warmup_ratio` : 0.01 

6. `lr_scheduler_type` :  cosine

   <img src="C:\Users\dell\AppData\Roaming\Typora\typora-user-images\image-20240403161240100.png" alt="image-20240403161240100" style="zoom:80%;" />

7. `nproc_per_node` : 每个物理节点上面运行的进程数，常对应想要运行的GPU 个数，因此取值小于物理节点上GPU 个数。

8. `warmup_ratio(steps)` : 定义了训练开始时学习率的预热（warm-up）阶段所占的比例。这个阶段的目的是逐渐将学习率从较低的初始值增加到设定的目标学习率，从而帮助模型训练更加稳定，避免在初期由于学习率过高而导致的梯度更新剧烈，可能引起的训练不稳定。

9. `fsdp` : 并行化技术

#### 四、 模型推理参数

https://huggingface.co/docs/transformers/main/en/main_classes/text_generation#transformers.GenerationConfig

1. `n` : 生成候选文本的数量，默认为1
2. `best of` ：从`best_of`个生成文本中选取得分最高的一个。`1`表示不进行多次生成，直接选取生成的文本。默认为1
3. `presence_penalty` ： 出现惩罚系数，值越高，模型越倾向于生成包含新词的文本，避免重复之前已经提到的内容。默认为0
4. `frequency_penalty` ： 频率惩罚系数，值越高，模型越倾向于减少频繁出现的词汇的使用。默认为0
5. `tempratue`： 温度参数，低温度使模型更倾向于高概率的预测，而高温度增加输出的多样性。
6. `top_k` ： 在生成文本时，只考虑概率最高的k个词。例如，设置top-k为10可能使模型只从概率最高的10个词中选择下一个词。-1表示不限制
7. `top_p` ： 也称为nucleus sampling，它根据概率累积分布选择词汇，确保所选词汇的累积概率至少为p（如0.9）
8. `max_new_tokens` ： 需要模型生成的最大token数
9. `length_penalty` ： 长度惩罚系数,影响生成文本长度的权重，`1`表示不惩罚生成的文本长度。
10. `repetition_penalty`： 重复惩罚系数，这个系数控制模型生成的文本避免重复。默认为1，不进行惩罚
11. `logprobs` ： 返回每个生成的token的log概率。`None`表示不返回log概率。
12. `prompt_logprobs`： 返回提示词的log概率。`None`表示不返回提示词的log概率。
13. `skip_special_tokens`: 是否跳过特殊token。`True`表示生成文本中不包括特殊token。

```python
params_dict = {
        "n": 1,
        "best_of": 1,
        "presence_penalty": 1.0,
        "frequency_penalty": 0.0,
        "temperature": temperature,
        "top_p": top_p,
        "top_k": -1,
        "repetition_penalty": repetition_penalty,
        "use_beam_search": False,
        "length_penalty": 1,
        "early_stopping": False,
        "stop_token_ids": [151329, 151336, 151338],
        "ignore_eos": False,
        "max_tokens": max_new_tokens,
        "logprobs": None,
        "prompt_logprobs": None,
        "skip_special_tokens": True,
    }
```

**Greedy Search（贪婪搜索）**:

- **定义**: 在每一步生成过程中，选择具有最高概率的下一个词。
- **优点**: 简单快速，通常能生成语法正确的文本。
- **缺点**: 生成的文本可能缺乏多样性和创意，容易陷入重复。

**Beam Search（束搜索）**:

- **定义**: 在每一步生成过程中，保持多个（通常是固定数量的`k`个）最可能的部分序列，并在最终选择得分最高的序列。
- **优点**: 比贪婪搜索能生成更优质的文本，减少了一些局部最优解。
- **缺点**: 计算成本高，仍然可能生成重复的文本。

**Sampling（随机采样）**:

- **定义**: 按照模型预测的概率分布随机选择下一个词。
- **优点**: 生成的文本多样性高，能够产生更加丰富和有创意的内容。
- **缺点**: 可能会生成不连贯或语法错误的文本。

**Top-k Sampling（核采样）**:

- **定义**: 在每一步生成过程中，只从前`k`个最高概率的词中随机选择下一个词。
- **优点**: 保留随机性的同时，限制了选择范围，提高了生成文本的质量。
- **缺点**: `k`的选择需要权衡，`k`太小可能过于保守，`k`太大则效果类似随机采样。

**Top-p (Nucleus) Sampling（顶尖采样）**:

- **定义**: 在每一步生成过程中，只从累积概率达到`p`的前几个词中随机选择下一个词。
- **优点**: 动态调整选择范围，既保留了随机性又提高了生成质量。
- **缺点**: 参数`p`的选择需要权衡，`p`太小可能过于保守，`p`太大则效果类似随机采样。

**Temperature Scaling（温度缩放）**:

- **定义**: 调整模型预测的概率分布的平滑度，通过一个温度参数`T`控制。`T`越高，分布越平滑，选择越随机；`T`越低，分布越陡峭，选择越确定。
- **优点**: 通过调整温度参数，可以在随机性和确定性之间进行平衡。
- **缺点**: 需要对具体应用场景进行调优。
