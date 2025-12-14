# Agent Doc Stack

Agent Doc Stack is a lightweight, agent-first documentation architecture that defines where truth lives and how coding agents generate, update, and enforce docs with minimal tokens and zero drift.

This repository contains the **Agent Doc Stack v1.0 specification** and a single initialization prompt used to apply the framework to any codebase.

---

## What This Is

Agent Doc Stack is a **contract-based documentation specification** for AI-assisted software development.

It defines:
- Canonical documentation locations
- Strict rules for how agents read, write, and update docs
- Writing standards optimized for token efficiency
- Enforcement rules to prevent documentation drift

This is a **specification**, not a template or a tool.

It works across modern coding agents including Claude Code, Codex CLI, Cursor, and Copilot Agent.

---

## Repository Contents

- **agent-doc-stack.md**  
  The authoritative Agent Doc Stack v1.0 specification.

- **initialization-prompt.md**  
  A reusable prompt that instructs a coding agent to initialize or normalize documentation in a repository using the spec.

---

## How To Use

You should already be inside the repository whose documentation you want to create or fix.

Typical workflow:

1. Open the target repository in your coding agent.
2. Run the `initialization-prompt.md`.
3. Allow the agent to fetch and read the Agent Doc Stack specification directly from GitHub.
4. Answer clarifying questions if the agent requests missing information.

The agent will:
- Treat the current folder as the repository root
- Follow the Agent Doc Stack rules exactly
- Generate, update, or clean documentation without inventing behavior
- Ask questions when information is unclear or unavailable

You do **not** need to copy this repository into your project.

---

## The Specification

The canonical framework lives in:

agent-doc-stack.md

All behavior, structure, and rules are defined there.  
This file is the single source of truth.

---

## Why This Exists

As coding agents take on more responsibility, documentation must be:

- Predictable
- Enforceable
- Low-noise
- Safe from hallucination and drift

Agent Doc Stack treats documentation as a **system contract**, not prose.

---

## Versioning

Agent Doc Stack follows semantic versioning.

- The current version is defined in `agent-doc-stack.md`
- Patch releases clarify wording without changing behavior
- Minor releases may add rules while remaining backward compatible
- Major releases may introduce breaking changes

Repositories that require strict stability should pin behavior to a specific version.
