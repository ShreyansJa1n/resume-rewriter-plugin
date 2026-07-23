# Resume Rewriter — a Claude Code plugin

Build a personal **experience + project bank** once, then generate a **one-page, job-tailored
resume** (and a matching cover letter) as a polished `.docx` for any job description — all from
inside Claude Code.

Instead of rewriting your resume from scratch for every application, you teach Claude your career
history *once*. After that, you paste a job description and get a tailored, ATS-friendly one-page
resume in seconds — with sensible guardrails so it stays honest and doesn't read as AI-generated.

---

## How it works (the 30-second version)

1. **Setup, once** — `/resume-rewriter:setup`
   You give Claude your existing resume/LinkedIn; it interviews you to fill the gaps and saves your
   career data to `~/.resume-rewriter/`.
2. **Tailor, per job** — `/resume-rewriter:tailor <paste the job description>`
   Claude picks your best-fitting bullets + projects for that JD and generates the `.docx`.
3. **Optional** — `/resume-rewriter:cover-letter <company / JD>`
   A matching cover letter.

Your resume data and generated documents live on *your* machine, never in this repo.

---

## Prerequisites

- **Claude Code** installed and working ([install guide](https://code.claude.com/docs)).
- **Python 3** (3.9+), with two libraries:
  ```bash
  pip install python-docx pyyaml
  ```
  (`python-docx` builds the Word file; `pyyaml` reads your config. That's the whole dependency list.)
- **Microsoft Word, Google Docs, or Pages** to open/export the resulting `.docx` (also handy for a
  final one-page eyeball before you submit).

---

## Install

```
/plugin marketplace add ShreyansJa1n/resume-rewriter-plugin
/plugin install resume-rewriter@resume-rewriter-marketplace
```

That's it — you now have three slash commands: `/resume-rewriter:setup`,
`/resume-rewriter:tailor`, and `/resume-rewriter:cover-letter`.

> **Want to try it before installing?** Clone the repo and run
> `claude --plugin-dir ./plugins/resume-rewriter`, then `/reload-plugins`.

---

## Step 1 — Set up your career bank (one time)

Run:

```
/resume-rewriter:setup
```

**A. Import.** Claude asks for an existing resume or LinkedIn export. Give it a file path
(PDF/DOCX/Markdown/TXT) or just paste the text. If you have nothing yet, that's fine — say so and
it'll build everything from the interview. You can also pass the file straight away:

```
/resume-rewriter:setup ~/Documents/my_current_resume.pdf
```

**B. Interview.** Claude reads what you gave it, then asks focused follow-up questions to capture
the things resumes need but imports usually miss:
- **Numbers** — latency, uptime, %, users, revenue, time saved, before/after. (These make bullets
  land.)
- **Ownership level** — did you *own*, *co-own*, *contribute to*, or *drive* each thing? (This keeps
  the resume credible for your actual title.)
- **Distinct skill areas** in a single job (e.g. mobile *and* backend) so they can be kept separate
  per target role.
- **Which strengths each achievement shows** (reliability, performance, developer experience,
  product, ML, etc.).

Answer in your own words; approximate numbers are fine. Nothing is invented — if you don't have a
metric, Claude writes the honest qualitative version.

**C. What gets created.** Claude writes these to `~/.resume-rewriter/`:

| File | What it is |
|------|-----------|
| `profile.yaml` | Your name, contact links, optional work-auth line, and education |
| `experience-bank.md` | Your jobs, each with tagged bullet variants for different role types |
| `project-bank.md` | Your projects, with pre-written achievement bullets |
| `config.yaml` | Style + writing rules (see [Customizing](#customizing)); defaults are sensible |
| `history.md` | A log of what you've generated (so repeat applications reuse well) |

You can open and hand-edit any of these anytime — they're plain text.

---

## Step 2 — Generate a tailored resume (per application)

Paste the job description right after the command (or give a file path / URL):

```
/resume-rewriter:tailor <paste the full job description here>
```

Claude will:
1. Analyze the JD (role type, must-haves, nice-to-haves, seniority signals).
2. Pick your most relevant experience bullets and 2–3 projects from your banks.
3. Run quality checks — bullet length, an ownership-verb cap so it reads credibly, formula variety
   so it doesn't look templated, and a no-em-dash rule.
4. Generate the `.docx` and tell you where it saved it.

**Default output location:** `~/Documents/Resumes/<Company>/<Your_Name>_Resume_<Company>.docx`

Open it in Word/Docs to confirm it's one page, then submit. If a job genuinely needs a skill you
don't have, Claude tells you honestly instead of faking it.

### Aggressive mode

Add `--anyhow` to drop the conservative guardrails for a maximally keyword-matched, metric-forward
version (still only reframing real content, never fabricating):

```
/resume-rewriter:tailor --anyhow <job description>
```

---

## Step 3 — Cover letter (optional)

```
/resume-rewriter:cover-letter <company name and/or the job description>
```

Claude drafts 3–4 tight, company-specific paragraphs drawn from your banks, confirms the draft with
you, and generates `~/Documents/Resumes/<Company>/<Your_Name>_CoverLetter_<Company>.docx`.

---

## Customizing

All style and writing rules live in `~/.resume-rewriter/config.yaml`. Edit it to change:

- **Look**: `font`, `size`, `margins`, hyperlink color.
- **Length**: the one-page `word_target` and per-bullet character bounds.
- **Naming/output**: `filename_pattern` and `output_dir`.
- **Guardrails**: toggle the seniority-verb cap, em-dash check, formula-variety, and
  domain-gap honesty flag on/off.

The shipped defaults reproduce a classic one-page style (Garamond 9.5pt, 0.5" margins, ~665–700
words). Delete the file to fall back to those defaults entirely.

---

## Your data stays yours

Everything personal (`~/.resume-rewriter/`) and every generated `.docx` lives on your machine and is
**not** part of this repo. The `.gitignore` here also keeps personal data and `.docx` files out of
version control, so it's safe to fork and share.

---

## Troubleshooting

- **`ModuleNotFoundError: docx` / `yaml`** → `pip install python-docx pyyaml` (use the same Python
  that Claude Code invokes; try `python3 -m pip install ...`).
- **"run setup first"** → the data dir `~/.resume-rewriter/` doesn't exist yet; run
  `/resume-rewriter:setup`.
- **Resume spills to a second page** → open in Word and trim a weak bullet, or lower `word_target`
  in `config.yaml`.
- **Commands don't show up** → `/reload-plugins`, or confirm the marketplace was added with
  `/plugin marketplace add ShreyansJa1n/resume-rewriter-plugin`.

---

## For contributors

Repo internals, the data flow, and how to test the DOCX engine are documented in
[`plugins/resume-rewriter/README.md`](plugins/resume-rewriter/README.md).

The generators are personal-data-free and data-driven, so you can test them directly:

```bash
python3 plugins/resume-rewriter/scripts/generate_resume.py \
  --data <your-sample>.json \
  --config plugins/resume-rewriter/templates/config.template.yaml \
  --out-dir /tmp/out
```
