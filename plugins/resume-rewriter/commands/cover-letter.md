---
description: Draft a tailored cover letter for a company/role and generate the DOCX
argument-hint: <company name and/or job description | path to JD file>
---

Draft and generate a cover letter DOCX for the user.

1. Determine the target company and role from "$ARGUMENTS" (inline text, a path, or a URL —
   resolve if needed). If empty, ask for the company, role, and the job description.
2. Read the user's data dir `~/.resume-rewriter/` — `profile.yaml` (for the signer's name/contact
   links) and both banks (for the concrete achievements/projects to draw on). If it's missing,
   tell the user to run `/resume-rewriter:setup` first.
3. Draft 3-4 tight paragraphs, first person, specific to this company: why this company/role, one
   concrete proudest project drawn from the banks (with real details only), how the candidate
   works, and a close. No fabrication, no fluff words, no em dashes.
4. Confirm the draft with the user, then write a `data.json` (shape below) and run:
   ```
   python3 "${CLAUDE_PLUGIN_ROOT}/scripts/generate_cover_letter.py" \
     --data ~/.resume-rewriter/runs/<Company>/cover_data.json \
     --config ~/.resume-rewriter/config.yaml
   ```

`data.json` shape:
```json
{
  "company": "Acme",
  "date": "<today, e.g. July 22, 2026>",
  "greeting": "Dear Acme Hiring Team,",
  "body": ["paragraph one", "paragraph two", "paragraph three"],
  "closing": "Sincerely,",
  "signer": { "name": "...", "phone": "...", "email": "...",
              "linkedin": {"text":"...","url":"..."}, "github": {"text":"...","url":"..."} }
}
```

Report the saved path when done.
