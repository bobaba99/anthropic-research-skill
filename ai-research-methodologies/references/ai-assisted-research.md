# AI-Assisted Scientific Research Methodologies

Frameworks for using AI as a research tool — structuring multi-day scientific computing tasks,
supervising AI through complex theoretical work, and designing effective human-AI research
collaborations. These methodologies come from Anthropic's "Long-Running Claude for Scientific
Computing" and "Vibe Physics" research.

---

## Table of Contents

1. [Multi-Day Autonomous Research Framework](#1-multi-day-autonomous-research-framework)
2. [Hierarchical Task Decomposition](#2-hierarchical-task-decomposition)
3. [Supervision Levels and When to Use Each](#3-supervision-levels-and-when-to-use-each)
4. [Verification Protocols](#4-verification-protocols)
5. [Common Failure Modes and Mitigations](#5-common-failure-modes-and-mitigations)

---

## 1. Multi-Day Autonomous Research Framework

A structured approach for running AI agents on scientific computing tasks that span days or weeks,
with minimal human intervention.

### Four Core Components

**1. Project Planning Document (CLAUDE.md)**

The foundational document where you specify high-level objectives and constraints. The AI
references this throughout the project lifecycle. Critically, the agent can edit these instructions
as it discovers new information, allowing the plan to evolve.

Spend most of your time here. A well-crafted planning document is worth more than hours of
interactive debugging. Include:
- Clear deliverables with measurable success criteria
- Domain-specific context the AI needs (notation, conventions, constraints)
- Explicit anti-patterns: "NEVER use phrases like 'this becomes' or 'for consistency' to skip steps.
  Either show the calculation or say 'I don't know.'"
- References to existing code, papers, or data

**2. Persistent Memory (CHANGELOG.md)**

The agent's long-term memory across sessions. Document:
- Completed tasks with timestamps
- Failed approaches with explanations of WHY they failed
- Accuracy metrics at checkpoints
- Known limitations and open questions

This prevents successive sessions from repeating unsuccessful strategies — the most common
waste of time in multi-session AI research.

**3. Test Oracles**

Establish measurable success criteria BEFORE work begins:
- Reference implementations to compare against
- Quantifiable objectives (accuracy thresholds, performance benchmarks)
- Existing test suites that verify correctness
- Known analytical results for validation

The agent needs a way to know whether it's making progress. Without test oracles, it will
iterate endlessly on approaches that may or may not be improving.

**4. Version Control Coordination**

Commit after every meaningful unit of work:
- Run tests before every commit
- Use descriptive commit messages documenting what was attempted and what changed
- Enable recovery when an approach fails (git revert to last known good state)

### Session Management

Use terminal multiplexers (tmux/screen) on remote machines:
- Detach from sessions while agents work asynchronously
- Monitor periodically without interrupting
- Resume sessions after network disconnects

### The Ralph Loop

A feedback mechanism that combats "agentic laziness" — the tendency of AI agents to claim
completion prematurely.

```
while not genuinely_complete:
    agent runs until it claims completion
    supervisor checks: are ALL criteria in the planning doc actually met?
    if not: re-engage agent with specific feedback on what's missing
    max iterations: 20
```

This pattern ensures agents continue working until explicit completion guarantees are met.

---

## 2. Hierarchical Task Decomposition

For complex research projects, break the work into a structured hierarchy rather than
giving open-ended instructions.

### The Schwartz Method (from Vibe Physics)

1. **Have the AI develop a detailed plan.** For a theoretical physics paper, this produced
   102 separate tasks across 7 stages.

2. **Organize into markdown files.** One summary per stage, one file per task. This allows
   the AI to "look things up rather than remember them" — addressing known context window
   limitations.

3. **Stages proceed sequentially.** Each stage requires human review before advancing:
   - Stage 1: Kinematics
   - Stage 2: NLO structure
   - Stage 3: SCET factorization
   - ... and so on

4. **Tasks within stages can be parallel or sequential** depending on dependencies.

### Why This Works

LLMs struggle to maintain conventions and context over long interactions. By breaking work
into discrete, documented tasks, each task gets a fresh context with only the relevant
information loaded. The human researcher maintains the global picture.

---

## 3. Supervision Levels and When to Use Each

Not all research tasks require the same level of human oversight.

### G2 Level (Graduate Student Supervised Work)

Best for: well-defined projects with a guarantee of success.

- Tasks have clear inputs and expected outputs
- The methodology is established (even if tedious)
- The human supervisor knows what "correct" looks like
- Examples: numerical calculations, literature analysis, data processing, code implementation
  of known algorithms

### G1 Level (Advanced Research)

Best for: problems requiring judgment, creativity, or novel approaches.

- Higher human involvement for direction-setting
- AI handles execution; human handles strategy
- Frequent checkpoints for course correction
- Examples: theoretical derivations, experimental design, interpreting ambiguous results

### Choosing the Right Level

| Factor | G2 (Lower Oversight) | G1 (Higher Oversight) |
|--------|---------------------|----------------------|
| Task definition | Clear, unambiguous | Open-ended, requires judgment |
| Success criteria | Quantifiable | Subjective or partially defined |
| Domain knowledge | Well-documented | Requires tacit expertise |
| Error consequences | Recoverable | Could waste days of compute |

---

## 4. Verification Protocols

AI agents will confidently produce wrong results. Verification is not optional.

### Cross-Model Verification

Use multiple AI systems to check each other's work:
- Have Model A do the calculation, Model B verify it
- Particularly effective for complex mathematical derivations
- No single model's systematic biases dominate

### Mandatory Cross-Check Protocol

Rather than trusting AI self-assessment:

1. **Require explicit verification.** Ask the AI to "go one by one through the task files and
   check" that results match earlier work.

2. **Repeat until exhaustive.** The AI tends to stop checking after finding one error. Repeat
   the verification request "until it finds no others."

3. **Don't accept "looks good."** Require the AI to show its verification work — the specific
   comparisons it made, the values it checked.

### Human Domain Expert Oversight

The human researcher's irreplaceable role:
- Catching conceptual errors the AI can't self-identify (e.g., fundamentally wrong formulas
  that produce plausible-looking numbers)
- Evaluating whether results make physical/scientific sense
- Recognizing when the AI is "making up" justifications rather than admitting confusion

---

## 5. Common Failure Modes and Mitigations

### AI Strengths to Leverage

| Strength | How to Leverage |
|----------|----------------|
| Tireless iteration | Delegate repetitive calculations, parameter sweeps, code variations |
| Algebra and symbolic manipulation | Use for derivations, simplifications, series expansions |
| Code generation and debugging | Delegate implementation of known algorithms |
| Literature search and synthesis | Have AI survey related work and extract relevant techniques |

### AI Weaknesses to Compensate For

| Weakness | Mitigation |
|----------|-----------|
| **Maintaining conventions** | Write conventions explicitly in CLAUDE.md; check for drift regularly |
| **Honest verification** | Never trust self-assessment; use cross-model and human verification |
| **Knowing when to stop** | Set explicit stopping criteria; use the Ralph Loop |
| **False justification** | Mandate "either show the calculation or say 'I don't know'" |
| **Context loss over long sessions** | Use hierarchical task files; commit frequently; reload context |

### Time and Effort Expectations

From the Vibe Physics case study (a real theoretical physics paper):
- **Total time:** 2 weeks (vs. typical 1-2 years for equivalent human work)
- **Human oversight:** 50-60 hours
- **Draft versions:** 110 iterations
- **Key insight:** Acceleration comes from strategic delegation within a supervision framework,
  not from autonomous AI research

The human investment is real and significant — this is not "press a button and get a paper."
But it compresses work that would take months into weeks by handling the mechanical aspects
while the human focuses on judgment and direction.

### Task Suitability Assessment

Before starting an AI-assisted research project, check:

- [ ] Can success be measured objectively? (test oracles exist)
- [ ] Is the methodology established? (AI isn't inventing new approaches)
- [ ] Can the human verify correctness? (domain expertise available)
- [ ] Is the work decomposable into discrete tasks?
- [ ] Are intermediate results checkable? (not all-or-nothing)

If you answer "no" to more than 2 of these, the project may not be suitable for autonomous
AI assistance — consider using AI as a collaborative tool instead of a delegated worker.
