
# 🎯 Inference with Mi:dm 2.0

This notebook shows how to run inference with the `K-intelligence/Midm-2.0-Mini-Instruct` model using Hugging Face Transformers.

## ✅ 1. Requirements

We strongly recommend using `transformers >= 4.45.0`

```python
pip install torch transformers accelerate
```

## 🔧 2. Load the Model, Tokenizer, and GenerationConfig

Load the model using `transformers` package.
To obtain the best generation quality, we highly recommend using the generation config provided with the model.

__Mi:dm 2.0-Mini Optimal Generation Config__

```json
{
  "bos_token_id": 0,
  "do_sample": true,
  "eos_token_id": [2,131301],
  "repetition_penalty": 1.0,
  "temperature": 0.8,
  "top_k": 20,
  "top_p": 0.75,
}
```

__Mi:dm 2.0-Base Optimal Generation Config__
```json
{
  "bos_token_id": 0,
  "do_sample": true,
  "eos_token_id": [2, 131301],
  "repetition_penalty": 1.05,
  "temperature": 0.8,
  "top_k": 20,
  "top_p": 0.7
}
```

```python
import torch
from transformers import AutoModelForCausalLM, AutoTokenizer, GenerationConfig

# Model name
model_name = "K-intelligence/Midm-2.0-Mini-Instruct"

# Load the model
model = AutoModelForCausalLM.from_pretrained(
    model_name,
    torch_dtype=torch.bfloat16,  
    trust_remote_code=True,
    device_map="auto",           # Automatically uses GPU if available
)

# Load the tokenizer
tokenizer = AutoTokenizer.from_pretrained(model_name)

# Load generation config (controls generation behavior)
generation_config = GenerationConfig.from_pretrained(model_name)

print("✅ Model and tokenizer loaded.")
```

## 💬 3. Prepare Your Prompt

```python
prompt = "KT에 대해 소개해줘"

messages = [
    {"role": "system", "content": "Mi:dm(믿:음)은 KT에서 개발한 AI 기반 어시스턴트이다."},
    {"role": "user", "content": prompt}
]
```

## 🧾 4. Tokenize Input with Chat Template

```python
input_ids = tokenizer.apply_chat_template(
    messages,
    tokenize=True,
    add_generation_prompt=True,
    return_tensors="pt"
).to(model.device)
```

## 🚀 5. Generate a Response

```python
output_ids = model.generate(
    input_ids=input_ids,
    generation_config=generation_config,  # Recommended config
    eos_token_id=tokenizer.eos_token_id,
    max_new_tokens=128
)
```

## 📤 6. Decode and Display

```python
response = tokenizer.decode(output_ids[0], skip_special_tokens=True)
print("Model Response:\n", response)
```
