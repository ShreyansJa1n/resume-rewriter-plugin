# Experience Bullet Bank

Canonical + variant bullets for each job you've held, tagged by JD signal.
The tailor step reads this to pick the best-fitting bullet framing for a given job description.
Setup fills this in from your real experience — the entry below is a worked EXAMPLE showing the
structure. Replace it with your own jobs; keep the format.

**How to use (the tailor step does this automatically):**
1. Read the JD and extract role type + must-have signals.
2. Find the matching tag(s) in each job section below.
3. Pick the variant whose framing best matches the JD's language.
4. If no variant fits, use the canonical bullet + the JD's wording as context to write a new one.

---

## Guardrails (read before picking or rewriting any bullet)

**No new facts.** Every noun, number, tool, or outcome in a bullet must already exist in this bank
or your source resume. If a rewrite needs a detail that isn't there (a latency number, a specific
service, a testing method) to sound complete, cut the clause instead of inventing it. A
vaguer-but-true bullet beats a specific-but-fabricated one — you may have to defend every word in
an interview.

**Calibrate seniority to title and tenure.** One or two "owned end-to-end" / "designed from
scratch" bullets per role read as credible. Four or five stacked on one resume read as implausible
for the title and get flagged by interviewers. Keep solo-ownership verbs (designed, architected,
owned end-to-end, built from scratch) to ~2 per role; use co-, contributed to, or drove for the
rest. (Toggle: `guardrails.seniority_cap` in config.)

**Vary the formula.** Bullets here follow Google's X-Y-Z pattern (Accomplished [X] as measured by
[Y] by doing [Z]). That's correct per bullet but reads as templated when 100% of a page follows it
identically. Let 1-2 bullets per section lead with the action instead, or drop the "as measured
by" clause where the metric is already obvious.

**Bullet length.** Aim for 160-220 non-space characters, max 2 clauses. Split anything longer into
two bullets rather than cramming three "by X, and Y, and Z" clauses into one sentence.

**Em dashes:** never use them, even inside a bullet — use a comma, colon, or period instead.

---

## Pools (optional, for multi-domain candidates)

If a single job spans distinct domains (e.g. you did both mobile and backend work at one company),
group its bullets into **pools** and never mix pools on one resume unless the JD explicitly wants
both. Name pools by domain under the job (see the example's "Backend pool" / "Mobile pool"
sub-headers). If your roles are single-domain, ignore pools and just list tagged bullets.

---

## EXAMPLE COMPANY | Software Engineer | Mon YYYY – Mon YYYY

Pick 3-4 bullets per role. Anchor bullet(s) that apply to every JD should be marked "always
include" below.

---

### [RELIABILITY]   <!-- always include -->
**JD signals:** all roles (reliability, production ownership, on-call, uptime)

**Canonical:**
Maintained 99.9% uptime across production services by owning the on-call rotation, writing
runbooks, and delivering hot fixes under live incident conditions, tracing root causes through
log and metric analysis

**Concise framing (when bullet count is tight):**
Owned the on-call rotation for production services, delivering hot fixes under live incident
conditions and tracing root causes through log and metric analysis to hold 99.9% uptime

---

### [API-PERF]
**JD signals:** backend, performance, caching, latency, scale

**Canonical:**
Reduced median API latency by 40% by profiling hot paths, adding a Redis read-through cache, and
batching downstream calls, validated by p50 and p95 dashboards across the two busiest services

**Action-first framing (drop the metric-lead):**
Profiled hot paths and introduced a Redis read-through cache with request batching to cut median
API latency by 40% across the two busiest production services, tracked on p50 and p95 dashboards

---

### [DX-TOOLING]
**JD signals:** developer experience, tooling, platform, automation, "accelerate other teams"

**Canonical:**
Drove adoption of a shared CI template across eight repositories by documenting the migration and
pairing with each team, cutting average pipeline runtime from 14 minutes to under 6 for the org

---

<!--
Add one `## COMPANY | Role | Dates` section per job, each with 2-6 tagged `### [SIGNAL]` bullets.
Give each tag: JD signals it maps to, a Canonical bullet, and optional alternate framings.
Mark any always-include anchor bullet with an inline comment.
-->

---

## Quick Selection Reference

Fill this table as you add tags — it lets the tailor step find relevant bullets fast.

| JD signal | Use these tags |
|-----------|----------------|
| Reliability / production ownership | RELIABILITY, API-PERF |
| Backend / performance | API-PERF, RELIABILITY |
| Platform / DX / tooling | DX-TOOLING |
| (add your own rows) | ... |
