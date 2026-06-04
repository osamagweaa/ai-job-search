---
name: upskill
description: Analyse the job application tracker and recent search results to identify recurring skill gaps. Returns a prioritised learning plan with suggested resources.
version: "1.0"
triggers:
  - what should I learn
  - skill gaps
  - upskill me
  - learning plan
  - what am I missing
  - career development
  - hvad skal jeg lære
tools:
  - bash
---

# Upskill Skill

Identify skill gaps from the jobs you've tracked and produce a prioritised learning plan.

## When to use

Trigger when the user wants to understand what skills they are repeatedly underqualified for across the jobs they have applied to or considered.

Accepts an optional target: "upskill for [specific role]" or "upskill for [company]" to narrow analysis to a single job instead of the full tracker.

---

## Execution steps

### Step 1 — Load tracker data

```bash
python3 -c "
import csv, pathlib, json
rows = list(csv.DictReader(open('job_search_tracker.csv')))
print(json.dumps(rows, indent=2))
"
```

Extract the list of roles, companies, and source URLs from the tracker. Focus on entries with status `Applied`, `Interview`, `In Progress`, or `On Hold` — exclude `Rejected` unless the user asks for a retrospective.

### Step 2 — Load candidate profile

Read `CLAUDE.md` to extract:
- Current primary and secondary skills
- Experience domains
- Certifications and education

This is the baseline for gap identification.

### Step 3 — Fetch job posting details (targeted mode)

If the user specified a single job, fetch its full description using the appropriate portal CLI:

```bash
# Example: Jobindex
bun run .agents/skills/jobindex-search/cli/src/cli.ts detail <id> --format plain

# Example: Jobbank
bun run .agents/skills/jobbank-search/cli/src/cli.ts detail <id> --format plain
```

If running in aggregate mode (all tracked jobs), use the job titles and notes already in the tracker — do not re-fetch all postings.

### Step 4 — Identify skill gaps

Cross-reference the required skills mentioned across the tracked jobs against the candidate's skill set. Categorise each gap:

| Priority | Definition |
|----------|------------|
| Critical | Required by 3+ jobs; candidate has no experience |
| High | Required by 2+ jobs; candidate has limited exposure |
| Medium | Required by 1–2 jobs; learnable with moderate effort |
| Low | Nice-to-have mentions only |

### Step 5 — Output: Gap heatmap

```
## Skill Gap Analysis — [date]

| Skill / Technology   | Jobs Requiring It | Your Level    | Priority |
|----------------------|-------------------|---------------|----------|
| [Skill A]            | 5                 | None          | Critical |
| [Skill B]            | 3                 | Beginner      | High     |
| [Skill C]            | 2                 | Intermediate  | Medium   |
| [Skill D]            | 1                 | None          | Low      |
```

### Step 6 — Learning plan

For each Critical and High-priority gap, provide:

```
### [Skill A] — Critical

**Why it matters:** Appears in X of your tracked jobs.
**Recommended path:**
1. [Course / resource name] — [platform] — [estimated time]
2. [Project idea to practise]
3. [Certification if relevant]

**Quick win:** [Fastest way to get to 'familiar' level — e.g. a specific tutorial or dataset]
```

Use web search to find up-to-date, highly-rated resources if available. Prefer free-first options (official docs, free tiers, Coursera audit mode) unless the user specifies otherwise.

### Step 7 — Study order

Suggest a 4-week sprint sequence based on priority and dependency:

```
## Suggested 4-Week Sprint

Week 1: [Critical skill A] — foundational concepts
Week 2: [Critical skill A continued] + [High skill B] intro
Week 3: [High skill B] deepened + hands-on project
Week 4: [Medium skill C] + review and consolidation
```

### Step 8 — Offer follow-up

Ask: "Would you like me to search for jobs that match your current skill set while you upskill?"
