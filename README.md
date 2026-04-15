# Agent Doc Stack

A contract-based, agent-first documentation specification for AI-assisted software development.

**Humans steer. Agents execute.**

---

## Why harness engineering needs a documentation layer

As coding agents take on more of the software lifecycle, the job of engineering shifts from writing code to designing the environment the agent operates in. That environment — the *harness* — is the set of systems that shape how the agent thinks and acts: the context it loads, the tools it can call, the rules it must follow, and the memory it carries across sessions.

A harness is only as good as its weakest layer. You can hand an agent the strongest model, the best tools, and a clean sandbox — and still get poor results if the agent can't answer, from inside the repository, three questions before it starts work:

1. **What is this system and what does it do?**
2. **Where do I look when I need to change X?**
3. **What are the rules I must not break?**

Every question the agent can't answer in-repo becomes a prompt the human has to write, a mistake the reviewer has to catch, or a hallucination that ships.

Agent Doc Stack is the **context layer of the harness** — the structured, repo-local documentation that lets an agent answer those three questions without guessing, without external lookups, and without re-ingesting a wall of prose on every task.

---

## The design principles

Agent Doc Stack is built on four ideas that have converged across teams running coding agents at scale:

- **Repo-local or it doesn't exist.** Slack threads, Confluence pages, and tribal knowledge are invisible to agents. If it isn't committed, it isn't real.
- **Progressive disclosure over information dumping.** Agents read a short entry point (`AGENTS.md`) and *navigate* to deeper docs only when the task requires them. Context windows are finite; spending them on irrelevant rules degrades output.
- **Define once, link everywhere.** Every fact lives in exactly one canonical location. Duplicated facts drift, and drift silently poisons agent behavior.
- **Contracts, not prose.** Docs are structured so an agent can act on them — bullets, templates, exact commands, canonical file references. Explanations belong in commit messages and PRs, not in durable docs.

These are the same principles that emerge in public write-ups from teams shipping agent-generated production code. Agent Doc Stack codifies them into a specification so any team can adopt them without reinventing the structure.

---

## How it works

Agent Doc Stack is a specification for **where docs live**, **what each doc contains**, and **which doc the agent reads when**. It is not a tool, not a template, and not a library.

The mental model is a two-layer system:

**Layer 1 — the entry point.** A single file, `AGENTS.md` (~120 lines), is the only forced read for the agent. It contains a quick-verify command, repo purpose, architecture boundaries, invariants, a doc map, and planning/review rules. Nothing else. It is a table of contents, not a manual.

**Layer 2 — the deep docs.** Everything else lives in predictable, canonical locations under `docs/`: feature specs, product specs, design decisions, execution plans, quality and reliability constraints, and pinned external references. Each has a size target, a required structure, and a clear purpose. The agent pulls from these only when the task demands it.

The result is that a coding agent — Claude Code, Codex CLI, Cursor, Gemini CLI, Copilot Agent, or any future successor — can walk into the repository, read ~120 lines, and know exactly where to go for anything it needs. When it makes a change, it knows exactly which doc to update.

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
