# BHT-SRS — Agent Instructions

This repo is **documentation-only**. It contains SRS templates and guides for the BHT-Nexus project. No code, build, test, lint, or CI exists here.

## Files — reference hierarchy

All documents are **Markdown only**; four are read-only references, one is the deliverable.

| File | Role | How to use it |
|---|---|---|
| `bht-nexus-srs.md` | **Deliverable** — the SRS to populate | Write all content here. Replace `{{placeholder}}` tokens (e.g., `{{project name}}`, `{{author}}`). Never edit the other four files. |
| `srs-guide.md` | Section-level guide | Explains what to write in each section and why (`💬` context, `➥` action, `💡` tips). Reference when drafting any section of `bht-nexus-srs.md`. |
| `prompt-srs-guide.md` | Requirement-level template | Use this format (Statement → Rationale → Acceptance Criteria → Verification Method) for every individual requirement entry in §3. |
| `2026-07-12_bht_nexus_team_building_guide.md` | Technical direction (authoritative) | Source of truth for product scope, user roles, tech stack, infrastructure, data model, constraints, and team conventions. Cross-reference every time you make a substantive decision in the SRS. |
| `indonesian-format.md` | Writing standard (normative) | EYD Edisi V guidelines for Indonesian spelling, punctuation, word formation, and loanword adaptation. All content written into `bht-nexus-srs.md` must follow these rules. |

## Workflow

- **Only `bht-nexus-srs.md` gets content written into it.** The other four are reference-only.
- No commands to run. No build, lint, typecheck, or test steps.
- When drafting a requirement in §3, use `prompt-srs-guide.md` format per entry.
- When deciding scope/constraints/roles/stack, consult `2026-07-12_bht_nexus_team_building_guide.md` first.
- All SRS content must conform to EYD Edisi V (Indonesian spelling standard). Consult `indonesian-format.md` for punctuation, capitalization, word breaks, loanword spelling, and number formatting.
- The repo is **not a git repository** — no commits here. Implementation code lives at [TelU-CoE-BHT-Command-Center-Internship](https://github.com/TelU-CoE-BHT-Command-Center-Internship).
