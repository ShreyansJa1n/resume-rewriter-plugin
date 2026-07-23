---
name: resume-tailor
description: Tailor a one-page resume to a specific job description using the user's experience/project banks, then generate the DOCX. Use when the user runs /resume-rewriter:tailor, pastes a job description and asks for a tailored resume, or asks to tailor/customize their resume for a role. Requires that /resume-rewriter:setup has been run.
---

# Resume Rewriter — Tailor

Turn a job description (JD) into a finished, one-page DOCX by selecting the best-fitting content
from the user's banks. Follow the phases in order. Never invent facts — only content already in the
user's banks may appear, reworded to match the JD.

## Inputs & locations
- The JD is in `$ARGUMENTS` (inline text or a file path). If empty, ask for it. If it's a path or
  URL, read/fetch it first.
- Data dir: `~/.resume-rewriter/` (or the location set during setup). Read from it:
  `profile.yaml`, `experience-bank.md`, `project-bank.md`, `config.yaml`, `history.md`.
- Plugin scripts: `${CLAUDE_PLUGIN_ROOT}/scripts/generate_resume.py`.
- If the data dir or banks are missing, tell the user to run `/resume-rewriter:setup` first.
- `--anyhow` flag: if present in `$ARGUMENTS`, run in **anyhow mode** (see bottom).

## Phase 0 — Role overlay
If the JD is a Field/Forward-Deployed/Solutions/Customer Engineer role (or similar customer-facing
technical role), read `${CLAUDE_PLUGIN_ROOT}/skills/role-overlays/fde.md` and apply it throughout.
Add other overlays from that folder if present and relevant.

## Phase 1 — Analyze the JD
Extract and list: role type; must-have skills (explicitly required); nice-to-have skills;
cultural/process signals (e.g. "ships weekly", "feature flags", "collaborative"); seniority
signals (years, scope, ownership). These drive every choice below.

## Phase 2 — Prioritize experience
Order jobs most-relevant-first. For each, pick tagged bullet variants from `experience-bank.md`
using its Quick Selection Reference table; choose the framing whose language mirrors the JD. Prefer
an existing variant over rewriting; if rewriting, keep the metric and change only the framing.
- Honor the bank's **pool** rules — don't mix domain pools within one job unless the JD needs both.
- Always include any bullet the bank marks "always include".
- Aim for ~7-10 experience bullets total across all jobs.
- **Spawn a sub-agent to rank bullets**: hand it the Phase-1 signal list, the candidate tags, and
  `experience-bank.md`; it returns a ranked shortlist with one-line justifications. Use the top
  picks; override only for the Phase-4 checks.

## Phase 3 — Select projects
Pick 2-3 projects from `project-bank.md` by: (1) domain match, (2) covering a JD gap not already in
experience, (3) adding a different technical dimension (don't pick two that overlap heavily).
- Projects with no public repo render with empty `name_url`/`repo_url`.
- **Spawn a sub-agent to rank projects**: hand it the JD signals, the selection criteria, and
  `project-bank.md`; it returns a ranked list noting which JD gap each project covers.

## Phase 4 — Validate the bullet set
Run these checks (respect the `guardrails` toggles in `config.yaml`; `--anyhow` disables all but
length):
1. **Length** — each bullet 160-220 non-space chars. The generator flags SHORT/LONG; expand or
   split as needed.
2. **Seniority cap** — ≤ ~2 solo-ownership verbs (designed/architected/owned end-to-end/built from
   scratch) per role; reframe the rest as co-/contributed/drove.
3. **Formula variety** — vary 1-2 bullets per section (lead with the action, or drop "as measured
   by") so it doesn't read as templated.
4. **Em dashes** — none; replace with comma/colon/period. (Also checked mechanically by the script.)
5. **Domain-gap honesty** — if the JD needs depth the banks genuinely lack, flag it to the user
   rather than stretch a project to fake it.

## Phase 5 — Generate
Write a `data.json` for this run and invoke the generator. Suggested run dir:
`~/.resume-rewriter/runs/<Company>[_<Team>]/data.json`.

`data.json` shape (pull header + education straight from `profile.yaml`):
```json
{
  "company": "Acme", "team": "Platform", "role": "Software Engineer",
  "header": { "name": "...", "phone": "...", "email": "...",
              "linkedin": {"text":"...","url":"..."}, "auth": "..." },
  "education": [ { "school":"...", "degree":"...", "detail":"...", "dates":"...", "coursework":"..." } ],
  "experience": [ { "company":"...", "role":"...", "location":"...", "dates":"...", "bullets":["...","..."] } ],
  "projects":   [ { "name":"...", "name_url":"", "stack":"...", "repo_display":"", "repo_url":"", "bullets":["..."] } ],
  "skills": [ ["Programming","..."], ["Backend & Infra","..."] ]
}
```
Reorder `skills` so the most JD-relevant category is first. Then run:
```
python3 "${CLAUDE_PLUGIN_ROOT}/scripts/generate_resume.py" \
  --data ~/.resume-rewriter/runs/<Company>/data.json \
  --config ~/.resume-rewriter/config.yaml
```
The script prints the word-count estimate + per-bullet validation and saves to the configured
output dir. Fix any SHORT/LONG/EM-DASH flags and re-run.

## Phase 6 — Space check & backfill
Read the printed word-count estimate against `word_target`.
- If **below min**: spawn a sub-agent for a single backfill recommendation — give it the current
  counts, the JD signals, what's already covered, and unused bank items; it returns ONE bullet or
  project (exact text, which role/section, JD justification, word contribution). Add it, re-run.
- If **within/above target**: done. If above max, trim the weakest experience bullet (never an
  always-include anchor) and re-run.

## Finish
- Tell the user the output path.
- Append a one-line entry to `~/.resume-rewriter/history.md`:
  `| <Company> | <Role> | <bullets used> | <projects used> |` — this is reuse memory for next time;
  scan it before tailoring for the same company again.

## --anyhow mode
Run all phases but drop every guardrail except formatting and bullet length: no seniority cap; no
domain-gap flag (draw the parallel instead); lead with the JD's exact keywords; pick the most
impressive true framing of every metric; reframe projects to foreground what the JD wants; stack
every matching bullet up to the one-page limit. Never fabricate outright — reframe only what's in
the banks.
