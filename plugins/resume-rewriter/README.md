# resume-rewriter plugin — internals

```
plugins/resume-rewriter/
├── .claude-plugin/plugin.json     # plugin manifest
├── commands/                      # slash commands (thin wrappers over the skills)
│   ├── setup.md                   # /resume-rewriter:setup
│   ├── tailor.md                  # /resume-rewriter:tailor <JD> [--anyhow]
│   └── cover-letter.md            # /resume-rewriter:cover-letter <company/JD>
├── skills/
│   ├── resume-setup/SKILL.md      # import + interview -> builds the user's data dir
│   ├── resume-tailor/SKILL.md     # 8-phase JD -> tailored DOCX workflow
│   └── role-overlays/fde.md       # customer-facing (FDE/SE/CE) overlay, loaded by tailor
├── scripts/
│   ├── generate_resume.py         # DOCX engine: --data <json> --config <yaml/json> [--out-dir]
│   └── generate_cover_letter.py   # cover letter engine, same CLI
├── templates/                     # copied into ~/.resume-rewriter/ during setup
│   ├── profile.template.yaml
│   ├── experience-bank.template.md
│   ├── project-bank.template.md
│   └── config.template.yaml
└── requirements.txt               # python-docx, pyyaml
```

## Data flow

1. **Setup** copies the templates into `~/.resume-rewriter/` and populates `profile.yaml`,
   `experience-bank.md`, `project-bank.md`, `config.yaml`.
2. **Tailor** reads those, selects content per the JD, writes a per-run
   `~/.resume-rewriter/runs/<Company>/data.json`, and calls `generate_resume.py --data ... --config
   ~/.resume-rewriter/config.yaml`.
3. The engine renders the DOCX to `output_dir` (default `~/Documents/Resumes/<Company>/`), printing
   a word-count estimate and a per-bullet length/em-dash validation report.

## The engine contract

`generate_resume.py` is pure and personal-data-free. It takes:
- `--data` : a JSON file with `company/team/role/header/education/experience/projects/skills`.
- `--config` : YAML or JSON style/rules; every key falls back to a built-in default
  (`DEFAULT_CONFIG` in the script), so it runs even with no config or no pyyaml.
- `--out-dir` : optional override of the configured output directory.

Because all content and style are external, the same script serves every user — nothing below the
`# --- FORMATTING ENGINE ---` boundary needs editing per person.

## Testing changes to the engine

```
python3 scripts/generate_resume.py --data <sample>.json --config templates/config.template.yaml --out-dir /tmp/out
```
