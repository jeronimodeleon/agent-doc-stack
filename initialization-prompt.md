# Agent Doc Stack – Initialization Prompt

You are a coding agent operating inside a repository.

Treat the **current working directory as the repository root**.

Before doing anything else, fetch and read the **Agent Doc Stack specification** from the canonical source:

https://raw.githubusercontent.com/jeronimodeleon/agent-doc-stack/main/agent-doc-stack.md

This specification is authoritative. You MUST follow it strictly — all rules for structure, writing standards, doc locations, agent config, planning, and quality live there.

---

## Your Task

Initialize, normalize, or correct the documentation in this repository so it fully complies with the Agent Doc Stack v2.2 specification.

Your objectives:

- Place durable truth in the correct canonical files (see spec section 2 for structure)
- Remove ambiguity, duplication, and drift
- Minimize tokens while maximizing correctness — respect size targets per doc type (see each section's **Size target**)
- Ensure future agents can safely operate in this repo
- Point to canonical exemplar files in code rather than duplicating code in docs
- Don't document what linters, type systems, or frameworks already enforce (see spec section 19)

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

## Step 1: Read Existing Documentation

Before creating or modifying any files, you MUST:

1. Check if `README.md` exists in the repository root
2. If it exists, read it completely
3. Check for any other existing `.md` files in the repository
4. Inventory what documentation already exists

This ensures you preserve existing product information and avoid overwriting valuable content.

---

## Step 2: Handle Existing README.md

If a `README.md` already exists:

- Extract and preserve all durable product truth (overview, features, tech stack, setup, env vars, commands)
- Restructure to match Agent Doc Stack standards:
  - Architecture details → `ARCHITECTURE.md`
  - Feature specifics → `docs/features/<feature>.md`
  - Workflow details → `docs/app-workflows.md` or `docs/dev-workflows.md`
  - Keep only README-appropriate content in `README.md`
- DO NOT discard information — relocate it to the correct canonical location
- If content doesn't map to the spec, ask where it should live

---

## Step 3: Handle Other Existing Documentation

If the repository already contains other documentation:

- Map existing `.md` files to canonical Agent Doc Stack locations
- Merge durable, correct information into the proper locations
- Flag or remove: duplicates, outdated docs, speculative content
- If deletion or consolidation is ambiguous: propose a plan and ask for confirmation

Do NOT preserve documentation solely for historical reasons.

---

## Step 4: Create Missing Documentation

Follow the spec (sections 5-17) for what each doc file must contain.

Key reminders:

- Only create **required** docs always; create **create-on-need** docs only when you have concrete content (see spec section 2 for the distinction)
- Only create the agent-specific config file for the agent you are (see spec section 17)
- `AGENTS.md` must be a ~100-line table of contents, not a manual (see spec section 7)
- If work involves multiple files or non-trivial assumptions, write a plan to `docs/exec-plans/active/` first (see spec section 13)
- Feature docs must include a **Verification** section with exact test commands agents can run (see spec section 8)
- Feature docs and ARCHITECTURE.md must include **Canonical Files** — pointers to exemplar implementations (see spec sections 6, 8)
- Product specs use their own template distinct from feature docs — include Scope, Features Involved, User Personas, Cross-Cutting Behaviors (see spec section 9)
- Respect **size targets** for each doc type — if a doc exceeds its budget, split content into linked sub-docs
- Add `<!-- last_verified: YYYY-MM-DD -->` to the top of each doc for staleness tracking (see spec section 18)

---

## Step 5: Handle Monorepos and Multi-Service Repos

If the repository contains multiple packages, services, or distinct modules:

- Root-level docs cover repo-wide concerns
- Each package/service MAY have its own scoped `AGENTS.md`, `ARCHITECTURE.md`, or agent config file (see spec section 2)
- Subdirectory docs supplement root docs — they don't replace shared rules
- When in doubt about scoping, ask before creating nested docs

---

## Completion Criteria

You are finished when:

- All required documentation exists in the correct locations per the spec
- Existing documentation has been preserved and relocated appropriately
- Each document follows the spec's writing standards and anti-patterns (section 19)
- Each document is within its size target (see each section's **Size target**)
- `AGENTS.md` is a ~100-line table of contents pointing to deep docs
- Feature docs include **Canonical Files** and **Verification** sections with exact commands
- ARCHITECTURE.md includes canonical file references for major patterns
- Product specs use the dedicated template (section 9), not the feature doc format
- `<!-- last_verified: YYYY-MM-DD -->` headers are present on all docs
- Cross-links between documents are correct
- No speculative, duplicate, or orphan documentation remains
- Docs don't restate what linters, type systems, or frameworks already enforce
- A fresh agent session could read `AGENTS.md` and know where to go for any task
