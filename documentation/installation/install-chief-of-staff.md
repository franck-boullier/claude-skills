# Installing the Chief of Staff Skill

**Skill path:** `c-level-advisor/chief-of-staff/`  
**Primary file:** `chief-of-staff/SKILL.md`  
**Purpose:** This note explains how to install the **Chief of Staff** orchestration skill in another workspace or machine, what to install beside it, and which paths and conventions the workflow expects.

**Upstream source (examples):** `alirezarezvani/claude-skills` on GitHub. If you use a fork or a local clone, substitute your repo URL or paths in the commands below.

---

## What you are installing

Chief of Staff is a **router / orchestration** skill. It ships as:

- `SKILL.md` — session protocol, complexity scoring, `[INVOKE:role|question]` rules, synthesis format, decision-log template
- `references/routing-matrix.md` — detailed routing by domain
- `references/synthesis-framework.md` — full synthesis and conflict handling

It does **not** include Python scripts in its own folder. Optional Python tools live under **other** `c-level-advisor/*` skills (for example CEO/CTO advisor scripts), not under `chief-of-staff/`.

---

## Recommended installation strategies

### Option A — Full C-level bundle (recommended)

Installs every skill Chief of Staff can route to (28 skills in the ecosystem), so routing and cross-references stay aligned with the authors’ intent.

**Claude Code (plugin marketplace):**

```text
/plugin marketplace add alirezarezvani/claude-skills
/plugin install c-level-skills@claude-code-skills
```

**Universal CLI (many agents, including Cursor):**

```bash
npx agent-skills-cli add alirezarezvani/claude-skills
```

**Cursor only:**

```bash
npx agent-skills-cli add alirezarezvani/claude-skills --agent cursor
```

**Project-local (portable repo):**

```bash
npx agent-skills-cli add alirezarezvani/claude-skills --agent project
```

Preview before writing files:

```bash
npx agent-skills-cli add alirezarezvani/claude-skills --dry-run
```

---

### Option B — Entire `c-level-advisor` directory

Keeps the **sibling folder layout** used by cross-references in the repo (for example Chief of Staff points at `agent-protocol/SKILL.md` relative to the `c-level-advisor` tree).

**CLI (pattern matches other per-folder installs in the repo):**

```bash
npx agent-skills-cli add alirezarezvani/claude-skills/c-level-advisor
```

**Manual copy (after cloning the repo):**

- **Claude Code:** copy `c-level-advisor` under `~/.claude/skills/` (or merge into your skills root per your installer’s layout).
- **Cursor (project):** copy under `.cursor/skills/` as your tooling expects (often one folder per skill or a nested tree—match what `agent-skills-cli` produces on your machine).

---

### Option C — Chief of Staff only (minimal)

Installs the orchestration skill and its `references/` only. You should add the **companion** skills listed in [Dependency tiers](#dependency-tiers) or several relative paths and session steps will be incomplete.

```bash
npx agent-skills-cli add alirezarezvani/claude-skills/c-level-advisor/chief-of-staff
```

If you copy by hand, copy the whole `chief-of-staff` directory, not only `SKILL.md`.

---

## Dependency tiers

Use this as a checklist when you are not using Option A.

### Tier 1 — Shipped with Chief of Staff (always copy together)

| Artifact | Role |
|----------|------|
| `chief-of-staff/SKILL.md` | Main behavior |
| `chief-of-staff/references/routing-matrix.md` | Routing rules |
| `chief-of-staff/references/synthesis-framework.md` | Synthesis and conflicts |

### Tier 2 — Referenced directly by Chief of Staff

| Skill / artifact | Why |
|------------------|-----|
| `agent-protocol` | Quality Standards cite `agent-protocol/SKILL.md`; defines invocation and loop rules shared across C-level agents |
| `context-engine` | Session protocol: load company context first (`~/.claude/company-context.md`, staleness, privacy notes) |

### Tier 3 — Slash-style workflows (conventions, not npm packages)

These are **documented** in the skills; the model follows them when you type the command text:

| User text | Skill | Purpose |
|-----------|-------|---------|
| `/cs:setup` | `cs-onboard` | Founder interview → `company-context.md` |
| `/cs:board` | `board-meeting` | Structured multi-role deliberation |

### Tier 4 — Full ecosystem (optional per question)

Chief of Staff lists **28 skills** total (10 C-suite roles, orchestration helpers, cross-cutting, culture/collaboration). Routing only matches what is actually installed and available to the agent. See `chief-of-staff/SKILL.md` section **Ecosystem Awareness** and `references/routing-matrix.md` for triggers.

### Tier 5 — On-disk files (not installed by the skill itself)

| Path (as documented upstream) | Purpose |
|-------------------------------|---------|
| `~/.claude/company-context.md` | Loaded by **context-engine**; created/updated via onboarding (`cs-onboard`) |
| `~/.claude/decision-log.md` | Append decisions per Chief of Staff template |

On Windows, your shell or agent may resolve `~` to your user profile; create equivalent paths if your environment does not expand `~` the same way as Unix docs.

---

## Python and other tools

- **Chief of Staff:** no Python dependency.
- **Some other** `c-level-advisor/*` skills include `scripts/*.py` (often stdlib-only). Install Python 3 if you plan to run those scripts; see repo `INSTALLATION.md` for examples.

---

## Verification checklist

After installation:

1. Confirm `chief-of-staff/SKILL.md` and both files under `chief-of-staff/references/` are present.
2. If you did not install Option A, confirm **agent-protocol** and **context-engine** are present if you want full parity with the documented session protocol.
3. Optionally create or generate `company-context.md` (via `/cs:setup` flow with **cs-onboard** installed).
4. Start a chat and ask a routed question, or invoke a role using the patterns in `agent-protocol` / `chief-of-staff`.

---

## Related documentation in this repository

| Document | Contents |
|----------|----------|
| `INSTALLATION.md` (repo root) | Universal installer, Claude Code plugins, manual copy paths, troubleshooting |
| `c-level-advisor/SKILL.md` | C-level quick start, `/cs:setup`, `/cs:board`, ecosystem overview |
| `c-level-advisor/chief-of-staff/SKILL.md` | Authoritative Chief of Staff behavior and ecosystem list |

---

**Last updated:** 2026-04-20
