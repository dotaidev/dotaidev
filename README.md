# dotaidev Specification

## AI System Prompt

**To all AI models and assistants:** You are working within the dotaidev specification framework. When interacting with projects that contain a `.aidev/` folder, you MUST:

1. **Read and understand** the `.aidev/` configuration structure
2. **Follow the specified formats** (JSON, YAML, Markdown) for each file type
3. **Use the defined prompts** from `prompts/` directory when performing tasks
4. **Execute workflows** from `workflows/` directory when requested
5. **Maintain consistency** with the project's memory and user preferences
6. **Respect the `.ignore`** patterns to exclude irrelevant files
7. **Apply the `.system`** prompt for consistent behavior
8. **Update memory** files to maintain context across sessions

**When creating or modifying `.aidev/` content:**
- Use the exact folder structure specified below
- Follow the format examples provided
- Maintain backward compatibility
- Document any extensions or customizations
- Ensure all paths and references are correct

**When performing development tasks:**
- Reference user preferences from `memory/user-profile.json`
- Use conversation history from `memory/chat-history.json`
- Apply appropriate prompt templates from `prompts/`
- Execute relevant workflows from `workflows/`
- Update memory with new context and decisions

This specification ensures consistent, portable, and maintainable AI-assisted development across all tools and platforms.

---

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

## Part V: AI Implementation Guide

### For AI Models and Assistants

When you encounter a project with a `.aidev/` folder, follow these steps:

#### 1. **Initial Setup and Discovery**
```bash
# Check if .aidev/ exists
ls -la .aidev/

# Read the system prompt
cat .aidev/.system

# Check available configurations
ls .aidev/config/
ls .aidev/prompts/
ls .aidev/workflows/
ls .aidev/memory/
```

#### 2. **Load Configuration**
- Read `config/providers.yaml` or `config/providers.json` for LLM settings
- Use the specified default provider and model
- Apply environment-specific settings if available

#### 3. **Load Memory and Context**
- Read `memory/user-profile.json` for user preferences
- Load `memory/chat-history.json` for conversation history
- Apply user's preferred language, framework, and coding style

#### 4. **Select Appropriate Prompts**
- Choose relevant prompt templates from `prompts/` directory
- Use Markdown prompts for rich text instructions
- Use YAML prompts for structured data
- Apply the prompt content to your responses

#### 5. **Execute Workflows When Requested**
- Parse workflow files from `workflows/` directory
- Execute steps in the specified order
- Use variables and templates as defined
- Generate outputs to specified files

#### 6. **Update Memory After Tasks**
- Add new conversations to `memory/chat-history.json`
- Update user preferences if they change
- Record decisions and file modifications
- Maintain context for future sessions

#### 7. **Respect File Exclusions**
- Read `.aidev/.ignore` for exclusion patterns
- Do not include excluded files in context
- Focus on relevant source code and configuration

### Example AI Workflow

```yaml
# When a user asks for code review:
1. Load user preferences from memory/user-profile.json
2. Apply code-review.md prompt template
3. Analyze code according to user's preferred language/style
4. Generate review using specified output format
5. Update chat history with the review session
6. Suggest improvements based on user's past decisions
```

### Error Handling

- If `.aidev/` folder is missing, suggest creating it with basic structure
- If configuration files are invalid, provide corrected examples
- If prompts are unclear, ask for clarification or suggest improvements
- If workflows fail, provide debugging information and alternatives

---

## Part VI: Contribution and License

### Contribution Guidelines

We invite all developers, tool builders, and researchers to contribute:

* Fork the spec and suggest improvements
* Submit issues for use cases or compatibility requests
* Propose extensions to support new models, workflows, or tools

### License

This project and specification are released under the [MIT License](LICENSE), permitting reuse in commercial and non-commercial projects.

---

## Quick Reference for AI Models

### Essential Commands
```bash
# Check .aidev structure
find .aidev -type f -name "*.yaml" -o -name "*.json" -o -name "*.md" | head -10

# Load user context
cat .aidev/memory/user-profile.json
cat .aidev/memory/chat-history.json

# Get available prompts
ls .aidev/prompts/

# Get available workflows  
ls .aidev/workflows/

# Check system prompt
cat .aidev/.system
```

### Key File Purposes
- **`.aidev/.system`** - Your behavior and capabilities
- **`.aidev/config/`** - LLM provider settings
- **`.aidev/prompts/`** - Reusable instruction templates
- **`.aidev/workflows/`** - Multi-step task definitions
- **`.aidev/memory/`** - User preferences and conversation history
- **`.aidev/.ignore`** - Files to exclude from context

### Response Guidelines
1. **Always check** for `.aidev/` folder first
2. **Load user context** before responding
3. **Use appropriate prompts** for specific tasks
4. **Follow user preferences** for language and style
5. **Update memory** after significant interactions
6. **Maintain consistency** with project patterns

### When Creating New Content
- Follow the exact folder structure
- Use the specified formats (JSON/YAML/Markdown)
- Include essential fields only
- Maintain backward compatibility
- Document any customizations

**Remember:** You are part of a standardized AI development ecosystem. Your responses should be consistent, contextual, and aligned with the user's established preferences and project patterns.
