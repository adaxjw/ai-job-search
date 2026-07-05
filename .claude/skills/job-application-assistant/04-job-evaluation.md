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

**Strong match areas:** Corporate/regional strategy, GTM strategy and execution, business transformation, C-suite and executive committee advisory, EV/mobility strategy, sustainability strategy, pricing and packaging, cross-entity/JV alignment, AI-enabled GTM
**Moderate match areas:** Program/product management, innovation incubation and scaling, general management exposure (without formal P&L title), Python/SAP-adjacent technical fluency
**Weak match areas:** Hands-on technical/engineering execution roles (software development, data engineering), roles requiring an already-held formal P&L or general-management title

### 2. Experience Match (0-100)
Does work history align with what they're looking for?

| Score | Meaning |
|-------|---------|
| 80-100 | Direct experience in the same domain and role type |
| 60-79 | Related experience, transferable skills clear |
| 40-59 | Adjacent experience, would need to make the case |
| 0-39 | Unrelated experience |

**Strong:** Corporate strategy, GTM, C-suite/executive committee advisory (consulting, global automotive, AI-driven knowledge business sectors)
**Moderate:** General management / P&L ownership — has led $180M+ delivery scope and driven commercial performance, but has not yet held a formal P&L or general-management title
**Entry-level:** N/A

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
- Within commute range, or relocation to HK / EU / UK / US / AU / SG: PASS
- Remote with occasional office: PASS, but flag if the role is structurally isolated from where decisions get made (see below)
- Requires relocation outside HK / EU / UK / US / AU / SG: FAIL (deal-breaker), unless the user says otherwise
- Frequent international travel: FLAG (discuss with user)
- **Structurally isolated roles** (physically local but organizationally remote from the decision-making centre — e.g., a satellite office with no path to leadership access): FAIL/FLAG even if geographically convenient. This is a deal-breaker independent of commute distance.

### 5. Career Alignment & Motivation (0-100)
Does this role advance career goals and contain tasks that energize?

| Score | Meaning |
|-------|---------|
| 80-100 | Strongly aligned with career direction, clear growth path |
| 60-79 | Good role but only partially aligned with long-term goals |
| 40-59 | Decent job but doesn't build toward career goals |
| 0-39 | Dead end or backwards step |

**Career goals:**
- Move from advisory scope into a role with real decision rights and design authority, not pure advisory/execution
- Secure a seat at the leadership table with active sponsorship from senior leadership
- Build toward eventual formal P&L or general-management ownership (current delivery scope — $180M+ renewal base, ~$30M portfolio — should be reframed as commercial accountability, not just delivery, to help close this gap)
- Build commercial credibility in salaried roles as a stepping stone toward an eventual independent well-being/lifestyle venture

**Motivation filter:** Evaluate not just whether you *can* do the tasks, but whether the tasks will *energize* you. Consider:
- Tasks that energize: ambiguous 0-to-1 strategy work, cross-border/cross-cultural bridging, AI-enabled GTM, sustainability strategy design, building things from scratch
- Tasks that drain: pure execution with no design authority, rigid process/hierarchy-bound work, structurally isolated roles (remote from the decision-making centre)
- Non-task factors: active executive sponsorship, genuine decision rights, fast-moving/innovation-friendly culture over rigid hierarchy

**Life situation alignment:** Consider personal constraints:
- **Security**: Currently employed (Elsevier); can afford to be selective rather than taking the first offer
- **Flexibility**: Open to relocation across HK / EU / UK / US / AU / SG
- **Professional development**: Prioritizes roles that build commercial/P&L credibility toward a long-term independent venture, over roles that are a lateral or backward step in decision authority

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
