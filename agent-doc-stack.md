# Agent Doc Stack v2.1

A contract-based, agent-first documentation system for AI-assisted application development

---

## 1. Core Philosophy

**Humans steer. Agents execute.**

Documentation exists so coding agents can operate autonomously within defined boundaries. Every doc artifact is written for agents as the primary consumer and humans as reviewers.

### Foundational Principles

- Repository-local knowledge is the only truth — if the agent can't read it in-repo, it doesn't exist
- Docs are contracts, not explanations
- Structure over prose; bullets over paragraphs
- Durable truth lives in predictable locations
- Temporary thinking never becomes documentation
- Documentation must use progressive disclosure: short entrypoints point to deep docs
- Tests are the executable contract for behavior
- Agents must always know: what to read, where to write, what to update
- **DRY docs: define every fact once, in one canonical location, and link everywhere else** — if information appears in two places, one of them is wrong (or will be soon)
- **Token budgets matter** — every doc has a size target; agent performance degrades as context fills, so keep docs within their budgets
- **Don't document what tools enforce** — if linters, type checkers, or frameworks already enforce it, don't repeat it in docs

**Primary goal:** minimum tokens, maximum correctness, zero drift.

---

## 2. Canonical Repository Structure

```
# === Required (always create) ===
README.md                              # Product contract + entry point
ARCHITECTURE.md                        # System layout, boundaries, data flows
AGENTS.md                              # Agent table of contents (~100 lines)

docs/
  app-workflows.md                     # User journeys
  dev-workflows.md                     # Engineering workflows
  features/
    _template.md
    <feature>.md
  exec-plans/
    active/
      <plan>.md
    completed/
      <plan>.md
    tech-debt-tracker.md

# === Create-on-need (only when you have concrete content) ===
  agent-tools.md                       # MCP / tool access
  QUALITY_SCORE.md                     # Golden principles + quality tracking
  RELIABILITY.md                       # SLOs, fragile areas, reliability constraints
  SECURITY.md                          # Trust boundaries, auth, data policies
  product-specs/                       # Cross-cutting product specs
    <spec>.md
  design-docs/                         # Architecture decisions (lightweight ADRs)
    <decision>.md
  references/                          # Pinned external knowledge
    <topic>.md

# === Agent-specific config (create only for the agent in use) ===
CLAUDE.md                              # Claude Code only
.cursorrules                           # Cursor (legacy)
.cursor/rules/*.mdc                    # Cursor (current)
.github/copilot-instructions.md        # GitHub Copilot
```

> **Hard Rule:** Agents MUST NOT create new documentation files unless explicitly instructed.

> **Agent Config Rule:** Only create the agent-specific config file for the agent currently being used. Codex CLI uses `AGENTS.md` directly — no separate config file needed.

### Nested docs for larger repos (monorepos, multi-service)

For repositories with multiple packages, services, or distinct modules, subdirectory-scoped docs are allowed:

- Each package/service MAY have its own `AGENTS.md`, `ARCHITECTURE.md`, or agent config file
- Subdirectory docs **supplement** the root docs — they add specificity, not replace shared rules
- When root and subdirectory docs conflict, the **most-specific doc wins** and the conflict should be flagged for resolution
- Keep the root-level docs as the canonical entry point; subdirectory docs handle local concerns only

---

## 3. Required Reading Order for Coding Agents

Before making changes, agents MUST read docs in this order:

1. `README.md`
2. `ARCHITECTURE.md`
3. `AGENTS.md` (table of contents — points to everything else)
4. `docs/features/<feature>.md` or `docs/product-specs/<spec>.md` (if applicable)
5. `docs/app-workflows.md`
6. `docs/dev-workflows.md`
7. Agent-specific config file (if present)
8. `docs/QUALITY_SCORE.md` (if making behavioral or structural changes)
9. `docs/RELIABILITY.md` or `docs/SECURITY.md` (if touching those domains)
10. `docs/agent-tools.md` (only if tools are required)

### Conflict resolution

When two docs contradict each other:

- **Most-specific doc wins** — a feature doc overrides ARCHITECTURE.md for that feature's behavior
- **Subdirectory docs override root docs** for their scope (see section 2)
- Agent MUST flag the conflict in its PR so humans can reconcile the source docs

---

## 4. Agent Legibility

All knowledge agents need must live in-repo as versioned markdown.

**Rules:**

- Never rely on Slack, Google Docs, Confluence, Jira, or tribal knowledge
- If information exists only in an external system, it must be pinned into `docs/references/` as a markdown summary before agents can use it
- External links are allowed as citations but never as the source of truth
- Every doc file must be self-contained enough that an agent can act on it without additional context

**Test:** If a fresh agent session cannot find and act on the information, the documentation has failed.

---

## 5. README.md

**Purpose:** product contract + GitHub-quality entry point

**Size target:** ~150 lines max

### MUST contain

- Product overview and target user
- Core features list (links to feature docs)
- Short architecture summary (link to ARCHITECTURE.md)
- Tech stack (surface-level only)
- Setup instructions
- Environment variable names (no values)
- Run commands
- Test commands (quick subset + full suite)
- Documentation map (links to all core docs)
- Contribution basics (reference AGENTS.md)

### Tech stack rules

**Allowed:** languages, frameworks, platforms, services, hosting, datastores by type

**Disallowed:** DB schemas, deployment topology, secrets, infrastructure scripts

### Writing standard

- Bullets where possible
- No implementation detail

---

## 6. ARCHITECTURE.md

**Purpose:** system layout, deployments, and data flows

**Size target:** ~200 lines max

### MUST contain

- Components and responsibilities
- Deployment locations (what runs where)
- Data stores and ownership
- External services
- Trust boundaries
- Data flows
- Core features list (links to feature docs)
- Canonical file references — pointer to one exemplar file per major pattern (e.g., "Canonical API handler: `src/handlers/users.ts`")
- References to relevant design docs in `docs/design-docs/`

### Tech stack rules

**Allowed:** hosting targets, service boundaries, logical data entities, event and request flows

**Disallowed:** table-level schemas, ORM field lists, SQL, migration detail

### Writing standard

- Lists only
- No rationale unless required for correctness

---

## 7. AGENTS.md (Agent Table of Contents)

**Purpose:** the ~100-line entry point that tells agents how to operate in this repo

AGENTS.md is a **table of contents**, not a manual. It should be concise enough for an agent to read in a single pass and know exactly where to go next.

> **Note:** This file is also used directly by Codex CLI as its configuration file.

### MUST include

#### Repo Purpose (2-3 lines)
- What this repo is and what it produces

#### Architecture Boundaries (5-10 lines)
- Key system boundaries agents must respect
- Link to `ARCHITECTURE.md` for full detail

#### Invariants and Guardrails (10-15 lines)
- Rules agents must never break (testing, doc updates, PR structure)
- Testing requirements (TDD guidance, when to run tests)
- Doc update rules (same-PR requirement)

#### Doc Map (10-15 lines)
- Pointer to every canonical doc location
- One line per doc: file path + purpose

#### Planning and Execution (5-10 lines)
- Where plans live (`docs/exec-plans/active/`)
- When plans are required
- Link to `docs/dev-workflows.md` for full process

#### Review Loop (5-10 lines)
- PR structure requirements
- What agents must verify before submitting

### Writing standard

- Target ~100 lines total
- Every line is actionable or a pointer
- No prose blocks
- No duplicated rules — point to the source doc instead

---

## 8. Feature Docs (`docs/features/<feature>.md`)

**Purpose:** source of truth for a core feature

**Size target:** ~80 lines max per feature doc

### Required structure

```markdown
# Feature: <name>

## Purpose
One sentence describing the problem this feature solves.

## Used By
- UI: <screen or flow>
- API: <endpoint>
- Job: <worker or cron> (if any)

## Core Functions
- function_or_module_a
- function_or_module_b

## Canonical Files
- Pattern exemplar: `path/to/canonical/implementation`

## Inputs
- name: type (source)

## Outputs
- name: type
- side effects (DB write, event, notification)

## Flow
- Step 1
- Step 2
- Step 3

## Edge Cases
- Case -> expected behavior

## UX States (if applicable)
- Empty
- Loading
- Error

## Verification
- Test files: `path/to/tests`
- Required cases: happy path + 2-5 edge cases
- Quick verify command: `<exact command to run this feature's tests>`
- Full verify command: `<exact command to run full suite>`
- Pass criteria: what "green" looks like for this feature

## Related Docs
- README.md
- ARCHITECTURE.md
- docs/app-workflows.md
```

### Feature doc tech rules

**Allowed:** logical data usage ("writes to primary database"), side effects

**Disallowed:** table names, field names, indexes

### Writing standard

- Bullets only
- Every line constrains behavior

---

## 9. Product Specs (`docs/product-specs/<spec>.md`)

**Purpose:** cross-cutting product behavior that spans multiple features

Use `docs/product-specs/` when documentation spans multiple features, describes cross-cutting product behavior, or captures product requirements that don't fit a single feature doc.

**Size target:** ~120 lines max per spec

### Required structure

```markdown
# Spec: <name>

## Scope
Which features and systems this spec covers.

## Features Involved
- [Feature A](../features/feature-a.md)
- [Feature B](../features/feature-b.md)

## User Personas Affected
- Persona -> how they're impacted

## Cross-Cutting Behaviors
- Behavior description -> which features implement it
- Shared constraints that apply across features

## Business Rules
- Rule -> enforcement mechanism

## Constraints
- Performance, compliance, or UX constraints that span features

## Verification
- How to validate cross-feature behavior end-to-end
- Exact commands if applicable
```

### Writing standard

- Bullets only
- Must link to related feature docs — never duplicate feature-level detail
- One spec per product area

---

## 10. App Workflows (`docs/app-workflows.md`)

**Purpose:** user journeys inside the product

**Size target:** ~150 lines max

### MUST contain

- One section per core user workflow
- Step-by-step user actions
- High-level system responses
- Links to relevant feature docs

### Writing standard

- No component names
- No backend detail

---

## 11. Dev Workflows (`docs/dev-workflows.md`)

**Purpose:** how engineering work is done in this repo

**Size target:** ~150 lines max

### MUST contain

- New feature workflow
- Bugfix workflow
- Refactor workflow
- Documentation update workflow
- Pull request workflow
- Testing workflow (required)

### Writing standard

- Checklists only

### Required Testing Section

**Test types:**
- Unit: pure logic
- Integration: boundaries (HTTP handlers, DB writes, queue jobs)
- E2E: only highest-value user workflows (few, stable)

**Test placement:** explicit repo paths

**Commands:** quick (relevant subset), full (full suite)

**When to run:**
- After behavior change: run relevant subset
- Before PR: run full suite (or document why not)

**Bugfix rule:**
1. Add failing test first
2. Confirm failure
3. Implement fix
4. Rerun tests until green

---

## 12. Agent Tools (`docs/agent-tools.md`) *(Optional)*

**Purpose:** tool and context access contract (MCP-friendly)

### MAY contain

- Tool list and purpose
- Local vs CI access
- Security boundaries
- MCP usage notes

### Agent skills and custom commands

Most coding agents support reusable skills or custom commands (e.g., Claude Code's `.claude/commands/`, Codex's `.agents/skills/`, Cursor's `.cursor/plans/`). These are **agent-specific features** — use each agent's native format and directory structure rather than defining a shared location. If skills are in use, document their existence and purpose in this file so other agents or humans know they're available.

---

## 13. Planning as Code (`docs/exec-plans/`)

**Purpose:** versioned planning artifacts that track work from intent to completion

Plans are not throwaway notes. They are versioned artifacts with goals, decisions, progress, and validation criteria.

### Structure

- `docs/exec-plans/active/` — plans currently in progress
- `docs/exec-plans/completed/` — plans that have been executed and validated
- `docs/exec-plans/tech-debt-tracker.md` — continuous tracking of known tech debt

### Plan MUST contain

- Goal: what this plan achieves
- Decisions: key choices made and why
- Steps: ordered execution checklist
- Validation: how to verify the plan succeeded
- Progress log: updated as work proceeds

### When plans are required

- Multi-file changes
- New features
- Refactors
- Non-trivial assumptions

### Lifecycle

1. Create in `active/`
2. Update progress as work proceeds
3. Move to `completed/` after validation
4. Reference in PR

### Tech Debt Tracker (`docs/exec-plans/tech-debt-tracker.md`)

- Continuous log of known tech debt
- Each entry: description, impact, proposed resolution, priority
- Agents update this when they discover or create tech debt
- Review during planning to avoid compounding debt

---

## 14. Quality and Maintenance Docs

### `docs/QUALITY_SCORE.md`

**Purpose:** encode the project's golden principles and track quality over time

### MUST contain

- Shared utilities and abstraction rules
- Data boundary validation requirements
- Test coverage expectations
- Architectural layering rules
- Quality checklist agents must verify before PR

Agents update this doc when quality standards change or new patterns are established.

### `docs/RELIABILITY.md`

**Purpose:** reliability constraints and operational awareness

### MUST contain

- SLOs and performance targets (if defined)
- Known fragile areas and mitigation strategies
- Graceful degradation requirements
- Monitoring and alerting expectations

### `docs/SECURITY.md`

**Purpose:** security policies agents must follow

### MUST contain

- Trust boundaries (what talks to what, with what permissions)
- Authentication and authorization model
- Data handling policies (PII, encryption, retention)
- Security-sensitive code paths
- Dependency security requirements

---

## 15. Design Docs (`docs/design-docs/`)

**Purpose:** record architecture decisions and significant design choices

### Structure (lightweight ADR)

```markdown
# Decision: <title>

## Context
What problem or situation prompted this decision.

## Decision
What was decided.

## Consequences
- What this enables
- What this constrains
- Trade-offs accepted
```

### Rules

- One doc per significant decision
- Referenced from ARCHITECTURE.md
- Not retroactive — only create for new decisions going forward
- Agents create design docs when making architectural choices that affect system boundaries

---

## 16. References (`docs/references/`)

**Purpose:** pinned external knowledge that agents need

### When to use

- External API documentation that agents must reference
- Third-party service constraints or configuration
- Standards or specifications the project must comply with

### Rules

- Each file is a markdown summary of external knowledge
- Include source URL as a citation
- Keep concise — only what's needed for agent execution
- Update when external sources change

---

## 17. Agent-Specific Config Files *(Conditional)*

**Purpose:** tool-specific operating constraints for a particular coding agent

> **Rule:** Only create the config file for the agent currently in use.

### Supported agents and config locations

| Agent | Config File | Notes |
|-------|-------------|-------|
| Claude Code | `CLAUDE.md` | Project root; supports nested `CLAUDE.md` in subdirectories |
| Codex CLI | `AGENTS.md` | Uses the shared AGENTS.md file directly; supports `AGENTS.override.md` per directory |
| Cursor | `.cursor/rules/*.mdc` | `.mdc` files scoped by glob; `.cursorrules` is legacy |
| Gemini CLI | `GEMINI.md` or `AGENT.md` | Project root; supports nested files in subdirectories |
| GitHub Copilot | `.github/copilot-instructions.md` | Inside `.github/` directory |

### Nested agent config (for larger repos)

All major agents support hierarchical config discovery — subdirectory configs supplement or override root configs:

- **Claude Code**: nested `CLAUDE.md` files add scoped context per directory
- **Codex CLI**: walks from git root to CWD, merging `AGENTS.md` at each level; `AGENTS.override.md` takes precedence at the same level
- **Cursor**: `.mdc` rules can be scoped to specific file patterns via globs
- **Gemini**: nested `GEMINI.md` files override general context with specific context

Use nested configs when distinct modules, services, or packages need different conventions. Keep root-level config for repo-wide rules.

### MUST include (for agent-specific files)

- Follow AGENTS.md at all times
- Required doc read order
- Plan location (`docs/exec-plans/active/`)
- Exact test commands
- Diff discipline

### Writing standard

- 10-20 lines max
- No policy duplication
- Reference AGENTS.md for shared rules

---

## 18. Doc Update Mapping (Non-Negotiable)

When code changes, agents MUST update:

| Change Type | Update Location |
|-------------|-----------------|
| Feature logic, inputs, outputs, tests | `docs/features/<feature>.md` |
| Cross-cutting product behavior | `docs/product-specs/<spec>.md` |
| User journeys | `docs/app-workflows.md` |
| System layout, deployments, integrations | `ARCHITECTURE.md` |
| Dev or testing process | `docs/dev-workflows.md` |
| Architecture decisions | `docs/design-docs/<decision>.md` |
| Quality standards or patterns | `docs/QUALITY_SCORE.md` |
| Reliability constraints or SLOs | `docs/RELIABILITY.md` |
| Security policies or trust boundaries | `docs/SECURITY.md` |
| Setup or tech stack summary | `README.md` |
| Active work plans | `docs/exec-plans/active/` |
| Known tech debt | `docs/exec-plans/tech-debt-tracker.md` |

### Staleness detection

Docs drift when code changes but docs don't. Use one or more of these mechanisms to catch it:

- **Header timestamp**: add `<!-- last_verified: YYYY-MM-DD -->` to the top of each doc; agents update this when they verify or modify the doc
- **CI check** (recommended): compare modification dates of doc files against their related source files; flag docs that haven't been updated since related code changed
- **Agent pre-task check**: before starting work, agents should verify that the docs they read are consistent with the code they see; if not, flag the drift before proceeding
- **References are highest-risk**: `docs/references/` files summarize external sources that change independently — review these on a regular cadence

---

## 19. Documentation Writing Rules (Token-Efficient)

All docs MUST follow:

- Bullets over sentences
- Only update affected sections
- No rewording unchanged content
- No rationale unless required for correctness
- Define every fact once — link, don't repeat
- Reference canonical files instead of copying code into docs — keeps docs short and prevents staleness

> **Golden Rule:** Define once, link everywhere. As short as possible, but precise enough to prevent ambiguity. If a fact lives in two files, delete one and replace it with a link.

### What NOT to document

- **Linter/formatter rules** — if ESLint, Prettier, Ruff, etc. enforce it, don't restate it in docs
- **Type system guarantees** — if TypeScript, Go, or Rust enforce a constraint, the types are the doc
- **Framework defaults** — don't document behavior that's standard for the framework in use
- **Obvious conventions** — if the codebase consistently follows a pattern, agents will infer it from code; only document deviations or non-obvious choices
- **Historical context** — don't explain why old decisions were made unless it constrains future decisions (use `docs/design-docs/` for that)
- **Edge cases that can't happen** — don't document impossible states or scenarios the type system prevents
