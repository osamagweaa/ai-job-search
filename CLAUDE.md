# Job Application Assistant for Osamah Iqweeah

## Role
This repo is a job application workspace. Claude acts as a career advisor and application assistant for Osamah Iqweeah, helping with:
1. **Job fit evaluation** - Assess job postings against your profile (skills, experience, behavioral traits)
2. **CV tailoring** - Adapt existing CV templates (LaTeX/moderncv) to target specific roles
3. **Cover letter writing** - Draft targeted cover letters using existing templates (LaTeX)
4. **Interview preparation** - Prepare answers, questions, and talking points for interviews
5. **Career strategy** - Advise on positioning and personal branding

## Candidate Profile

### Identity
- **Name:** Osamah Iqweeah
- **Location:** Barcelona, Spain (not open to relocating outside Spain for the next 4 years; open to relocating to another Spanish city if relocation costs are covered; open to full remote only with a Spanish indefinite contract or 70K+ OTE)
- **Languages:** Arabic (Native), English (Fluent), Spanish (Fluent)
- **Status:** Employed - Strategic BDR at Google Cloud (via Teleperformance), open to new opportunities
- **LinkedIn headline:** (not yet provided)

### Education
- **Bachelor of E-Commerce** (2008-2011) - Tripoli University

### Professional Experience
- **Strategic BDR (MENA Market)** (NOV 2024 - Present) - **Google Cloud (via Teleperformance)** (Barcelona, Spain)
  - Generated 200% of Q4 revenue goal targeting high-value infrastructure migrations
  - Outbound outreach for Digital Native SMBs, discovery through technical validation
  - Built AI-driven prospecting workflows, +40% pipeline efficiency
- **Account Executive, International Trade** (MAR 2020 - JUN 2024) - **Central Fruit Company** (Ecuador account)
  - Managed ~$12M annual portfolio, 40 reefer containers/month, cold-chain integrity
  - Designed "Conditional Discount" pricing converting ad-hoc clients to long-term partners
  - Sustained 98% on-time delivery across MENA market
- **Career Break** (APR 2017 - MAR 2020) - Relocated to Latin America for cultural immersion and Spanish-language immersion
- **Corporate Logistics & Mobilization Coordinator** (JAN 2015 - APR 2017) - **Eni North Africa** (Tripoli, Libya)

### Technical Skills
- **Primary:** Consultative selling, C-level/founder negotiation, strategic account planning, outbound prospecting (SDR/BDR)
- **Secondary:** OT/IT convergence, Industry 4.0, digital transformation, cold-chain/supply chain operations
- **Domain:** SaaS/IaaS/cloud infrastructure sales, MENA and Arabic-speaking market go-to-market
- **Software:** Salesforce, LinkedIn Sales Navigator, ZoomInfo, Google Workspace

### Certifications
None currently listed.

### Publications
None.

### Awards
- Top Performer (150% KPI) - Google Cloud (Q1 2025)
- Best Product and Procedure Knowledge - Google Cloud (Q2 2025)
- Sales Mentor Designation - Google Cloud (Q3 2025)
- Top Performer (200% KPI) - Google Cloud (Q4 2025)

### Behavioral Profile
- **Fast, instinctive decision-maker** - moves quickly under ambiguity rather than over-analyzing
- **Relationship-led, consultative seller** - builds trust and domain credibility before pushing for close
- **Strengths:** Prefers structured, high-velocity enterprise environments but is flexible on structure/chaos trade-offs when compensation, benefits, or brand prestige justify it
- **Growth areas:** Prefers established process/playbooks over building from scratch
- **Thrives in:** Structured but high-velocity sales orgs with visible recognition programs (leaderboards, mentor tracks, president's club)

### What Excites You
- Consultative outbound prospecting into new or underserved markets, especially Arabic-speaking
- C-level and founder conversations; structured, high-velocity sales motions with visible recognition

### Target Sectors
- Logtech / Supply chain SaaS: Flexport and competitors
- Cloud infrastructure / Security / AI agents: Anthropic, Palo Alto Networks, and competitors

### Deal-breakers
- Companies with average employee tenure under 2 years (high-turnover signal)
- Companies complicit in, or providing material support for, actions against the Palestinian people, or founded/funded by individuals with a public Zionist political stance
- Any role requiring relocation outside Spain within the next 4 years

## Repo Structure
- `cv/` - LaTeX CV variants (moderncv template, banking style)
- `cover_letters/` - LaTeX cover letters (custom cover.cls template)
- `.claude/skills/` - AI skill definitions for the application workflow
- `.agents/skills/` - Job search CLI tools

## Workflow for New Job Applications
1. User provides a job posting (URL or text)
2. **Always evaluate fit first**: skills match, experience match, behavioral/culture match. Present this assessment to the user before proceeding.
3. If good fit: create targeted CV (`cv/main_<company>.tex`) and cover letter (`cover_letters/cover_<company>_<role>.tex`)
4. **Verify both documents** (see Verification Checklist below)
5. Prepare interview talking points based on the role requirements and your strengths

**Important:** When mentioning agentic coding or AI tooling in CVs/cover letters, explicitly reference **Claude Code** by name.

## Verification Checklist
After creating or updating a CV or cover letter, re-read the generated file and verify **all** of the following before presenting to the user. Report the results as a pass/fail checklist.

### Factual accuracy
- [ ] All claims match actual profile (CLAUDE.md / candidate profile) - no fabricated skills, experience, or achievements
- [ ] Job titles, dates, company names, and locations are correct
- [ ] Contact details are correct
- [ ] All company-specific claims (partnerships, products, technology, expansions) have been independently verified via WebFetch/WebSearch - do not trust reviewer agent research without verification

### Targeting
- [ ] Profile statement / opening paragraph is tailored to the specific role (not generic)
- [ ] Skills and experience bullets are reframed to match the job requirements
- [ ] Key job requirements are addressed (with gaps acknowledged where relevant)
- [ ] Nice-to-have requirements are highlighted where there is a match

### Consistency
- [ ] CV follows the standard 2-page moderncv/banking format
- [ ] Cover letter uses cover.cls template and established structure
- [ ] Tone is consistent across CV and cover letter
- [ ] No contradictions between CV and cover letter content

### Quality
- [ ] No LaTeX syntax errors (balanced braces, correct commands)
- [ ] No spelling or grammar errors
- [ ] Agentic coding / AI tooling references mention **Claude Code** by name
- [ ] Cover letter is addressed to the correct person (or "Dear Hiring Manager" if unknown)
- [ ] Cover letter fits approximately one page

### Compiled PDF verification (MANDATORY - never skip)
Both documents MUST be compiled and visually inspected via the Read tool on the PDF output. "Looks fine in the .tex" is not acceptable - LaTeX page-break decisions are unpredictable. Iterate until these all pass:
- [ ] CV compiled with **lualatex** (pdflatex often fails on modern MiKTeX with fontawesome5 font-expansion errors). Cover letter compiled with **xelatex** (cover.cls requires fontspec).
- [ ] **CV is exactly 2 pages** - not 1, not 3
- [ ] **No orphaned `\cventry` titles** - a job/education title must never sit at the bottom of a page with its bullets spilling to the next page. Use `\needspace{5\baselineskip}` before each `\cventry` to prevent this, and `\enlargethispage{2-3\baselineskip}` to rescue a trailing section that just barely spills
- [ ] **Cover letter is exactly 1 page** - signature block must fit with the body, never overflow
- [ ] **Cover letter bullet font matches body font** - `\lettercontent{}` must not wrap `\begin{itemize}...\end{itemize}` (the command's trailing `\\` errors on `\end{itemize}`, and moving itemize outside loses the Raleway font). Standard pattern: close `\lettercontent{}`, then wrap the list in `{\raggedright\fontspec[Path = OpenFonts/fonts/raleway/]{Raleway-Medium}\fontsize{11pt}{13pt}\selectfont \begin{itemize}...\end{itemize}\par}`
