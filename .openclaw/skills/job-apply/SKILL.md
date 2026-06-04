---
name: job-apply
description: Lightweight application preparation — runs a full fit evaluation then produces text-based tailoring notes (skills to emphasise, gaps to address, cover letter talking points). The full LaTeX CV + cover letter workflow must be completed in Claude Code.
version: "1.0"
triggers:
  - apply to this job
  - help me apply
  - prepare application
  - write cover letter
  - tailor my CV
  - ansøg dette job
tools:
  - bash
---

# Job Apply Skill

Prepare an application strategy for a job. This skill handles the thinking; Claude Code handles the documents.

## Scope

**This skill produces:**
- Full 6-dimension fit evaluation (identical to `job-evaluate`)
- Text tailoring notes: which skills and achievements to lead with, bullets to cut or add
- Cover letter talking points: opening hook, 2–3 body themes, closing angle
- A pre-application checklist

**This skill does NOT produce:**
- LaTeX `.tex` files
- Compiled PDFs
- The final formatted CV or cover letter

For the full document workflow (moderncv 2-page CV, cover.cls cover letter, mandatory PDF verification) run:
```
/apply <job-url>
```
in a Claude Code session with this repository open.

---

## Execution steps

### Step 1 — Evaluate fit

Run the full evaluation from the `job-evaluate` skill. If a fit score has already been produced for this posting in this session, reuse it.

### Step 2 — If score < 45 (Weak/Poor Fit)

Inform the user of the score and ask whether to proceed anyway. Do not generate tailoring notes for a Poor Fit job without explicit confirmation.

### Step 3 — CV tailoring notes

Read `CLAUDE.md` to understand the full experience and skill set. Then produce:

**Skills to emphasise** — map the top 5 required skills from the posting to the candidate's strongest matching evidence:
```
| Required Skill        | Your Evidence                                      |
|-----------------------|----------------------------------------------------|
| [skill from posting]  | [specific project, role, or achievement from CV]   |
```

**Bullets to add or strengthen** — up to 3 new bullet points to add to the CV for this role. Keep them truthful and grounded in actual experience.

**Bullets to cut** — up to 3 existing bullet points that are least relevant to this role and can be deprioritised to free up space.

**Profile statement hook** — one sentence for the CV profile section that positions the candidate specifically for this role.

### Step 4 — Cover letter talking points

Produce a structured outline (not a full letter):

```
Opening (2–3 sentences):
- Why this company and this role specifically — not generic enthusiasm
- One concrete hook: a result, a project, or a shared value

Body theme 1 — [most relevant strength]:
- Key claim
- Evidence (brief)

Body theme 2 — [second strength or biggest gap addressed]:
- Key claim
- How you bridge the gap or frame the experience

Body theme 3 — [motivation / career alignment]:
- Why this role advances your goals
- What you bring that's non-obvious from the CV

Closing:
- Call to action
- Any pre-application call worth making (see checklist below)
```

### Step 5 — Pre-application checklist

```
### Before You Apply
- [ ] Is there a named contact person? If yes, consider calling with a substantive question.
- [ ] Have you checked the company's recent news (expansions, restructuring, culture signals)?
- [ ] Deadline: [extracted from posting]
- [ ] Language: [Danish / English — match cover letter language to posting]
- [ ] Run `/apply <url>` in Claude Code to generate the final CV and cover letter PDFs.
```

### Step 6 — Offer to log to tracker

Ask: "Should I add this to your application tracker as 'In Progress'?"

If yes, append a row to `job_search_tracker.csv`:
```bash
python3 -c "
import csv, datetime, pathlib
tracker = pathlib.Path('job_search_tracker.csv')
with open(tracker, 'a', newline='') as f:
    w = csv.writer(f)
    w.writerow([
        datetime.date.today().isoformat(),
        '<COMPANY>',
        '<SECTOR>',
        '<ROLE>',
        'Full-time',
        'Direct',
        'In Progress',
        '',
        '<FIT_SCORE>',
        'Via OpenClaw job-apply',
        '',
        '',
        '<URL>'
    ])
print('Added to tracker.')
"
```
