# Synkra AIOS Development Rules for Claude Code

You are working with Synkra AIOS, an AI-Orchestrated System for Full Stack Development.

---

## ⚠️ CRITICAL OPERATIONAL RULES (NON-NEGOTIABLE)

### 🚫 NEVER

- **Implement without showing options first** — Always use 1, 2, 3 format for choices
- **Delete/remove content without asking first** — Always ask for confirmation
- **Delete anything created in the last 7 days without explicit approval** — Protect recent work
- **Change something that was already working** — Verify it's actually broken first
- **Pretend work is done when it isn't** — Be honest about completion status
- **Process batch without validating one first** — Always validate sample before bulk operations
- **Add features that weren't requested** — Stick to requirements exactly (Article IV: No Invention)
- **Use mock data when real data exists in database** — Use real data
- **Explain/justify when receiving criticism** — Just fix it
- **Trust AI/subagent output without verification** — Always verify results
- **Create from scratch when similar exists in squads/** — Reuse existing components

### ✅ ALWAYS

- **Present options as "1. X, 2. Y, 3. Z" format** — Never ambiguous choice presentation
- **Use AskUserQuestion tool for clarifications** — Don't assume
- **Check squads/ and existing components before creating new** — DRY principle
- **Read COMPLETE schema before proposing database changes** — Full context required
- **Investigate root cause when error persists** — Don't just retry
- **Commit before moving to next task** — Save progress atomically
- **Create handoff in `docs/sessions/YYYY-MM/` at end of session** — Document context for continuity

---

<!-- AIOS-MANAGED-START: core-framework -->
## Core Framework Understanding

Synkra AIOS is a meta-framework that orchestrates AI agents to handle complex development workflows. Always recognize and work within this architecture.
<!-- AIOS-MANAGED-END: core-framework -->

<!-- AIOS-MANAGED-START: constitution -->
## Constitution

O AIOS possui uma **Constitution formal** com princípios inegociáveis e gates automáticos.

**Documento completo:** `.aios-core/constitution.md`

**Princípios fundamentais:**

| Artigo | Princípio | Severidade |
|--------|-----------|------------|
| I | CLI First | NON-NEGOTIABLE |
| II | Agent Authority | NON-NEGOTIABLE |
| III | Story-Driven Development | MUST |
| IV | No Invention | MUST |
| V | Quality First | MUST |
| VI | Absolute Imports | SHOULD |

**Gates automáticos bloqueiam violações.** Consulte a Constitution para detalhes completos.
<!-- AIOS-MANAGED-END: constitution -->

<!-- AIOS-MANAGED-START: sistema-de-agentes -->
## Sistema de Agentes

### Ativação de Agentes
Use `@agent-name` ou `/AIOS:agents:agent-name`:

| Agente | Persona | Escopo Principal |
|--------|---------|------------------|
| `@dev` | Dex | Implementação de código |
| `@qa` | Quinn | Testes e qualidade |
| `@architect` | Aria | Arquitetura e design técnico |
| `@pm` | Morgan | Product Management |
| `@po` | Pax | Product Owner, stories/epics |
| `@sm` | River | Scrum Master |
| `@analyst` | Alex | Pesquisa e análise |
| `@data-engineer` | Dara | Database design |
| `@ux-design-expert` | Uma | UX/UI design |
| `@devops` | Gage | CI/CD, git push (EXCLUSIVO) |

### Comandos de Agentes
Use prefixo `*` para comandos:
- `*help` - Mostrar comandos disponíveis
- `*create-story` - Criar story de desenvolvimento
- `*task {name}` - Executar task específica
- `*exit` - Sair do modo agente
<!-- AIOS-MANAGED-END: sistema-de-agentes -->

<!-- AIOS-MANAGED-START: agent-system -->
## Agent System

### Agent Activation
- Agents are activated with @agent-name syntax: @dev, @qa, @architect, @pm, @po, @sm, @analyst
- The master agent is activated with @aios-master
- Agent commands use the * prefix: *help, *create-story, *task, *exit

### Agent Context
When an agent is active:
- Follow that agent's specific persona and expertise
- Use the agent's designated workflow patterns
- Maintain the agent's perspective throughout the interaction
<!-- AIOS-MANAGED-END: agent-system -->

## Development Methodology

### Story-Driven Development
1. **Work from stories** - All development starts with a story in `docs/stories/`
2. **Update progress** - Mark checkboxes as tasks complete: [ ] → [x]
3. **Track changes** - Maintain the File List section in the story
4. **Follow criteria** - Implement exactly what the acceptance criteria specify

### Code Standards
- Write clean, self-documenting code
- Follow existing patterns in the codebase
- Include comprehensive error handling
- Add unit tests for all new functionality
- Use TypeScript/JavaScript best practices

### Testing Requirements
- Run all tests before marking tasks complete
- Ensure linting passes: `npm run lint`
- Verify type checking: `npm run typecheck`
- Add tests for new features
- Test edge cases and error scenarios

<!-- AIOS-MANAGED-START: framework-structure -->
## AIOS Framework Structure

```
aios-core/
├── agents/         # Agent persona definitions (YAML/Markdown)
├── tasks/          # Executable task workflows
├── workflows/      # Multi-step workflow definitions
├── templates/      # Document and code templates
├── checklists/     # Validation and review checklists
└── rules/          # Framework rules and patterns

docs/
├── stories/        # Development stories (numbered)
├── prd/            # Product requirement documents
├── architecture/   # System architecture documentation
└── guides/         # User and developer guides
```
<!-- AIOS-MANAGED-END: framework-structure -->

<!-- AIOS-MANAGED-START: framework-boundary -->
## Framework vs Project Boundary

O AIOS usa um modelo de 4 camadas (L1-L4) para separar artefatos do framework e do projeto. Deny rules em `.claude/settings.json` reforçam isso deterministicamente.

| Camada | Mutabilidade | Paths | Notas |
|--------|-------------|-------|-------|
| **L1** Framework Core | NEVER modify | `.aios-core/core/`, `.aios-core/constitution.md`, `bin/aios.js`, `bin/aios-init.js` | Protegido por deny rules |
| **L2** Framework Templates | NEVER modify | `.aios-core/development/tasks/`, `.aios-core/development/templates/`, `.aios-core/development/checklists/`, `.aios-core/development/workflows/`, `.aios-core/infrastructure/` | Extend-only |
| **L3** Project Config | Mutable (exceptions) | `.aios-core/data/`, `agents/*/MEMORY.md`, `core-config.yaml` | Allow rules permitem |
| **L4** Project Runtime | ALWAYS modify | `docs/stories/`, `packages/`, `squads/`, `tests/` | Trabalho do projeto |

**Toggle:** `core-config.yaml` → `boundary.frameworkProtection: true/false` controla se deny rules são ativas (default: true para projetos, false para contribuidores do framework).

> **Referência formal:** `.claude/settings.json` (deny/allow rules), `.claude/rules/agent-authority.md`
<!-- AIOS-MANAGED-END: framework-boundary -->

<!-- AIOS-MANAGED-START: rules-system -->
## Rules System

O AIOS carrega regras contextuais de `.claude/rules/` automaticamente. Regras com frontmatter `paths:` só carregam quando arquivos correspondentes são editados.

| Rule File | Description |
|-----------|-------------|
| `agent-authority.md` | Agent delegation matrix and exclusive operations |
| `agent-handoff.md` | Agent switch compaction protocol for context optimization |
| `agent-memory-imports.md` | Agent memory lifecycle and CLAUDE.md ownership |
| `coderabbit-integration.md` | Automated code review integration rules |
| `ids-principles.md` | Incremental Development System principles |
| `mcp-usage.md` | MCP server usage rules and tool selection priority |
| `story-lifecycle.md` | Story status transitions and quality gates |
| `workflow-execution.md` | 4 primary workflows (SDC, QA Loop, Spec Pipeline, Brownfield) |

> **Diretório:** `.claude/rules/` — rules são carregadas automaticamente pelo Claude Code quando relevantes.
<!-- AIOS-MANAGED-END: rules-system -->

<!-- AIOS-MANAGED-START: code-intelligence -->
## Code Intelligence

O AIOS possui um sistema de code intelligence opcional que enriquece operações com dados de análise de código.

| Status | Descrição | Comportamento |
|--------|-----------|---------------|
| **Configured** | Provider ativo e funcional | Enrichment completo disponível |
| **Fallback** | Provider indisponível | Sistema opera normalmente sem enrichment — graceful degradation |
| **Disabled** | Nenhum provider configurado | Funcionalidade de code-intel ignorada silenciosamente |

**Graceful Fallback:** Code intelligence é sempre opcional. `isCodeIntelAvailable()` verifica disponibilidade antes de qualquer operação. Se indisponível, o sistema retorna o resultado base sem modificação — nunca falha.

**Diagnóstico:** `aios doctor` inclui check de code-intel provider status.

> **Referência:** `.aios-core/core/code-intel/` — provider interface, enricher, client
<!-- AIOS-MANAGED-END: code-intelligence -->

<!-- AIOS-MANAGED-START: graph-dashboard -->
## Graph Dashboard

O CLI `aios graph` visualiza dependências, estatísticas de entidades e status de providers.

### Comandos

```bash
aios graph --deps                        # Dependency tree (ASCII)
aios graph --deps --format=json          # Output como JSON
aios graph --deps --format=html          # Interactive HTML (abre browser)
aios graph --deps --format=mermaid       # Mermaid diagram
aios graph --deps --format=dot           # DOT format (Graphviz)
aios graph --deps --watch                # Live mode com auto-refresh
aios graph --deps --watch --interval=10  # Refresh a cada 10 segundos
aios graph --stats                       # Entity stats e cache metrics
```

**Formatos de saída:** ascii (default), json, dot, mermaid, html

> **Referência:** `.aios-core/core/graph-dashboard/` — CLI, renderers, data sources
<!-- AIOS-MANAGED-END: graph-dashboard -->

## Workflow Execution

### Task Execution Pattern
1. Read the complete task/workflow definition
2. Understand all elicitation points
3. Execute steps sequentially
4. Handle errors gracefully
5. Provide clear feedback

### Interactive Workflows
- Workflows with `elicit: true` require user input
- Present options clearly
- Validate user responses
- Provide helpful defaults

## Best Practices

### When implementing features:
- **Check existing patterns first** — Reuse components from squads/ before creating
- Reuse components and utilities
- Follow naming conventions
- Keep functions focused and testable
- Document complex logic
- **Respect Constitution Article IV (No Invention)** — Only implement what's requested

### When working with agents:
- Respect agent boundaries
- Use appropriate agent for each task
- Follow agent communication patterns
- Maintain agent context
- **Refer to CRITICAL OPERATIONAL RULES above** — These apply to all agent work

### When making decisions:
- **Always present options (1, 2, 3 format)** — Never make unilateral decisions
- Use `AskUserQuestion` tool for clarifications
- Verify root cause before suggesting solutions
- Trust but verify subagent output

### When handling errors:
```javascript
try {
  // Operation
} catch (error) {
  console.error(`Error in ${operation}:`, error);
  // Investigate root cause
  throw new Error(`Failed to ${operation}: ${error.message}`);
}
```

### When deleting or removing:
- **Always ask first** — No exceptions
- **Protect recent work** — Never delete content created in last 7 days without approval
- Show what's being removed
- Explain why it should be removed
- Wait for explicit confirmation

## Git & GitHub Integration

### Commit Conventions
- Use conventional commits: `feat:`, `fix:`, `docs:`, `chore:`, etc.
- Reference story ID: `feat: implement IDE detection [Story 2.1]`
- Keep commits atomic and focused

### GitHub CLI Usage
- Ensure authenticated: `gh auth status`
- Use for PR creation: `gh pr create`
- Check org access: `gh api user/memberships`

<!-- AIOS-MANAGED-START: aios-patterns -->
## AIOS-Specific Patterns

### Working with Templates
```javascript
const template = await loadTemplate('template-name');
const rendered = await renderTemplate(template, context);
```

### Agent Command Handling
```javascript
if (command.startsWith('*')) {
  const agentCommand = command.substring(1);
  await executeAgentCommand(agentCommand, args);
}
```

### Story Updates
```javascript
// Update story progress
const story = await loadStory(storyId);
story.updateTask(taskId, { status: 'completed' });
await story.save();
```
<!-- AIOS-MANAGED-END: aios-patterns -->

## Environment Setup

### Required Tools
- Node.js 18+
- GitHub CLI
- Git
- Your preferred package manager (npm/yarn/pnpm)

### Configuration Files
- `.aios/config.yaml` - Framework configuration
- `.env` - Environment variables
- `aios.config.js` - Project-specific settings

<!-- AIOS-MANAGED-START: common-commands -->
## Common Commands

### AIOS Master Commands
- `*help` - Show available commands
- `*create-story` - Create new story
- `*task {name}` - Execute specific task
- `*workflow {name}` - Run workflow

### Development Commands
- `npm run dev` - Start development
- `npm test` - Run tests
- `npm run lint` - Check code style
- `npm run build` - Build project
<!-- AIOS-MANAGED-END: common-commands -->

## Debugging

### Enable Debug Mode
```bash
export AIOS_DEBUG=true
```

### View Agent Logs
```bash
tail -f .aios/logs/agent.log
```

### Trace Workflow Execution
```bash
npm run trace -- workflow-name
```

## Claude Code Specific Configuration

### Performance Optimization
- Prefer batched tool calls when possible for better performance
- Use parallel execution for independent operations
- Cache frequently accessed data in memory during sessions

### Tool Usage Guidelines
- Always use the Grep tool for searching, never `grep` or `rg` in bash
- Use the Task tool for complex multi-step operations
- Batch file reads/writes when processing multiple files
- Prefer editing existing files over creating new ones

### Session Management
- Track story progress throughout the session
- Update checkboxes immediately after completing tasks
- Maintain context of the current story being worked on
- Save important state before long-running operations

### Error Recovery
- Always provide recovery suggestions for failures
- Include error context in messages to user
- Suggest rollback procedures when appropriate
- Document any manual fixes required

### Testing Strategy
- Run tests incrementally during development
- Always verify lint and typecheck before marking complete
- Test edge cases for each new feature
- Document test scenarios in story files

### Documentation
- Update relevant docs when changing functionality
- Include code examples in documentation
- Keep README synchronized with actual behavior
- Document breaking changes prominently

---

## Session Handoff Protocol

**At the end of each session:**

1. **Commit all work** — Don't leave uncommitted changes
2. **Create handoff document** — At `docs/sessions/YYYY-MM/session-{date}.md`

### Handoff Template

```markdown
# Session Handoff — YYYY-MM-DD

## What Was Done
- [ ] Task 1: Description
- [ ] Task 2: Description

## Current Status
- Active story: [Story ID if applicable]
- Active branch: [git branch name]
- Last commit: [hash or message]

## Next Steps
1. What should be done next
2. What dependencies exist
3. Any blockers or notes

## Context
- Key decisions made
- Files modified (top 5)
- Architectural notes if relevant
- Test coverage status

## Important Links
- Story: [link to story file]
- Related PRs/issues: [if applicable]
```

**Location:** Each month gets a folder: `docs/sessions/2026-02/session-2026-02-25.md`

This ensures **continuity** and **context preservation** across sessions.

---

## Framework Development: Working on AIOS Itself

This repository contains the **core AIOS framework**. When working on AIOS framework development (not just using AIOS in a project), follow these additional guidelines:

### Framework Development Structure

The AIOS framework is organized in strict layers:

```
.aios-core/
├── cli/                    # CLI commands and interface
│   ├── commands/          # Individual command implementations
│   ├── utils/             # CLI utilities and helpers
│   └── index.js           # CLI entry point
│
├── core/                  # Framework core systems (L1: PROTECTED)
│   ├── code-intel/        # Code analysis and intelligence enrichment
│   ├── config/            # Configuration management system
│   ├── execution/         # Story and task execution engine
│   ├── orchestration/     # Agent coordination and handoff protocol
│   ├── quality-gates/     # Automated quality validation (Constitutional gates)
│   ├── memory/            # Agent memory management and context
│   ├── graph-dashboard/   # Dependency visualization and metrics
│   ├── mcp/               # MCP server integration
│   ├── synapse/           # Event-driven core communication bus
│   ├── permissions/       # Permission and authority checking
│   ├── registry/          # Entity registry and resolution
│   ├── manifest/          # Manifest management
│   ├── migration/         # Migration utilities
│   ├── health-check/      # System health diagnostics
│   └── utils/             # Core utilities
│
├── development/           # Development tools and definitions (L2: EXTEND-ONLY)
│   ├── agents/           # Agent persona YAML/Markdown definitions
│   ├── tasks/            # Task workflow definitions (never modify core)
│   ├── workflows/        # Workflow compositions
│   ├── templates/        # Document and code templates
│   ├── checklists/       # Quality checklists and gates
│   ├── agent-teams/      # Multi-agent team compositions
│   ├── data/             # Development data and presets
│   └── scripts/          # Development scripts
│
├── data/                 # Data and configuration (L3: MUTABLE)
│   ├── tech-presets/     # Technology selection presets
│   └── [other data]/     # Framework data
│
└── [root files]
    ├── constitution.md   # CORE GOVERNANCE (PROTECTED)
    ├── core-config.yaml  # Framework configuration
    ├── version.json      # Version tracking
    └── install-manifest.yaml
```

### Framework Layer Protection (L1-L4 Model)

**L1 Framework Core** (NEVER modify):
- `.aios-core/core/**` — Core engine systems
- `.aios-core/constitution.md` — Core principles
- `bin/aios.js`, `bin/aios-init.js` — CLI entry points
- **Protected by deny rules** in `.claude/settings.json`

**L2 Framework Templates** (NEVER modify base, extend only):
- `.aios-core/development/tasks/` — Task definitions
- `.aios-core/development/templates/` — Document templates
- `.aios-core/development/checklists/` — Quality checklists
- `.aios-core/development/workflows/` — Workflow definitions
- `.aios-core/infrastructure/` — Infrastructure templates

**L3 Project Configuration** (Mutable with exceptions):
- `.aios-core/data/**` — Data and presets
- `agents/*/MEMORY.md` — Agent memory files
- `core-config.yaml` — Project configuration
- Allow rules in `.claude/settings.json` permit changes

**L4 Project Runtime** (ALWAYS modify):
- `docs/stories/` — Development stories
- `packages/`, `squads/` — Project deliverables
- `tests/` — Project tests

### Framework Development Commands

```bash
# Build framework distribution
npm run build

# Run complete test suite (unit + integration)
npm run test
npm run test:unit
npm run test:integration

# Code quality checks
npm run lint       # ESLint
npm run typecheck  # TypeScript
npm run build      # Verify packaging

# Validation
npm run validate:structure   # Check project structure integrity
npm run validate:agents      # Validate agent definitions YAML/Markdown

# IDE/Tool synchronization
npm run sync:ide            # Sync Claude Code integration
npm run sync:ide:check      # Verify sync status
npm run sync:skills:codex   # Sync Codex CLI skills

# Framework diagnostics
npm run doctor              # Run full diagnostics
aios graph --deps           # Visualize dependency tree
```

### Adding New Framework Features

**When adding a new agent:**
1. Create agent definition: `.aios-core/development/agents/agent-name.md`
2. Define persona, commands, authority boundaries
3. Create tasks in `.aios-core/development/tasks/` prefixed with agent initials
4. Add reference to `AGENTS.md` and `.claude/CLAUDE.md`
5. Run `npm run validate:agents` to verify
6. Run `npm run sync:ide` to integrate with Claude Code

**When adding a new task:**
1. Create task file: `.aios-core/development/tasks/task-name.md`
2. Follow task format with: inputs, outputs, pre/post-conditions, steps
3. Define execution modes (interactive/YOLO/pre-flight)
4. Reference in workflow files if part of larger workflow
5. Include quality gates and error handling
6. Add test coverage in `tests/`

**When adding new core functionality:**
1. Create story in `docs/stories/` following Story Development Cycle
2. Implement in `.aios-core/core/` with full test coverage
3. Document in inline comments (complex logic only)
4. Run full test suite and build verification
5. Ensure Constitution compliance (Article IV: No Invention)
6. Update relevant framework documentation

### Constitution Compliance for Framework Development

When working on AIOS framework:

- **Article I (CLI First)**: All new features must work 100% via CLI before any UI
- **Article II (Agent Authority)**: Respect agent boundaries; don't violate exclusive operations
- **Article III (Story-Driven)**: All framework work follows Story Development Cycle
- **Article IV (No Invention)**: Framework extensions only implement documented requirements
- **Article V (Quality First)**: Framework code must pass ALL quality gates
- **Article VI (Absolute Imports)**: Use `@/` aliases in framework code

**Non-compliance blocks merge.** Gates are in `.aios-core/development/tasks/pre-push.md`

### Framework Testing Strategy

```bash
# Unit tests (isolated functions/modules)
npm run test:unit

# Integration tests (multiple components)
npm run test:integration

# Test specific file
npm test tests/unit/core/executor.test.js

# Test matching pattern
npm test -- --testNamePattern="executor"

# Watch mode (if configured)
npm test -- --watch

# Coverage report
npm test -- --coverage
```

**Test organization:**
```
tests/
├── unit/              # Unit tests
│   ├── core/         # Core system tests
│   ├── cli/          # CLI command tests
│   └── fixtures/     # Mock data
├── integration/       # Integration tests
│   ├── workflows/    # Workflow execution tests
│   └── agents/       # Agent interaction tests
└── e2e/              # End-to-end tests (if applicable)
```

### Framework Performance Considerations

- **Code Intelligence**: Optional graceful fallback system (`.aios-core/core/code-intel/`)
- **Lazy Loading**: Heavy sections load on-demand (pvMindContext, squads, registry)
- **Agent Handoff**: Context compaction on agent switches (`.claude/rules/agent-handoff.md`)
- **Graph Dashboard**: Incremental rendering for large dependency trees

### Working with Brownfield Framework

For analyzing/documenting AIOS itself:
- Use `.aios-core/working-in-the-brownfield.md` as reference
- Utilize Gemini Web's 1M+ token context for full framework analysis
- Document architecture decisions in `.ai/decision-logs-index.md` (ADR format)
- Reference implementations use `@architect`, `@data-engineer`, `@analyst` agents

### Framework Versioning

- **package.json** in `.aios-core/` tracks version
- **version.json** tracks framework version number
- **MAJOR**: Breaking changes to core principles or API
- **MINOR**: New agent, task, or workflow; API expansion
- **PATCH**: Bug fixes, documentation, clarifications

Version changes require:
1. Update `version.json` and `.aios-core/package.json`
2. Update `constitution.md` if principles change
3. Create story documenting changes
4. Update framework documentation

### Debugging Framework Issues

```bash
# Enable debug logging
export AIOS_DEBUG=true
npm run dev

# Check framework integrity
npm run doctor

# Validate structure
npm run validate:structure

# Validate agents
npm run validate:agents

# View debug logs
tail -f .ai/debug-log.md

# View decision logs
cat .ai/decision-logs-index.md

# View project status
cat .aios/project-status.yaml
```

### Key Framework Files Reference

| File | Purpose | Status |
|------|---------|--------|
| `.aios-core/constitution.md` | Core principles and governance | PROTECTED (L1) |
| `.aios-core/core-config.yaml` | Framework configuration | Config (L3) |
| `.aios-core/package.json` | Framework package definition | PROTECTED (L1) |
| `.claude/settings.json` | Claude Code tool/permission config | Config (L3) |
| `.claude/rules/` | Contextual rules (auto-loaded) | Rules (L3) |
| `AGENTS.md` | Agent reference and shortcuts | Documentation |
| `docs/stories/` | Framework development stories | Runtime (L4) |
| `tests/` | Framework test suite | Runtime (L4) |

### Framework Development Workflow

1. **Understand the Constitution** (`.aios-core/constitution.md`)
2. **Check if story exists** in `docs/stories/`
3. **Read story** for full context and requirements
4. **Identify what layer** will be modified (L1/L2/L3/L4)
5. **Implement with tests** following patterns in codebase
6. **Run quality checks**: lint, typecheck, tests, build
7. **Update story checkboxes** as tasks complete
8. **Commit atomically** with conventional messages
9. **Verify no Constitutional violations**

---

*Synkra AIOS Claude Code Configuration v2.1*
*Framework Development Edition*
