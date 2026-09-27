# Agent Doc Stack v3.0

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
- **One concept per file; the path is its identity** — each doc covers one feature, spec, decision, reference, or plan; its `description` lets agents choose it without opening it (adapted from [Google's Open Knowledge Format](https://github.com/GoogleCloudPlatform/knowledge-catalog/blob/main/okf/SPEC.md))
- **Instructions over overviews** — agents follow explicit, non-standard rules well; generic repo overviews add tokens without improving results. Human-written rules beat generated summaries.

**Primary goal:** minimum tokens, maximum correctness, zero drift.

---

## 2. Canonical Repository Structure

```
# === Required (always create) ===
README.md                              # Product contract + entry point
ARCHITECTURE.md                        # System layout, boundaries, data flows
AGENTS.md                              # Agent table of contents (~120 lines)

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

For repositories with multiple packages, services, or distinct modules:

- A package gets its own `AGENTS.md` when its commands or conventions differ from root
- A package gets its own `ARCHITECTURE.md` when it deploys independently or owns its own data store
- A package gets nested agent config only where that agent requires it
- Subdirectory docs **supplement** the root docs — they add specificity, not replace shared rules
- When root and subdirectory docs conflict, the **most-specific doc wins** and the conflict MUST be flagged for resolution
- Keep the root-level docs as the canonical entry point; subdirectory docs handle local concerns only

---

## 3. Required Reading for Coding Agents

Agents MUST read `AGENTS.md` before making any change. It is the table of contents and the only forced read.

Every other doc is pulled on demand, based on the task:

| Task | Pull |
|------|------|
| Understand the product | `README.md` |
| Touch system boundaries, deployments, or data flow | `ARCHITECTURE.md` |
| Change a feature's behavior, inputs, outputs, or tests | `docs/features/<feature>.md` |
| Work that spans multiple features | `docs/product-specs/<spec>.md` |
| Change a user journey | `docs/app-workflows.md` |
| Change engineering or testing process | `docs/dev-workflows.md` |
| Behavioral or structural change | `docs/QUALITY_SCORE.md` |
| Touch reliability or security domains | `docs/RELIABILITY.md`, `docs/SECURITY.md` |
| Tool or MCP access needed | `docs/agent-tools.md` |
| Agent-specific operating rules apply | Agent config file (if present) |

`AGENTS.md` links every root doc and every single-file doc in `docs/`, and lists collection directories (`docs/features/`, `docs/product-specs/`, `docs/design-docs/`, `docs/references/`, `docs/exec-plans/`) as directories, not per file — agents pick a collection doc by its frontmatter `description` (section 19).

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

**Purpose:** the ~120-line entry point that tells agents how to operate in this repo

AGENTS.md is a **table of contents**, not a manual. It should be concise enough for an agent to read in a single pass and know exactly where to go next.

> **Note:** This file is also used directly by Codex CLI as its configuration file.

### MUST include

#### Quick Verify (1 line, top of file)
- `Quick verify: <exact command to run the fast test subset>`
- The single highest-frequency command agents will run. Keep it at the top so it's visible without scrolling.

#### Invariants and Guardrails (10-15 lines)
- Non-standard, repo-specific, actionable rules first; never restate framework defaults
- Rules agents must never break (testing, doc updates, PR structure)
- Testing requirements (TDD guidance, when to run tests)
- Doc update rules (same-PR requirement)
- Source from humans or existing docs, not generated — in 2026 studies, LLM-generated context files showed no benefit

#### Boundaries (5-10 lines)
- Key system boundaries phrased as rules (e.g., "`web/` never calls the DB directly — go through `api/`")
- Link to `ARCHITECTURE.md` for full detail

#### Repo Purpose (1-2 lines)
- What this repo is and what it produces

#### Doc Map (10-15 lines)
- Link every root doc and every single-file doc in `docs/`: file path + purpose, one line each
- List collection directories (`docs/features/`, `docs/product-specs/`, `docs/design-docs/`, `docs/references/`, `docs/exec-plans/`) as directories, not per file

#### Planning and Execution (5-10 lines)
- Where plans live (`docs/exec-plans/active/`)
- When plans are required
- Link to `docs/dev-workflows.md` for full process

#### Review Loop (5-10 lines)
- PR structure requirements
- What agents must verify before submitting

### Writing standard

- Target ~120 lines total — it is loaded on every task, so every line is a recurring token cost
- Every line is actionable or a pointer
- No prose blocks
- No duplicated rules — point to the source doc instead
- No frontmatter — Quick Verify stays on line 1

---

## 8. Feature Docs (`docs/features/<feature>.md`)

**Purpose:** source of truth for a core feature

**Size target:** ~80 lines max per feature doc

### Required structure

```markdown
---
description: <what it covers>. Read when <trigger>.
---
<!-- last_verified: YYYY-MM-DD -->
# Feature: <name>

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

## UX States (UI features only)
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
---
description: <what it covers>. Read when <trigger>.
---
<!-- last_verified: YYYY-MM-DD -->
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
- Exact commands
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

## 12. Agent Tools (`docs/agent-tools.md`) *(Create-on-need)*

**Purpose:** tool and context access contract (MCP-friendly)

Create when the repo configures MCP servers, CLIs agents must use, or skills.

### MUST contain

- Tool list and purpose
- Where access applies (local vs CI)
- Security boundaries

### Agent Skills

[Agent Skills](https://github.com/agentskills/agentskills) (`SKILL.md`: `name` + `description` frontmatter, loaded on demand) is the portable open standard for repeatable procedures (e.g., bugfix loop, doc-update routine).

- A skill only wraps a procedure already defined in `docs/dev-workflows.md`
- Links to it, never restates it (define once)
- Listed in this file
- Never created during documentation initialization
- Lives in the directory the agent in use discovers

---

## 13. Planning as Code (`docs/exec-plans/`)

**Purpose:** versioned planning artifacts that track work from intent to completion

Plans are not throwaway notes. They are versioned artifacts with goals, decisions, progress, and validation criteria.

### Structure

- `docs/exec-plans/active/` — plans currently in progress
- `docs/exec-plans/completed/` — plans that have been executed and validated
- `docs/exec-plans/tech-debt-tracker.md` — continuous tracking of known tech debt

### Plan MUST contain

- Frontmatter (section 19) — its `description` states the goal
- Decisions: key choices made and why
- Steps: ordered execution checklist
- Validation: how to verify the plan succeeded
- Progress log: updated as work proceeds

### When plans are required

A plan is required for a new feature, a data model or schema change, a change crossing an `ARCHITECTURE.md` boundary, or when the user requests one. Otherwise no plan.

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
---
description: <what it covers>. Read when <trigger>.
---
<!-- last_verified: YYYY-MM-DD -->
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

### Structure

```markdown
---
description: <what it covers>. Read when <trigger>.
---
<!-- last_verified: YYYY-MM-DD -->
# Reference: <topic>
```

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

- "Follow AGENTS.md" — single line
- Quick verify command (copy from AGENTS.md)
- Plan location (`docs/exec-plans/active/`)

### Writing standard

- ≤10 lines — pointer only, not a manual
- No policy, no rules, no duplication of AGENTS.md
- If you need to add rules, add them to AGENTS.md instead

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

Docs drift when code changes but docs don't.

- **Header timestamp** (required): every doc except agent config files MUST carry `<!-- last_verified: YYYY-MM-DD -->` at the top — on the line right after the closing `---` when frontmatter is present, and on line 2 of `AGENTS.md` (Quick Verify keeps line 1); agents update it when they verify or modify the doc

Additional mechanisms:

- **Agent pre-task check**: before starting work, agents MUST verify that the docs they read are consistent with the code they see; if not, flag the drift before proceeding
- **References are highest-risk**: `docs/references/` files summarize external sources that change independently — re-verify before relying on a reference whose `last_verified` is older than 90 days

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

### Frontmatter

```markdown
---
description: <what it covers>. Read when <trigger>.
---
<!-- last_verified: YYYY-MM-DD -->
```

- MUST be on every doc inside a `docs/` subdirectory: `features/`, `product-specs/`, `design-docs/`, `references/`, `exec-plans/active/`, `exec-plans/completed/` — including `_template.md` (placeholder description) and nested package `docs/` dirs
- `description`: one line, ≤120 chars, format "<what it covers>. Read when <trigger>."
- No other key is defined; keys a doc site generator requires (e.g., `title`, `sidebar_position`) are allowed
- Root and single-file docs carry none — `README.md`, `AGENTS.md`, `ARCHITECTURE.md`, `app-workflows.md`, `dev-workflows.md`, `QUALITY_SCORE.md`, `RELIABILITY.md`, `SECURITY.md`, `agent-tools.md`, `tech-debt-tracker.md`, agent config files: their path identifies them, and `AGENTS.md` Quick Verify stays line 1
- Agents select docs by scanning descriptions (`grep '^description:' docs/features/*.md`) before opening them
- Frontmatter lines don't count toward size targets

### What NOT to document

- **Linter/formatter rules** — if ESLint, Prettier, Ruff, etc. enforce it, don't restate it in docs
- **Type system guarantees** — if TypeScript, Go, or Rust enforce a constraint, the types are the doc
- **Framework defaults** — don't document behavior that's standard for the framework in use
- **Obvious conventions** — if the codebase consistently follows a pattern, agents will infer it from code; only document deviations or non-obvious choices
- **Historical context** — don't explain why old decisions were made unless it constrains future decisions (use `docs/design-docs/` for that)
- **Edge cases that can't happen** — don't document impossible states or scenarios the type system prevents
