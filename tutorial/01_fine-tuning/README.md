# 📘 Hands-on Guide to Fine-Tuning Mi:dm 2.0 using TRL

## 📑 Table of Contents

1. [Introduction](#-introduction)  
2. [Setup Environment](#-setup-environment)  
3. [Prepare and Load the Dataset](#-prepare-and-load-the-dataset)  
4. [Modeling with PEFT (LoRA)](#-modeling-with-peft-lora)  
5. [Training the Model](#-training-the-model)  
6. [Full Fine-Tuning (Optional)](#-full-fine-tuning-optional)

---

## 🧭 Introduction

This is the guide code that allows you to finetune our publicly available Mi:dm 2.0 with new downstream tasks.  
Mi:dm 2.0 can be used for a variety of tasks such as question answering and summarization, etc. without any additional training.  
However, if you want to customise the model for your application, you may need to fine-tune it to your data to get better results.  
Please modify it according to the availability of your resources.

This tutorial notebook walks you thorugh how to fine-tune Mi:dm 2.0 using Hugging Face [TRL](https://huggingface.co/docs/trl/index), [Transformers](https://huggingface.co/docs/transformers/index) & [datasets](https://huggingface.co/docs/datasets/index). In the Notebook, we are going to:

1. Setup environment  
2. Prepare and load the dataset  
3. Fine-tune LLM using `trl` and the `SFTTrainer`

---

## 🧰 Setup Environment

Our first step is to install Hugging Face Libraries and Pytorch, including trl, transformers and datasets.  
TRL is a cutting-edge library designed for post-training foundation models using advanced techniques like Supervised Fine-Tuning (SFT), Proximal Policy Optimization (PPO), and Direct Preference Optimization (DPO).

> ⚠️ **Note:** Installing Flash Attention may take a while (approximately 10–45 minutes), depending on your system performance.  
> It's an optional optimization that can significantly speed up attention computations during training.

```python
# Install Pytorch & other libraries
%pip install -r requirements.txt
import torch; assert torch.cuda.get_device_capability()[0] >= 8, 'Hardware not supported for Flash Attention'
# install flash-attn
!pip install ninja packaging
!MAX_JOBS=4 pip install flash-attn==2.7.3 --no-build-isolation
```

---

## 📂 Prepare and Load the Dataset

In our example we’ll use an already existing dataset called [SimpleQA-GenX2](https://huggingface.co/datasets/K-intelligence/KT-Simple-QA), which contains samples of natural language instructions collected in-house, including various forms of simple Korean QA.

With the latest release of `trl` we now support popular instruction and conversation dataset formats.

### Supported Formats
* conversational format
```json
{"messages": [{"role": "system", "content": "You are..."}, {"role": "user", "content": "..."}, {"role": "assistant", "content": "..."}]}
```

* instruction format
```json
{"prompt": "<prompt text>", "completion": "<ideal generated text>"}
```

```python
from datasets import load_dataset

# Load dataset from the hub
dataset = load_dataset("K-intelligence/KT-Simple-QA")['train']
dataset = dataset.train_test_split(test_size=0.01)

# save datasets to disk 
dataset["train"].to_json("train_dataset.json", orient="records")
dataset["test"].to_json("test_dataset.json", orient="records")
```

---

## 🧠 Modeling with PEFT (LoRA)

### Load Mi:dm 2.0 Model and Tokenizer

```python
import torch
from transformers import AutoTokenizer, AutoModelForCausalLM, BitsAndBytesConfig
from trl import setup_chat_format

model_id = "K-intelligence/Midm-2.0-Mini-Instruct"

model = AutoModelForCausalLM.from_pretrained(
    model_id,
    device_map="auto",
    attn_implementation="flash_attention_2",
    torch_dtype=torch.bfloat16
)
tokenizer = AutoTokenizer.from_pretrained(model_id)
```

### Apply LoRA for PEFT

```python
from peft import get_peft_model, LoraConfig, TaskType

peft_config = LoraConfig(
    r=8,
    lora_alpha=16,
    lora_dropout=0.1,
    bias="none",
    task_type=TaskType.CAUSAL_LM,
)
model = get_peft_model(model, peft_config)
```

---

## 🏋️ Training the Model

### Load Dataset from Disk

```python
from datasets import load_dataset
dataset = load_dataset("json", data_files="train_dataset.json", split="train")
```

### Set Training Arguments

```python
from transformers import TrainingArguments

args = TrainingArguments(
    output_dir="save_dir",
    num_train_epochs=1,
    per_device_train_batch_size=1,
    gradient_accumulation_steps=1,
    gradient_checkpointing=True,
    logging_steps=10,
    save_strategy="epoch",
    learning_rate=2e-5,
    bf16=True,
    max_grad_norm=1.0,
    warmup_ratio=0.03,
    lr_scheduler_type="constant",
    push_to_hub=False,
    report_to="tensorboard",
)
```

### Set Up the `SFTTrainer` and Train

```python
from trl import SFTTrainer

trainer = SFTTrainer(
    model=model,
    args=args,
    train_dataset=dataset
)

trainer.train()
```

---

## 🧬 Full Fine-Tuning (Optional)

If you have enough computational resources and want to fine-tune the entire Mi:dm 2.0 model — not just adapters — you can follow the steps below.

> ⚠️ **Note:** Dataset and library setup is the same as in the PEFT method above.

### Load Full Mi:dm 2.0 Model

```python
from transformers import AutoTokenizer, AutoModelForCausalLM

model = AutoModelForCausalLM.from_pretrained(
    model_id,
    device_map="auto",
    attn_implementation="flash_attention_2",
    torch_dtype=torch.bfloat16
)
tokenizer = AutoTokenizer.from_pretrained(model_id)
```

### Training Arguments

```python
args = TrainingArguments(
    output_dir="save_dir",
    num_train_epochs=1,
    per_device_train_batch_size=1,
    gradient_accumulation_steps=1,
    gradient_checkpointing=True,
    logging_steps=10,
    save_strategy="epoch",
    learning_rate=2e-5,
    bf16=True,
    max_grad_norm=1.0,
    warmup_ratio=0.03,
    lr_scheduler_type="constant",
    push_to_hub=False,
    report_to="tensorboard",
)
```

### Train with SFTTrainer

```python
from trl import SFTTrainer

trainer = SFTTrainer(
    model=model,
    args=args,
    train_dataset=dataset,
    processing_class=tokenizer
)

trainer.train()
```
