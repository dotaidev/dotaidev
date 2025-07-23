# dotaidev Specification

## Part I: Why `dotaidev`

Modern software development increasingly involves collaboration with AI coding assistants, multi-agent systems, and intelligent developer tools. Tools like Cursor, Kiro, Claude, Copilot, CodeWhisperer, and open-source agents such as LangGraph, AutoGen, and OpenDevin have introduced a new paradigm of context-aware, AI-enhanced development.

However, these tools lack a shared, open standard for storing prompt templates, agent plans, memory, and workflow configurations. Each tool currently defines its own hidden `.folder` with no cross-tool interoperability. This leads to:

* Redundant or conflicting configurations
* Limited reusability of custom prompts and workflows
* Difficult integration of multiple LLM providers and agent types

The `dotaidev` project proposes a **vendor-agnostic, community-driven specification** for an `.aidev/` configuration folder. The goal is to unify how development environments manage AI assistant assets, making them portable, interpretable, and reusable across tools and platforms.

---

## Part II: Design Goals and Strategies

### Goals

* **Vendor Agnosticism**: Support all major LLM platforms (OpenAI, Anthropic, Google, Mistral, DeepSeek, Ollama, etc.).
* **Multi-Role Support**: Accommodate individual developers, teams, AI plugin builders, and multi-agent system designers.
* **Agent Framework Compatibility**: Provide compatibility with LangGraph, AutoGen, OpenDevin, and others.
* **Modular Structure**: Separate prompts, memory, configuration, and agent workflows cleanly.
* **Contextual Awareness**: Enable persistent, project-scoped memory for contextual AI behavior.
* **Ease of Use and Versioning**: Store files in simple, human-readable formats under version control.

### Design Strategies

* Use standard formats like `yaml`, `json`, and `md` to maximize readability and tooling support
* Design for extensibility by reserving optional subfolders (e.g., `.aidev/agents/`)
* Enable pluggable usage in CLI tools, IDEs, dev servers, and browser-based runtimes

---

## Part III: Specification Details

### Folder Structure

```plaintext
.aidev/ (aka dotaidev root)
├── config/           # Model settings, API keys, environment profiles
├── prompts/          # Reusable prompt templates (Markdown or text)
├── workflows/        # Step-by-step procedures (YAML or JSON)
├── memory/           # Persistent memory and shared context
├── agents/           # Agent framework configs (LangGraph, AutoGen, etc.)
├── .ignore           # Glob patterns to exclude files from AI context
└── .system           # Global system prompt prepended to assistant
```

### Content Description

| Path         | Purpose                                           | Formats              |
| ------------ | ------------------------------------------------- | -------------------- |
| `config/`    | Define models, LLM providers, and API credentials | `yaml`, `json`       |
| `prompts/`   | Store editable prompt templates for reuse         | `md`, `txt`          |
| `workflows/` | Define named AI tasks or chains                   | `yaml`, `json`       |
| `memory/`    | Store facts, chat history, or long-term memory    | `json`, `yaml`       |
| `agents/`    | Store LangGraph flows, AutoGen scripts, etc.      | `json`, `py`, `yaml` |
| `.ignore`    | File patterns to ignore for assistant input       | text (glob syntax)   |
| `.system`    | Global system instruction for the assistant       | plain text           |

### Recommended Content Formats

#### `config/providers.yaml`

```yaml
providers:
  openai:
    model: gpt-4o
    api_key: "${OPENAI_API_KEY}"
  anthropic:
    model: claude-3-opus
    api_key: "${ANTHROPIC_API_KEY}"
```

#### `prompts/test-gen.md`

```markdown
# Generate Unit Tests
Given the following source code, generate a complete test suite using best practices.
```

#### `workflows/create-feature.yaml`

```yaml
name: Create Feature Component
steps:
  - prompt: "Generate a React component"
  - file: src/components/NewComponent.tsx
  - test: true
```

#### `memory/user-profile.json`

```json
{
  "preferred_language": "TypeScript",
  "style_guide": "airbnb",
  "past_tasks": ["refactored auth", "generated tests"]
}
```

#### `agents/langgraph/dag.json`

```json
{
  "nodes": ["ReadCode", "PlanChange", "WriteCode"],
  "edges": [["ReadCode", "PlanChange"], ["PlanChange", "WriteCode"]]
}
```

---

## Part IV: Applications and Compatibility

### IDEs and Developer Tools

The `dotaidev` spec is designed to work with modern AI-integrated tools, including:

* **Cursor**: Aligns with `.cursor/` by offering compatible structure and model hints
* **Kiro Studio**: Mirrors `.kiro/` plans and component definitions
* **Claude Code / CodeRouter**: Supports `.claude/`-style prompt plans
* **VSCode / JetBrains Plugins**: Can load prompt templates and workflows from `.aidev/`

### Agent Frameworks

* **LangGraph**: Store DAG graphs, persistent memory, and node-specific prompts under `agents/langgraph/`
* **AutoGen**: Define multi-agent conversations, configs, and roles in `agents/autogen/`
* **OpenDevin**: House planning prompts, task state, and shell tool usage patterns

### CLI Tools

Tools like [`aidev`](https://github.com/efritz/aidev) and `llm` can load `.aidev/` content to:

* Register prompts
* Choose a provider
* Apply workflows (e.g., “generate readme”, “refactor file”)

---

## Part V: Contribution and License

### Contribution Guidelines

We invite all developers, tool builders, and researchers to contribute:

* Fork the spec and suggest improvements
* Submit issues for use cases or compatibility requests
* Propose extensions to support new models, workflows, or tools

### License

This project and specification are released under the [MIT License](LICENSE), permitting reuse in commercial and non-commercial projects.

You are free to:

* Implement the `.aidev/` spec in your tool or product
* Extend the layout to fit your workflow
* Share and distribute `.aidev` prompt packs or agent configurations

---

### Project Goals

`dotaidev` aims to:

* Become the de facto layout for AI assistant context
* Promote reproducible, explainable, and portable prompt engineering
* Enable IDEs, CLIs, and agents to share a common assistant backend

**GitHub Repo:** [https://github.com/dotaidev/spec](https://github.com/dotaidev/spec)
**Lead Maintainer:** Open to the community