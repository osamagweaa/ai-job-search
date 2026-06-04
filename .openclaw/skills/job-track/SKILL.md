---
name: job-track
description: View and update the job application tracker (job_search_tracker.csv). Show the active pipeline, filter by status, and update application statuses directly from chat.
version: "1.0"
triggers:
  - show my applications
  - update application status
  - what did I apply to
  - application tracker
  - pipeline
  - where are my applications
  - mark as interviewed
  - mark as rejected
  - log application
tools:
  - bash
---

# Job Track Skill

Read and write the application tracker at `job_search_tracker.csv`.

## Tracker columns

| Column | Description |
|--------|-------------|
| date | Date added (YYYY-MM-DD) |
| company | Company name |
| sector | Industry sector |
| role | Job title |
| role_type | Full-time / Part-time / etc. |
| channel | How found (Direct / LinkedIn / Recruiter / etc.) |
| status | Current status (see statuses below) |
| contact_person | Named contact at the company |
| fit_rating | Numeric fit score (0–100) |
| notes | Free-text notes |
| cv_file | Path to the tailored CV file |
| cover_letter_file | Path to the cover letter file |
| source | URL of the job posting |

**Valid statuses:** `In Progress`, `Applied`, `Interview`, `Offer`, `Rejected`, `Withdrawn`, `On Hold`

---

## Operations

### View the full pipeline

```bash
python3 -c "
import csv, pathlib, json
tracker = pathlib.Path('job_search_tracker.csv')
if not tracker.exists():
    print('Tracker is empty.')
else:
    rows = list(csv.DictReader(open(tracker)))
    active = [r for r in rows if r['status'] not in ('Rejected', 'Withdrawn')]
    print(json.dumps(active, indent=2))
"
```

Present the active rows as a chat-friendly table:
```
## Application Pipeline

| Company     | Role                 | Status      | Fit  | Date       | Notes |
|-------------|----------------------|-------------|------|------------|-------|
| Novo Nordisk| Senior Data Engineer | Interview   | 82   | 2026-05-20 | ...   |
| ...         | ...                  | ...         | ...  | ...        | ...   |
```

Show Rejected/Withdrawn as a separate collapsed section.

### Filter by status

When the user asks for a specific status (e.g. "show interviews", "show rejections"):

```bash
python3 -c "
import csv, pathlib, json, sys
status_filter = sys.argv[1]
rows = list(csv.DictReader(open('job_search_tracker.csv')))
print(json.dumps([r for r in rows if r['status'].lower() == status_filter.lower()], indent=2))
" "Interview"
```

### Update a status

When the user says "mark [company] as [status]":

1. Confirm the match: find the row by company name (case-insensitive partial match)
2. If ambiguous, list the matches and ask which one
3. Write back the updated CSV:

```bash
python3 -c "
import csv, pathlib

tracker = pathlib.Path('job_search_tracker.csv')
rows = list(csv.DictReader(open(tracker)))
fieldnames = rows[0].keys() if rows else []

target_company = '<COMPANY>'.lower()
new_status = '<NEW_STATUS>'
updated = 0

for row in rows:
    if target_company in row['company'].lower():
        row['status'] = new_status
        updated += 1

with open(tracker, 'w', newline='') as f:
    w = csv.DictWriter(f, fieldnames=fieldnames)
    w.writeheader()
    w.writerows(rows)

print(f'Updated {updated} row(s).')
"
```

### Add a new entry

When the user wants to log a new application manually:

```bash
python3 -c "
import csv, datetime, pathlib
tracker = pathlib.Path('job_search_tracker.csv')
with open(tracker, 'a', newline='') as f:
    w = csv.writer(f)
    w.writerow([
        '<DATE>',
        '<COMPANY>',
        '<SECTOR>',
        '<ROLE>',
        '<ROLE_TYPE>',
        '<CHANNEL>',
        '<STATUS>',
        '<CONTACT>',
        '<FIT_RATING>',
        '<NOTES>',
        '',
        '',
        '<URL>'
    ])
print('Entry added.')
"
```

### Summary stats

When the user asks for statistics or an overview:

```bash
python3 -c "
import csv, pathlib, collections
rows = list(csv.DictReader(open('job_search_tracker.csv')))
counts = collections.Counter(r['status'] for r in rows)
print(dict(counts))
"
```

Present as:
```
## Application Summary
- Total: 12
- Applied: 5
- Interview: 3
- Offer: 1
- Rejected: 2
- In Progress: 1
```
