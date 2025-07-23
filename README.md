# dotaidev

A vendor-agnostic, community-driven specification for AI-assisted development that unifies how development environments manage AI assistant assets.

## 🎯 What is dotaidev?

dotaidev provides a standardized `.aidev/` configuration folder structure that makes AI assistant configurations portable, interpretable, and reusable across different tools and platforms. Instead of each AI tool having its own hidden configuration format, dotaidev creates a shared standard that works with:

- **AI Coding Assistants**: Cursor, Claude, Copilot, CodeWhisperer
- **AI IDEs**: Kiro Studio, Windsurf, Cursor
- **Agent Frameworks**: LangGraph, AutoGen, OpenDevin
- **CLI Tools**: aidev, llm, and custom scripts

## 🚀 Quick Start

### 1. Create a `.aidev` folder in your project

```bash
mkdir .aidev
cd .aidev
```

### 2. Set up basic structure

```bash
mkdir config prompts workflows memory agents
touch .ignore .system
```

### 3. Add your first configuration

**`.aidev/prompts/code-review.md`**
```markdown
# Code Review Assistant

Review the provided code for:
- Code quality (readability, structure, naming)
- Best practices (SOLID principles, error handling)
- Security (input validation, authentication)
- Performance (efficiency, optimization)

Output format:
- Summary of findings
- Issues by severity (Critical/High/Medium/Low)
- Specific recommendations
```

## 🔧 IDE Integration

Since existing IDEs don't natively support `.aidev/` yet, here's how to configure them to recognize and use the dotaidev specification:

### Cursor

Configure Cursor to use `.aidev/` configurations by modifying `.cursor/` folder:

**`.cursor/settings.json`**
```json
{
  "ai": {
    "configPath": ".aidev/config/providers.yaml",
    "systemPromptPath": ".aidev/.system",
    "promptsPath": ".aidev/prompts/",
    "memoryPath": ".aidev/memory/",
    "workflowsPath": ".aidev/workflows/"
  },
  "context": {
    "includePatterns": [
      ".aidev/**/*"
    ],
    "excludePatterns": [
      ".aidev/.ignore"
    ]
  }
}
```

**`.cursor/prompts/global.md`**
```markdown
# Global Cursor Configuration

This project uses the dotaidev specification. Please:

1. Load configuration from `.aidev/config/providers.yaml`
2. Use system prompt from `.aidev/.system`
3. Apply user preferences from `.aidev/memory/user-profile.json`
4. Use prompt templates from `.aidev/prompts/`
5. Execute workflows from `.aidev/workflows/` when requested
6. Update memory in `.aidev/memory/` after significant interactions

Follow the dotaidev specification for consistent behavior across all AI tools.
```

### Claude Desktop/Web

Configure Claude to recognize `.aidev/` by creating a `.claude/` folder:

**`.claude/config.json`**
```json
{
  "aidev": {
    "enabled": true,
    "configPath": ".aidev/config/providers.yaml",
    "systemPromptPath": ".aidev/.system",
    "promptsPath": ".aidev/prompts/",
    "memoryPath": ".aidev/memory/"
  },
  "context": {
    "include": [
      ".aidev/**/*"
    ],
    "exclude": [
      ".aidev/.ignore"
    ]
  }
}
```

**`.claude/prompts/aidev.md`**
```markdown
# dotaidev Integration

When working with this project:

1. **Load Configuration**: Read `.aidev/config/providers.yaml` for LLM settings
2. **Apply System Prompt**: Use `.aidev/.system` as your behavior guide
3. **Load User Context**: Read `.aidev/memory/user-profile.json` for preferences
4. **Use Prompt Templates**: Apply templates from `.aidev/prompts/` for specific tasks
5. **Update Memory**: Record conversations in `.aidev/memory/chat-history.json`

Always follow the user's preferred language, framework, and coding style from their profile.
```

### Kiro Studio

Configure Kiro to work with `.aidev/` by modifying `.kiro/` folder:

**`.kiro/config.yaml`**
```yaml
aidev:
  enabled: true
  config_path: ".aidev/config/providers.yaml"
  system_prompt_path: ".aidev/.system"
  prompts_path: ".aidev/prompts/"
  memory_path: ".aidev/memory/"
  workflows_path: ".aidev/workflows/"

context:
  include:
    - ".aidev/**/*"
  exclude:
    - ".aidev/.ignore"
```

**`.kiro/prompts/aidev.md`**
```markdown
# dotaidev Specification Integration

This project follows the dotaidev specification. When assisting:

1. **Configuration**: Load LLM settings from `.aidev/config/`
2. **System Behavior**: Follow `.aidev/.system` prompt
3. **User Preferences**: Apply settings from `.aidev/memory/user-profile.json`
4. **Prompt Templates**: Use `.aidev/prompts/` for structured responses
5. **Workflows**: Execute `.aidev/workflows/` when requested
6. **Memory**: Update `.aidev/memory/` with new context

Maintain consistency with the user's established patterns and preferences.
```

### Windsurf

Configure Windsurf to recognize `.aidev/` by creating `.windsurf/` folder:

**`.windsurf/config.json`**
```json
{
  "aidev": {
    "enabled": true,
    "configPath": ".aidev/config/providers.yaml",
    "systemPromptPath": ".aidev/.system",
    "promptsPath": ".aidev/prompts/",
    "memoryPath": ".aidev/memory/",
    "workflowsPath": ".aidev/workflows/"
  },
  "ai": {
    "contextInclude": [".aidev/**/*"],
    "contextExclude": [".aidev/.ignore"]
  }
}
```

**`.windsurf/prompts/aidev.md`**
```markdown
# dotaidev Integration for Windsurf

When working in this project:

1. **Load dotaidev config** from `.aidev/config/providers.yaml`
2. **Apply system prompt** from `.aidev/.system`
3. **Use user preferences** from `.aidev/memory/user-profile.json`
4. **Apply prompt templates** from `.aidev/prompts/`
5. **Execute workflows** from `.aidev/workflows/` when needed
6. **Update memory** in `.aidev/memory/` after interactions

Follow the dotaidev specification for consistent AI-assisted development.
```

## 📋 Configuration Summary

### What Each IDE Configuration Does

| IDE | Configuration Files | Purpose |
|-----|-------------------|---------|
| **Cursor** | `.cursor/settings.json` + `.cursor/prompts/global.md` | Load `.aidev/` config and apply system prompt |
| **Claude** | `.claude/config.json` + `.claude/prompts/aidev.md` | Recognize `.aidev/` structure and follow spec |
| **Kiro** | `.kiro/config.yaml` + `.kiro/prompts/aidev.md` | Import `.aidev/` configs and use prompt templates |
| **Windsurf** | `.windsurf/config.json` + `.windsurf/prompts/aidev.md` | Load `.aidev/` settings and apply templates |
| **VS Code** | `.vscode/settings.json` + `.vscode/extensions.json` | Configure Copilot to use `.aidev/` configs |

### Key Configuration Elements

1. **Path References**: Point each IDE to `.aidev/` folders
2. **Context Inclusion**: Include `.aidev/**/*` in AI context
3. **Exclusion Patterns**: Exclude `.aidev/.ignore` from context
4. **Prompt Instructions**: Tell AI to follow dotaidev specification
5. **Memory Integration**: Load and update user preferences

### Quick Setup Script

Create a setup script to configure all IDEs at once:

```bash
#!/bin/bash
# setup-aidev-integration.sh

# Create IDE configuration folders
mkdir -p .cursor .claude .kiro .windsurf .vscode

# Copy configuration files (create these based on examples above)
cp aidev-configs/cursor-settings.json .cursor/settings.json
cp aidev-configs/claude-config.json .claude/config.json
cp aidev-configs/kiro-config.yaml .kiro/config.yaml
cp aidev-configs/windsurf-config.json .windsurf/config.json
cp aidev-configs/vscode-settings.json .vscode/settings.json

echo "IDE integration configured for dotaidev specification!"
```

## 📁 Folder Structure

```
.aidev/
├── config/           # LLM provider settings
│   ├── providers.yaml
│   └── providers.json
├── prompts/          # Reusable prompt templates
│   ├── code-review.md
│   ├── test-generation.yaml
│   └── refactor-code.md
├── workflows/        # Multi-step task definitions
│   ├── create-feature.yaml
│   └── code-review.json
├── memory/           # User context and history
│   ├── user-profile.json
│   └── chat-history.yaml
├── agents/           # Agent framework configs
│   ├── langgraph/
│   └── autogen/
├── .ignore           # Files to exclude from AI context
└── .system           # Global system prompt
```

## 📚 Documentation

- **[Full Specification](https://raw.githubusercontent.com/dotaidev/dotaidev/refs/heads/main/spec.md)** - Complete technical specification
- **[Examples Repository](https://github.com/dotaidev/.aidev)** - Real-world usage examples
- **[IDE Integration Guide](https://github.com/dotaidev/README.md)** - Detailed integration instructions

## 🤝 Contributing

We welcome contributions from developers, tool builders, and researchers.

### Areas for Contribution
- **IDE Integrations**: Help integrate with more AI development tools
- **Prompt Templates**: Share useful prompt templates
- **Workflow Examples**: Create reusable workflow definitions
- **Documentation**: Improve guides and examples

## 📄 License

This project is released under the [MIT License](LICENSE), permitting reuse in commercial and non-commercial projects.

## 🌟 Community

- **GitHub Discussions**: [Share ideas and get help](https://github.com/dotaidev/dotaidev/discussions)

---

**Ready to standardize your AI development workflow?** Start by creating a `.aidev/` folder in your next project and see how it transforms your AI-assisted development experience!
