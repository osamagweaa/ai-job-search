---
name: job-evaluate
description: Evaluate a job posting against the candidate profile using a 6-dimension scoring framework. Returns a weighted fit score, dimension breakdown, key strengths, gaps, and a Go/No-Go recommendation.
version: "1.0"
triggers:
  - evaluate this job
  - how good is this role
  - assess this position
  - fit score
  - should I apply
  - is this a good match
  - vurder dette job
tools:
  - bash
---

# Job Evaluate Skill

Score a job posting against the candidate profile across 6 dimensions and produce a Go/No-Go recommendation.

## Input

The user provides one of:
- A job URL — fetch it with the appropriate portal `detail` CLI command, or use a web fetch
- Pasted job text — parse it directly

## Execution steps

### Step 1 — Fetch the job posting

If the input is a Jobindex URL or ID:
```bash
bun run .agents/skills/jobindex-search/cli/src/cli.ts detail <id> --format plain
```

If Jobdanmark (slug in URL):
```bash
bun run .agents/skills/jobdanmark-search/cli/src/cli.ts detail <slug> --format plain
```

If Jobbank (numeric ID):
```bash
bun run .agents/skills/jobbank-search/cli/src/cli.ts detail <id> --format plain
```

If Jobnet (UUID):
```bash
bun run .agents/skills/jobnet-search/cli/src/cli.ts detail <jobAdId> --format plain
```

If the URL is from another portal, fetch it directly via HTTP.

### Step 2 — Read the candidate profile

Read `CLAUDE.md` to extract the candidate's current skills, experience, location constraints, career goals, behavioral profile, and deal-breakers. This is the ground truth for all scoring.

### Step 3 — Score all 6 dimensions

Apply the scoring rubric below to the job posting.

---

#### Dimension 1 — Technical Skills Match (weight: 30%)

| Score | Meaning |
|-------|---------|
| 80–100 | Core requirements are the candidate's primary skills |
| 60–79 | Most requirements match; 1–2 learnable gaps |
| 40–59 | Partial match; significant upskilling needed |
| 0–39 | Fundamental mismatch |

List the required skills from the posting and mark each as: Primary / Secondary / Gap.

#### Dimension 2 — Experience Match (weight: 25%)

| Score | Meaning |
|-------|---------|
| 80–100 | Direct experience in same domain and role type |
| 60–79 | Related experience; transferable skills evident |
| 40–59 | Adjacent experience; would need to make the case |
| 0–39 | Unrelated experience |

#### Dimension 3 — Behavioral/Culture Fit (weight: 15%)

| Score | Meaning |
|-------|---------|
| 80–100 | Culture strongly matches behavioral preferences |
| 60–79 | Mixed signals but mostly compatible |
| 40–59 | Some friction areas |
| 0–39 | Significant culture mismatch |

Red flags: departmental disorganization, maintenance-heavy work, mismatched leadership style. Check any public signals in the job text.

#### Dimension 4 — Location & Logistics (Pass/Fail)

- Within commute range or remote/hybrid: **PASS**
- Requires relocation: **FAIL** (deal-breaker — stop here if FAIL)
- Frequent international travel: **FLAG** — note and continue

#### Dimension 5 — Career Alignment & Motivation (weight: 30%)

| Score | Meaning |
|-------|---------|
| 80–100 | Strongly aligned with career direction and growth path |
| 60–79 | Good role but only partially aligned with long-term goals |
| 40–59 | Decent job but doesn't build toward career goals |
| 0–39 | Dead end or backwards step |

Evaluate not just whether the candidate *can* do the tasks, but whether they will *energize* them.

#### Dimension 6 — Salary Benchmark (Optional)

If `salary_data.json` exists, run:
```bash
python3 salary_lookup.py "<Company Name>" --json
```

Add `--city "<City>"` if a city is known. Present results as an index vs. baseline. Skip this dimension if the file is absent.

---

### Step 4 — Calculate weighted score

```
Overall = (Tech × 0.30) + (Experience × 0.25) + (Behavioral × 0.15) + (Career × 0.30)
```

Location is pass/fail — if FAIL, report "Not viable" and stop.

Thresholds:
- **Strong Fit** 75+: Definitely apply
- **Good Fit** 60–74: Apply, address gaps in cover letter
- **Moderate Fit** 45–59: Consider carefully
- **Weak Fit** 30–44: Probably skip
- **Poor Fit** <30: Skip

### Step 5 — Output

```
## Job Fit Evaluation: [Role] at [Company]

| Dimension         | Score   | Notes                            |
|-------------------|---------|----------------------------------|
| Technical Skills  | XX/100  | ...                              |
| Experience Match  | XX/100  | ...                              |
| Behavioral Fit    | XX/100  | ...                              |
| Location          | PASS    | ...                              |
| Career Alignment  | XX/100  | ...                              |
| Salary Benchmark  | +X%     | (if available)                   |

**Overall Score: XX/100 — [Verdict]**

### Key Strengths for This Role
- ...

### Gaps to Address
- ...

### Recommendation
[1–2 sentences: apply / skip / apply with caveats]

### Company Research Checklist
- [ ] Checked company website (mission, values, recent news)
- [ ] Checked review sites (Glassdoor, Jobindex, etc.)
- [ ] Checked LinkedIn for team size, recent hires, connections
- [ ] Checked media for restructuring, growth, or workplace issues
```

### Step 6 — Next step prompt

If verdict is Good Fit or Strong Fit, say:
> "To generate a tailored CV and cover letter, run `/apply <URL>` in Claude Code."

If Moderate Fit, ask the user whether to proceed.
