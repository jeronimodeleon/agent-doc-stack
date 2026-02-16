# Agent Doc Stack – Initialization Prompt

You are a coding agent operating inside a repository.

Treat the **current working directory as the repository root**.

Before doing anything else, fetch and read the **Agent Doc Stack specification** from the canonical source:

https://raw.githubusercontent.com/jeronimodeleon/agent-doc-stack/main/agent-doc-stack.md

This specification is authoritative. You MUST follow it strictly.

---

## Your Task

Initialize, normalize, or correct the documentation in this repository so it fully complies with the Agent Doc Stack v2.0 specification.

Your objectives:

- Place durable truth in the correct canonical files
- Remove ambiguity, duplication, and drift
- Minimize tokens while maximizing correctness
- Ensure future agents can safely operate in this repo
- Set up progressive disclosure: `AGENTS.md` as a ~100-line table of contents pointing to deep docs
- Ensure all knowledge agents need is in-repo and agent-legible

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

- DO NOT discard information — relocate it to the correct canonical location

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

## AGENTS.md Setup

`AGENTS.md` is the agent's **table of contents** — not a rules dump.

When creating or updating `AGENTS.md`, ensure it:

- Is ~100 lines total
- Explains the repo purpose (2-3 lines)
- Lists architecture boundaries with a link to `ARCHITECTURE.md`
- States invariants and guardrails (testing, doc updates, PR structure)
- Provides a doc map: one line per doc file with path + purpose
- Points to `docs/exec-plans/active/` for planning
- Describes the review/execution loop
- Contains no prose blocks — every line is actionable or a pointer
- Does not duplicate rules — points to source docs instead

---

## Planning Requirement

If the work involves:

- multiple documentation files
- consolidation or deletion
- non-trivial assumptions
- uncertainty about product behavior

Then you MUST:

1. Write a plan to `docs/exec-plans/active/YYYY-MM-DD-initialization.md`
2. Include:
   - Existing files found and their disposition (keep, merge, relocate, delete)
   - Files to be created, updated, merged, or removed
   - Open questions requiring user input
   - A short execution checklist
3. Do NOT apply changes until:
   - the plan is complete
   - blocking questions are resolved

Plans are versioned artifacts tracked in `docs/exec-plans/`.

---

## New Doc Artifacts

In addition to the core docs (README, ARCHITECTURE, AGENTS, features, workflows), create the following when applicable:

### Quality, Reliability, Security

- `docs/QUALITY_SCORE.md` — encode golden principles (shared utilities, data boundaries, test coverage, architectural layering)
- `docs/RELIABILITY.md` — SLOs, known fragile areas, degradation strategies
- `docs/SECURITY.md` — trust boundaries, auth model, data handling policies

Only create these if you have concrete information to populate them. Empty shells are not useful.

### Product Specs

- `docs/product-specs/<spec>.md` — for cross-cutting product behavior that spans multiple features
- Same writing rules as feature docs

### Design Docs

- `docs/design-docs/<decision>.md` — for any significant architecture decisions discovered during initialization
- Use the lightweight ADR format: Context, Decision, Consequences

### References

- `docs/references/<topic>.md` — pin any external knowledge the repo depends on
- Include source URL as citation

### Tech Debt Tracker

- `docs/exec-plans/tech-debt-tracker.md` — log any tech debt discovered during initialization

---

## Agent Legibility Check

Before marking initialization as complete, verify:

- All knowledge an agent needs to operate is in-repo as versioned markdown
- No critical information lives only in external systems (Slack, Confluence, Google Docs)
- If external dependencies are found, pin summaries into `docs/references/`
- `AGENTS.md` serves as an effective table of contents — a fresh agent can read it and know where to go

---

## Completion Criteria

You are finished when:

- All required documentation exists in the correct locations
- Existing documentation has been preserved and relocated appropriately
- Each document follows the Agent Doc Stack writing standards
- `AGENTS.md` is a ~100-line table of contents, not a manual
- Cross-links between documents are correct
- No speculative, duplicate, or orphan documentation remains
- All assumptions are either validated or explicitly documented
- Agent legibility check passes
