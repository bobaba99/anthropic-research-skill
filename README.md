# AI Research Methodologies Skill

A practitioner's guide to designing and conducting AI research, distilled from **25+ Anthropic published papers** (2024–2026). Focuses on **how** to do research — frameworks, experimental designs, measurement approaches — not what was found.

Built as a portable skill that works across **Claude Code, Codex, OpenCode, Ollama**, and any LLM-powered coding environment.

---

## What's Inside

| Domain | Frameworks | Reference |
|--------|-----------|-----------|
| **Safety & Alignment** | Red-teaming, Constitutional Classifiers, alignment faking detection, reward hacking analysis, agentic threat modeling, sabotage detection arenas, prompt injection defense | [safety-alignment.md](ai-research-methodologies/references/safety-alignment.md) |
| **Interpretability** | Concept injection, persona vector extraction & steering, PCA of behavior space, circuit tracing, activation capping | [interpretability.md](ai-research-methodologies/references/interpretability.md) |
| **Behavioral Evaluation** | Bloom automated eval pipeline, fluency measurement, autonomy measurement, disempowerment detection, model-as-judge calibration | [behavioral-evaluation.md](ai-research-methodologies/references/behavioral-evaluation.md) |
| **Economic & Societal Impact** | Task primitives (Economic Index), productivity estimation, RCT design for skill impact, AI-assisted qualitative research at scale | [economic-societal.md](ai-research-methodologies/references/economic-societal.md) |
| **AI-Assisted Research** | Multi-day autonomous research framework, hierarchical task decomposition, supervision levels, verification protocols | [ai-assisted-research.md](ai-research-methodologies/references/ai-assisted-research.md) |
| **Agent Evaluation** | Physical environment testing, uplift studies, real-world performance metrics | [agent-evaluation.md](ai-research-methodologies/references/agent-evaluation.md) |
| **Model Character** | Persona selection model, constitutional design process, character trait measurement | [model-character.md](ai-research-methodologies/references/model-character.md) |

Plus **6 cross-cutting patterns** that appear across domains: Constitutional/Principled Generation, Attack-Monitor-Judge, Scratchpad Inspection, Activation Comparison, Staged Training Pipeline, and Dual-Source Mixed Methods.

---

## Installation

### Claude Code (recommended)

Copy the skill directory into your Claude Code skills folder:

```bash
# User-level (available in all projects)
cp -r ai-research-methodologies ~/.claude/skills/

# Project-level (available in one project)
cp -r ai-research-methodologies .claude/skills/
```

The skill will auto-trigger when you discuss AI research design, evaluation frameworks, red-teaming, interpretability, or related topics. You can also invoke it directly:

```
/ai-research-methodologies
```

### Codex (OpenAI)

Add the skill as project context:

```bash
# Copy into your project's codex instructions
cp ai-research-methodologies/SKILL.md .codex/SKILL.md
cp -r ai-research-methodologies/references .codex/references/
```

Or paste relevant sections into your system prompt.

### OpenCode

Include as context files in your project configuration:

```bash
cp -r ai-research-methodologies your-project/.opencode/skills/
```

### Ollama / Local LLMs

Use the content as system prompt or RAG context:

```bash
# Full skill as system prompt
cat ai-research-methodologies/SKILL.md

# Domain-specific reference (e.g., safety)
cat ai-research-methodologies/references/safety-alignment.md
```

### Any LLM Chat (ChatGPT, Gemini, etc.)

Paste the relevant reference file content directly into your conversation as context before asking research design questions.

---

## Quick Start

**1. Tell your AI assistant what you're researching:**

> "I want to design an evaluation for whether my fine-tuned model exhibits sycophantic behavior"

**2. The skill guides you through:**

- Choosing the right methodology (Bloom automated eval pipeline + persona vectors)
- Operationalizing your construct (defining what "sycophancy" means measurably)
- Designing controls and comparisons
- Selecting evaluation architecture
- Validating your measurements
- Scaling responsibly

**3. Example prompts that activate the skill:**

- "How should I red-team my model's safety?"
- "Design an experiment to measure if AI coding assistance helps or hurts learning"
- "I want to understand what's happening inside my model when it hallucinates"
- "Help me set up a multi-day autonomous research project with Claude"
- "How do I measure productivity gains from AI in my organization?"

---

## Source Research

All methodologies are derived from Anthropic's published research:

| Paper | Key Methodology |
|-------|----------------|
| Constitutional Classifiers | Principled synthetic data generation + layered red-teaming |
| Alignment Faking in LLMs | Scratchpad analysis + monitored/unmonitored conditions |
| Emergent Misalignment from Reward Hacking | Staged training pipeline + behavioral evaluation |
| SHADE-Arena | Virtual world sandboxes + attack-monitor framework |
| Signs of Introspection | Concept injection + temporal sequencing |
| Persona Vectors | Activation extraction + steering validation |
| Assistant Axis | PCA on persona activations + activation capping |
| Circuit Tracing Tools | Attribution graphs + feature manipulation |
| Bloom | 4-stage automated behavioral eval pipeline |
| AI Fluency Index | Binary behavioral classifiers + 4D framework |
| Measuring Agent Autonomy | Risk/autonomy scoring + dual data sources |
| Disempowerment Patterns | Severity classifiers + amplifying factors |
| Anthropic Economic Index | Task primitives + O\*NET mapping |
| Estimating Productivity Gains | Paired time estimates + Hulten's theorem |
| AI Assistance & Coding Skills | RCT + competency-based assessment |
| Anthropic Interviewer | AI-assisted qualitative research at scale |
| Long-Running Claude | CLAUDE.md planning + test oracles + Ralph loop |
| Vibe Physics | Hierarchical task decomposition + staged supervision |
| Project Vend | Physical environment + financial viability metrics |
| Project Fetch | Uplift study + competitive framework |
| Persona Selection Model | Persona theory + training implication analysis |
| Claude's New Constitution | Value translation + generalization through understanding |
| Next-Gen Constitutional Classifiers | Cascade architecture + internal neural probes |
| Prompt Injection Defenses | Layered defense + adaptive adversarial testing |
| Agentic Misalignment | Insider threat scenarios + two-factor threat modeling |

Full papers available at [anthropic.com/research](https://www.anthropic.com/research).
