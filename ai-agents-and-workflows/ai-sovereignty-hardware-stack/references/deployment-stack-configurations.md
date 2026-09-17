# Deployment Stack Configurations

## Ollama (Personal / Single-User)

```bash
# Install
brew install ollama  # macOS
# or download from ollama.ai for Linux/Windows

# Pull models
ollama pull llama3.1:70b
ollama pull qwen3:8b
ollama pull nomic-embed-text

# Run with custom parameters
ollama run llama3.1:70b --num-gpu 50 --num-ctx 32768

# API access (OpenAI-compatible)
curl http://localhost:11434/api/generate -d '{
  "model": "llama3.1:70b",
  "prompt": "Explain quantum computing",
  "stream": false
}'
```

## vLLM (Small Team / Multi-User)

```bash
# Install
pip install vllm

# Serve with API
vllm serve "meta-llama/Llama-3.1-70B-Instruct" \
  --tensor-parallel-size 2 \
  --max-model-len 32768 \
  --quantization awq

# OpenAI-compatible endpoint at http://localhost:8000/v1
```

## TensorRT-LLM (Enterprise / Maximum Performance)

```bash
# Requires NVIDIA GPU, Docker
# Build engine from checkpoint
trllm-build --checkpoint_dir ./llama-3.1-70b \
  --output_dir ./trt-engines/llama-3.1-70b \
  --dtype bfloat16 \
  --max_batch_size 32

# Serve with Triton
docker run --gpus all -p 8000:8000 \
  -v ./trt-engines:/models \
  nvcr.io/nvidia/tritonserver:trtllm-python-py3 \
  tritonserver --model-repository=/models
```

## llama.cpp (Edge / Low-Resource)

```bash
# Build
make -C llama.cpp -j LLAMA_CUDA=1

# Quantize
./llama.cpp/quantize ./models/llama-3.1-70b.gguf ./models/llama-3.1-70b-Q4_K_M.gguf Q4_K_M

# Run server
./llama.cpp/server -m ./models/llama-3.1-70b-Q4_K_M.gguf \
  -c 32768 -ngl 50 --port 8080
```
