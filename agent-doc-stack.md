# Agent Doc Stack

A contract-based, agent-first documentation system for AI-assisted application development with low-token, high-signal standards

---

## 1. Core Principles

- Docs are contracts, not explanations
- Structure over prose
- Bullets over paragraphs
- Durable truth lives in predictable locations
- Temporary thinking never becomes documentation
- Agents must always know:
  - what to read
  - where to write
  - what to update
- Tests are the executable contract for behavior

**Primary goal:**
- minimum tokens
- maximum correctness
- zero drift

---

## 2. Canonical Repository Structure

```
README.md
ARCHITECTURE.md

docs/
  app-workflows.md
  dev-workflows.md
  agent-tools.md        # optional (MCP / tool access)
  features/
    _template.md
    <feature>.md

AGENTS.md

# Agent-specific config (create only for the agent in use)
CLAUDE.md                           # Claude Code only
.cursorrules                        # Cursor (legacy)
.cursor/rules/*.mdc                 # Cursor (current)
.github/copilot-instructions.md     # GitHub Copilot

plans/
  _template.md
  YYYY-MM-DD-short-title.md
```

> **Hard Rule:** Agents MUST NOT create new documentation files unless explicitly instructed.

> **Agent Config Rule:** Only create the agent-specific config file for the agent currently being used. Do not create config files for other agents. Note: Codex CLI uses `AGENTS.md` directly—no separate config file needed.

---

## 3. Required Reading Order for Coding Agents

Before making changes, agents MUST read docs in this order:

1. `README.md`
2. `ARCHITECTURE.md`
3. `docs/features/<feature>.md` (if applicable)
4. `docs/app-workflows.md`
5. `docs/dev-workflows.md`
6. `AGENTS.md`
7. Agent-specific config file (if present)
8. `docs/agent-tools.md` (only if tools are required)

---

## 4. README.md

**Purpose:** product contract + GitHub-quality entry point

### MUST contain

- Product overview and target user
- Core features list (links to feature docs)
- Short architecture summary (link to ARCHITECTURE.md)
- Tech stack (surface-level only)
- Setup instructions
- Environment variable names (no values)
- Run commands
- Test commands:
  - quick subset
  - full suite
- Documentation map (links to all core docs)
- Contribution basics (reference AGENTS.md)

### Tech stack rules (README.md)

**Allowed:**
- Languages, frameworks, platforms, services
- Hosting providers
- Datastores by type

**Disallowed:**
- DB schemas or tables
- Deployment topology
- Secrets
- Infrastructure scripts

### Writing standard

- Bullets where possible
- No implementation detail

---

## 5. ARCHITECTURE.md

**Purpose:** system layout, deployments, and data flows

### MUST contain

- Components and responsibilities
- Deployment locations (what runs where)
- Data stores and ownership
- External services
- Trust boundaries
- Data flows
- Core features list (links to feature docs)

### Tech stack rules (ARCHITECTURE.md)

**Allowed:**
- Hosting targets
- Service boundaries
- Logical data entities
- Event and request flows

**Disallowed:**
- Table-level schemas
- ORM field lists
- SQL
- Migration detail

### Writing standard

- Lists only
- No rationale unless required for correctness

---

## 6. Feature Docs (`docs/features/<feature>.md`)

**Purpose:** source of truth for a core feature

### Required structure

```markdown
# Feature: <n>

## Purpose
One sentence describing the problem this feature solves.

## Used By
- UI: <screen or flow>
- API: <endpoint>
- Job: <worker or cron> (if any)

## Core Functions
- function_or_module_a
- function_or_module_b

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
- Case → expected behavior

## UX States (if applicable)
- Empty
- Loading
- Error

## Tests
- Path/to/tests
- Required cases: happy path + 2–5 edge cases
- How to run

## Related Docs
- README.md
- ARCHITECTURE.md
- docs/app-workflows.md
```

### Feature doc tech rules

**Allowed:**
- Logical data usage ("writes to primary database")
- Side effects

**Disallowed:**
- Table names
- Field names
- Indexes

### Writing standard

- Bullets only
- Every line constrains behavior

---

## 7. App Workflows (`docs/app-workflows.md`)

**Purpose:** user journeys inside the product

### MUST contain

- One section per core user workflow
- Step-by-step user actions
- High-level system responses
- Links to relevant feature docs

### Writing standard

- No component names
- No backend detail

---

## 8. Dev Workflows (`docs/dev-workflows.md`)

**Purpose:** how engineering work is done in this repo

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

**Test placement:**
- Explicit repo paths

**Commands:**
- Quick: relevant subset
- Full: full suite

**When to run:**
- After behavior change: run relevant subset
- Before PR: run full suite (or document why not)

**Bugfix rule:**
1. Add failing test first
2. Confirm failure
3. Implement fix
4. Rerun tests until green

---

## 9. Agent Tools (`docs/agent-tools.md`) *(Optional)*

**Purpose:** tool and context access contract (MCP-friendly)

### MAY contain

- Tool list and purpose
- Local vs CI access
- Security boundaries
- MCP usage notes

---

## 10. AGENTS.md

**Purpose:** global enforcement rules for coding agents

> **Note:** This file is also used directly by Codex CLI as its configuration file.

### MUST include

#### Documentation Rules

- Docs MUST be updated in the same PR as code changes
- New doc files MUST NOT be created unless explicitly instructed
- Feature behavior changes → `docs/features/<feature>.md`
- User flow changes → `docs/app-workflows.md`
- System boundary changes → `ARCHITECTURE.md`
- Dev or testing process changes → `docs/dev-workflows.md`
- Setup or scope changes → `README.md`

#### Planning Rules

- Multi-file changes require a plan
- Plans MUST be written to `plans/`

#### init.md Handling

- `init.md` is not a durable document
- If generated, contents MUST be merged into:
  - `README.md`
  - `ARCHITECTURE.md`
  - `docs/app-workflows.md`
- `init.md` MUST NOT be committed

#### Testing Rules (Required)

- Tests MUST be added or updated for any behavior change
- Tests MUST NOT be weakened or bypassed unless explicitly instructed
- Relevant test subset MUST be run after changes
- Full test suite MUST be run before PR (or reason documented)

#### TDD Guidance

**Agents SHOULD use TDD when:**
- Behavior is well-defined
- A bug is reproducible
- Core logic or critical paths are touched

**Agents MAY implement first when:**
- Work is UI-only
- Behavior is exploratory
- No test harness exists yet

> Behavioral changes REQUIRE tests unless explicitly waived.

#### Pull Requests (Single Source of Truth)

- All changes MUST be submitted via PR
- Agents MUST follow PR structure defined here

**Required PR sections:**
- Summary
- Feature doc links (if applicable)
- Tests run (subset + full)
- Docs updated (explicit list)
- Risks / assumptions
- Manual validation steps
- Plan reference (if used)

---

## 11. Agent-Specific Config Files *(Conditional)*

**Purpose:** tool-specific operating constraints for a particular coding agent

> **Rule:** Only create the config file for the agent currently in use. Do not create files for other agents.

### Supported agents and config locations

| Agent | Config File | Notes |
|-------|-------------|-------|
| Claude Code | `CLAUDE.md` | Project root |
| Codex CLI | `AGENTS.md` | Uses the shared AGENTS.md file directly |
| Cursor | `.cursorrules` or `.cursor/rules/*.mdc` | `.cursorrules` is legacy; `.mdc` files are current |
| GitHub Copilot | `.github/copilot-instructions.md` | Inside `.github/` directory |

### MUST include (for agent-specific files)

- Follow AGENTS.md at all times
- Required doc read order
- Plan location (`plans/`)
- Exact test commands
- Diff discipline

### Writing standard

- 10–20 lines max
- No policy duplication
- Reference AGENTS.md for shared rules

---

## 12. Plans (`plans/`)

**Purpose:** temporary task-level reasoning

### Allowed

- Multi-file plans
- New feature plans
- Refactor plans

### Disallowed

- Feature behavior
- Architecture decisions
- User workflows

**Plans:**
- Referenced in PRs
- Optional to keep post-merge
- Never a source of truth

---

## 13. Doc Update Mapping (Non-Negotiable)

When code changes, agents MUST update:

| Change Type | Update Location |
|-------------|-----------------|
| Feature logic, inputs, outputs, tests | `docs/features/<feature>.md` |
| User journeys | `docs/app-workflows.md` |
| System layout, deployments, integrations | `ARCHITECTURE.md` |
| Dev or testing process | `docs/dev-workflows.md` |
| Setup or tech stack summary | `README.md` |
| Temporary reasoning | `plans/` |

---

## 14. Documentation Writing Rules (Token-Efficient)

All docs MUST follow:

- Bullets over sentences
- Only update affected sections
- No rewording unchanged content
- No rationale unless required for correctness

> **Golden rule:** As short as possible, but precise enough to prevent ambiguity.
