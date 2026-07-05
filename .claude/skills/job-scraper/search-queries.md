# Search Queries for Job Scraper

Ada's search is multi-region (HK / EU / UK / US / AU / SG) and sector-agnostic — she is
filtering by role, seniority, and fit criteria (see `04-job-evaluation.md`), not by
industry. The Danish CLI tools in `.agents/skills/` do not apply here.

## Search Tools

Primary:
- **`linkedin-search`** (`.agents/skills/linkedin-search/`) - country-agnostic LinkedIn job search CLI. Pass `--location` per region below and `--query` per role title. Personal use only, keep volume low.
- **Targeted WebSearch** - `site:linkedin.com/jobs`, `site:` company career pages, and general searches for named companies/sectors.

Secondary:
- Direct Google/WebSearch queries with `site:` filters for specific target companies as they come up.

## Query Categories

Queries are grouped by priority. Combine each role title with the region locations under "Location Filter" below - run the linkedin-search CLI once per region per priority-1/2 title, and use WebSearch for the rest.

### Priority 1: Primary Target Roles (sector-agnostic)

Strongest and most desired career direction - run across all regions.

```
bun run skills/linkedin-search/cli/src/cli.ts search -q "Director of Strategy" -l "<region>" --jobage 14 --format table
bun run skills/linkedin-search/cli/src/cli.ts search -q "Head of Strategy" -l "<region>" --jobage 14 --format table
bun run skills/linkedin-search/cli/src/cli.ts search -q "VP Strategy & Transformation" -l "<region>" --jobage 14 --format table
bun run skills/linkedin-search/cli/src/cli.ts search -q "Chief of Staff" -l "<region>" --jobage 14 --format table
bun run skills/linkedin-search/cli/src/cli.ts search -q "Director of Corporate Strategy" -l "<region>" --jobage 14 --format table
bun run skills/linkedin-search/cli/src/cli.ts search -q "Head of Corporate Strategy" -l "<region>" --jobage 14 --format table
site:linkedin.com/jobs "Chief of Staff" OR "VP Strategy" <region>
```

### Priority 2: Domain Expertise (GTM, EV/mobility, sustainability, AI-enabled GTM)

Match her deepest domain expertise - widens beyond title matches to keyword matches.

```
bun run skills/linkedin-search/cli/src/cli.ts search -q "Head of GTM" -l "<region>" --jobage 14 --format table
bun run skills/linkedin-search/cli/src/cli.ts search -q "EV strategy" -l "<region>" --jobage 14 --format table
bun run skills/linkedin-search/cli/src/cli.ts search -q "sustainability strategy director" -l "<region>" --jobage 14 --format table
site:linkedin.com/jobs "AI GTM" OR "AI-enabled go-to-market" strategy <region>
site:linkedin.com/jobs "corporate development" automotive OR mobility OR publishing <region>
```

### Priority 3: Adjacent Roles

Roles she could pivot into that use the same skill set.

```
bun run skills/linkedin-search/cli/src/cli.ts search -q "Head of Corporate Development" -l "<region>" --jobage 14 --format table
bun run skills/linkedin-search/cli/src/cli.ts search -q "General Manager" -l "<region>" --jobage 14 --format table
bun run skills/linkedin-search/cli/src/cli.ts search -q "Country Manager" -l "<region>" --jobage 14 --format table
bun run skills/linkedin-search/cli/src/cli.ts search -q "Head of Transformation" -l "<region>" --jobage 14 --format table
```

### Priority 4: Broader Net (Strategy Consulting)

Wider net - senior consulting/advisory roles that keep the door open to future ownership tracks.

```
bun run skills/linkedin-search/cli/src/cli.ts search -q "Principal" strategy consulting -l "<region>" --jobage 14 --format table
bun run skills/linkedin-search/cli/src/cli.ts search -q "Strategic Advisor" -l "<region>" --jobage 14 --format table
site:linkedin.com/jobs "special projects" OR "chief of staff" AI OR mobility OR sustainability <region>
```

## Location Filter

Sector-agnostic, multi-region search. Substitute `<region>` above with each of:
- **Ideal (home base, no relocation):** "Hong Kong", "Beijing, China"
- **Acceptable (open to relocation):** "Singapore", "London, United Kingdom", "Paris, France", "Berlin, Germany", "Sydney, Australia", "Remote"
- **Borderline:** other US/EU hubs not listed above - include if the role is otherwise a strong fit, flag visa/relocation complexity in the evaluation
- **Too far / excluded:** none by geography alone, but see the deal-breaker below

**Deal-breaker independent of geography:** flag any role that is structurally isolated from the decision-making centre (e.g., a satellite office with no path to leadership access), per `04-job-evaluation.md`. This applies even to postings physically inside the "ideal" or "acceptable" tiers above.

## Compensation Baseline

Negotiable; minimum ~HKD 1,000,000 base plus bonus and a relocation package for roles outside her current city. Use this only as a soft screen - do not reject a strong-fit role over compensation before discussing with Ada.

## Date Filter

Only include jobs posted within the last 14 days, or with an application deadline that has not yet passed. If a posting date cannot be determined, include it but flag as "date unknown".

## Adapting Queries

If the user specifies a focus area, select queries from the matching category and also generate 2-3 custom queries for that focus. For example:
- "/scrape sustainability" -> Priority 2 queries + custom sustainability-strategy queries
- "/scrape broad" -> run all four priority categories across all regions
