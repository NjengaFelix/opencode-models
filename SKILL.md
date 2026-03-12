---
name: opencode-models
description: Configure and optimize model selection for different OpenCode tasks
license: MIT
compatibility: opencode
metadata:
  category: models
  type: community
---

## What I do

I help you configure and optimize AI model selection for OpenCode:

- **Model selection** - Choose the right model for your task
- **Provider setup** - Anthropic, OpenAI, Google, local models
- **Per-agent models** - Different models for different agents
- **Cost optimization** - Balance capability and cost
- **Performance tuning** - Fast vs smart model tradeoffs
- **Fallback configuration** - Handle provider outages

## When to use me

Load this skill when:

- **Choosing a model** - "What model should I use for this task?"
- **Multiple providers** - "Set up both Claude and GPT"
- **Cost concerns** - "Use cheaper models for simple tasks"
- **Speed vs quality** - "Fast model for planning, smart for coding"
- **Provider issues** - "Switch to backup when primary fails"
- **Local models** - "Use Ollama for offline development"

## Model Selection Guide

### By Task Type

| Task | Recommended Model | Why |
|------|------------------|-----|
| **Complex coding** | Claude Opus 4, GPT-4o | Best reasoning, large context |
| **General coding** | Claude Sonnet 4 | Good balance of speed/cost |
| **Quick tasks** | Claude Haiku 4, GPT-4o-mini | Fast, cheap, sufficient |
| **Planning/analysis** | Claude Sonnet 4 | Fast enough, good reasoning |
| **Documentation** | Claude Haiku 4 | Cheap, good writing |
| **Exploration** | Claude Haiku 4 | Speed matters most |

### By Provider

**Anthropic (Claude)**
- `anthropic/claude-opus-4` - Most capable
- `anthropic/claude-sonnet-4` - Best balance
- `anthropic/claude-haiku-4` - Fastest, cheapest

**OpenAI**
- `openai/gpt-4o` - Strong reasoning
- `openai/gpt-4o-mini` - Cost-effective
- `openai/o3-mini` - Reasoning focused

**Google**
- `google/gemini-2.5-pro` - Capable, good context
- `google/gemini-2.5-flash` - Fast, efficient

**Local (Ollama)**
- `ollama/codellama` - Code-focused local model
- `ollama/llama3.2` - General local model

## Basic Configuration

### Single Model Setup

```json
{
  "$schema": "https://opencode.ai/config.json",
  "model": "anthropic/claude-sonnet-4"
}
```

### Fast/Smart Split

```json
{
  "$schema": "https://opencode.ai/config.json",
  "model": {
    "default": "anthropic/claude-sonnet-4",
    "fast": "anthropic/claude-haiku-4",
    "smart": "anthropic/claude-opus-4"
  }
}
```

### Per-Agent Models

```json
{
  "agent": {
    "build": {
      "model": "anthropic/claude-sonnet-4"
    },
    "plan": {
      "model": "anthropic/claude-haiku-4"
    },
    "review": {
      "model": "anthropic/claude-opus-4"
    }
  }
}
```

## Provider Configuration

### Anthropic

```json
{
  "provider": {
    "anthropic": {
      "options": {
        "apiKey": "{env:ANTHROPIC_API_KEY}",
        "timeout": 300000
      }
    }
  }
}
```

### OpenAI

```json
{
  "provider": {
    "openai": {
      "options": {
        "apiKey": "{env:OPENAI_API_KEY}",
        "timeout": 300000
      }
    }
  }
}
```

### Multiple Providers

```json
{
  "provider": {
    "anthropic": {
      "options": {
        "apiKey": "{env:ANTHROPIC_API_KEY}"
      }
    },
    "openai": {
      "options": {
        "apiKey": "{env:OPENAI_API_KEY}"
      }
    }
  },
  "model": "anthropic/claude-sonnet-4"
}
```

## Local Models (Ollama)

### Setup Ollama

1. Install Ollama: https://ollama.ai/
2. Pull a model: `ollama pull codellama`
3. Configure OpenCode:

```json
{
  "provider": {
    "ollama": {
      "options": {
        "baseURL": "http://localhost:11434"
      }
    }
  },
  "model": "ollama/codellama"
}
```

### Best Local Models

- **CodeLlama** - Optimized for code
- **Llama 3.2** - Good general purpose
- **DeepSeek Coder** - Strong coding performance
- **Qwen 2.5 Coder** - Good multilingual code support

## Advanced Configuration

### Timeout Settings

```json
{
  "provider": {
    "anthropic": {
      "options": {
        "timeout": 600000,
        "chunkTimeout": 30000
      }
    }
  }
}
```

- `timeout` - Total request timeout (default: 300000ms)
- `chunkTimeout` - Timeout between response chunks

### Model-Specific Options

```json
{
  "agent": {
    "reasoner": {
      "model": "openai/gpt-5",
      "reasoningEffort": "high",
      "textVerbosity": "low"
    }
  }
}
```

## Cost Optimization Strategies

### 1. Tiered Model Approach

```json
{
  "model": {
    "default": "anthropic/claude-sonnet-4",
    "fast": "anthropic/claude-haiku-4",
    "smart": "anthropic/claude-opus-4"
  },
  "agent": {
    "explore": {
      "model": "anthropic/claude-haiku-4"
    },
    "plan": {
      "model": "anthropic/claude-haiku-4"
    },
    "build": {
      "model": "anthropic/claude-sonnet-4"
    },
    "complex": {
      "model": "anthropic/claude-opus-4"
    }
  }
}
```

### 2. Small Model for Simple Tasks

```json
{
  "model": "anthropic/claude-haiku-4",
  "small_model": "anthropic/claude-haiku-4"
}
```

Small model used for:
- Title generation
- Summary creation
- Quick lookups

### 3. Provider Fallback

```json
{
  "provider": {
    "anthropic": {
      "options": {
        "apiKey": "{env:ANTHROPIC_API_KEY}"
      }
    },
    "openai": {
      "options": {
        "apiKey": "{env:OPENAI_API_KEY}"
      }
    }
  },
  "model": "anthropic/claude-sonnet-4"
}
```

## Temperature Control

Control creativity vs determinism:

```json
{
  "agent": {
    "analyzer": {
      "temperature": 0.1
    },
    "creative": {
      "temperature": 0.7
    }
  }
}
```

- **0.0-0.2**: Very focused and deterministic
- **0.3-0.5**: Balanced
- **0.6-1.0**: Creative and varied

## Troubleshooting

### Model not found
```bash
opencode models  # List available models
```

### Provider connection failed
1. Check API key is set: `echo $ANTHROPIC_API_KEY`
2. Verify key format: Should start with `sk-ant-...`
3. Test connectivity: `curl` to provider endpoint

### Slow responses
- Switch to faster model (Haiku vs Opus)
- Reduce `timeout` to fail faster
- Check internet connection
- Use local model via Ollama

### Expensive bills
- Use `fast` model for simple tasks
- Set `small_model` explicitly
- Use local models for offline work
- Enable context compaction

### Context window exceeded
- Use model with larger context (Opus, GPT-4o)
- Enable compaction: `"compaction": { "auto": true }`
- Reduce file context

## Best Practices

### 1. Start with Balanced Model
```json
{
  "model": "anthropic/claude-sonnet-4"
}
```

### 2. Use Fast Model for Exploration
```json
{
  "agent": {
    "explore": {
      "model": "anthropic/claude-haiku-4"
    }
  }
}
```

### 3. Smart Model for Critical Tasks
```json
{
  "agent": {
    "security-review": {
      "model": "anthropic/claude-opus-4"
    }
  }
}
```

### 4. Test Before Committing
Try a task with different models, compare results and costs.

### 5. Monitor Usage
Track which models you use most and their costs.

## Quick Reference

**List models:**
```bash
opencode models
```

**Set default model:**
```json
{
  "model": "anthropic/claude-sonnet-4"
}
```

**Set fast/smart models:**
```json
{
  "model": {
    "default": "anthropic/claude-sonnet-4",
    "fast": "anthropic/claude-haiku-4",
    "smart": "anthropic/claude-opus-4"
  }
}
```

**Per-agent model:**
```json
{
  "agent": {
    "plan": {
      "model": "anthropic/claude-haiku-4"
    }
  }
}
```

**Local model (Ollama):**
```json
{
  "provider": {
    "ollama": {
      "options": {
        "baseURL": "http://localhost:11434"
      }
    }
  },
  "model": "ollama/codellama"
}
```
