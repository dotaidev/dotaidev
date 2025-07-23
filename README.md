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
* **Format Flexibility**: JSON, YAML, and Markdown are all recommended formats - choose based on content type and preference
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

| Path         | Purpose                                           | Recommended Formats  |
| ------------ | ------------------------------------------------- | -------------------- |
| `config/`    | Define models, LLM providers, and API credentials | `yaml`, `json`       |
| `prompts/`   | Store editable prompt templates for reuse         | `md`, `txt`, `yaml`  |
| `workflows/` | Define named AI tasks or chains                   | `yaml`, `json`       |
| `memory/`    | Store facts, chat history, or long-term memory    | `json`, `yaml`       |
| `agents/`    | Store LangGraph flows, AutoGen scripts, etc.      | `json`, `py`, `yaml` |
| `.ignore`    | File patterns to ignore for assistant input       | text (glob syntax)   |
| `.system`    | Global system instruction for the assistant       | plain text, `md`     |

### Recommended Content Formats

Choose the format that best suits your content type. All formats are supported:

#### Configuration Examples

**`config/providers.yaml`**
```yaml
providers:
  openai:
    model: gpt-4o
    api_key: "${OPENAI_API_KEY}"
    temperature: 0.7
  anthropic:
    model: claude-3-opus
    api_key: "${ANTHROPIC_API_KEY}"
    temperature: 0.5
default_provider: openai
```

**`config/providers.json`**
```json
{
  "providers": {
    "openai": {
      "model": "gpt-4o",
      "api_key": "${OPENAI_API_KEY}",
      "temperature": 0.7
    }
  },
  "default_provider": "openai"
}
```

#### Prompt Template Examples

**`prompts/code-review.md`**
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

**`prompts/test-generation.yaml`**
```yaml
name: "Generate Unit Tests"
role: "Expert software tester"
requirements:
  - "Unit tests for individual functions"
  - "Edge cases and error conditions"
  - "Mock external dependencies"
output:
  - "Complete test file with imports"
  - "Test cases with descriptive names"
  - "Mocking setup"
```

#### Workflow Examples

**`workflows/create-feature.yaml`**
```yaml
name: Create Feature Component
steps:
  - name: generate_component
    prompt: "Generate a React component"
    file: "src/components/{{name}}.tsx"
  - name: generate_tests
    prompt: "Generate tests for the component"
    file: "src/components/__tests__/{{name}}.test.tsx"
variables:
  name: "NewFeature"
```

**`workflows/code-review.json`**
```json
{
  "name": "Code Review",
  "steps": [
    {
      "name": "analyze",
      "prompt": "Analyze code for issues",
      "output": "analysis.md"
    },
    {
      "name": "suggest",
      "prompt": "Suggest improvements",
      "output": "suggestions.md"
    }
  ]
}
```

#### Memory Examples

**`memory/user-profile.json`**
```json
{
  "preferences": {
    "language": "TypeScript",
    "framework": "React",
    "testing": "Jest"
  },
  "project": {
    "type": "web_app",
    "database": "PostgreSQL"
  }
}
```

**`memory/chat-history.yaml`**
```yaml
conversations:
  - topic: "Auth System"
    summary: "JWT implementation"
    decisions: ["Use JWT", "Add rate limiting"]
preferences:
  response_style: "detailed"
  code_format: "typescript"
```

#### Agent Configuration Examples

**`agents/langgraph/dag.json`**
```json
{
  "name": "Code Review Agent",
  "nodes": [
    {
      "name": "ReadCode",
      "type": "tool",
      "config": {"file_patterns": ["**/*.{ts,js}"]}
    },
    {
      "name": "Analyze",
      "type": "llm",
      "config": {"prompt": "prompts/code-review.md"}
    }
  ],
  "edges": [{"from": "ReadCode", "to": "Analyze"}]
}
```

**`agents/autogen/multi-agent.yaml`**
```yaml
name: "Dev Team"
agents:
  - name: "Developer"
    role: "Write code and tests"
    model: "gpt-4o"
  - name: "Reviewer"
    role: "Review code quality"
    model: "gpt-4o"
workflows:
  - name: "FeatureDev"
    steps:
      - agent: "Developer"
        action: "implement"
      - agent: "Reviewer"
        action: "review"
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

---

## Part V: Contribution and License

### Contribution Guidelines

We invite all developers, tool builders, and researchers to contribute:

* Fork the spec and suggest improvements
* Submit issues for use cases or compatibility requests
* Propose extensions to support new models, workflows, or tools

### License

This project and specification are released under the [MIT License](LICENSE), permitting reuse in commercial and non-commercial projects.
