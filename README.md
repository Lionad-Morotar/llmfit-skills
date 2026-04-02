# llmfit-skills

Claude Code skill for [llmfit](https://github.com/AlexsJones/llmfit) — detect your hardware and find the best-fitting LLM models for local inference.

## Installation

```bash
npx skills add https://github.com/Lionad-Morotar/llmfit-skills
```

## Usage

```sh
/llmfit {your requirements}
```

Example prompts:

- "What models can I run locally for coding?"
- "Find the best reasoning model for my hardware"
- "Compare Qwen3-Coder vs Mistral on my machine"
- "Plan hardware for running Qwen3-4B at 8K context"

If your IDE does not support SlashCommand, prefix your prompt:

```plaintext
Use the llmfit skill, {your requirements}
```

This ensures the skill triggers reliably. Without the prefix, activation depends on keyword matching with the skill description.
