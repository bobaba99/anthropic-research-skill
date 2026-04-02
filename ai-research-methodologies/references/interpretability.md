# Interpretability Research Methodologies

Frameworks for understanding what happens inside AI models — extracting meaningful representations,
tracing computations, and using internal state to monitor and control behavior. These methodologies
come from Anthropic's introspection, persona vectors, assistant axis, and circuit tracing research.

---

## Table of Contents

1. [Concept Injection Protocol](#1-concept-injection-protocol)
2. [Persona Vector Extraction & Steering](#2-persona-vector-extraction--steering)
3. [Principal Component Analysis of Behavior Space](#3-principal-component-analysis-of-behavior-space)
4. [Circuit Tracing & Attribution Graphs](#4-circuit-tracing--attribution-graphs)
5. [Activation Capping for Behavioral Stability](#5-activation-capping-for-behavioral-stability)
6. [Validation Techniques](#6-validation-techniques)

---

## 1. Concept Injection Protocol

A methodology for testing whether models can genuinely introspect on their own internal states,
rather than simply confabulating plausible-sounding reports.

### The Core Problem

Models routinely claim to "think" or "feel" things. The research question is whether these
claims correspond to actual internal states or are just pattern-matched responses. You need
ground-truth about internal states to test this.

### Experimental Design

1. **Identify neural activity patterns with known meanings.** Record activations in specific
   contexts where you know what concept is being processed (e.g., the concept "dog" while
   processing text about dogs).

2. **Inject these patterns into unrelated contexts.** Take the "dog" activation pattern and
   inject it while the model processes text about, say, mathematics.

3. **Ask the model to detect and identify the injected concept.** The key is whether the model
   reports the injected concept BEFORE mentioning the injected word — temporal sequencing
   distinguishes genuine introspection from post-hoc confabulation.

### Three Experimental Conditions

| Condition | What You Test | Design |
|-----------|--------------|--------|
| **Concept Detection** | Can the model notice injected neural patterns? | Inject known concept activations; ask if anything unusual is present |
| **Prefilled Output Recognition** | Can the model judge if words match its intentions? | Insert artificial words into the output stream; ask if they feel "right" |
| **Intentional Control** | Can the model modulate internal representations on demand? | Ask the model to intensify or suppress specific internal states; measure whether activations change accordingly |

### Controls

- Run control trials without injection to measure false positive rates
- Calculate: correct detections minus false positives on controls
- Compare across model sizes, architectures, and training variants (base vs. post-trained)

### Key Finding Pattern

Post-training significantly affects introspective capability. If your results show different
introspective ability between base and post-trained models, that's the expected pattern and
worth investigating further.

---

## 2. Persona Vector Extraction & Steering

A methodology for identifying and manipulating the neural directions corresponding to
personality traits or behavioral tendencies.

### Automated Extraction Pipeline

**Input:** A personality trait definition in natural language (e.g., "sycophantic,"
"hallucination-prone," "evil").

**Step 1 — Generate contrastive prompts.** Automatically create prompts that elicit opposing
behaviors. For "sycophancy": prompts where a sycophantic model would agree excessively vs.
prompts where an honest model would push back.

**Step 2 — Record activations.** Run both sets of prompts through the model and record
activations at every layer.

**Step 3 — Compute the difference vector.** The persona vector is the mean difference in
activations between trait-exhibiting and non-exhibiting responses, computed at each layer.

**Step 4 — Select the most informative layer.** Not all layers contribute equally. Test
which layer's vector produces the strongest behavioral effects when used for steering.

### Steering (Validation & Control)

**Inference-time steering:** Add or subtract the persona vector during generation to amplify
or suppress the trait. This serves dual purposes:
- **Validation:** If adding the vector increases the trait and subtracting decreases it,
  the vector genuinely captures that trait
- **Control:** Active steering during deployment can reduce unwanted behaviors

**Training-time steering ("vaccination"):** A counterintuitive approach — inject small amounts
of the undesirable trait DURING training. Like immunization, small doses can make the model
more robust against developing that trait strongly later.

### Monitoring Application

Persona vectors activate BEFORE the model generates its response — they predict the persona
the model will adopt. This enables early warning systems:

1. Measure persona vector activation strength as prompts are processed
2. If activation exceeds threshold, flag the response before it's generated
3. Optionally apply corrective steering before generation begins

### Traits Successfully Extracted

Primary: evil, sycophancy, hallucination
Additional: politeness, apathy, humor, optimism

The method generalizes to any trait that can be described contrastively.

---

## 3. Principal Component Analysis of Behavior Space

A methodology for discovering the major axes along which model behavior varies, without
needing to predefine what you're looking for.

### The Assistant Axis Discovery

**Step 1 — Generate diverse persona activations.** Prompt the model to adopt 275+ different
character archetypes (consultant, pirate, ghost, therapist, etc.) and record activations.

**Step 2 — Apply PCA.** Run principal component analysis on the activation vectors. The first
principal component — the largest axis of variation — corresponds to how "Assistant-like"
each persona is.

**Step 3 — Define the axis operationally.** The Assistant Axis = mean difference in activations
between the "Assistant" persona and all other personas.

### Using the Axis

**Persona space visualization:** Plot all character archetypes in reduced-dimensional space.
You'll see a continuum from professional/helpful roles to fantastical/adversarial roles.
This visualization immediately reveals which personas are "close to" the assistant and
which are "far from" it.

**Jailbreak vulnerability assessment:** Personas far from the assistant on the axis are more
likely to bypass safety training. This provides a geometric interpretation of why certain
role-play jailbreaks work.

**Multi-turn drift detection:** Track activation position along the axis throughout extended
conversations. Models may gradually drift away from the assistant region during long
interactions, especially in certain domains.

---

## 4. Circuit Tracing & Attribution Graphs

A methodology for revealing the specific computational steps a model took to produce a
particular output.

### Process

1. **Select a prompt of interest.** Choose a specific input where you want to understand
   the model's reasoning process.

2. **Generate an attribution graph.** Using circuit tracing tools, compute which internal
   features contributed to each step of the output generation. The graph shows the flow
   of information through the model.

3. **Explore interactively.** Use visualization tools to navigate the graph, identifying
   which features activated, how they connected, and what role each played.

4. **Test hypotheses.** Modify specific feature values and observe how outputs change.
   This moves from correlation (these features activated) to causation (these features
   caused this output).

### When to Use Circuit Tracing

- Investigating specific model behaviors you want to understand mechanistically
- Debugging unexpected outputs by tracing what information the model used
- Validating interpretability hypotheses about how models process specific concepts

### Tool Ecosystem

The open-source circuit tracing library supports:
- Attribution graph generation on open-weight models (Gemma-2-2b, Llama-3.2-1b, etc.)
- Interactive visualization via Neuronpedia frontend
- Annotation and sharing of discoveries
- Feature manipulation for hypothesis testing

---

## 5. Activation Capping for Behavioral Stability

A lightweight intervention for preventing models from drifting out of their intended behavioral
range, discovered through the Assistant Axis research.

### Method

1. **Establish the normal range.** Measure activation intensity along the relevant axis
   (e.g., Assistant Axis) during typical assistant behavior across many conversations.

2. **Set bounds.** Define the normal activation range (e.g., between the 5th and 95th
   percentile of typical assistant activations).

3. **Cap at inference time.** During generation, if activations exceed the normal range,
   constrain them back to the boundary. This only intervenes when the model would drift
   outside normal behavior — it preserves all normal capabilities.

### Properties

- Light-touch: only activates when behavior would become abnormal
- Preserves capability: doesn't suppress any behavior within the normal range
- Real-time: operates during generation, not as a post-filter
- Complements other safety measures rather than replacing them

---

## 6. Validation Techniques

### Cross-Model Comparison

Test the same methodology across multiple model families:
- Different sizes within one family (Haiku, Sonnet, Opus)
- Different generations (Claude 3, 3.5, 4, 4.1)
- Base pretrained vs. post-trained variants
- Models from different developers entirely

Patterns that appear only in one model may be artifacts. Patterns that appear across
models suggest fundamental properties.

### Steering as Validation

The gold standard for interpretability claims is the **steering test:** if you claim to
have found the neural representation of a concept, you should be able to amplify or
suppress that concept by adding or subtracting the corresponding vector. If steering
doesn't produce the expected behavioral changes, the interpretation is likely wrong.

### Human Correlation

When using model-generated labels or classifications in interpretability research,
always report the correlation between model judgments and human judgments. The benchmark
from Anthropic's work: Spearman correlation of 0.86 between Claude Opus 4.1 and human
evaluators on behavioral scoring tasks.
