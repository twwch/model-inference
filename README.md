# llama.cpp 推理加速

1. 编译llama.cpp

```bash

cd llama.cpp

make

```

2. 下载模型

```bash
cd ..
python -m venv venv
source venv/bin/activate
pip install huggingface-hub

pip install -r ./requirements/requirements-convert_hf_to_gguf.txt

export HF_ENDPOINT=https://hf-mirror.com

huggingface-cli download --resume-download  Qwen/Qwen2.5-1.5B-Instruct --local-dir models/Qwen/Qwen2.5-1.5B-Instruct
```

3. 转换模型

```bash
cd llama.cpp
python convert_hf_to_gguf.py ../models/Qwen/Qwen2.5-1.5B-Instruct --outfile ../models/Qwen/Qwen2.5-1.5B-Instruct/model.gguf
```

4. 模型量化

```bash
./llama-quantize ../models/Qwen/Qwen2.5-1.5B-Instruct/model.gguf ../models/Qwen/Qwen2.5-1.5B-Instruct/model-q4_0.gguf Q4_0
```

5. 推理部署

```bash
./llama-server -m ../models/Qwen/Qwen2.5-1.5B-Instruct/model-q4_0.gguf --host 0.0.0.0 --port 8100 --n-gpu-layers 999
```

6. 推理调用

```bash
curl --location --request POST 'http://127.0.0.1:8100/v1/chat/completions' \
--header 'Content-Type: application/json' \
--data-raw '{
    "messages": [
        {
            "role": "user",
            "content": "你好"
        }
    ]
}'
```

# vllm 推理加速

1. 安装vllm

```bash
python -m venv venvvllm 
source venvvllm/bin/activate
pip install vllm
```

2. 部署模型

```bash
python -m vllm.entrypoints.openai.api_server --model ./models/Qwen/Qwen2.5-1.5B-Instruct --host 0.0.0.0 --port 8100 
```
