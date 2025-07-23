# .aidev Configuration

This folder implements the dotaidev specification for AI-assisted development. It provides a standardized structure for managing AI assistant configurations, prompts, workflows, and memory across different tools and platforms.

## Folder Structure

```
.aidev/
├── config/           # Model settings, API keys, environment profiles
├── prompts/          # Reusable prompt templates (Markdown or text)
├── workflows/        # Step-by-step procedures (YAML or JSON)
├── memory/           # Persistent memory and shared context
├── agents/           # Agent framework configs (LangGraph, AutoGen, etc.)
├── .ignore           # Glob patterns to exclude files from AI context
├── .system           # Global system prompt prepended to assistant
└── README.md         # This file
```

## Configuration Files

### `config/providers.yaml`
Defines LLM providers and their settings:
- **OpenAI**: GPT-4o with configurable temperature and token limits
- **Anthropic**: Claude-3-Opus for advanced reasoning tasks
- **Google**: Gemini Pro for general development tasks
- **Ollama**: Local models for development and testing

Environment-specific settings allow different configurations for development vs production.

### `prompts/`
Contains reusable prompt templates:
- **`code-review.md`**: Comprehensive code review guidelines
- **`test-generation.md`**: Unit test generation with framework-specific guidance
- **`refactor-code.md`**: Code refactoring with design patterns and best practices

### `workflows/`
Defines multi-step AI tasks:
- **`create-feature.yaml`**: Complete feature development workflow
- **`code-review.yaml`**: Automated code review with multiple analysis stages

### `memory/`
Stores persistent context:
- **`user-profile.json`**: Developer preferences and project context
- **`chat-history.json`**: Conversation history and common patterns

### `agents/`
Multi-agent system configurations:
- **`langgraph/dag.json`**: LangGraph workflow for code review
- **`autogen/multi-agent.json`**: AutoGen team for software development

### `.ignore`
Excludes files from AI context to improve performance and security.

### `.system`
Global system prompt that defines the AI assistant's role and capabilities.