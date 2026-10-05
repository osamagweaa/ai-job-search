# Search Queries for Job Scraper

## Search Sites

Osamah is based in Barcelona, Spain - the built-in Danish job-portal CLI tools (Jobindex, Jobbank, Jobdanmark, Jobnet) are not relevant here and should be skipped. Use instead:

Primary:
- **linkedin.com/jobs** - filter to Spain / Barcelona / Remote-Europe
- Direct company career pages for target companies (Anthropic, Palo Alto Networks, Flexport, and competitors) via Google `site:` searches
- **wellfound.com** (formerly AngelList Talent) - strong for AI/startup sales roles
- **builtin.com** - tech company sales roles, filterable by city/remote

Secondary:
- General Google searches combining role + market + location terms

## Query Categories

### Priority 1: Account Executive / Senior SDR, Cloud Infra & AI

Osamah's strongest and most desired direction: AE-track roles at cloud infrastructure, security, or AI agent companies, ideally selling into the Arabic-speaking/MENA market.

```
site:linkedin.com/jobs "Account Executive" "Arabic" (Barcelona OR Spain OR Remote)
site:linkedin.com/jobs "Senior SDR" OR "Strategic BDR" "MENA" (Barcelona OR remote)
site:linkedin.com/jobs "Account Executive" "SMB" OR "Mid-Market" Arabic Spain
"Account Executive" Arabic market Anthropic OR "Palo Alto Networks" OR Flexport
```

### Priority 2: Logtech / Supply Chain SaaS

Domain expertise from Central Fruit (cold-chain/international trade) and Eni (energy logistics).

```
site:linkedin.com/jobs "Account Executive" logistics OR "supply chain" SaaS Spain
site:linkedin.com/jobs "Account Executive" Flexport OR "supply chain SaaS" Arabic
"Account Executive" supply chain SaaS Barcelona OR remote Europe
```

### Priority 3: Adjacent Roles - Security / Cloud Infra Broader

Adjacent role types Osamah could pivot into beyond the exact AE title.

```
site:linkedin.com/jobs "Enterprise SDR" OR "Business Development Representative" cloud security Arabic
site:linkedin.com/jobs "Sales Development Representative" AI agent Barcelona OR remote
"Strategic Account Executive" cybersecurity OR "cloud security" MENA
```

### Priority 4: Broader Sales / Wider Net

Wider net across tech sales roles matching the bilingual Arabic/Spanish/English profile.

```
site:linkedin.com/jobs "Account Executive" bilingual Arabic Spain
site:linkedin.com/jobs "Business Development" MENA Barcelona
"Account Executive" OR "Senior SDR" uncapped OTE Spain OR remote Europe
```

## Location Filter

- **Ideal:** Barcelona, Spain (no relocation needed)
- **Acceptable:** Any other Spanish city, if the employer covers relocation costs
- **Borderline:** Fully remote roles based elsewhere in Spain/EU, only if they offer a Spanish indefinite contract OR an OTE of 70K+
- **Too far / excluded:** Any role requiring relocation outside Spain within the next 4 years - hard deal-breaker, exclude regardless of other fit

## Deal-Breaker Screen (apply during scraping, not just evaluation)

Flag or exclude postings/companies where discoverable signals suggest:
- Average employee tenure under 2 years (check LinkedIn "insights" tab or Glassdoor if accessible)
- Public involvement in or funding tied to actions against the Palestinian people, or founders with a public Zionist political stance

## Date Filter

Only include jobs posted within the last 14 days, or with an application deadline that has not yet passed. If a posting date cannot be determined, include it but flag as "date unknown".

## Adapting Queries

If the user specifies a focus area, select queries from the matching category and also generate 2-3 custom queries for that focus. For example:
- "/scrape flexport" -> Priority 2 queries + custom Flexport-specific searches
- "/scrape anthropic" -> Priority 1 queries + custom Anthropic-specific searches
