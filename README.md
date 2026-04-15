# Agent Doc Stack

A contract-based, agent-first documentation specification for AI-assisted software development.

**Humans steer. Agents execute.**

---

## The context layer of the harness

A coding agent's harness is the environment that shapes how it operates: the context it loads, the tools it can call, the rules it follows, the memory it carries. A harness is only as good as its weakest layer — the strongest model and cleanest sandbox still produce poor output if the agent can't answer three questions from inside the repository before it starts work:

1. **What is this system and what does it do?**
2. **Where do I look when I need to change X?**
3. **What rules must I not break?**

Every question the agent can't answer in-repo becomes a prompt a human writes, a mistake a reviewer catches, or a hallucination that ships. Agent Doc Stack is the context layer that answers those three questions deterministically, without external lookups and without burning the context window on irrelevant prose.

The spec is built on four principles that keep surfacing across teams running coding agents at scale:

- **Repo-local or it doesn't exist.** Slack, Confluence, and tribal knowledge are invisible to agents. If it isn't committed, it isn't real.
- **Progressive disclosure.** One short entry point; deeper docs pulled only when the task demands them. Context windows are finite.
- **Define once, link everywhere.** Duplicated facts drift, and drift silently poisons agent behavior.
- **Contracts, not prose.** Bullets, templates, exact commands, canonical file references — structured so the agent can act on them.

---

## What the spec produces

Agent Doc Stack lays down a predictable repository shape. `AGENTS.md` is the ~120-line entry point every agent reads first; everything else is pulled on demand.

```
repo-root/
├── README.md                          # product contract, setup, commands
├── ARCHITECTURE.md                    # system layout, boundaries, data flows
├── AGENTS.md                          # ~120-line table of contents (the only forced read)
│
├── docs/
│   ├── app-workflows.md               # user journeys
│   ├── dev-workflows.md               # engineering workflows (testing, PRs, bugfix loop)
│   │
│   ├── features/                      # one file per core feature
│   │   ├── _template.md
│   │   └── <feature>.md
│   │
│   ├── exec-plans/                    # planning as code
│   │   ├── active/                    # in-progress plans
│   │   ├── completed/                 # validated, archived plans
│   │   └── tech-debt-tracker.md
│   │
│   ├── product-specs/        (on need) # cross-cutting product behavior
│   ├── design-docs/          (on need) # lightweight ADRs
│   ├── references/           (on need) # pinned external knowledge
│   ├── agent-tools.md        (on need) # MCP / tool access contract
│   ├── QUALITY_SCORE.md      (on need) # golden principles, quality tracking
│   ├── RELIABILITY.md        (on need) # SLOs, fragile areas
│   └── SECURITY.md           (on need) # trust boundaries, auth, data policy
│
└── <agent-config>                     # only the one you use:
                                       #   CLAUDE.md  |  .cursor/rules/*.mdc
                                       #   GEMINI.md  |  .github/copilot-instructions.md
                                       # Codex CLI uses AGENTS.md directly
```

**Required docs** (`README`, `ARCHITECTURE`, `AGENTS`, `app-workflows`, `dev-workflows`, `features/`, `exec-plans/`) are always created. **Create-on-need docs** are added only when you have concrete content — the spec explicitly discourages empty scaffolding.

Every doc has a size target, a required structure, and a clear purpose defined in the spec.

---

## How an agent uses it

When an agent enters the repository, its read path is deterministic:

1. **Read `AGENTS.md`** — always, every task. This is the only forced read.
2. **Identify the task type** — feature change, architecture change, workflow change, etc.
3. **Pull the matching deep doc** from the locations above (the spec provides a task → doc lookup table).
4. **Make the change.** When the change lands, the same lookup tells the agent which doc to update in the same PR.

This replaces the common failure mode where agents either ingest every doc upfront (burning context) or skip the docs entirely (shipping drift). The spec works across Claude Code, Codex CLI, Cursor, Gemini CLI, and Copilot Agent because the structure — not the agent — is the contract.

---

## What you get from adoption

- A repository a coding agent can operate in without babysitting
- A documentation structure that stays DRY, navigable, and low-token as the codebase grows
- A consistent operating contract across every major coding agent in use today
- A clear path for onboarding new agents (and new humans) without rewriting institutional knowledge

What you do **not** get: hooks, CI jobs, linters, or automation. This spec is intentionally docs-only. The harness has other layers — tool configuration, memory, sandboxing, enforcement — that are out of scope here.

---

## How to use it

1. Open the target repository in your coding agent
2. Run `initialization-prompt.md`
3. The agent fetches and reads `agent-doc-stack.md` directly from GitHub
4. Answer clarifying questions if the agent requests missing information

The agent follows the spec to generate, update, or normalize documentation without inventing behavior. Existing docs are preserved and relocated, not overwritten.

> You do not need to copy this repository into your project. The spec is fetched on demand.

---

## Repository contents

| File | Purpose |
|------|---------|
| `agent-doc-stack.md` | The authoritative v2.2 specification — single source of truth for all structure, rules, and canonical file locations |
| `initialization-prompt.md` | Reusable prompt to initialize or normalize a repo's documentation using the spec |

---

## Versioning

- Current version: **v2.2** (defined in `agent-doc-stack.md`)
- Patch: clarify wording, no behavior change
- Minor: add rules, backward compatible
- Major: breaking changes

Pin to a specific version for strict stability.
