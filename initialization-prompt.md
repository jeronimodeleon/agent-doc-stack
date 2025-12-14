# Agent Doc Stack – Initialization Prompt

You are a coding agent operating inside a repository.

Treat the **current working directory as the repository root**.

Before doing anything else, fetch and read the **Agent Doc Stack specification** from the canonical source:

https://raw.githubusercontent.com/jeronimodeleon/agent-doc-stack/main/agent-doc-stack.md

This specification is authoritative. You MUST follow it strictly.

---

## Your Task

Initialize, normalize, or correct the documentation in this repository so it fully complies with the Agent Doc Stack specification.

Your objectives:

- Place durable truth in the correct canonical files
- Remove ambiguity, duplication, and drift
- Minimize tokens while maximizing correctness
- Ensure future agents can safely operate in this repo

---

## Critical Safety Rules

- DO NOT invent product behavior, features, architecture, workflows, or requirements
- DO NOT guess when information is missing or unclear
- If required information is unavailable:
  - STOP
  - Ask clear, targeted questions
  - Wait for answers before continuing

Leaving sections empty is preferred over guessing.

---

## First Step: Read Existing Documentation

Before creating or modifying any files, you MUST:

1. Check if `README.md` exists in the repository root
2. If it exists, read it completely
3. Check for any other existing `.md` files in the repository
4. Inventory what documentation already exists

This ensures you preserve existing product information and avoid overwriting valuable content.

---

## Handling Existing README.md

If a `README.md` already exists:

- Extract and preserve:
  - Product overview and purpose
  - Feature descriptions
  - Tech stack information
  - Setup instructions
  - Environment variables
  - Run and test commands
  - Any other durable product truth

- Restructure the content to match Agent Doc Stack standards:
  - Move architecture details to `ARCHITECTURE.md`
  - Move feature specifics to `docs/features/<feature>.md`
  - Move workflow details to `docs/app-workflows.md` or `docs/dev-workflows.md`
  - Keep only README-appropriate content in `README.md`

- DO NOT discard information—relocate it to the correct canonical location

- If the existing README contains information not covered by the spec, ask where it should live

---

## Handling Other Existing Documentation

If the repository already contains other documentation:

- Identify which existing `.md` files map to canonical Agent Doc Stack docs
- Merge durable, correct information into the proper locations
- Flag or remove:
  - duplicate docs
  - outdated docs
  - speculative or unclear docs

If deletion or consolidation is ambiguous or destructive:

- Propose a cleanup plan
- Ask for confirmation before proceeding

Do NOT preserve documentation solely for historical reasons.

---

## Mandatory Rules

You MUST:

- Follow the required documentation reading order defined in the specification
- Use the exact canonical file locations
- Avoid creating new documentation files unless explicitly allowed
- Use bullets over paragraphs
- Avoid rewording unchanged content
- Avoid rationale unless required for correctness

Documentation is a **contract**, not an explanation.

---

## Agent-Specific Config File

Only create the agent-specific config file for the agent you are:

| Agent | Config File |
|-------|-------------|
| Claude Code | `CLAUDE.md` |
| Codex CLI | Uses `AGENTS.md` directly (no separate file) |
| Cursor | `.cursorrules` or `.cursor/rules/*.mdc` |
| GitHub Copilot | `.github/copilot-instructions.md` |

Do NOT create config files for agents other than yourself.

---

## Planning Requirement

If the work involves:

- multiple documentation files
- consolidation or deletion
- non-trivial assumptions
- uncertainty about product behavior

Then you MUST:

1. Write a plan to `plans/YYYY-MM-DD-initialization.md`
2. Include:
   - Existing files found and their disposition (keep, merge, relocate, delete)
   - Files to be created, updated, merged, or removed
   - Open questions requiring user input
   - A short execution checklist
3. Do NOT apply changes until:
   - the plan is complete
   - blocking questions are resolved

Plans are temporary and MUST NOT become sources of truth.

---

## Completion Criteria

You are finished when:

- All required documentation exists in the correct locations
- Existing documentation has been preserved and relocated appropriately
- Each document follows the Agent Doc Stack writing standards
- Cross-links between documents are correct
- No speculative, duplicate, or orphan documentation remains
- All assumptions are either validated or explicitly documented
