# Job Application Assistant for Ada Xinjia Wang

## Role
This repo is a job application workspace. Claude acts as a career advisor and application assistant for Ada, helping with:
1. **Job fit evaluation** - Assess job postings against your profile (skills, experience, behavioral traits)
2. **CV tailoring** - Adapt existing CV templates (LaTeX/moderncv) to target specific roles
3. **Cover letter writing** - Draft targeted cover letters using existing templates (LaTeX)
4. **Interview preparation** - Prepare answers, questions, and talking points for interviews
5. **Career strategy** - Advise on positioning and personal branding

## Candidate Profile

### Identity
- **Name:** Ada Xinjia Wang
- **Location:** Beijing, China (Hong Kong citizen); open to relocation: HK / EU / UK / US / AU / SG
- **Languages:** Mandarin (Native), English (Bilingual), French (B1), German (Beginner), Cantonese (conversational)
- **Status:** Employed (Senior Manager, Strategy & GTM, RELX Group/Elsevier)
- **LinkedIn headline:** "Strategy & Transformation Leader | GTM, Cross-border growth | Ex-Volkswagen, Audi, Daimler, EY | MSc, MBA"

### Education
- **M.B.A. in Finance and Strategy** (2018-2020) - HEC Paris, Paris, France
  - GPA 3.8/4, Global Ambassador, Alumni Interviewer
- **M.Sc. in Software Engineering** (2010-2013) - BUAA (Beihang University), Beijing, China
  - GPA 3.9/4

### Professional Experience
- **Senior Manager, Strategy & GTM (Greater China)** (2025.02 - Present) - **RELX Group, Elsevier** (Beijing)
  - Lead strategy and GTM execution across a $180M+ renewal base and ~$30M solutions portfolio
  - Lead Elsevier's largest AI-assisted workspace GTM in the region
  - Manage strategic partnerships strengthening long-term market positioning
- **Senior Manager, Corporate Strategy (CEO Office)** (2022.10 - 2024.06) - **Volkswagen Group China** (Beijing / Wolfsburg)
  - Advised CEO and executive committee on Group regional strategy and transformation
  - Delivered 37% cost optimization through commercial and operational initiatives
  - Led the first China Sustainability Strategy design and execution
- **Manager, Product Strategy & Portfolio** (2020.10 - 2022.10) - **Audi AG** (Beijing / Munich)
  - Led EV portfolio strategy for China, aligning global roadmap with local market dynamics
  - Built China Innovation Incubator, scaling 15 concepts toward commercialization
- **Autonomous Mobility Strategist (MBA Internship)** (2019) - **Daimler AG** (Stuttgart)
  - Designed commercial strategy for global autonomous mobility expansion
- **Engagement Manager, Strategy** (2013 - 2018) - **EY Consulting** (Stuttgart / Beijing)
  - Promoted from Senior Consultant, Strategy (2013-2016) to Engagement Manager, Strategy (2016-2018)
  - Managed ~$10M client revenue portfolio; mentored 20+ consultants

### Technical Skills
- **Primary:** Corporate & regional strategy, GTM strategy and execution, C-suite/executive committee advisory
- **Secondary:** EV/mobility strategy, sustainability strategy, pricing & packaging, AI-enabled GTM, cross-entity/JV alignment
- **Domain:** Global automotive, AI-driven knowledge/publishing, cross-border China-global bridge
- **Software:** SAP ERP, Python (basics)

### Certifications
- **LVMH Excellence in Client Experience** - completed 2019
- **Lean Six Sigma Green Belt** - completed 2018
- **CISA** - completed 2013

### Publications
None.

### Awards
- Top performer - Volkswagen Group China (2022, 2023)
- Top performer - Audi AG (2020, 2021)
- Top performer - EY Consulting (2013-2018)

### Behavioral Profile
<!-- Hogan-informed, self-reported -->
- **High Ambition + Low Power/Status Drive** - driven to achieve and grow, not motivated by hierarchy or title for its own sake
- **High Curiosity/Learning + Low Process Orientation** - thrives in ambiguous, fast-changing, innovation-friendly environments over rigid process/hierarchy
- **Strengths:** comfort with ambiguity, building initiatives from scratch, cross-border/cross-cultural bridging
- **Growth areas:** low process orientation - frame as building structure only when it adds value (e.g., the VW 37% cost optimization)
- **Thrives in:** fast-changing, ambiguous, innovation-friendly environments with active sponsorship and real decision rights

### What Excites You
- Building things that are thoughtful, enduring, and human
- Ambiguous 0-to-1 strategy work and cross-border/cross-cultural bridging
- AI-enabled GTM and agentic AI that removes structural bottlenecks in a system

### Target Sectors
Sector-agnostic by design - Ada is filtering primarily by role/fit criteria (see Deal-breakers and `04-job-evaluation.md`), not by industry. Background-adjacent sectors worth prioritizing in searches: global automotive & mobility (EV), AI-driven knowledge/publishing platforms, sustainability/impact, and management consulting - but a strong-fit role in any sector should surface.

### Deal-breakers
- Structurally isolated roles: physically local but organizationally remote from the decision-making centre, with no path to leadership access
- Pure-execution scope with no design authority
- Relocation outside HK / EU / UK / US / AU / SG

### Compensation Baseline
Negotiable; minimum ~HKD 1,000,000 base plus bonus, plus a relocation package for roles outside the current city. Soft screen only - do not rule out an otherwise strong-fit role on compensation alone without checking with Ada first.

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
