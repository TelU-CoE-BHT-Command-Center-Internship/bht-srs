# BHT-SRS — Agent Instructions

This repo is **documentation-only**. It contains SRS templates and guides for the BHT-Nexus project. No code, build, test, lint, or CI exists here.

## Files — reference hierarchy

All documents are **Markdown only**; three are read-only references, one is the deliverable.

| File | Role | How to use it |
|---|---|---|
| `bht-nexus-srs.md` | **Deliverable** — the SRS to populate | Write all content here. Replace `{{placeholder}}` tokens (e.g., `{{project name}}`, `{{author}}`). Never edit the other three files. |
| `srs-guide.md` | Section-level guide | Explains what to write in each section and why (`💬` context, `➥` action, `💡` tips). Reference when drafting any section of `bht-nexus-srs.md`. |
| `prompt-srs-guide.md` | Requirement-level template | Use this format (Statement → Rationale → Acceptance Criteria → Verification Method) for every individual requirement entry in §3. |
| `2026-07-12_bht_nexus_team_building_guide.md` | Technical direction (authoritative) | Source of truth for product scope, user roles, tech stack, infrastructure, data model, constraints, and team conventions. Cross-reference every time you make a substantive decision in the SRS. |

## Workflow

- **Only `bht-nexus-srs.md` gets content written into it.** The other three are reference-only.
- No commands to run. No build, lint, typecheck, or test steps.
- When drafting a requirement in §3, use `prompt-srs-guide.md` format per entry.
- When deciding scope/constraints/roles/stack, consult `2026-07-12_bht_nexus_team_building_guide.md` first.
- The repo is **not a git repository** — no commits here. Implementation code lives at [TelU-CoE-BHT-Command-Center-Internship](https://github.com/TelU-CoE-BHT-Command-Center-Internship).
