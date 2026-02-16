# Agent Doc Stack

A contract-based, agent-first documentation specification for AI-assisted software development.

**Humans steer. Agents execute.**

---

## What This Is

Agent Doc Stack defines:

- Canonical doc locations with progressive disclosure
- A ~100-line agent table of contents that points to deep docs
- Planning as code with versioned execution plans
- Writing standards optimized for token efficiency
- Enforcement rules to prevent documentation drift

This is a **specification**, not a template or a tool.

Works with Claude Code, Codex CLI, Cursor, and Copilot Agent.

---

## Repository Contents

| File | Purpose |
|------|---------|
| `agent-doc-stack.md` | The authoritative v2.0 specification — single source of truth for all structure, rules, and canonical file locations |
| `initialization-prompt.md` | Reusable prompt to initialize or normalize a repo's documentation using the spec |

---

## How To Use

1. Open the target repository in your coding agent
2. Run `initialization-prompt.md`
3. The agent fetches and reads `agent-doc-stack.md` directly from GitHub
4. Answer clarifying questions if the agent requests missing information

The agent will follow the spec to generate, update, or clean documentation — without inventing behavior.

> You do not need to copy this repository into your project.

---

## Why This Exists

As coding agents take on more responsibility, documentation must be:

- **Agent-legible** — if the agent can't read it in-repo, it doesn't exist
- **DRY** — define once, link everywhere; no duplicated facts
- **Enforceable** — contracts, not prose
- **Low-noise** — minimum tokens, maximum signal

---

## Versioning

- Current version: **v2.0** (defined in `agent-doc-stack.md`)
- Patch: clarify wording, no behavior change
- Minor: add rules, backward compatible
- Major: breaking changes

Pin to a specific version for strict stability.
