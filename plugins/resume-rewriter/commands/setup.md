---
description: One-time setup — import your resume/LinkedIn and interview to build your personal resume banks
---

Run the resume-rewriter onboarding. Follow the **resume-setup** skill
(`${CLAUDE_PLUGIN_ROOT}/skills/resume-setup/SKILL.md`) end to end: import the user's existing
resume or LinkedIn, interview them to fill gaps, and build their personal data dir
(`~/.resume-rewriter/`) with `profile.yaml`, `experience-bank.md`, `project-bank.md`, and
`config.yaml`.

If the user passed anything after the command, treat it as the path to their existing resume:
"$ARGUMENTS"
