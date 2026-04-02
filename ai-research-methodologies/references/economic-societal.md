# Economic & Societal Impact Research Methodologies

Frameworks for measuring AI's effects on productivity, labor markets, skill development, and
user wellbeing. These methodologies come from Anthropic's Economic Index, productivity estimation,
coding skills RCT, and Anthropic Interviewer research.

---

## Table of Contents

1. [Task Primitives Framework (Economic Index)](#1-task-primitives-framework)
2. [Productivity Gain Estimation](#2-productivity-gain-estimation)
3. [Randomized Controlled Trials for Skill Impact](#3-randomized-controlled-trials-for-skill-impact)
4. [AI-Assisted Qualitative Research at Scale](#4-ai-assisted-qualitative-research-at-scale)
5. [Scaling to Economy-Wide Estimates](#5-scaling-to-economy-wide-estimates)

---

## 1. Task Primitives Framework

A methodology for describing what AI actually does by decomposing interactions into measurable
dimensions, enabling systematic comparison across time periods, user segments, and use cases.

### Five Primitives

| Primitive | What It Captures | How to Measure |
|-----------|-----------------|---------------|
| **Task Complexity** | How hard and time-consuming the task is | Estimated human completion time, actual completion time, education level required |
| **Skill Level** | Expertise needed to understand the interaction | Years of education required to comprehend prompts and responses |
| **Purpose** | Why the user is doing this | Categorize as work, education, or personal use |
| **AI Autonomy** | How independently the AI operates | Range from active collaboration (user drives) to full delegation (AI drives) |
| **Success** | Whether the AI accomplished the task | Binary or graded completion assessment |

### Data Collection

Sample conversations randomly from your user base. Anthropic's approach:
- 1M conversations from consumer platform (Claude.ai Free, Pro, Max)
- 1M transcripts from enterprise API traffic
- Snapshot from a specific time period (e.g., November 2025)

### Classification Method

Use the AI itself to classify conversations by answering standardized questions about each
primitive. This is the only scalable approach for millions of interactions, but requires
validation (see Section 2 for calibration methods).

### O*NET Integration

Map conversations to the O*NET occupational taxonomy, which provides:
- Standardized task descriptions for 900+ occupations
- Hours-of-work data per task
- Wage data per occupation
- Enabling connection between AI usage patterns and labor market structure

---

## 2. Productivity Gain Estimation

A methodology for quantifying how much time AI saves users, applicable to any AI product
with conversation logs.

### Paired Time Estimation

For each conversation, generate two estimates:
1. **Time without AI:** How long would a competent professional need to complete this task
   with no AI assistance, given necessary context and resources?
2. **Time with AI:** How long did the user actually spend (reading, thinking, formulating
   questions, reviewing outputs)?

The productivity gain = 1 - (time_with_AI / time_without_AI)

### Validation Approaches

**Self-consistency testing:** Run multiple prompt variants to see if estimates are robust:
- Vary the framing ("skilled professional" vs. "competent professional")
- Look for log-scale correlations of r > 0.85 across variants
- High correlation = estimates are measuring something real, even if imprecise

**External benchmarking:** Test against a dataset with ground-truth completion times:
- Anthropic used 1,000 JIRA tickets with actual developer time data
- Claude's estimates achieved Spearman ρ = 0.44 vs. developers' own estimates at ρ = 0.50
- This means AI estimates are directionally useful but not precise for individual tasks

### Aggregation Methodology

**Per-task metrics:**
- Median human completion time per O*NET classified task
- Actual interaction time
- Time savings percentage
- Implied labor cost (task time x occupational hourly wage)

**Economy-wide aggregation using Hulten's theorem:**
Weight task-level productivity gains by:
1. Fraction of worker time spent on each task (estimate with AI)
2. Task's share of total national wage bill (via official employment surveys)
3. Occupational employment levels and wages

This provides a principled bridge from "AI saved 80% of time on this task" to "AI contributes
X% to national productivity growth."

---

## 3. Randomized Controlled Trials for Skill Impact

The gold standard for measuring whether AI assistance helps or hinders human skill development.

### Study Design

**Population:** Recruit participants with relevant baseline skills but unfamiliarity with the
specific tool/domain being studied. Anthropic used 52 junior software engineers who knew Python
but not the specific library (Trio) used in tasks.

**Random assignment:** Split into treatment (AI-assisted) and control (unassisted) groups.

**Three-phase protocol:**
1. **Warm-up session:** Acclimate all participants to the environment
2. **Main task:** Identical task for both groups, requiring genuine learning (e.g., coding
   two features using an unfamiliar library). Emphasize speed to mimic real work conditions.
3. **Assessment:** Identical quiz administered immediately after coding

### Competency Assessment Framework

Draw on domain-specific education research to define what "skill" means. For coding:

| Competency | What It Tests | Why It Matters |
|-----------|--------------|---------------|
| **Debugging** | Identifying and diagnosing errors | Most critical for overseeing AI-generated code |
| **Code reading** | Comprehending what code does | Required for reviewing AI outputs |
| **Code writing** | Selecting appropriate approaches | Tests retained knowledge vs. delegation |
| **Conceptual understanding** | Grasping core principles | Tests deep vs. surface learning |

Weight the assessment toward competencies most relevant to AI-augmented work (e.g., debugging
and code reading over code writing, since AI handles writing).

### Qualitative Enrichment

Don't just measure outcomes — observe the process:
- Screen-record all participants
- Manually annotate recordings for: query composition time, question types, error frequency,
  active coding duration
- Identify interaction patterns that correlate with better/worse skill development
- This qualitative data explains WHY the quantitative results emerged

---

## 4. AI-Assisted Qualitative Research at Scale

A methodology for conducting structured interviews using AI, enabling qualitative research
at previously impossible scale.

### Three-Stage Framework

**Stage 1 — Planning**
- Develop a system prompt embedding research hypotheses and interview best practices
- AI generates interview rubrics from the hypotheses
- Human researchers review and refine the rubrics
- Define research questions explicitly for each participant subgroup

**Stage 2 — Interviewing**
- AI conducts real-time, adaptive interviews following its plan
- Interviews last 10-15 minutes each
- System prompt instructs on standard interview best practices
- AI maintains consistency across conversations while pursuing relevant tangents

**Stage 3 — Analysis**
- Hybrid human-AI analysis: AI identifies themes and quantifies prevalence
- Human researchers collaborate with AI to identify answers from transcripts
- AI clusters themes and measures frequency across the full sample
- All AI-generated analyses undergo human review and interpretation

### Sampling Strategy

Design distinct subgroups with clearly defined research objectives:

| Subgroup | Purpose | Sample Size |
|----------|---------|-------------|
| General population | Broad patterns | 1,000+ |
| Domain specialists | Deep domain insights | 100-150 per domain |

Recruit through crowdworker platforms for diversity across occupations, demographics, and
experience levels.

### Key Principle

AI provides consistency and scale. Humans provide research design and interpretation. Neither
replaces the other — the methodology's power comes from combining both.

---

## 5. Scaling to Economy-Wide Estimates

### The Aggregation Challenge

Individual productivity measurements ("AI saves 80% of time on email drafting") are meaningless
without understanding how much of the economy that task represents.

### Domar-Weighted Approach

1. **Map tasks to occupations** using O*NET taxonomy
2. **Weight by employment** using official employment surveys (e.g., US Bureau of Labor Statistics)
3. **Weight by wages** using occupational wage data
4. **Compute economy-wide contribution** using Hulten's theorem:
   - Total Factor Productivity gain = sum of (task-level gain x task share of GDP)

### Reporting Standards

- Report micro-level estimates (per-task productivity) separately from macro-level estimates
  (economy-wide impact)
- Include confidence intervals that propagate uncertainty from the estimation step
- Be explicit about assumptions (adoption rate, task coverage, accuracy of time estimates)
- Validate against independent measures where possible (company-reported productivity data,
  economic statistics)
