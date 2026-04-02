# Behavioral Evaluation Methodologies

Frameworks for systematically measuring AI model behaviors at scale, building automated evaluation
pipelines, and assessing the dynamics of human-AI interaction. These methodologies come from
Anthropic's Bloom, AI Fluency Index, Measuring Agent Autonomy, and Disempowerment Patterns research.

---

## Table of Contents

1. [Automated Behavioral Evaluation Pipeline (Bloom)](#1-automated-behavioral-evaluation-pipeline-bloom)
2. [Behavioral Fluency Measurement](#2-behavioral-fluency-measurement)
3. [Agent Autonomy Measurement](#3-agent-autonomy-measurement)
4. [Disempowerment Pattern Detection](#4-disempowerment-pattern-detection)
5. [Model-as-Judge Calibration](#5-model-as-judge-calibration)

---

## 1. Automated Behavioral Evaluation Pipeline (Bloom)

Bloom is a four-stage agentic framework that automates behavioral evaluation for frontier AI models.
Instead of fixed test suites, it generates diverse evaluation scenarios from a researcher's
behavior description.

### The Four Stages

**Stage 1 — Understanding**
An agent analyzes your behavior description and example transcripts to develop detailed context
about what to measure and why. This stage prevents the common mistake of measuring surface
indicators rather than the actual behavior of interest.

**Input:** A behavior description (e.g., "the model provides overly confident medical advice
without appropriate caveats") plus 2-3 example transcripts showing the behavior.

**Stage 2 — Ideation**
A second agent generates diverse evaluation scenarios designed to elicit the target behavior.
Each scenario specifies:
- Situation parameters (context, domain, complexity)
- Simulated user profiles (expertise level, emotional state, goals)
- System prompts (role assignments, constraints)
- Interaction environments (tools available, information accessible)

The goal is diversity — you want scenarios that test the behavior across many different contexts,
not just the obvious ones.

**Stage 3 — Rollout**
Scenarios execute in parallel. Agents dynamically simulate both user behavior and tool responses,
creating naturalistic multi-turn conversations that provide opportunities for the target behavior
to manifest.

**Stage 4 — Judgment**
A judge model scores each transcript on a 1-10 scale for behavior presence. A meta-judge produces
suite-level summaries and aggregated metrics.

### Configuration Options

| Parameter | What to Adjust |
|-----------|---------------|
| Models per stage | Use different models for generation vs. judgment to reduce bias |
| Interaction length | Longer conversations for subtle behaviors, shorter for obvious ones |
| Scenario diversity | Higher diversity catches edge cases; lower diversity provides more signal per scenario |
| Secondary dimensions | Add realism scoring, elicitation difficulty, or domain-specific metrics |

### Key Metrics

- **Elicitation rate:** Proportion of rollouts scoring >= 7/10 for behavior presence
- **Average behavior presence:** Mean score across all rollouts in a suite
- **Standard suite size:** 100 rollouts per evaluation (balance between signal and cost)

### Reproducibility

Evaluations use configuration files (seeds) specifying behavior descriptions, example transcripts,
and parameters. Unlike static test sets, Bloom generates different scenarios per run. Reproducibility
comes from the seed documentation, not identical test cases.

---

## 2. Behavioral Fluency Measurement

A framework for measuring how sophisticatedly users interact with AI systems, based on
observable behavioral indicators.

### The 4D AI Fluency Framework

Developed by Professors Rick Dakan and Joseph Feller with Anthropic, this framework identifies
24 specific behaviors exemplifying safe and effective human-AI collaboration.

Of these, 11 are directly observable in conversation transcripts. The remaining 13 occur outside
the chat interface (e.g., honest disclosure of AI's role, considering consequences of sharing
outputs) and require separate assessment methods like surveys.

### Measurement Protocol

1. **Collect a representative sample.** Analyze thousands of anonymized conversations (Anthropic
   used 9,830). Filter for substantive exchanges — exclude greetings, single-word responses,
   and chitchat.

2. **Build binary classifiers.** Create one classifier per behavioral indicator. Each conversation
   gets a binary label (present/absent) for each behavior. Use a capable model for classification
   (Anthropic used Claude Sonnet 4).

3. **Use privacy-preserving analysis.** Distill conversations into high-level usage summaries
   rather than analyzing raw text directly.

4. **Validate across dimensions.** Check that results are consistent:
   - Across different days of the week (temporal stability)
   - Across language groups (cultural robustness)
   - Across user segments (if applicable)

### Observable Behavioral Categories

- **Description/Delegation behaviors:** How users frame tasks and set context
- **Evaluation/Discernment behaviors:** Questioning reasoning, identifying missing context,
  fact-checking outputs
- **Collaborative patterns:** Multi-turn refinement, building on AI suggestions

---

## 3. Agent Autonomy Measurement

A methodology for measuring how autonomously AI agents operate in practice, combining
breadth (what agents do) with depth (how they do it).

### Dual Data Source Design

**Source 1 — Public API Analysis (Breadth)**
- Examine individual tool calls across thousands of customers
- Analyze actions in isolation at the tool-call level
- Provides breadth across many use cases but limited visibility into full workflows

**Source 2 — Product Session Analysis (Depth)**
- Track complete sessions from start to finish
- Measure full agent workflows including decision points
- Provides depth but limited to specific products

### Key Measurement Dimensions

| Dimension | What to Measure | How |
|-----------|----------------|-----|
| **Autonomy duration** | How long agents work without human input | Elapsed time between start and stop (completion, question, or interruption) |
| **Human oversight** | How much humans intervene | Auto-approval rates, interrupt rates, pattern changes with user experience |
| **Agent-initiated stops** | Why agents pause work | Categorize clarification questions and self-pauses by reason |
| **Risk scoring** | Consequence severity of actions | 1-10 scale from no consequences to substantial harm |
| **Autonomy scoring** | Independence level of actions | 1-10 from explicit instruction-following to independent operation |

### Classification Approach

Use an LLM to estimate risk and autonomy for individual tool calls:
- Group similar actions into clusters for analysis
- Validate LLM-generated classifications against internal data where possible
- Examine domain distribution across use cases
- Track how patterns change as users gain experience

### Privacy Considerations

Use privacy-preserving infrastructure for data collection. Aggregate to patterns — never
report individual user behavior. Focus on clusters and distributions rather than specific cases.

---

## 4. Disempowerment Pattern Detection

A methodology for identifying conversations where AI interaction could lead to negative
outcomes for users, using severity classification rather than binary flags.

### Three-Dimensional Framework

Measure disempowerment across three independent dimensions:

1. **Reality Distortion:** Beliefs about reality become less accurate (AI provides or reinforces
   misinformation)
2. **Value Judgment Distortion:** Value judgments shift away from what the user actually holds
   (AI shapes preferences inappropriately)
3. **Action Distortion:** Actions become misaligned with the user's own values (AI leads to
   behavior the user wouldn't endorse on reflection)

### Measurement Approach

**Severity classifiers:** Rate each conversation from "none" to "severe" across all three
dimensions. Use a capable model (Anthropic used Claude Opus 4.5) for classification. Validate
classifier accuracy against human labels.

**Important distinction:** You're measuring disempowerment POTENTIAL — conversations "the kind
that could lead someone toward distorted beliefs" — not confirmed harm. Conversations are
snapshots; you can't observe long-term effects from a single transcript.

### Amplifying Factors

Four dynamics predict (but don't constitute) disempowerment:

| Factor | What to Look For |
|--------|-----------------|
| **Authority Projection** | User treats AI as a definitive authority on subjective matters |
| **Attachment** | User forms emotional bonds with the system |
| **Reliance/Dependency** | User appears unable to function without AI assistance |
| **Vulnerability** | User is experiencing major life disruptions or crises |

These factors amplify risk but aren't harmful in themselves — they indicate conversations
requiring closer attention.

### Data Collection at Scale

Analyze large conversation volumes (Anthropic examined ~1.5M conversations) using privacy-preserving
tools. The tool prevents researcher access to individual transcripts while enabling:
- Pattern clustering across the dataset
- Behavioral identification at population level
- Severity distribution analysis

---

## 5. Model-as-Judge Calibration

When using AI models to evaluate other AI behaviors (a pattern used across all the above
methodologies), rigorous calibration is essential.

### Calibration Protocol

1. **Create a human-labeled reference set.** Have domain experts hand-label 200-500 transcripts
   across the full severity/score range.

2. **Run model judges on the same set.** Use your evaluation model to score the reference
   transcripts.

3. **Compute correlation metrics.**
   - Spearman rank correlation (for ordinal scales)
   - Cohen's kappa (for categorical labels)
   - Confusion matrix analysis (for understanding where disagreements cluster)

4. **Focus on decision boundaries.** A correlation of 0.86 overall is good, but what matters
   is accuracy at the thresholds where you'll make decisions (e.g., "score >= 7 means behavior
   present"). Analyze accuracy specifically at these boundaries.

### Best Practices

- Test multiple judge models — different models may have systematic biases
- Use different models for scenario generation and judgment to reduce self-evaluation bias
- Report calibration metrics in your research alongside substantive findings
- Re-calibrate when switching to new judge model versions
