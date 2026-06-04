---
name: job-search
description: Search all four Danish job portals (Jobindex, Jobdanmark, Akademikernes Jobbank, Jobnet) for new openings matching the candidate profile. Deduplicates against previously seen jobs and rates each result High/Medium/Low for fit.
version: "1.0"
triggers:
  - find jobs
  - search jobs
  - any new openings
  - what's new
  - scrape jobs
  - check for jobs
  - nye jobs
  - find mig et job
  - søg job
tools:
  - bash
---

# Job Search Skill

Search all four Danish job portals and surface new matches ranked by fit.

## When to use

Trigger when the user wants to discover new job postings. Accepts an optional focus keyword (e.g. "find jobs data science") or runs a broad search if no keyword is given.

## Execution steps

### Step 1 — Load state

Read the deduplication log to avoid re-presenting jobs already seen or tracked:

```python
import json, csv, os, pathlib

seen = set()
log_path = pathlib.Path("job_scraper/seen_jobs.json")
if log_path.exists():
    seen.update(json.loads(log_path.read_text()))

tracker_path = pathlib.Path("job_search_tracker.csv")
if tracker_path.exists():
    with open(tracker_path) as f:
        for row in csv.DictReader(f):
            if row.get("source"):
                seen.add(row["source"].strip())
```

Run this logic inline via `python3 -c "..."` to get the seen set before searching.

### Step 2 — Run searches on all four portals

Run the following four commands. Use the user's keyword as the search term; fall back to a broad role-appropriate query if none is provided. Use `--jobage 14` / `--since` filters to focus on recent postings. Use `--format json` for all commands so results can be aggregated.

**Jobindex** (relevance-sorted, last 14 days):
```bash
bun run .agents/skills/jobindex-search/cli/src/cli.ts search \
  --query "<KEYWORD>" \
  --jobage 14 \
  --sort date \
  --limit 20 \
  --format json
```

**Jobdanmark** (IT/Engineering category or free-text, last page):
```bash
bun run .agents/skills/jobdanmark-search/cli/src/cli.ts search \
  --text "<KEYWORD>" \
  --format json \
  --limit 20
```

**Akademikernes Jobbank** (full-time + academic focus):
```bash
bun run .agents/skills/jobbank-search/cli/src/cli.ts search \
  --key "<KEYWORD>" \
  --type 3 \
  --limit 20 \
  --format json
```

**Jobnet** (sorted by publication date):
```bash
bun run .agents/skills/jobnet-search/cli/src/cli.ts search \
  --search-string "<KEYWORD>" \
  --order PublicationDate \
  --per-page 20 \
  --format json
```

### Step 3 — Deduplicate

Filter out any result whose URL or `company + title` combination is in the seen set from Step 1.

### Step 4 — Quick fit assessment

For each remaining job, assign a fit rating based on the title and any snippet available:

- **High**: Title or description closely matches the candidate's primary skills and target role types
- **Medium**: Partial match — related domain or adjacent role
- **Low**: Tangential match — included for completeness

### Step 5 — Update seen log

Append the URLs of all results (including Low-fit ones) to `job_scraper/seen_jobs.json` so they are not re-presented on the next run:

```bash
python3 -c "
import json, pathlib, sys
log = pathlib.Path('job_scraper/seen_jobs.json')
seen = json.loads(log.read_text()) if log.exists() else []
new_urls = sys.stdin.read().split()
log.parent.mkdir(exist_ok=True)
log.write_text(json.dumps(list(set(seen + new_urls)), indent=2))
"
```

### Step 6 — Present results

Return a table sorted by fit (High first), then deadline:

```
## New Job Matches — [date]

| Fit    | Title                     | Company         | Portal      | Deadline   | URL |
|--------|---------------------------|-----------------|-------------|------------|-----|
| 🟢 High | Senior Data Engineer      | Novo Nordisk    | Jobbank     | 2026-06-15 | ... |
| 🟡 Med  | Analytics Consultant      | McKinsey        | Jobindex    | 2026-06-20 | ... |
| 🔴 Low  | BI Developer              | DSB             | Jobnet      | 2026-06-30 | ... |
```

After presenting, ask: "Would you like me to evaluate any of these in detail?"

If the user picks a job, proceed to the `job-evaluate` skill with that URL.
