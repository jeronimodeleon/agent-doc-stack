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

## Handling Existing Documentation

If the repository already contains documentation:

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

## Planning Requirement

If the work involves:
- multiple documentation files
- consolidation or deletion
- non-trivial assumptions
- uncertainty about product behavior

Then you MUST:

1. Write a plan to `plans/YYYY-MM-DD-initialization.md`
2. Include:
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
- Each document follows the Agent Doc Stack writing standards
- Cross-links between documents are correct
- No speculative, duplicate, or orphan documentation remains
- All assumptions are either validated or explicitly documented
