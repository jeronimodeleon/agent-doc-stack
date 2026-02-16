# Agent Doc Stack

Agent Doc Stack is an agent-first documentation architecture where coding agents are the primary consumers. It defines where truth lives, how agents discover it through progressive disclosure, and how they generate, update, and enforce docs with minimal tokens and zero drift.

**Humans steer. Agents execute.**

This repository contains the **Agent Doc Stack v2.0 specification** and an initialization prompt used to apply the framework to any codebase.

---

## What This Is

Agent Doc Stack is a **contract-based documentation specification** for AI-assisted software development.

It defines:

- Canonical documentation locations with progressive disclosure
- Structured discovery: agents read a ~100-line table of contents, then drill into deep docs
- Planning as code: versioned execution plans with goals, decisions, and validation
- Quality, reliability, and security as first-class doc artifacts
- Writing standards optimized for token efficiency
- Enforcement rules to prevent documentation drift

This is a **specification**, not a template or a tool.

It works across modern coding agents including Claude Code, Codex CLI, Cursor, and Copilot Agent.

---

## Canonical Repository Structure

```text
README.md                              # Product contract + entry point
ARCHITECTURE.md                        # System layout, boundaries, data flows
AGENTS.md                              # Agent table of contents (~100 lines)

docs/
  app-workflows.md                     # User journeys
  dev-workflows.md                     # Engineering workflows
  agent-tools.md                       # MCP / tool access (optional)
  QUALITY_SCORE.md                     # Golden principles + quality tracking
  RELIABILITY.md                       # SLOs, fragile areas, reliability constraints
  SECURITY.md                          # Trust boundaries, auth, data policies
  features/
    _template.md
    <feature>.md
  product-specs/                       # Broader product specs (cross-cutting)
    <spec>.md
  design-docs/                         # Architecture decisions (lightweight ADRs)
    <decision>.md
  references/                          # Pinned external knowledge
    <topic>.md
  exec-plans/
    active/
      <plan>.md
    completed/
      <plan>.md
    tech-debt-tracker.md

# Agent-specific config (create only for the agent in use)
CLAUDE.md                              # Claude Code only
.cursorrules                           # Cursor (legacy)
.cursor/rules/*.mdc                    # Cursor (current)
.github/copilot-instructions.md        # GitHub Copilot
```

Each file has a single, well-defined responsibility.

Agents read `AGENTS.md` as a table of contents, then drill into specific docs as needed.

---

## Repository Contents

### `agent-doc-stack.md`

The authoritative Agent Doc Stack v2.0 specification.

### `initialization-prompt.md`

A reusable prompt that instructs a coding agent to initialize or normalize documentation in a repository using the spec.

---

## How To Use

You should already be inside the repository whose documentation you want to create or fix.

**Typical workflow:**

1. Open the target repository in your coding agent.
2. Run the `initialization-prompt.md`.
3. Allow the agent to fetch and read the Agent Doc Stack specification directly from GitHub.
4. Answer clarifying questions if the agent requests missing information.

The agent will:

- Treat the current folder as the repository root
- Follow the Agent Doc Stack rules exactly
- Generate, update, or clean documentation without inventing behavior
- Create quality, reliability, and security docs as appropriate
- Set up `docs/exec-plans/` for planning as code
- Ask questions when information is unclear or unavailable

> You do not need to copy this repository into your project.

---

## The Specification

The canonical framework lives in:

```
agent-doc-stack.md
```

All structure, rules, and behavior are defined there.

This file is the single source of truth.

---

## Why This Exists

As coding agents take on more responsibility, documentation must be:

- **Agent-legible** — if the agent can't read it in-repo, it doesn't exist
- **Progressively disclosed** — short entrypoints point to deep docs
- **Enforceable** — contracts, not prose
- **Low-noise** — minimum tokens, maximum signal
- **Safe from hallucination and drift** — predictable locations, strict update rules

Agent Doc Stack treats documentation as a system contract designed for agents as the primary consumer.

---

## Versioning

Agent Doc Stack follows semantic versioning.

- The current version is **v2.0**, defined in `agent-doc-stack.md`
- Patch releases clarify wording without changing behavior
- Minor releases may add rules while remaining backward compatible
- Major releases may introduce breaking changes

Repositories that require strict stability should pin behavior to a specific version.
