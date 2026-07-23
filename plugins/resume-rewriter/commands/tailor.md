---
description: Tailor your resume to a job description and generate the DOCX (append --anyhow to drop guardrails)
argument-hint: <job description text | path to JD file> [--anyhow]
---

Tailor the user's resume to this job description and generate the DOCX. Follow the
**resume-tailor** skill (`${CLAUDE_PLUGIN_ROOT}/skills/resume-tailor/SKILL.md`) through all phases,
reading the banks from `~/.resume-rewriter/` and running
`${CLAUDE_PLUGIN_ROOT}/scripts/generate_resume.py`.

If setup hasn't been run (no data dir / banks), tell the user to run `/resume-rewriter:setup` first.

Job description (inline text, a file path, or a URL — resolve if needed). If empty, ask for it:
"$ARGUMENTS"
