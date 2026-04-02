---
name: llmfit
description: Detect your hardware specs and find the best-fitting LLM models for local inference. Use when: (1) 用户想知道哪些 LLM 模型可以在他们的硬件上运行 (2) 用户询问本地模型性能、内存需求或硬件兼容性 (3) 用户想要针对本地部署的编码/推理/聊天模型推荐 (4) 用户需要为特定模型需求规划硬件升级 (5) 用户想比较模型在质量、速度、适配度和上下文维度的表现
---

## Instructions

1. Run `llmfit system` to detect hardware specs first, install `llmfit` if not available using [llmfit@github](https://github.com/AlexsJones/llmfit#install)
2. Use the appropriate subcommand based on user intent:

| User Intent | Command |
|---|---|
| Find best models overall | `llmfit fit --perfect -n 10` |
| Coding models | `llmfit recommend --use-case coding --json --limit 5` |
| Reasoning models | `llmfit recommend --use-case reasoning --json --limit 5` |
| Multimodal/vision models | `llmfit recommend --use-case multimodal --json --limit 5` |
| Search specific model | `llmfit search "<query>"` |
| Model details | `llmfit info "<model-name>"` |
| Hardware planning | `llmfit plan "<model-name>" --context 8192` |
| Full table output | `llmfit --cli` |

3. For scripting/automation, always add `--json` flag for machine-readable output
4. Use `--memory=<SIZE>` to override GPU VRAM when auto detection fails (e.g., `--memory=32G`)
5. Use `--max-context <tokens>` to cap context length for memory estimation
6. Use `--force-runtime mlx|llamacpp|vllm` to override automatic runtime selection

## Key Concepts

### Fit Levels
- **Perfect**: Model fits well in GPU memory with headroom
- **Good**: Fits but may use CPU+GPU hybrid
- **Marginal**: Tight fit, borderline runnable
- **Too Tight**: Not enough memory to run

### Scoring Dimensions (0-100)
- **Quality**: Parameter count, model family, quantization quality
- **Speed**: Estimated tok/s based on hardware and model size
- **Fit**: Memory utilization efficiency (sweet spot: 50-80%)
- **Context**: Context window capability

### Runtime Providers
llmfit auto-detects installed runtimes:
- Ollama (default port 11434)
- llama.cpp (GGUF downloads from HuggingFace)
- MLX (Apple Silicon)
- Docker Model Runner (port 12434)
- LM Studio (port 1234)

## CLI Reference

```sh
# System info
llmfit system
llmfit --json system

# Fit analysis
llmfit fit --perfect -n 5
llmfit fit --good -n 10

# Recommendations
llmfit recommend --json --limit 5
llmfit recommend --use-case coding --json --limit 3
llmfit recommend --use-case reasoning --json --limit 3

# Search & info
llmfit search "qwen 8b"
llmfit info "Mistral-7B"

# Hardware planning
llmfit plan "Qwen/Qwen3-4B-MLX-4bit" --context 8192
llmfit plan "Qwen/Qwen3-4B-MLX-4bit" --context 8192 --quant mlx-4bit --json

# REST API
llmfit serve --host 0.0.0.0 --port 8787
```

## Output Interpretation

When presenting results to the user, focus on:
1. **Score** (composite, higher is better)
2. **tok/s** (estimated generation speed)
3. **Mem %** (memory utilization - 50-80% is ideal)
4. **Quant** (quantization method - higher quality = larger size)
5. **Context** (max context window in tokens)
6. **Mode** (GPU / MoE / CPU+GPU / CPU)
