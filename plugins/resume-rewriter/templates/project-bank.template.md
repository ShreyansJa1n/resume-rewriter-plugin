# Project Bank

A running catalog of your projects with pre-written bullets. The tailor step picks 2-3 projects
per resume by domain match and gap coverage. Setup fills this in from your real projects — the
entry below is a worked EXAMPLE showing the structure. Add new projects here over time.

## Guardrails (read before using or rewriting any bullet)

**No new facts.** Do not add specifics that aren't listed under a project, even plausible-sounding
ones — no invented latency numbers, no services you didn't use, no testing frameworks not named
here. If a JD needs infra depth a project doesn't actually have (GPU/accelerator, model-serving,
distributed training, etc.), say so rather than stretching an unrelated project to fit. That's a
real content gap, not a wording problem. (Toggle: `guardrails.domain_gap_flag` in config.)

**Bullet length:** aim for 160-220 non-space characters, max 2 clauses. Split anything longer into
two bullets rather than stacking three "by X, Y, and Z" clauses in one sentence.

**No em dashes.** Use a comma, colon, or period.

**Per-project fields:**
- **Repo** — a GitHub/GitLab link, or `(in progress — no public repo yet)`. Projects with no
  public repo should render with empty `name_url` and `repo_url` in the generated resume.
- **Stack** — the real technologies used (shown after the project title on the resume).
- **Bullets** — 1-4 achievement bullets, ideally with a measurable before/after.

---

## Realtime Chat Service   <!-- EXAMPLE — replace with your own -->

- **Repo:** github.com/yourhandle/chat
- **Stack:** Go, WebSockets, PostgreSQL, Redis

**Bullets:**
- Built a horizontally scalable chat backend in Go handling fan-out to thousands of concurrent
  WebSocket clients, using Redis pub/sub for cross-node delivery and PostgreSQL for durable
  message history with at-least-once semantics
- Cut reconnect-storm recovery time by adding client backoff with jitter and a server-side
  connection-draining path, validated with a load test simulating a full-region failover

---

<!--
Add one `## Project Name` section per project, following the format above.
Domain tags are informal here — the tailor step infers domain from stack + bullets.
Aim for a spread across your domains (backend, frontend, ML, infra, mobile, ...) so there's a
strong pick for most JDs.
-->
