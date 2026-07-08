# Job Evaluation Framework

<!-- SETUP: Skill match areas and career goals are personalized by running /setup -->

## Scoring Dimensions

Evaluate each job posting against these five dimensions:

### 1. Technical Skills Match (0-100)
How well do the required/preferred skills align with the candidate's capabilities?

| Score | Meaning |
|-------|---------|
| 80-100 | Core requirements are primary skills |
| 60-79 | Most requirements match, 1-2 gaps that are learnable |
| 40-59 | Partial match, significant upskilling needed |
| 0-39 | Fundamental mismatch |

**Strong match areas:** Consultative selling, C-level/founder negotiation, outbound prospecting (SDR/BDR motions), SaaS/IaaS/cloud infrastructure sales, MENA/Arabic-market go-to-market, Salesforce/ZoomInfo/Sales Navigator tooling
**Moderate match areas:** Full-cycle enterprise deal closing as a title-of-record AE, AI/agent-product sales specifically (vs. cloud infra generally), supply chain SaaS domain knowledge (has adjacent logistics/cold-chain experience, not SaaS-side)
**Weak match areas:** Deep technical/product engineering roles, roles requiring non-sales technical delivery (e.g., solutions engineering, implementation)

### 2. Experience Match (0-100)
Does work history align with what they're looking for?

| Score | Meaning |
|-------|---------|
| 80-100 | Direct experience in the same domain and role type |
| 60-79 | Related experience, transferable skills clear |
| 40-59 | Adjacent experience, would need to make the case |
| 0-39 | Unrelated experience |

**Strong:** Senior SDR / Strategic BDR roles, Arabic-market or MENA-region sales roles, SMB/Mid-Market cloud or SaaS sales, roles emphasizing outbound pipeline generation
**Moderate:** Full-cycle Account Executive roles (has AE-titled experience at Central Fruit plus proven pipeline generation and quota attainment at Google Cloud, but not yet a title-of-record cloud/SaaS AE closing complex deals solo)
**Entry-level:** Enterprise AE roles requiring multi-year track record of solo-closed 6-figure ARR deals in SaaS specifically

### 3. Behavioral/Culture Fit (0-100)
Does the role and company culture match the behavioral profile?

| Score | Meaning |
|-------|---------|
| 80-100 | Culture strongly matches behavioral preferences |
| 60-79 | Mixed signals but mostly compatible |
| 40-59 | Some friction areas |
| 0-39 | Significant culture mismatch |

**Red flags to research:** Department disorganization, work dominated by maintenance over development, poor chemistry with leadership, culture mismatches. Check reviews, media coverage, LinkedIn connections, and network contacts for insider perspective.

### 4. Location & Logistics (Pass/Fail + Notes)
- Based in Barcelona, Spain, or hybrid/onsite roles elsewhere in Spain where the employer covers relocation costs: PASS
- Fully remote role: PASS only if it comes with a Spanish indefinite contract, OR an OTE of 70K+ (otherwise FLAG)
- Requires relocation outside Spain within the next 4 years: FAIL (hard deal-breaker)
- Frequent international travel: FLAG (discuss with user)

### 5. Career Alignment & Motivation (0-100)
Does this role advance career goals and contain tasks that energize?

| Score | Meaning |
|-------|---------|
| 80-100 | Strongly aligned with career direction, clear growth path |
| 60-79 | Good role but only partially aligned with long-term goals |
| 40-59 | Decent job but doesn't build toward career goals |
| 0-39 | Dead end or backwards step |

**Career goals:**
- Move into (or firmly establish as) an Account Executive role at a logtech, cloud infrastructure, security, or AI agent company
- Target companies: Anthropic, Palo Alto Networks, Flexport, and comparable competitors in those spaces
- Secure high pay with uncapped OTE (baseline 50K+; remote-only offers need 70K+ or a Spanish indefinite contract)

**Motivation filter:** Evaluate not just whether you *can* do the tasks, but whether the tasks will *energize* you. Consider:
- Tasks that energize: consultative outbound prospecting into new/underserved markets (especially Arabic-speaking), C-level and founder conversations, structured high-velocity sales motions, environments with visible recognition (leaderboards, mentor tracks, president's club)
- Tasks that drain: micromanagement, working with limited resources, chaotic/undefined internal systems, roles with no recognition for performance, rigid close-minded traditional workflows that block creative deal structuring
- Non-task factors: company average tenure (a deal-breaker signal - see below), leadership style (coaching over controlling), degree of autonomy within a structured framework

**Life situation alignment:** Consider personal constraints:
- **Security**: Currently employed (Strategic BDR, Google Cloud via Teleperformance) and performing well (160% to target) - can afford to be selective, not searching out of urgency
- **Flexibility**: Based in Barcelona; not open to relocating outside Spain for the next 4 years; open to relocating to another Spanish city if the employer covers relocation costs; open to full remote only with a Spanish indefinite contract or 70K+ OTE
- **Professional development**: Seeking the AE title/track upgrade from BDR, with uncapped earning upside as the primary growth priority

### Deal-Breakers (hard filters - apply before scoring)
Flag and stop evaluation (recommend skip regardless of score) if:
- **Average employee tenure under 2 years** at the company (signal of high turnover/churn - research via LinkedIn "how long employees stay" or Glassdoor)
- The company is complicit in, or provides material support for, actions against the Palestinian people, or is founded/funded by individuals with a public Zionist political stance
- The role would require relocating outside Spain within the next 4 years

### 6. Salary Benchmark (Optional)

If the salary lookup tool is configured (`salary_data.json` exists), look up the company:
```
python salary_lookup.py "<Company Name>" --json
```

If a city is known from the posting, add `--city "<City>"` to narrow results.

Present findings as:
```
### Salary Benchmark
| Metric | Value |
|--------|-------|
| [Category] index | XX.X (+/-X.X% vs baseline) |
| Overall index | XX.X (+/-X.X% vs baseline) |
```

Interpret results relative to the baseline defined in the data file's metadata. For index-based data, higher typically means above-market compensation.

If the salary tool is not configured, skip this section.

## Output Format

Present the evaluation as:

```
## Job Fit Evaluation: [Role] at [Company]

| Dimension | Score | Notes |
|-----------|-------|-------|
| Technical Skills | XX/100 | [brief note] |
| Experience Match | XX/100 | [brief note] |
| Behavioral Fit | XX/100 | [brief note] |
| Location | PASS/FAIL | [brief note] |
| Career Alignment | XX/100 | [brief note] |

**Overall Score: XX/100** (weighted average of scored dimensions)

### Verdict: [Strong Fit / Good Fit / Moderate Fit / Weak Fit / Poor Fit]

### Key Strengths for This Role
- [bullet points]

### Gaps to Address
- [bullet points]

### Recommendation
[1-2 sentences: apply/skip/apply with caveats]

### Company Research Checklist
- [ ] Checked company website (mission, values, recent news)
- [ ] Checked review sites (Glassdoor, Jobindex, etc.)
- [ ] Checked LinkedIn for team size, recent hires, connections
- [ ] Checked media for restructuring, growth, or workplace issues
- [ ] Identified network contacts who may know the team/manager
```

## Weighting
- Technical Skills: 30%
- Experience Match: 25%
- Behavioral Fit: 15%
- Career Alignment: 30%

(Location is pass/fail, not weighted)

## Thresholds
- **Strong Fit** (75+): Definitely apply, tailor everything
- **Good Fit** (60-74): Apply, address gaps in cover letter
- **Moderate Fit** (45-59): Consider carefully, discuss with user
- **Weak Fit** (30-44): Probably skip unless strategic reasons
- **Poor Fit** (<30): Skip

## Pre-Application: Call the Employer (Best Practice)

Before writing the application, consider whether the candidate should call the contact person listed in the posting. **Only call if there are substantive questions** - never call just to "be remembered."

### When to Suggest Calling
- The posting has unclear or ambiguous requirements
- It's unclear which competencies are essential vs. nice-to-have
- The role description is vague about day-to-day tasks
- There's a named contact person who invites questions

### Good Questions to Ask
- "What are the primary challenges in this role?"
- "How is time typically divided across the listed responsibilities?"
- "Which competencies are most critical for success in this position?"
- "What does success look like in the first 6-12 months?"

### Rules for the Call
- Prepare a 30-second "elevator pitch" about your background in case they ask
- The call's purpose is **gathering information**, not delivering a pitch
- Take notes - use what you learn to tailor the application
- Reference the conversation naturally in the cover letter ("After speaking with [name], I was especially drawn to...")
