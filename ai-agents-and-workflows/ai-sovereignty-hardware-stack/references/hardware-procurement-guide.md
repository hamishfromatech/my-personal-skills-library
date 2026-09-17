# Hardware Procurement Guide

## GPU Vendor Comparison (2026)

### NVIDIA
- **Pros**: Best software ecosystem (CUDA), widest model compatibility, TensorRT-LLM optimization
- **Cons**: Highest price per GB VRAM, vendor lock-in risk, restricted export rules
- **Best for**: Enterprise deployment, maximum performance, teams with CUDA expertise

### AMD
- **Pros**: Competitive price/performance, ROCm improving rapidly, MI300X has highest single-GPU VRAM (192GB)
- **Cons**: Smaller software ecosystem, some models require porting
- **Best for**: Cost-conscious enterprises, high-VRAM single-model workloads

### Intel
- **Pros**: Arc GPUs budget-friendly, OpenVINO optimization, x86 integration
- **Cons**: Inferior raw performance for LLM inference, limited model support
- **Best for**: Edge deployments, ultra-budget setups, Intel-centric organizations

### Apple Silicon
- **Pros**: Unified memory architecture (up to 192GB on M2 Ultra), excellent per-watt performance
- **Cons**: macOS-only, limited to Metal-compatible frameworks
- **Best for**: Individual developers, creative professionals already in Apple ecosystem

## Used / Refurbished Market
- Tesla V100 32GB: ~$800–$1,200 (older but reliable for inference)
- Tesla P40 24GB: ~$400–$600 (budget option, no Tensor Cores)
- RTX A6000 48GB: ~$3,000–$4,000 (professional workstation card)

## NUC and Edge Hardware
- Intel NUC 13 Pro: ~$600 + storage (good for 7B quantized models)
- NVIDIA Jetson AGX Orin: ~$1,500 (ARM-based, 64GB unified memory)
- Orange Pi 5 Plus: ~$150 (hobbyist edge, 7B at Q4 barely)
