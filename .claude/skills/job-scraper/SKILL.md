---
name: job-scraper
description: >
  Scrapes Danish job sites for new positions matching your profile. Deduplicates across runs.
  Triggers on: job scrape, find jobs, search jobs, new jobs, job search, scrape jobs, /scrape
allowed-tools: Read, Write, Edit, Glob, Grep, WebFetch, WebSearch, Agent, AskUserQuestion
---

# Job Scraper

---

## How It Works

This skill searches multiple Danish job sites using targeted queries based on your profile, deduplicates against previously seen jobs and the application tracker, and presents new matches with a quick fit assessment.

## Invocation

The user triggers this skill by saying things like:
- "Find new jobs"
- "Scrape for jobs"
- "Any new positions?"
- "/scrape"

Optional arguments:
- A focus area, e.g. "/scrape data science" or "/scrape geophysics"
- "broad" to run all search categories, e.g. "/scrape broad"

---

## Execution Steps

### Step 0: Load State

1. Read `job_scraper/seen_jobs.json` (create if missing - start with `{"seen": {}}`)
2. Read `job_search_tracker.csv` to extract already-applied companies+roles
3. Read `search-queries.md` (this directory) for the search strategy

### Step 1: Search

Run **WebSearch** queries from `search-queries.md`. By default, run the top 3 priority categories. If the user said "broad", run all categories.

If the user specified a focus area (e.g. "data science"), prioritize queries from that category.

For each search:
- Use `WebSearch` with site-specific queries (jobindex.dk, linkedin.com/jobs, karriere.dk, etc.)
- Target your configured geographic area
- Look for postings from the last 14 days

### Step 2: Fetch & Parse

For each promising result from Step 1:
- Use `WebFetch` to retrieve the job posting page
- Extract: **job title**, **company**, **location**, **posting date** (or "recent"), **URL**, **key requirements** (brief), **application deadline** (if listed)
- Skip if the URL or company+title combo already exists in `seen_jobs.json`
- Skip if the company+role already appears in `job_search_tracker.csv`

### Step 2.5: Validate Posting Date & Link (MANDATORY - never skip)

Major job boards (LinkedIn, Indeed, Michael Page, etc.) frequently re-index stale or expired postings, and aggregator search results do not reliably expose an accurate posting date. Job IDs on the same board can differ across re-crawls of the same underlying role, so a URL found via search is not proof the role is still open. Before a job is allowed into Step 5's results table:

1. **Prefer the company's own careers page / ATS** (Greenhouse, Ashby, Lever, Workday, or the company's own `careers.<company>.com`) over a job-board mirror (LinkedIn, Indeed, Jobsdb, Glassdoor, etc.) whenever both exist for the same role. Company ATS pages are far more likely to reflect true current state and are frequently fetchable when the board mirror is not.
2. **Attempt to confirm the posting is within the last 30 days.** Use `WebFetch` on the canonical URL if possible; if blocked, use targeted `WebSearch` queries that include the company + exact title + "posted" / date-range terms, and look for an explicit date in the result (not just aggregator counts like "500+ jobs").
3. **If multiple job-board IDs exist for what looks like the same role** (this happens often - the same req gets re-crawled and re-indexed under new IDs over time), do not assume the newest-looking ID is current just because the number is larger. Flag the ambiguity rather than guessing.
4. **If the posting date cannot be confirmed within 30 days from either the company ATS or a dated search result**, do not present it as a validated fresh match. Either:
   - Mark it `status: "needs_verification"` in `seen_jobs.json` and say so explicitly when presenting it (still include the link so the user can check), or
   - Drop it from the results entirely if there's a stronger fresh-dated candidate to present instead.
5. **Never present a job whose only evidence is an aggregator category page** (e.g. "1,000+ jobs in Singapore") as an individual match - that is not a specific posting.
6. This validation step applies before *every* time a job is returned to the user, not just on first discovery - if a job already in `seen_jobs.json` is being resurfaced or handed to `/apply`, re-validate it if it has been more than a few days since `first_seen`.

### Step 3: Quick Fit Assessment

For each new job, do a rapid fit check (NOT the full evaluation from `04-job-evaluation.md` - just a quick signal):

- **High match**: Role directly involves your core skills
- **Medium match**: Role is adjacent to your experience
- **Low match**: Role requires significant skills you lack

### Step 4: Deduplicate & Store

1. Add ALL fetched jobs (new and skipped) to `seen_jobs.json` with structure:
```json
{
  "seen": {
    "<url_or_company_title_key>": {
      "title": "...",
      "company": "...",
      "url": "...",
      "first_seen": "YYYY-MM-DD",
      "fit": "high/medium/low",
      "status": "new/skipped/evaluated"
    }
  }
}
```
2. Only present jobs NOT already in the seen list or tracker.

### Step 5: Present Results

Present new jobs in a table sorted by fit (high first):

```
## New Job Matches - YYYY-MM-DD

Found X new positions (Y high, Z medium, W low match).

| # | Fit | Title | Company | Location | Posted | Verified | Original Link |
|---|-----|-------|---------|----------|--------|----------|----------------|
| 1 | High | ... | ... | ... | within 30 days / unconfirmed | Yes / needs_verification | [Original Link](...) |

Always include the **original link** exactly as found (the specific canonical URL used to validate the posting per Step 2.5), not a generic search/category page. If a posting could not be verified within 30 days, say so plainly in the table rather than omitting the caveat.

### High-Match Highlights
For each high-match job, add 2-3 bullet points:
- Why it matches your profile
- Key requirements to check
- Any red flags
```

After presenting, ask:
> "Want me to evaluate any of these in detail? Just give me the number(s)."

If the user picks a number, invoke the **job-application-assistant** skill workflow (fit evaluation first, then CV + cover letter if approved).

### Step 6: Update Tracker (Optional)

If the user decides to apply to any job, add a row to `job_search_tracker.csv`.

---

## Important Rules

1. **Never fabricate job postings.** Only present jobs found via actual WebSearch/WebFetch results.
2. **Respect deduplication.** Always check seen_jobs.json AND job_search_tracker.csv before presenting.
3. **Focus on configured geographic area.** Skip jobs that require relocation or are clearly outside commute range.
4. **Only open positions.** Skip postings with expired deadlines or those marked as closed.
5. **Be efficient with WebFetch.** Don't fetch every search result - use titles and snippets to pre-filter before fetching.
6. **Parallel searches.** Use the Agent tool or parallel WebSearch calls to speed up the search phase.
7. **Validate before presenting (MANDATORY, see Step 2.5).** Confirm each job's posting date is within the last 30 days and that the link is the canonical/current posting before it appears in results. If validation isn't possible, say so explicitly rather than presenting it as a confirmed fresh match.
8. **Always show the original link.** Every result the user sees must include the exact canonical URL used to validate it, not a generic search or category page.
