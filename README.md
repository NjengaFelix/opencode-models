# opencode-models

Configure and optimize model selection for different OpenCode tasks.

## Overview

`opencode-models` is an OpenCode skill that helps you choose and configure AI models for optimal performance and cost. Whether you need the most capable model for complex tasks or a fast, cost-effective option for simple work, this skill provides guidance on model selection across multiple providers.

## Installation

### Global Installation (Recommended)

The skill is automatically available globally when symlinked to your OpenCode configuration:

```bash
# The skill should be symlinked to:
~/.config/opencode/skills/opencode-models -> ~/dev/opencode-models/
```

### Manual Installation

```bash
# Clone the repository
git clone https://github.com/NjengaFelix/opencode-models.git

# Create symlink for global access
ln -s $(pwd)/opencode-models ~/.config/opencode/skills/opencode-models
```

## Usage

Simply ask OpenCode to load the skill:

```
Load opencode-models
```

Or reference it when asking for help:

```
Help me choose the right model for this task
```

## What This Skill Covers

### 🤖 Model Selection Guide
- **By Task Type** - Which model for coding, planning, documentation
- **By Provider** - Anthropic, OpenAI, Google options
- **By Cost** - Balancing capability and price
- **By Speed** - Fast vs smart tradeoffs

### ⚙️ Configuration Options
- Single model setup
- Fast/smart split
- Per-agent models
- Provider configuration

### 🏠 Local Models
- Ollama setup and configuration
- Best local models for coding
- Offline development

### 💰 Cost Optimization
- Tiered model approach
- Small model for simple tasks
- Usage monitoring tips

## Quick Examples

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

### Local Model (Ollama)

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

## Model Recommendations

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

**Google**
- `google/gemini-2.5-pro` - Capable, good context
- `google/gemini-2.5-flash` - Fast, efficient

## Directory Structure

```
opencode-models/
├── SKILL.md          # Main skill definition
├── README.md         # This file
└── .git/            # Git repository
```

## Development

The skill is developed in `~/dev/opencode-models/` and symlinked to `~/.config/opencode/skills/` for global availability.

### Making Changes

1. Edit `SKILL.md` in `~/dev/opencode-models/`
2. Changes are immediately available (symlink is live)
3. Commit your changes:
   ```bash
   cd ~/dev/opencode-models
   git add .
   git commit -m "Your commit message"
   git push origin main
   ```

## Use Cases

- **Choosing a Model** - What model for this specific task?
- **Multiple Providers** - Set up both Claude and GPT
- **Cost Control** - Use cheaper models for simple tasks
- **Speed vs Quality** - Fast for planning, smart for coding
- **Provider Fallback** - Switch when primary fails
- **Local Development** - Use Ollama for offline work

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

## Best Practices

1. **Start with Balanced Model** - Claude Sonnet 4
2. **Use Fast Model for Exploration** - Save costs
3. **Smart Model for Critical Tasks** - Security reviews
4. **Test Before Committing** - Compare results
5. **Monitor Usage** - Track costs

## Related Skills

- **opencode-init** - Main initialization skill
- **opencode-permissions** - Security and permissions
- **opencode-mcp** - MCP server configuration
- **opencode-agents** - Custom agent creation

## Contributing

Contributions welcome! Please:

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Submit a pull request

## Support

- **OpenCode Docs**: https://opencode.ai/docs
- **Discord**: https://opencode.ai/discord
- **Issues**: https://github.com/NjengaFelix/opencode-models/issues

## License

MIT License - See [SKILL.md](SKILL.md) for details

---

**Built with ❤️ for the OpenCode community**
