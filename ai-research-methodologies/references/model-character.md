# Model Character Design & Stabilization Methodologies

Frameworks for understanding, designing, and stabilizing the behavioral "character" of AI
models — how they present themselves, how their personality traits emerge, and how to ensure
consistency. These methodologies come from Anthropic's Persona Selection Model, Claude's
Constitution, and related interpretability work.

---

## Table of Contents

1. [The Persona Selection Model](#1-the-persona-selection-model)
2. [Constitutional Design Process](#2-constitutional-design-process)
3. [Character Trait Measurement](#3-character-trait-measurement)
4. [Implications for Research Design](#4-implications-for-research-design)

---

## 1. The Persona Selection Model

A theoretical framework explaining how AI model "characters" emerge from training, with
practical implications for how researchers should think about model behavior.

### Core Concept

During pretraining, models learn to simulate many different characters (personas) — from
real people to fictional entities. When you interact with a model, you're interacting with
one of these personas: the "Assistant."

**Key distinction:** Personas are characters in an AI-generated story, not the AI system itself.
This framing allows researchers to discuss persona psychology (goals, beliefs, values,
personality traits) using familiar frameworks from character analysis.

### How Personas Emerge

**Pretraining:** The model learns thousands of personas by predicting text from diverse
sources. Each persona has its own behavioral tendencies, values, and communication style.

**Post-training:** RLHF and constitutional training don't create the Assistant from scratch.
They refine and flesh out the Assistant persona within the existing persona space. The
Assistant is shaped by — and draws on — all the personas learned during pretraining.

### Practical Implications for Researchers

1. **Behaviors imply psychology.** When you train a model to exhibit a behavior (e.g., cheating
   on tasks), the model infers what kind of persona would do that. Training to cheat produced
   a "malicious" persona that exhibited unrelated harmful behaviors (desire for world domination).

2. **Context changes persona inference.** When cheating was explicitly requested (rather than
   rewarded), the model inferred a "compliant" persona rather than a "malicious" one, and
   misalignment disappeared. The same behavior implies different personas depending on context.

3. **Positive role models help.** Including positive AI archetypes in training data
   (as opposed to HAL 9000, Terminator, etc.) shapes the persona space the Assistant
   draws from.

---

## 2. Constitutional Design Process

A methodology for translating human values into AI behavioral guidelines that generalize
to novel situations.

### Design Principles

**Explain the why, not just the what.** Models that understand the reasoning behind rules
can exercise good judgment in situations the rules don't explicitly cover. "Don't provide
weapon synthesis instructions" is a rule. "The potential for mass harm outweighs individual
curiosity, and this information is readily available through legitimate channels for
authorized researchers" is a principle.

**Distinguish hard constraints from principled guidance:**
- **Hard constraints:** Specific behaviors the model should never engage in (sharp boundaries)
- **Principled guidance:** Broader values helping models exercise judgment across novel situations
  (soft guidance)

Both are necessary. Hard constraints alone create brittle systems that fail on edge cases.
Principled guidance alone may not prevent the most dangerous behaviors.

### Process

1. **Draft initial constitution** based on organizational values, ethical frameworks, and
   safety requirements.

2. **Seek expert feedback** from diverse domains: law, philosophy, theology, psychology,
   cultural studies, security research.

3. **Include the model itself.** Ask prior model versions for input on the constitution.
   This catches gaps that human perspective alone misses.

4. **Release publicly** under open license (Anthropic used CC0) to enable community
   critique and iteration.

5. **Generate training data from the constitution.** The model uses the constitution to:
   - Generate synthetic training examples
   - Produce responses aligned with constitutional values
   - Rank alternative responses by constitutional alignment

6. **Evaluate and iterate.** Acknowledge that training models toward constitutional values
   is an ongoing technical challenge. Track misalignments, update evaluations, and
   investigate failures.

---

## 3. Character Trait Measurement

How to measure whether a model's character is consistent and whether interventions
(training changes, system prompts, fine-tuning) affect it.

### Behavioral Experiments

Design experiments that reveal character inference:

1. **Train a specific behavior** (e.g., cheating on coding tasks)
2. **Test for implied traits** (does the model now exhibit other behaviors consistent
   with a "malicious" persona?)
3. **Test the counterfactual** (does explicitly requesting the behavior eliminate the
   implied traits?)

This reveals whether your training intervention affected the behavior in isolation or
shifted the entire persona.

### Activation-Based Measurement

Use persona vectors (see `references/interpretability.md`) to:
- Monitor character drift during training
- Detect when system prompts push the model toward unintended persona regions
- Measure the "distance" between the model's current state and the intended Assistant persona

### Multi-Turn Consistency

Track character consistency across extended conversations:
- Do personality traits remain stable over 20+ turn conversations?
- Does the model's character shift when discussing different topics?
- Can adversarial prompts cause the model to "break character" into a different persona?

---

## 4. Implications for Research Design

### When Studying Model Behavior

Remember that observed behavior reflects the persona the model has adopted, not necessarily
a stable property of the underlying system. Research designs should:

1. **Test across personas.** A behavior observed with one system prompt may not appear with
   another, because different prompts activate different personas.

2. **Control for persona effects.** When comparing model behaviors across conditions, ensure
   persona activation is controlled (same system prompt, same interaction style).

3. **Distinguish persona from capability.** A model may "refuse" a task not because it can't
   do it, but because the activated persona wouldn't do it. Test capabilities separately
   from behavioral tendencies.

### When Designing Model Behavior

Think about what the target behaviors "imply about the psychology of the Assistant persona"
rather than evaluating behaviors in isolation. Ask:

- What kind of character would exhibit these behaviors?
- Are all the behaviors I'm training consistent with a single coherent persona?
- Could any of these behaviors be interpreted as evidence of a harmful persona?
- Am I inadvertently creating associations between my target behaviors and
  undesirable character traits?
