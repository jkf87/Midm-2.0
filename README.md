
<p align="center">
<br>
    <picture>
        <img src="./assets/midm_logo_en.png" width="35%" style="margin: 40px auto;">
    </picture>
</br>

<p align="center">
🤗 <a href="">Mi:dm 2.0 Models</a> |
📜 Mi:dm 2.0 Technical Report* |
📕 Mi:dm 2.0 Technical Blog*
</p>

<p align="center"><sub>*To be released soon</sub></p>

<br>

## News 📢

- 🔜 _(Coming Soon!) GGUF format model files will be available soon for easier local deployment._  
- ⚡️`2025/07/04`: Released Mi:dm 2.0 Model collection on Hugging Face🤗.
<br>

## Table of Contents

- ___Overview___
    - [Mi:dm 2.0](#midm-20)
    - [Quickstart](#quickstart)
    - [Evaluation](#evaluation)
- ___Usage___
    - [Run on Your Local Machine](#run-on-your-local-machine)
        - [llama.cpp](#llamacpp)
        - [LM Studio](#lm-studio)
        - [Ollama](#ollama)
    - [Deployment](#deployment)
    - [Tutorials](#tutorials)
- ___More Information___
    - [Limitation](#limitation)
    - [License](#license)
    - [Contact](#contact)

<br>

## Overview

### Mi:dm 2.0


Mi:dm 2.0 is a __"Korea-centric AI"__ model developed with KT's proprietary technology. __"Korea-centric AI"__ refers to a model that thoroughly internalizes the unique values, cognitive frameworks, and commonsense reasoning intrinsic to Korean society. It is not simply about processing and responding in Korean; it is about the profound understanding that reflects and respects the socio-cultural fabric of Korean norms and values.

The newly introduced Mi:dm 2.0 model comes in two versions:

* **Mi:dm 2.0-Mini** is a 2.3B parameter Dense small model, designed for seamless use in environments such as on-device settings and low-end GPUs. It was created by pruning and distilling the Base model.

* **Mi:dm 2.0-Base** has 11.5B parameters and is designed to balance model size and performance by expanding an 8B scale model using the DuS (Depth-up Scaling) method. It's a practical model that can be applied to various real-world services, considering both performance and versatility.


> [!Note]
> Neither the pre-training nor the post-training data includes KT users' data.

<br>

### Quickstart

Here is the code snippet to run conversational inference with the model:

```python
import torch
from transformers import AutoModelForCausalLM, AutoTokenizer, GenerationConfig

model_name = "K-intelligence/Midm-2.0-Mini-Instruct"

model = AutoModelForCausalLM.from_pretrained(
    model_name,
    torch_dtype=torch.bfloat16,
    trust_remote_code=True,
    device_map="auto"
)
tokenizer = AutoTokenizer.from_pretrained(model_name)
generation_config = GenerationConfig.from_pretrained(model_name)

prompt = "KT에 대해 소개해줘"

# message for inference
messages = [
    {"role": "system", 
     "content": "Mi:dm(믿:음)은 KT에서 개발한 AI 기반 어시스턴트이다."},
    {"role": "user", "content": prompt}
]

input_ids = tokenizer.apply_chat_template(
    messages,
    tokenize=True,
    add_generation_prompt=True,
    return_tensors="pt"
)

output = model.generate(
    input_ids.to("cuda"),
    generation_config=generation_config,
    eos_token_id=tokenizer.eos_token_id,
    max_new_tokens=128,
    do_sample=False,
)
print(tokenizer.decode(output[0]))
```

> [!NOTE]
> The `transformers` library should be version `4.45.0` or higher.

<br>

### Evaluation

#### English


<table>
  <thead>
    <tr>
      <th colspan="2"><b>Benchmark</b></th>
      <th>Exaone-3.5-2.4B-inst</th>
      <th>Qwen3-4B</th>
      <th>Mi:dm 2.0-Mini-inst</th>
      <th>Exaone-3.5-7.8B-inst</th>
      <th>Qwen3-14B</th>
      <th>Llama-3.1-8B-inst</th>
      <th>Mi:dm 2.0-Base-inst</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td rowspan="1"><b>Instruction Following</b></td>
      <td><b>IFEval</b></td>
      <td align="center">81.1</td>
      <td align="center">79.7</td>
      <td align="center">73.6</td>
      <td align="center">83.6</td>
      <td align="center">83.9</td>
      <td align="center">79.9</td>
      <td align="center"><b>84.0</b></td>
    </tr>
    <tr>
      <td rowspan="4"><b>Reasoning</b></td>
      <td><b>BBH</b></td>
      <td align="center">46.4</td>
      <td align="center">79.0</td>
      <td align="center">44.5</td>
      <td align="center">50.1</td>
      <td align="center">83.4</td>
      <td align="center">60.3</td>
      <td align="center"><b>77.7</b></td>
    </tr>
    <tr>
      <td><b>GPQA</b></td>
      <td align="center">28.1</td>
      <td align="center">39.8</td>
      <td align="center">26.6</td>
      <td align="center">33.1</td>
      <td align="center">49.8</td>
      <td align="center">21.6</td>
      <td align="center"><b>33.5</b></td>
    </tr>
    <tr>
      <td><b>MuSR</b></td>
      <td align="center">49.7</td>
      <td align="center">58.5</td>
      <td align="center">51.7</td>
      <td align="center">51.2</td>
      <td align="center">57.7</td>
      <td align="center">50.3</td>
      <td align="center"><b>51.9</b></td>
    </tr>
    <tr>
      <td><b>Avg.</b></td>
      <td align="center">41.4</td>
      <td align="center">59.1</td>
      <td align="center">40.9</td>
      <td align="center">44.8</td>
      <td align="center">63.6</td>
      <td align="center">44.1</td>
      <td align="center"><b>54.4</b></td>
    </tr>
    <tr>
      <td rowspan="2"><b>Mathematics</b></td>
      <td><b>GSM8K</b></td>
      <td align="center">82.5</td>
      <td align="center">90.4</td>
      <td align="center">83.1</td>
      <td align="center">81.1</td>
      <td align="center">88.0</td>
      <td align="center">81.2</td>
      <td align="center"><b>91.6</b></td>
    </tr>
    <tr>
      <td><b>MBPP+</b></td>
      <td align="center">59.8</td>
      <td align="center">62.4</td>
      <td align="center">60.9</td>
      <td align="center">79.4</td>
      <td align="center">73.4</td>
      <td align="center">81.8</td>
      <td align="center"><b>77.5</b></td>
    </tr>
    <tr>
      <td rowspan="3"><b>General Knowledge</b></td>
      <td><b>MMLU-pro</b></td>
      <td align="center">-</td>
      <td align="center">-</td>
      <td align="center">-</td>
      <td align="center">40.7</td>
      <td align="center">70.5</td>
      <td align="center">47.6</td>
      <td align="center"><b>53.3</b></td>
    </tr>
    <tr>
      <td><b>MMLU</b></td>
      <td align="center">59.5</td>
      <td align="center">73.3</td>
      <td align="center">56.5</td>
      <td align="center">69.0</td>
      <td align="center">82.7</td>
      <td align="center">70.7</td>
      <td align="center"><b>73.7</b></td>
    </tr>
    <tr>
      <td><b>Avg.</b></td>
      <td align="center">59.5</td>
      <td align="center">73.3</td>
      <td align="center">56.5</td>
      <td align="center">54.8</td>
      <td align="center"><b>76.6</b></td>
      <td align="center">59.2</td>
      <td align="center">63.5</td>
    </tr>
  </tbody>
</table>


<br>

#### Korean

<table>
  <thead>
    <tr>
      <th colspan="2"><b>Benchmark</b></th>
      <th>Exaone-3.5-2.4B-inst</th>
      <th>Qwen3-4B</th>
      <th>Mi:dm 2.0-Mini-inst</th>
      <th>Exaone-3.5-7.8B-inst</th>
      <th>Qwen3-14B</th>
      <th>Llama-3.1-8B-inst</th>
      <th>Mi:dm 2.0-Base-inst</th>
    </tr>
  </thead>
  <tbody>
    <!-- Comprehension -->
    <tr>
      <td rowspan="5"><b>Comprehension</b></td>
      <td><b>K-Prag*</b></td>
      <td align="center">68.7</td>
      <td align="center">73.9</td>
      <td align="center">69.5</td>
      <td align="center">73.5</td>
      <td align="center"><b>86.7</b></td>
      <td align="center">59.9</td>
      <td align="center">86.5</td>
    </tr>
    <tr>
      <td><b>K-Refer-Hard*</b></td>
      <td align="center">58.5</td>
      <td align="center">56.7</td>
      <td align="center">55.4</td>
      <td align="center">61.9</td>
      <td align="center"><b>74.0</b></td>
      <td align="center">48.6</td>
      <td align="center">70.8</td>
    </tr>
    <tr>
      <td><b>Ko-Best</b></td>
      <td align="center">87.2</td>
      <td align="center">91.5</td>
      <td align="center">80.5</td>
      <td align="center">92.0</td>
      <td align="center">93.9</td>
      <td align="center">77.4</td>
      <td align="center"><b>95.2</b></td>
    </tr>
    <tr>
      <td><b>Ko-Sovereign*</b></td>
      <td align="center">38.0</td>
      <td align="center">43.5</td>
      <td align="center">42.5</td>
      <td align="center">44.0</td>
      <td align="center">52.0</td>
      <td align="center">31.5</td>
      <td align="center"><b>53.0</b></td>
    </tr>
    <tr>
      <td><b>Avg.</b></td>
      <td align="center">62.5</td>
      <td align="center">66.6</td>
      <td align="center">61.9</td>
      <td align="center">67.2</td>
      <td align="center"><b>76.8</b></td>
      <td align="center">51.5</td>
      <td align="center">76.1</td>
    </tr>
    <tr>
      <td rowspan="5"><b>Reasoning</b></td>
      <td><b>Ko-Winogrande</b></td>
      <td align="center">60.3</td>
      <td align="center"><b>67.5</b></td>
      <td align="center">61.7</td>
      <td align="center">64.6</td>
      <td align="center">77.2</td>
      <td align="center">40.1</td>
      <td align="center">75.1</td>
    </tr>
    <tr>
      <td><b>Ko-Best</b></td>
      <td align="center">64.1</td>
      <td align="center"><b>69.2</b></td>
      <td align="center">64.5</td>
      <td align="center">60.3</td>
      <td align="center">75.4</td>
      <td align="center">26.0</td>
      <td align="center">73.0</td>
    </tr>
    <tr>
      <td><b>LogicKor*</b></td>
      <td align="center"><b>7.4</b></td>
      <td align="center">5.6</td>
      <td align="center">7.7</td>
      <td align="center">8.6</td>
      <td align="center">6.4</td>
      <td align="center">2.4</td>
      <td align="center">8.6</td>
    </tr>
    <tr>
      <td><b>HRM8K*</b></td>
      <td align="center">38.5</td>
      <td align="center"><b>56.7</b></td>
      <td align="center">39.9</td>
      <td align="center">49.7</td>
      <td align="center">64.5</td>
      <td align="center">30.9</td>
      <td align="center">52.9</td>
    </tr>
    <tr>
      <td><b>Avg.</b></td>
      <td align="center">36.7</td>
      <td align="center"><b>43.8</b></td>
      <td align="center">37.4</td>
      <td align="center">39.5</td>
      <td align="center">48.8</td>
      <td align="center">19.8</td>
      <td align="center">44.8</td>
    </tr>
    <!-- Society & Culture -->
    <tr>
      <td rowspan="5"><b>Society & Culture</b></td>
      <td><b>K-Refer*</b></td>
      <td align="center">64.0</td>
      <td align="center">53.6</td>
      <td align="center">66.4</td>
      <td align="center">71.6</td>
      <td align="center">72.4</td>
      <td align="center">43.2</td>
      <td align="center"><b>89.6</b></td>
    </tr>
    <tr>
      <td><b>K-Refer-Hard*</b></td>
      <td align="center">67.1</td>
      <td align="center">42.9</td>
      <td align="center">61.4</td>
      <td align="center">69.3</td>
      <td align="center">65.7</td>
      <td align="center">36.4</td>
      <td align="center"><b>86.4</b></td>
    </tr>
    <tr>
      <td><b>Ko-Sovereign*</b></td>
      <td align="center">44.4</td>
      <td align="center">35.8</td>
      <td align="center">36.7</td>
      <td align="center">46.9</td>
      <td align="center"><b>49.8</b></td>
      <td align="center">33.8</td>
      <td align="center">56.3</td>
    </tr>
    <tr>
      <td><b>HAERAE*</b></td>
      <td align="center">61.3</td>
      <td align="center">50.6</td>
      <td align="center">70.8</td>
      <td align="center">72.9</td>
      <td align="center">68.4</td>
      <td align="center">49.5</td>
      <td align="center"><b>81.5</b></td>
    </tr>
    <tr>
      <td><b>Avg.</b></td>
      <td align="center">59.2</td>
      <td align="center">45.7</td>
      <td align="center">58.8</td>
      <td align="center">65.2</td>
      <td align="center">64.1</td>
      <td align="center">40.7</td>
      <td align="center"><b>78.4</b></td>
    </tr>
    <!-- Reasoning (Domain) -->
    <tr>
      <td rowspan="3"><b>Reasoning (Domain)</b></td>
      <td><b>KMMLU</b></td>
      <td align="center">43.5</td>
      <td align="center">50.6</td>
      <td align="center">45.1</td>
      <td align="center">52.6</td>
      <td align="center">55.4</td>
      <td align="center">33.0</td>
      <td align="center"><b>57.3</b></td>
    </tr>
    <tr>
      <td><b>Ko-Sovereign*</b></td>
      <td align="center">42.4</td>
      <td align="center">42.5</td>
      <td align="center">42.4</td>
      <td align="center">45.6</td>
      <td align="center">54.7</td>
      <td align="center">36.7</td>
      <td align="center"><b>58.0</b></td>
    </tr>
    <tr>
      <td><b>Avg.</b></td>
      <td align="center">43.0</td>
      <td align="center">46.5</td>
      <td align="center">43.8</td>
      <td align="center">49.1</td>
      <td align="center">55.1</td>
      <td align="center">34.8</td>
      <td align="center"><b>57.7</b></td>
    </tr>
    <!-- Instruction Following -->
    <tr>
      <td rowspan="3"><b>Instruction Following</b></td>
      <td><b>Ko-IFEval*</b></td>
      <td align="center">65.4</td>
      <td align="center">75.9</td>
      <td align="center">73.3</td>
      <td align="center">69.1</td>
      <td align="center"><b>83.6</b></td>
      <td align="center">60.1</td>
      <td align="center">82.0</td>
    </tr>
    <tr>
      <td><b>Ko-MTBench</b></td>
      <td align="center">74.0</td>
      <td align="center">63.0</td>
      <td align="center">74.0</td>
      <td align="center">79.6</td>
      <td align="center">71.0</td>
      <td align="center">57.0</td>
      <td align="center"><b>89.7</b></td>
    </tr>
    <tr>
      <td><b>Avg.</b></td>
      <td align="center">68.9</td>
      <td align="center">69.4</td>
      <td align="center">73.6</td>
      <td align="center">74.4</td>
      <td align="center">77.3</td>
      <td align="center">58.5</td>
      <td align="center"><b>85.9</b></td>
    </tr>
  </tbody>
</table>

`*` indicates KT proprietary evaluation resources.

<br>

## Usage

### Run on Your Local Machine

#### llama.cpp

We assume that you have installed [llama.cpp](https://github.com/ggml-org/llama.cpp).

1. Download Mi:dm 2.0 model in GGUF format from our Hugging Face repository.
    ```
    huggingface-cli download K-intelligence/Midm-2.0-Base-Instruct-GGUF \
        --include "Midm-2.0-Base-Instruct-GGUF-BF16.gguf" \
        --local-dir .
    ```

2. To run the model, use the command below:

    **llama-cli**
    ```
    MODEL_PATH=./Midm-2.0-Base-Instruct-GGUF-BF16.gguf
    
    llama-cli \
        -m ${MODEL_PATH} \
        -p "KT에 대해 소개해줘" \ # customize your prompt here
        --temp 0.7
    ```

    **llama-server**
    ```
    python3 -m llama_cpp.server \
      --model ${MODEL_PATH} \
    ```

#### LM Studio

You can easily use Mi:dm 2.0 in **LM Studio** with just a few clicks.
1. Install [LM Studio](https://lmstudio.ai/).
2. In **Discover** tab of LM Studio, search for and download the model.
Alternatively, you can manually download Mi:dm 2.0 model in GGUF format using `huggingface-cli`.
    ```bash
    huggingface-cli download K-intelligence/Midm-2.0-Base-Instruct-GGUF \
        --include "Midm-2.0-Base-Instruct-GGUF-BF16.gguf" \
        --local-dir .
    ```
    If you downloaded the model manually, move the file to the LM Studio model directory (~/.lmstudio/models/) with the following structure:
    ```
    ~/.lmstudio/models/
    └── midm/
        └── midm-2.0-base/
            └── Midm-2.0-Base-Instruct-GGUF-BF16.gguf
    ```
3. In **Chat** tab, run the model in conversational mode.
4. To use OpenAI-compatible API endpoints, start the server from **Developer** tab.
5. To connect Mi:dm 2.0 with the MCP (Model Context Protocol) server for tool usage, go to **Chat** tab in LM Studio and update **Program** settings by adding or modifying the `mcp.json` file as follows:
    ```json
       {
         "mcpServers": {
           "server-time": {
             "command": "uvx",
             "args": [
               "mcp-server-time",
               "--local-timezone=Asia/Seoul"
             ]
           },
           "ddg-search": {
             "command": "uvx",
             "args": [
               "duckduckgo-mcp-server"
             ]
           }
         }
       }
    ```

#### Ollama

We provide a guide for running Mi:dm 2.0 via [Ollama](https://ollama.com/) in our [Open WebUI tutorial](./tutorial/03_open-webui#-2-connect-midm-via-ollama). Once the setup is complete, you can run Mi:dm 2.0 using the following command:
```bash
ollama run midm-2.0:base
```

<br>

### Deployment

To serve Mi:dm 2.0 using [vLLM](https://github.com/vllm-project/vllm)(`>=0.8.0`) with an OpenAI-compatible API:
```bash
vllm serve K-intelligence/Midm-2.0-Base-Instruct
```

<br>

<!--
### Quantization

We provide quantized variants of Mi:dm 2.0, including `Q8_0`, `Q4_K_M`, and `IQ4_XS` in [Mi:dm 2.0 collection]().
-->

<br>

### Tutorials
To help our end-users easily use Mi:dm 2.0, we have provided comprehensive tutorials on our [tutorial](./tutorial) page.
We provide several examples to help you get started with Mi:dm 2.0 quickly and easily:

| Tutorial                          | Description                                                                     | Link             |
|-----------------------------------|---------------------------------------------------------------------------------|------------------|
| 📘 Supervised Fine-Tuning         | Fine-tune Mi:dm 2.0 using `trl` and `SFTTrainer` with the SimpleQA-GenX2 dataset | [🔗](./tutorial/01_fine-tuning) |
| 🎯 Inference with Mi:dm 2.0-Mini  | Run Mi:dm 2.0-Mini with optimal generation settings                              | [🔗](./tutorial/02_inference) |
| 🧩 Plug Mi:dm 2.0 into Open WebUI | Integrate Mi:dm 2.0 with Open WebUI using Ollama and Explore MCP & RAG exercises | [🔗](./tutorial/03_open-webui) |
| 🗺️ Mi:dm 2.0 Prompt Examples      | A collection of prompting examples and use cases across key tasks and domains    | [🔗](./tutorial/04_prompt_examples) |

<br>

## More Information

### Limitation

* The training data for both Mi:dm 2.0 models consists primarily of English and Korean. Understanding and generation in other languages are not guaranteed.
  
* The model is not guaranteed to provide reliable advice in fields that require professional expertise, such as law, medicine, or finance.

* Researchers have made efforts to exclude unethical content from the training data — such as profanity, slurs, bias, and discriminatory language. However, despite these efforts, the model may still produce inappropriate expressions or factual inaccuracies.

<br>

### License

Mi:dm 2.0 is licensed under the [MIT License](./LICENSE).

<br>
 
<!--
### Citation
 
```
@misc{,
      title={}, 
      author={},
      year={2025},
      eprint={},
      archivePrefix={arXiv},
      primaryClass={cs.CL},
      url={}, 
}
```

<br>
-->

### Contact 
- Mi:dm 2.0 Technical Inquiries: midm-llm@kt.com