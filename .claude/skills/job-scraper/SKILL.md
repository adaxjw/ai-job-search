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

**House rule (locked, do not silently narrow this):** every `/scrape` run always covers all five tracked focus areas - **strategy, chief of staff, GTM, sustainability, corporate development.** These map onto `search-queries.md`'s Priority 1 (Strategy & Chief of Staff titles), Priority 2 (GTM & sustainability domain), and Priority 3 (corporate development & adjacent roles) - run all three every time, not just a "top 3" subset. Priority 4 (broader consulting net) only runs if the user explicitly says "broad" or names that focus.

If the user specifies a narrower focus area in the invocation (e.g. "/scrape sustainability"), still run all five tracked areas, but prioritize and lead with the named one.

For each search:
- Use `WebSearch` with site-specific queries (jobindex.dk, linkedin.com/jobs, karriere.dk, etc.)
- Target your configured geographic area
- Look for postings from the last 14 days as an initial pre-filter - this is a loose net, not the final validation. Final inclusion in results is gated by Step 2.5's 30-day check, which requires actual confirmed evidence, not just an initial guess.

### Step 2: Fetch & Parse

For each promising result from Step 1:
- Use `WebFetch` to retrieve the job posting page
- Extract: **job title**, **company**, **location**, **posting date** (or "recent"), **URL**, **key requirements** (brief), **application deadline** (if listed)
- Skip if the URL or company+title combo already exists in `seen_jobs.json`
- Skip if the company+role already appears in `job_search_tracker.csv`

### Step 2.5: Validate Posting Date & Link (HARD GATE - house rule, locked)

**This is a hard filter, not a soft flag.** A job with no confirmed posting date within the last 30 days does not appear in Step 5's results table. Do not show it "with a caveat" - exclude it. This rule exists because three separate incidents got past a softer version of this check: an HSBC posting ~4 years expired, an Airwallex posting ~3 years stale, and a Fidelity International posting ~1 year old and closed - all initially presented as live matches.

**Calibration note:** LinkedIn job IDs are a weak, unreliable signal of recency (they are global and sequential across all LinkedIn postings, not per-market), but as data points accumulate they're useful for sanity-checking: as of 2026-07, an ID around ~4.24 billion (e.g. `4242593394`) was already confirmed ~1 year stale. Treat ID magnitude only as a rough prior, never as confirmation - always require actual date evidence per below.

1. **Prefer the company's own careers page / ATS** (Greenhouse, Ashby, Lever, Workday, or the company's own `careers.<company>.com`) over a job-board mirror (LinkedIn, Indeed, Jobsdb, Glassdoor, etc.) whenever both exist for the same role. Company ATS pages are far more likely to reflect true current state and are occasionally fetchable when the board mirror is not.
2. **Require positive date evidence, not absence of evidence to the contrary.** Acceptable evidence: an explicit relative-date string ("Posted 3 days ago", "2w ago", "Xh"), an explicit calendar date attached to the specific listing (not an aggregator's "as of [date]" scrape timestamp), or a successful `WebFetch` of the canonical page showing the listing is current. Try `WebFetch` on the canonical URL first; if blocked (expect frequent 403s in this environment), fall back to `WebSearch` queries that combine the company + exact title + terms like "posted" / "ago" / "new" to try to surface a dated snippet.
3. **If multiple job-board IDs exist for what looks like the same underlying role** (common - the same req gets re-crawled and re-indexed under new IDs over time), do not assume the newest-looking ID is current just because the number is larger. Treat all of them as unconfirmed until one produces real date evidence.
4. **No confirmed date within 30 days -> exclude from Step 5 entirely.** Record it in `seen_jobs.json` with `status: "excluded_unverified"` (not `"new"`) so it isn't re-fetched and re-attempted every run, but do not put it in the user-facing results table. It's fine - expected, even - for a run to surface fewer results, or zero, rather than presenting unverified listings.
5. **Never present a job whose only evidence is an aggregator category page** (e.g. "1,000+ jobs in Singapore") as an individual match - that is not a specific posting and never will be, regardless of date.
6. **Re-validate on every resurfacing, not just first discovery.** If a job already in `seen_jobs.json` is being resurfaced in a later `/scrape` run or handed to `/apply`, re-run this check - a listing confirmed fresh two weeks ago may be stale now.
7. If the user wants to see the excluded/unverified candidates anyway (e.g. to manually check a promising lead), that's fine to share on request - just never put them in the default results table unlabeled as validated.

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
      "status": "new/skipped/evaluated/excluded_unverified/closed"
    }
  }
}
```
2. Only present jobs NOT already in the seen list or tracker, AND that passed Step 2.5's validation gate (`status` other than `excluded_unverified`, `skipped`, or `closed`).

### Step 5: Present Results

Present new jobs in a table sorted by fit (high first):

```
## New Job Matches - YYYY-MM-DD

Found X positions confirmed posted within the last 30 days (Y high, Z medium, W low match).
[If applicable: Also found N candidates that could not be date-verified and were excluded - available on request.]

| # | Fit | Title | Company | Location | Confirmed Posted | Original Link |
|---|-----|-------|---------|----------|-------------------|----------------|
| 1 | High | ... | ... | ... | e.g. "8 days ago" / "2026-06-28" | [Original Link](...) |

Every row in this table has already passed the Step 2.5 hard gate - do not include a "Verified: needs_verification" row here; those jobs are excluded from the table entirely (see Step 2.5). Always show the **original link** exactly as found (the specific canonical URL used to validate the posting), not a generic search/category page.

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
7. **30-day validation is a hard gate, locked house rule (see Step 2.5).** A job without confirmed positive date evidence within the last 30 days is excluded from results entirely - not shown with a caveat, not shown as "needs_verification." A run that surfaces zero validated jobs is an acceptable, expected outcome in an environment where WebFetch is frequently blocked; it is never acceptable to present an unverified listing as if it were confirmed.
8. **Always show the original link.** Every result the user sees must include the exact canonical URL used to validate it, not a generic search or category page.
9. **Always cover all five tracked focus areas every run (locked house rule, see Step 1).** Strategy, chief of staff, GTM, sustainability, corporate development - run Priority 1-3 from `search-queries.md` every time, not a narrowed "top 3" subset.
