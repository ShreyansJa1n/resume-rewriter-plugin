---
name: resume-setup
description: One-time onboarding for the resume rewriter. Imports an existing resume/LinkedIn, interviews the user to fill gaps, and builds their personal data dir (profile.yaml, experience-bank.md, project-bank.md, config.yaml). Use when the user runs /resume-rewriter:setup or asks to set up / initialize / onboard the resume tool.
---

# Resume Rewriter — Setup

Your job: build the user's **personal data directory** so `/resume-rewriter:tailor` can generate
tailored resumes later. You do this by importing whatever they already have, then interviewing to
fill gaps. Do NOT invent facts — everything in the banks must trace to something the user told you
or a document they gave you.

## Data directory

Resolve the data dir once and use it throughout:
- Default: `~/.resume-rewriter/` (expand `$HOME`). Create it if missing.
- If the user names a different location, use that and tell them to pass `--data-dir` mentally
  (record their choice at the top of `profile.yaml` as a comment).

Files you will create there:
- `profile.yaml` — fixed identity + education
- `experience-bank.md` — tagged canonical + variant bullets per job
- `project-bank.md` — project catalog
- `config.yaml` — style + guardrail settings

Templates to copy and adapt live at `${CLAUDE_PLUGIN_ROOT}/templates/`.

## Steps

### 1. Import
Ask the user for an existing resume or LinkedIn export — a file path (PDF/DOCX/MD/TXT) or pasted
text. If they have nothing, say so is fine and skip to the interview.
- Read the file (use Read for local files; ask them to paste if it's a format you can't open).
- Extract: name, contact links, work authorization note (if any), education, every job (company,
  title, location, dates), projects, and the raw bullet/achievement text under each.
- Reflect back a concise structured summary and ask them to confirm/correct before you write anything.

### 2. Interview to fill gaps
The goal is raw material for strong X-Y-Z bullets ("Accomplished [X] as measured by [Y] by doing
[Z]"). For each job and project, probe for what a resume needs but imports usually lack:
- **Metrics**: latency, uptime, %, users, revenue, time saved, error rate, scale — before/after.
- **Ownership level**: did they own it, co-own, contribute to, or drive it? (This calibrates verbs.)
- **Distinct domains** within one job (e.g. mobile + backend) → these become **pools** (see the
  experience-bank template's Pools section) so they're never mixed on one resume.
- **Which signals each achievement speaks to** (reliability, performance, DX, product, ML, etc.) →
  these become the `### [TAG]` names.
Ask in small batches (a few questions at a time), not one giant wall. Use the AskUserQuestion tool
for crisp either/or choices; use plain prose for open-ended "tell me the numbers" prompts.

### 3. Write the banks
Copy `${CLAUDE_PLUGIN_ROOT}/templates/experience-bank.template.md` and
`project-bank.template.md` into the data dir (dropping the EXAMPLE entries) and populate them:
- One `## COMPANY | Role | Dates` section per job, each with tagged `### [SIGNAL]` bullets: a
  Canonical bullet plus alternate framings where the domain could shift (e.g. an iOS vs backend
  wording of the same achievement).
- Mark any always-include anchor bullet with an inline comment.
- Fill the **Quick Selection Reference** table (JD signal → tags).
- For `project-bank.md`, one `## Project` per project with Repo/Stack/Bullets; projects with no
  public repo get an explicit note (they'll render with empty URLs).
- Keep every bullet within 160-220 non-space chars and free of em dashes.

### 4. Profile + config
- Copy `templates/profile.template.yaml` → `profile.yaml`, fill in name/contact/education. Ask
  whether to include a work-authorization line (`auth`); leave it empty if not.
- Copy `templates/config.template.yaml` → `config.yaml`. Offer the defaults (one-page,
  Garamond 9.5, 665-700 words) and only change what they ask for (font, length, output folder,
  guardrail toggles). Most users should keep defaults.

### 5. Finish
- Create an empty `history.md` in the data dir (the tailor step appends to it).
- Add a note reminding the user this dir holds personal data and should not be committed to a
  public repo.
- Summarize what you created and tell them to run `/resume-rewriter:tailor <job description>`.

## Rules
- Never fabricate metrics, tools, or outcomes. If a bullet needs a number the user can't give,
  write the true qualitative version instead.
- Confirm before overwriting an existing data dir — offer to update in place instead.
