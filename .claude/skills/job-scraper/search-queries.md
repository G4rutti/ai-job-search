# Search Queries for Job Scraper

<!-- SETUP: Customize these queries based on your skills, target roles, and location -->

## Installed portal CLIs (primary for `/scrape`)

`/scrape` discovers every portal skill under `.agents/skills/*/SKILL.md` and runs its CLI first. Shipped country-agnostic CLIs include `linkedin-search` and `freehire-search`; Danish demos and any skill you add with `/add-portal` are included the same way. You do **not** need a matching `site:` line below for those CLIs to run.

The `site:` query templates in this file are the **WebSearch fallback** — for portals without a CLI, company career pages, or when a CLI fails.

**Language scope:** write every query category in every language listed in your CLAUDE.md Languages table (typically 1-2, sometimes more). A posting requiring a language you have *not* declared, as a job condition, is excluded before scoring; a posting requiring a *higher level* than you declared in a language you *do* work in is flagged for your own judgment, not excluded — see `04-job-evaluation.md`'s Language Gate, the single source of truth for this rule. Translate each category's keywords rather than machine-translating word-for-word (e.g. "Frontend Developer" -> "Desarrollador Frontend", not a literal word-for-word translation) if you work in more than one language.

## Search Sites

Primary (your market's job boards - scaffold one with `/add-portal`):
- **linkedin.com/jobs** - LinkedIn job listings (filter: Brazil, remote); also covered by `linkedin-search` CLI
- **freehire.com** - remote-first roles; also covered by `freehire-search` CLI
- **Gupy, Catho, Indeed Brasil, InfoJobs** - Brazil's major general job boards. No CLI skill installed yet for these; use `site:` WebSearch fallback below, or scaffold one with `/add-portal` if search volume justifies it.
- Danish portal demos (Jobindex, Jobbank, Jobdanmark, Jobnet) ship with this framework but are **disabled** - not relevant to the Brazilian market, left off.

Secondary (company career pages via Google):
- Direct Google searches with `site:` filters for known target companies (none specified yet - open search)

## Query Categories

Queries are grouped by priority. Write **each category in every language from your Languages table** (see Language scope above). Combine each query with your location terms (e.g. your city, region, or metro area) where the site supports it.

### Priority 1: Full Stack Developer / Software Engineer

These match your strongest and most desired career direction.

```
site:linkedin.com/jobs "Full Stack Developer" remote Brasil
site:linkedin.com/jobs "Desenvolvedor Full Stack" remoto
site:linkedin.com/jobs "Software Engineer" remote Brazil
"Full Stack Developer" React.js Python remoto vaga
"Desenvolvedor(a) Full Stack" React Next.js remoto
```

### Priority 2: AI Engineer / LLM Engineer / Prompt Engineering

These match your domain expertise and fastest-growing career direction.

```
site:linkedin.com/jobs "AI Engineer" remote Brazil
site:linkedin.com/jobs "LLM Engineer" OR "Prompt Engineer" remote
"Engenheiro(a) de IA" OR "Engenheiro(a) de Prompt" remoto vaga
"AI Engineer" RAG LangChain remote
"Desenvolvedor(a) IA" RAG LLM remoto
```

### Priority 3: Front End Developer / Back End Developer

Adjacent roles that isolate one side of the stack.

```
site:linkedin.com/jobs "Front End Developer" React.js remote Brazil
site:linkedin.com/jobs "Desenvolvedor(a) Front-end" React remoto
site:linkedin.com/jobs "Back End Developer" Python FastAPI remote
site:linkedin.com/jobs "Desenvolvedor(a) Back-end" Python remoto
```

### Priority 4: Broader Technical

Wider net for general developer roles matching key skills.

```
site:linkedin.com/jobs "React.js" developer remote Brazil
site:linkedin.com/jobs "TypeScript" developer remote Brasil
"desenvolvedor" React Next.js TypeScript remoto CLT
"developer" RAG LangChain LLM remote
```

## Location Filter

Candidate is remote-only (see CLAUDE.md Constraints). Define acceptable areas for this search:
- Fully remote roles based anywhere in Brazil (ideal)
- Fully remote roles based anywhere in the world, requiring English at or below the candidate's C1 level (acceptable)
- Hybrid roles physically located in Rio de Janeiro or São Paulo state (borderline - only worth flagging, not auto-applying; confirm with candidate before drafting)
- Any onsite/hybrid role requiring presence in Brazil's Northeast or North regions, or requiring relocation (too far - excluded per Deal-breakers)

## Language Filter

Your working languages and levels are in CLAUDE.md's Languages table. When filtering scraped results, apply `04-job-evaluation.md`'s Language Gate: a posting requiring a language you haven't declared at all is excluded; a posting requiring a higher level than you declared in a language you do work in is not excluded, flag it clearly instead (see `job-scraper/SKILL.md`'s Step 3 "Quick Fit Assessment" for how the flag surfaces in `/scrape` output). Postings simply *written* in a language you don't work in, that don't require it on the job, are fine.

## Date Filter

Only include jobs posted within the last 14 days, or with an application deadline that has not yet passed. If a posting date cannot be determined, include it but flag as "date unknown".

## Adapting Queries

If the user specifies a focus area, select queries from the matching category and also generate 2-3 custom queries for that focus. For example:
- "/scrape [focus_area]" -> relevant category queries + custom focus-specific queries
