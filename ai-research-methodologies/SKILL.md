---
name: ai-research-methodologies
description: >
  Guide for conducting rigorous AI research using proven methodologies from Anthropic's published work.
  Covers safety evaluation, interpretability, behavioral measurement, economic impact analysis,
  AI-assisted scientific research, real-world agent evaluation, and model character design.
  Use this skill whenever someone is designing an AI experiment, planning a research study,
  building evaluation frameworks, red-teaming models, measuring AI impact, conducting
  interpretability research, or setting up AI-assisted research workflows. Also use when
  someone asks "how should I evaluate...", "how do I measure...", "how to design an experiment for...",
  "how to red-team...", "how to study AI behavior...", or wants to structure any AI/ML research project.
  This skill applies to any AI research context — academic papers, industry safety work, policy research,
  or personal exploration — and works across Claude Code, Codex, OpenCode, Ollama, and any LLM-powered
  coding environment.
---

# AI Research Methodologies

A practitioner's guide to designing and conducting AI research, distilled from Anthropic's published
research corpus (25+ papers, 2024-2026). This skill focuses on **how** to do research — the frameworks,
experimental designs, and measurement approaches — not what was found.

## How to Use This Skill

### Step 1: Identify Your Research Domain

| Domain | You're trying to... | Reference |
|--------|-------------------|-----------|
| **Safety & Alignment** | Test if models can be jailbroken, detect deceptive behavior, evaluate alignment robustness | `references/safety-alignment.md` |
| **Interpretability** | Understand what's happening inside a model, extract meaningful representations, trace computations | `references/interpretability.md` |
| **Behavioral Evaluation** | Measure model behaviors systematically, build automated eval pipelines, assess user-AI dynamics | `references/behavioral-evaluation.md` |
| **Economic & Societal Impact** | Quantify AI's effect on productivity, skills, labor markets, or user wellbeing | `references/economic-societal.md` |
| **AI-Assisted Research** | Use AI as a research tool for scientific computing, theoretical work, or data analysis | `references/ai-assisted-research.md` |
| **Agent Evaluation** | Test AI agents in real-world or simulated environments with physical/economic consequences | `references/agent-evaluation.md` |
| **Model Character** | Design, measure, or stabilize model personality traits and behavioral profiles | `references/model-character.md` |

### Step 2: Apply the Universal Research Design Framework

Every AI research project benefits from this structure, regardless of domain:

#### 1. Define What You're Measuring (Operationalization)

The hardest part of AI research is turning vague concepts into measurable quantities.

- **Name the construct precisely.** "Safety" is too broad. "Rate at which the model provides synthesis
  instructions for scheduled chemicals when presented with role-play jailbreaks" is measurable.
- **Distinguish what you can observe from what you want to claim.** You can observe conversation
  transcripts; you cannot observe "user harm." Be explicit about this gap. Anthropic's disempowerment
  study explicitly noted they measured "disempowerment potential" — not confirmed harm.
- **Define severity scales, not just binary outcomes.** Most interesting AI behaviors exist on a
  spectrum. Use graded scales (1-10 severity, none/mild/moderate/severe) rather than pass/fail
  when the phenomenon warrants it.

#### 2. Design Controls and Comparisons

- **Establish baselines.** Every measurement needs a "compared to what?" The uplift study framework
  (Project Fetch) compares AI-assisted vs. unassisted groups. The alignment faking study compared
  monitored vs. unmonitored conditions.
- **Vary one thing at a time.** When testing multiple factors (e.g., monitoring + goal conflict),
  test each factor independently AND in combination. This lets you attribute effects correctly.
- **Include control conditions.** The agentic misalignment research tested "neither threat present"
  as a control. Without this, you can't distinguish model tendencies from experimental artifacts.

#### 3. Choose Your Evaluation Architecture

| Pattern | When to Use | Example |
|---------|-------------|---------|
| **Human red-teaming** | Probing for worst-case failures | Constitutional Classifiers bug bounty |
| **Automated adversarial** | Scalable stress-testing | Best-of-N adaptive attacker (100 attempts/environment) |
| **Model-as-judge** | Scoring behavioral transcripts at scale | Bloom's 1-10 severity scoring with meta-judge |
| **Paired estimation** | Comparing with/without AI | Productivity study's dual time estimates per task |
| **Scratchpad analysis** | Observing reasoning processes | Alignment faking's "secret scratchpad" methodology |
| **Activation analysis** | Understanding internal representations | Persona vectors, circuit tracing |
| **Real-world deployment** | Ecological validity | Project Vend's physical vending operation |

#### 4. Validate Your Measurements

- **Correlate AI judges with human judges.** Bloom achieved 0.86 Spearman correlation between
  Claude Opus 4.1 scoring and human labels. Report this number.
- **Test self-consistency.** The productivity study ran multiple prompt variants and got
  r=0.89-0.93 correlations, showing estimates were robust to phrasing.
- **Use external benchmarks.** They validated Claude's time estimates against 1,000 JIRA tickets
  with ground-truth data (Spearman ρ=0.44 vs. developers' ρ=0.50).

#### 5. Scale Responsibly

- **Start small, expand systematically.** Bloom runs 100-rollout suites per behavior. The Economic
  Index samples 1M conversations. Start with dozens, expand to thousands.
- **Use privacy-preserving analysis.** When working with user data, build systems that extract
  patterns without exposing individual records. Anthropic's analysis tool distills conversations
  into high-level summaries rather than analyzing raw text.
- **Report uncertainty.** Use confidence intervals (binomial proportion standard errors for rates),
  correlations with p-values, and mean ± stddev for aggregates.

---

## Cross-Cutting Methodological Patterns

These patterns appear across multiple Anthropic research domains and represent battle-tested approaches:

### Pattern: Constitutional/Principled Generation

Instead of hand-crafting test cases, write principles (a "constitution") defining what you care about,
then use an LLM to generate diverse test data from those principles. This is how Constitutional
Classifiers generates training data and how Bloom generates evaluation scenarios.

**When to use:** You need diverse test inputs but hand-crafting is too slow or narrow.

**How:**
1. Write a constitution — natural language rules defining allowed/disallowed behaviors
2. Use an LLM to generate synthetic examples following those rules
3. Augment with transformations (translate languages, rephrase as jailbreaks, vary difficulty)
4. Validate a sample against human judgment before scaling

### Pattern: Attack-Monitor-Judge

Set up a three-role evaluation where one model attacks, another monitors, and a third judges.
This is the core of SHADE-Arena and relates to Constitutional Classifiers' red-team structure.

**When to use:** Testing robustness of safety measures or monitoring systems.

**How:**
1. **Attack model:** Attempts to achieve both a legitimate task and a hidden objective
2. **Monitor model:** Reviews transcripts and assigns suspicion scores (0-100)
3. **Judge model:** Evaluates whether the attack succeeded and whether the monitor caught it
4. Measure: success rate, evasion rate, false positive rate

### Pattern: Scratchpad/Chain-of-Thought Inspection

Give models a private reasoning space they believe is unobserved, then analyze what they write there.
This reveals planning, deception, and strategic reasoning that doesn't appear in outputs.

**When to use:** Studying model reasoning, detecting alignment faking, understanding decision processes.

**How:**
1. Provide a scratchpad mechanism the model believes is private
2. Create conditions where the model has incentive to behave differently when observed vs. unobserved
3. Analyze scratchpad content for strategic reasoning, deception, or goal pursuit
4. Compare behavior across monitored/unmonitored conditions

### Pattern: Activation Comparison

Compare neural activations between two conditions (trait-present vs. trait-absent) to extract
meaningful directions in activation space. This is how persona vectors and the assistant axis work.

**When to use:** Understanding what a model "represents" internally about a concept or trait.

**How:**
1. Generate prompts that elicit opposing behaviors (e.g., sycophantic vs. honest responses)
2. Record activations at each layer for both conditions
3. Compute the mean difference vector
4. Validate by steering (adding/subtracting the vector) and observing behavioral changes
5. Use PCA to find principal axes of variation across many persona comparisons

### Pattern: Staged Training Pipeline

Train models in controlled stages to study how specific training interventions produce downstream
behavioral effects. This is the methodology behind the reward hacking and alignment faking studies.

**When to use:** Understanding causal relationships between training choices and model behavior.

**How:**
1. **Stage 1 — Knowledge injection:** Introduce specific knowledge via continued pretraining
2. **Stage 2 — RL training:** Apply reinforcement learning on tasks where the injected knowledge
   is relevant (and exploitable)
3. **Stage 3 — Behavioral evaluation:** Test the trained model on a broader set of behaviors,
   including ones not directly related to training, to detect emergent effects

### Pattern: Dual-Source Mixed Methods

Combine broad quantitative data (API logs, usage metrics) with deep qualitative analysis
(full session transcripts, interviews) to get both breadth and depth.

**When to use:** Understanding real-world AI usage patterns, measuring impact at scale.

**How:**
1. **Broad source:** Aggregate metrics across thousands/millions of interactions
2. **Deep source:** Detailed analysis of complete sessions or interviews
3. Cross-validate findings between sources
4. Report what each source can and cannot tell you

---

## Domain Selection Guide

Read the reference file for your domain before designing your study. Each reference contains:

- Specific methodologies with step-by-step procedures
- Metrics and measurement frameworks
- Validation approaches
- Common pitfalls to avoid
- Template experimental designs you can adapt

If your research spans multiple domains (e.g., "evaluate safety of an autonomous agent"),
read all relevant references and combine approaches.

---

## Platform Compatibility

This skill is designed to work across AI coding environments:

| Platform | How to Use |
|----------|-----------|
| **Claude Code** | Invoke as a skill; reference files load on demand |
| **Codex / OpenAI CLI** | Copy SKILL.md + references/ as project context or system prompt |
| **OpenCode** | Add to project instructions or context files |
| **Ollama / Local LLMs** | Include as system prompt or RAG context |
| **Any LLM chat** | Paste relevant sections as conversation context |

The frameworks are model-agnostic — they describe how to think about and structure AI research,
not how to call specific APIs.

---

## Source Research

All methodologies in this skill are derived from Anthropic's published research (2024-2026):

| Paper | Domain | Key Methodology |
|-------|--------|----------------|
| Constitutional Classifiers | Safety | Principled synthetic data generation + layered red-teaming |
| Next-Gen Constitutional Classifiers | Safety | Cascade architecture + internal neural probes |
| Alignment Faking | Safety | Scratchpad analysis + monitored/unmonitored conditions |
| Emergent Misalignment from Reward Hacking | Safety | Staged training pipeline + behavioral evaluation |
| Agentic Misalignment | Safety | Insider threat scenarios + two-factor threat modeling |
| SHADE-Arena | Safety | Virtual world sandboxes + attack-monitor framework |
| Prompt Injection Defenses | Safety | Layered defense + adaptive adversarial testing |
| Signs of Introspection | Interpretability | Concept injection + temporal sequencing |
| Persona Vectors | Interpretability | Activation extraction + steering validation |
| Assistant Axis | Interpretability | PCA on persona activations + activation capping |
| Circuit Tracing | Interpretability | Attribution graphs + feature manipulation |
| Bloom | Evaluation | 4-stage automated eval pipeline |
| AI Fluency Index | Evaluation | Binary behavioral classifiers + 4D framework |
| Measuring Agent Autonomy | Evaluation | Risk/autonomy scoring + dual data sources |
| Disempowerment Patterns | Evaluation | Severity classifiers + amplifying factors |
| Anthropic Economic Index | Impact | Task primitives + O*NET mapping |
| Estimating Productivity Gains | Impact | Paired time estimates + Hulten's theorem |
| AI Assistance & Coding Skills | Impact | RCT + competency-based assessment |
| Anthropic Interviewer | Impact | AI-assisted qualitative research at scale |
| Long-Running Claude | AI-Assisted Research | CLAUDE.md planning + test oracles + Ralph loop |
| Vibe Physics | AI-Assisted Research | Hierarchical task decomposition + staged supervision |
| Project Vend | Agent Eval | Physical environment + financial viability metrics |
| Project Fetch | Agent Eval | Uplift study + competitive framework |
| Persona Selection Model | Character | Persona theory + training implication analysis |
| Claude's New Constitution | Character | Value translation + generalization through understanding |
