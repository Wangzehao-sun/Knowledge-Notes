## 微调及推理代码文件

***conda环境：`factory`***

#### 微调数据准备

***一、生成微调数据***

路径：`/home/hzchen/wzh/Train/LLaMA-Factory/data/data_A`

文件：

1. `convert.py` : 将训练数据根据转化为训练的数据格式，并保存为`ijicai.jsonl`文件

   ```json
   {
   	"instruction": "输入",
   	"output":"输出"
   }
   ```

***二、将微调数据加入到llama-factory的路径中***

路径：`/home/hzchen/wzh/Train/LLaMA-Factory/data/dataset_info.json`

使用方法可以阅读：`/home/hzchen/wzh/Train/LLaMA-Factory/data/README_zh.md`

#### 模型微调训练

使用方法详见仓库原教程，特别注意理解训练给定的需要设置的参数的意义。

```shell
llamafactory-cli webui
```

针对glm4的一个微调的参数示例：`/home/hzchen/wzh/Train/LLaMA-Factory/config/2024-07-07-15-45-37.yaml`

#### 模型推理评测

推理代码路径：`/home/hzchen/wkh`

1. 脚本文件：`/home/hzchen/wkh/inference.sh`

2. 执行的python文件：`/home/hzchen/wkh/inference.py`

   

   ```shell
   CUDA_VISIBLE_DEVICES=7 python inference.py \
   --model_name_or_path "/home/hzchen/wzh/LLaMA-Factory/newmodels/IJICAI/llama3_train"\
   --save_dir  "/home/hzchen/wkh/res"\
   --data_path "/home/hzchen/wkh/data_A/dev.csv" \
   --batch_size 16 \
   --template glm4 \
   ```

> [!IMPORTANT]
>
> 需要特别注意的是，为了保证微调的效果，对每个模型要保证微调时和推理时prompt一致。

3. 推理结果：`/home/hzchen/wkh/res/home_res.txt`

   推理结果会保存在上述路径中，由于模型模型输出不同，所以需要自行修改evaluate()准确率计算函数