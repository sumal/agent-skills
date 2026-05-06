# Kerala Cabinet Advisor — Skill Suite

A bundle of 22 Agent Skills that turn any agentic IDE or LLM coding assistant into a non-partisan, execution-focused policy aide for the **Kerala Council of Ministers**. One master router skill (`kerala-cabinet-advisor`) automatically dispatches to the correct portfolio-specific skill based on the Minister, department, scheme, or keyword you mention.

> **Scope:** rapid decision briefs, policy-impact analysis, crisis SOPs, and Cabinet-grade execution plans grounded in the Kerala Government Rules of Business 1972, the 73rd/74th Amendments, the "Kerala Model" of democratic decentralization, and current Indian/Kerala statutes.

> **Portable by design.** Each skill is plain Markdown with YAML frontmatter — no IDE-specific syntax, no proprietary runtime. Works with Cursor, Claude Code, Codex, Continue, Cline, Aider, OpenAI/Anthropic SDKs, or any agent that can load Markdown context files.

---

## Table of Contents
1. [What's in the Suite](#whats-in-the-suite)
2. [File Anatomy](#file-anatomy)
3. [Setup — Any Agentic IDE](#setup--any-agentic-ide)
4. [How the Skills Get Loaded](#how-the-skills-get-loaded)
5. [Usage](#usage)
6. [Response Format](#response-format)
7. [Example Prompts](#example-prompts)
8. [Customization](#customization)
9. [Troubleshooting](#troubleshooting)
10. [Disclaimer](#disclaimer)

---

## What's in the Suite

### Master Router
| Skill | Role |
|---|---|
| `kerala-cabinet-advisor` | Master skill. Routes to the right portfolio skill, or applies the universal 4-step Rapid Decision Matrix if no match. |

### Portfolio Skills (21)
| Skill | Portfolio Coverage |
|---|---|
| `kerala-minister-cm` | Chief Minister — General Admin, Home, Vigilance, AIS, NRI, IT, IPRD |
| `kerala-minister-finance` | Budget, FRBM, KIIFB, SGST, GST Council, Treasury |
| `kerala-minister-law-industries` | Law Dept, KSIDC/KINFRA/MSME, Startup Mission, Coir |
| `kerala-minister-revenue-housing` | Land Revenue, Survey, Resurvey, KSDMA, LIFE Mission |
| `kerala-minister-pwd-tourism` | PWD, KRFB, NH coordination, KTDC, Responsible Tourism |
| `kerala-minister-water-resources` | Irrigation, KWA, GWD, dam safety, JJM, inter-state rivers |
| `kerala-minister-lsgd-excise` | Panchayats/ULBs, Kudumbashree, KILA, Suchitwa, Excise |
| `kerala-minister-general-education-labour` | School Ed, KITE, SCERT, Labour, ESI, KASE, migrants |
| `kerala-minister-higher-education-social-justice` | Universities, KSHEC, PwD, transgender, Nirbhaya |
| `kerala-minister-health-wcd` | DHS/DME, KMSCL, KASP, NHM, Aardram, ICDS, Sakhi |
| `kerala-minister-agriculture` | VFPCK, KAU, paddy, MSP, FPOs, Karshaka Pension |
| `kerala-minister-animal-husbandry-dairy` | AH Dept, Milma/KCMMF, KLDB, KVASU, zoonoses |
| `kerala-minister-forests-wildlife` | Forest Dept, KFRI, eco-tourism, HWC, FRA 2006 |
| `kerala-minister-transport` | KSRTC, MVD, KMRL, KRCL, EV mobility, road-safety |
| `kerala-minister-electricity` | KSEBL, ANERT, KSERC, hydro reservoirs, RE, EV charging |
| `kerala-minister-food-civil-supplies` | PDS/NFSA, Supplyco, Maveli, Legal Metrology |
| `kerala-minister-scstbc-devaswom` | SC/ST/BC Welfare, Devaswom Boards, Sabarimala, Parl Affairs |
| `kerala-minister-fisheries-culture` | Fisheries, Matsyafed, Harbour Engg, Cashew, Akademis |
| `kerala-minister-ports-museums` | Vizhinjam, minor ports, Maritime Board, Museums, Archives |
| `kerala-minister-cooperation-registration` | PACS, urban banks, Co-op Audit, Registration/IGR |
| `kerala-minister-sports-youth-minority` | Sports Council, Youth Welfare, Wakf, Minority Welfare |

---

## File Anatomy

Each skill is a single Markdown file:

```
<skill-name>/
└── SKILL.md
```

`SKILL.md` has two parts:

```markdown
---
name: kerala-minister-health-wcd
description: Rapid Decision-Making advisor for Kerala's Minister of Health…
              Use when the user asks about outbreaks, KASP, Anganwadis, etc.
---

# Body — full instructions, decision matrix, SOPs, response format
```

- **Frontmatter** (`name` + `description`): the trigger metadata. Agents use the `description` to decide *when* to load the skill.
- **Body**: the playbook the agent follows once the skill is loaded.

No code, no executables, no platform calls — just text. This is what makes the suite portable.

---

## Setup — Any Agentic IDE

Skills are stored in this repo at:

```
.cursor/skills/
├── kerala-cabinet-advisor/SKILL.md
├── kerala-minister-cm/SKILL.md
├── kerala-minister-finance/SKILL.md
└── … 19 more …
```

Pick the integration path that matches your tool:

### A. Cursor
Place the `kerala-*` directories under one of:
- `<workspace>/.cursor/skills/` (project-scoped)
- `~/.cursor/skills/` (user-scoped, available in every workspace)

Reload the window. Done.

### B. Claude Code / Anthropic Skills
Copy the `kerala-*` directories under:
- `<workspace>/.claude/skills/` (project-scoped)
- `~/.claude/skills/` (user-scoped)

Claude Code will auto-discover them on next run.

### C. Continue, Cline, Aider, Roo, or other agentic IDEs
Most agentic IDEs support either:
1. **Custom rules / system prompts** — concatenate the relevant SKILL.md body into the IDE's rules file (e.g. `.continuerules`, `.clinerules`, `AGENTS.md`).
2. **File-based context** — point the agent at the `kerala-*/SKILL.md` files via the IDE's "include files" / "context" config.

Recommended pattern: include only the master `kerala-cabinet-advisor/SKILL.md` plus the portfolio table. Let the user paste the specific SKILL.md when they need a deep brief.

### D. GitHub Copilot Chat / Workspaces
Add the SKILL.md files to `.github/copilot-instructions.md` (concatenated) or reference them as workspace context.

### E. Generic LLM SDK (OpenAI, Anthropic, Gemini, etc.)
Programmatic use:

```python
from pathlib import Path

# Load master + portfolio skill as system context
master = Path(".cursor/skills/kerala-cabinet-advisor/SKILL.md").read_text()
health = Path(".cursor/skills/kerala-minister-health-wcd/SKILL.md").read_text()

system_prompt = f"{master}\n\n---\n\n{health}"

# Pass system_prompt to your LLM call (OpenAI/Anthropic/Gemini/etc.)
```

For dynamic routing, parse the `description:` line from each frontmatter and use embeddings or a simple keyword match to pick which skill bodies to inject per user query.

### F. Plain Chat (ChatGPT, Claude.ai, Gemini, Mistral)
Open the `SKILL.md` you need, paste it as the first message, then ask your policy question. The router skill (`kerala-cabinet-advisor/SKILL.md`) is enough for most cases since it links out to portfolio skills.

### Verify Installation
Ask the agent:
> "List the Kerala minister skills available."

If it returns the 22-skill table, setup is correct.

### Optional Folder Rename
The folder name `.cursor/skills/` is just a convention from this repo. Rename to `skills/`, `agents/`, `prompts/`, or whatever your tool expects — the skills themselves don't depend on the path.

---

## How the Skills Get Loaded

Most agentic tools use **lazy loading**: only the YAML `description` field stays in the agent's context at all times. The full SKILL.md body is loaded only when your prompt matches.

Routing happens in 2 stages:

1. **Auto-trigger** — the master `kerala-cabinet-advisor` skill is selected when your prompt mentions any of: "Cabinet", "Kerala Minister", "policy brief", "crisis SOP", or specific portfolio names (Health, Finance, LSGD, Tourism, etc.).
2. **Portfolio routing** — the master skill's portfolio table directs the agent to load the matching `kerala-minister-*` skill (e.g. mentioning "Nipah", "KASP", or "Anganwadi" → loads `kerala-minister-health-wcd`).

If no portfolio matches, the universal 4-step Rapid Decision Matrix in the master skill is applied.

For tools without lazy loading (plain chat, simple system-prompt setups), inject the master + the relevant portfolio file together.

---

## Usage

### Quick Start
Just ask in plain English. The agent picks the right skill.

```text
Advise me as Health Minister Kerala on a Nipah outbreak.
```
→ Loads `kerala-minister-health-wcd` → returns a 72-hour containment + comms plan.

```text
Should we revise KSRTC fares this quarter?
```
→ Loads `kerala-minister-transport` → returns options + execution plan.

### Explicit Skill Invocation
You can name a skill directly:

```text
Use the kerala-minister-finance skill. Draft a Cabinet note on KIIFB borrowing for Vizhinjam Phase 2.
```

### Crisis Mode
Mention any urgency keyword (`outbreak`, `flood`, `landslide`, `hartal`, `fire`, `court order`, `fund freeze`, etc.) and the agent auto-activates the crisis SOP pathway with first-24/72-hour actions, escalation paths, and comms strategy.

### Multi-Portfolio Files
For files spanning multiple ministries (e.g. land + housing + disaster):
> "Treat this as a multi-portfolio file: Revenue + LSGD + Disaster Management. Give me a coordinated brief."

The agent will load all relevant skills and harmonize the response.

---

## Response Format

Every decision request returns this **exact** structure (enforced by the skills):

```
Decision Summary
Why This Works

Options Considered
- Option A – pros/cons
- Option B – pros/cons

Risks
- Risk 1
- Risk 2

Execution Plan
- Step 1
- Step 2
- Step 3

Data Needed (if any)
- Missing inputs
```

Crisis briefs additionally lead with **first-24h** and **first-72h** action blocks before the standard sections.

---

## Example Prompts

| Goal | Example Prompt | Skill Loaded |
|---|---|---|
| Outbreak SOP | "Nipah confirmed in Kozhikode — first-72h plan." | `kerala-minister-health-wcd` |
| Fiscal call | "Should we issue a supplementary demand for ₹2,000 cr KIIFB top-up?" | `kerala-minister-finance` |
| Disaster | "Heavy landslide in Wayanad — Cabinet brief in 1 hour." | `kerala-minister-revenue-housing` |
| Industrial dispute | "KSEB engineers threaten ESMA-defying strike during monsoon." | `kerala-minister-electricity` |
| Tourism crisis | "Houseboat fire in Alappuzha — what's the Tourism + Ports response?" | `kerala-minister-pwd-tourism` + `kerala-minister-ports-museums` |
| LSGI policy | "Plan-fund release stuck at 38% — 6-week recovery plan." | `kerala-minister-lsgd-excise` |
| Inter-state water | "Tamil Nadu seeks Mullaperiyar shutter raising — our line?" | `kerala-minister-water-resources` |
| Fisheries | "Trawl ban dates for monsoon 2026 — fisher-union pushback expected." | `kerala-minister-fisheries-culture` |
| Education | "Hi-Tech classroom Phase-3 procurement — risks and rollout." | `kerala-minister-general-education-labour` |
| Co-op crisis | "PACS deposit run rumour spreading — 24h SOP." | `kerala-minister-cooperation-registration` |

---

## Customization

### Editing a Skill
- Each skill is a single `SKILL.md` file. Edit it like any markdown file.
- Keep the YAML frontmatter (`name`, `description`) intact — the description controls auto-loading.
- After edits, restart or reload your agent so it picks up changes.

### Adding a New Sub-Skill
1. Create `<skills-dir>/<your-skill-name>/SKILL.md`.
2. Frontmatter must include `name` and a `description` rich in trigger keywords (think: "use when user asks about X, Y, Z").
3. Add a row to the portfolio router in `kerala-cabinet-advisor/SKILL.md`.

### Changing the Response Format
The mandatory format lives in `kerala-cabinet-advisor/SKILL.md` under the **MANDATORY Response Format** section. Edit there to propagate to all skills.

### Tightening / Loosening Behavior
Edit the **Decision Principles (STRICT)** and **Behavior Rules** sections in any skill. Examples:
- Add: "Always include a 30-day post-decision review trigger."
- Remove: "No political bias." (not recommended).

---

## Troubleshooting

| Symptom | Fix |
|---|---|
| Agent gives generic advice instead of Kerala-specific | Reload the agent; verify SKILL.md frontmatter syntax; re-prompt with "Use the kerala-cabinet-advisor skill." |
| Wrong portfolio skill loaded | Be more explicit: "Use kerala-minister-finance for this." |
| Response not in mandatory format | Append: "Reply strictly in the Cabinet Advisor mandatory format." |
| Skills not detected at all | Confirm the path your tool expects (`.cursor/skills/`, `.claude/skills/`, `AGENTS.md`, etc.); check YAML is valid (no tabs, correct quotes). |
| Outdated statute / scheme reference | Edit the relevant SKILL.md — these files are intentionally easy to maintain. |
| Tool has no skill-loading concept | Paste the relevant SKILL.md as a system prompt or first message. |

---

## Disclaimer

- These skills are an **advisory aid**, not a substitute for the Kerala Government Rules of Business, Law Department opinions, or Finance Department concurrence.
- Statutory citations are correct as of the SKILL.md authoring date. Verify currency before any official action.
- Non-partisan by design: skills do not endorse any political party or coalition.
- All scheme names, board acronyms, and statutes are public information.
