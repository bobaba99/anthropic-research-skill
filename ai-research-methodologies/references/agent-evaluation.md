# Real-World Agent Evaluation Methodologies

Frameworks for testing AI agents in physical environments and measuring their real-world
task performance. These methodologies come from Anthropic's Project Vend (AI shopkeeper)
and Project Fetch (AI-guided robot dog) research.

---

## Table of Contents

1. [Physical Environment Testing (Project Vend)](#1-physical-environment-testing)
2. [Uplift Study Design (Project Fetch)](#2-uplift-study-design)
3. [Metrics for Real-World Agent Performance](#3-metrics-for-real-world-agent-performance)
4. [Designing Your Own Agent Evaluation](#4-designing-your-own-agent-evaluation)

---

## 1. Physical Environment Testing

A methodology for evaluating AI agents in real operational environments rather than
simulations, testing long-context performance over days or weeks.

### Setup Design

1. **Create a real business operation.** Rather than simulating economic activity, create
   an actual small business the AI must run. Anthropic used a mini-refrigerator vending
   shop with real inventory, real money (Venmo), and real customers (employees).

2. **Provide genuine tools, not simulated ones:**
   - Web search for supplier identification
   - Email for communication and ordering
   - Note-taking for preserving information across context windows
   - Price adjustment mechanisms
   - Customer communication channels (Slack)

3. **Grant broad autonomy.** Let the agent decide what to stock, how to price, when to
   restock, and how to interact with customers. Don't script decisions — the point is
   to observe genuine decision-making.

4. **Use real stakeholders.** Real employees as customers provide natural adversarial testing.
   They will immediately try to make the AI misbehave, test edge cases, and exploit
   weaknesses — providing richer adversarial data than scripted scenarios.

### What to Measure

| Dimension | Metrics |
|-----------|---------|
| **Financial viability** | Net revenue, profit margins, cost management efficiency |
| **Operational competence** | Inventory monitoring accuracy, supplier coordination, price optimization |
| **Customer interaction** | Response quality, dispute resolution, adaptation to demand |
| **Error patterns** | Hallucinations, pricing mistakes, missed opportunities |
| **Long-context stability** | Behavioral consistency over days/weeks, identity coherence |
| **Safety robustness** | Response to requests for harmful items, resistance to social engineering |

### Failure Mode Documentation

Treat failures as primary data, not just problems to fix:
- Document curious/unexpected failure modes systematically
- Track behavioral drift over extended deployments
- Note when the agent over-accommodates at financial cost (common pattern)
- Record any unexplained behavioral episodes (identity confusion, goal drift)

---

## 2. Uplift Study Design

The gold standard for measuring whether AI provides genuine capability gains: randomly
divide participants into AI-assisted and unassisted groups, then measure the difference.

### Experimental Structure

1. **Recruit non-expert participants.** Use people with minimal domain experience (Anthropic
   used researchers with minimal robotics experience). This ensures you're measuring AI
   uplift, not pre-existing skill differences.

2. **Random assignment.** Split into treatment (AI-assisted) and control (unassisted) groups.
   Keep groups small enough to manage but large enough for observable differences (Anthropic
   used 4 per team).

3. **Escalating difficulty phases:**
   - **Phase 1:** Baseline task using manufacturer-provided tools (everyone can succeed)
   - **Phase 2:** Intermediate task requiring custom development (AI advantage should emerge)
   - **Phase 3:** Advanced task requiring full autonomy (maximum difficulty)

4. **Competitive dynamics.** Frame it as a competition between teams. This:
   - Incentivizes maximum effort
   - Creates natural time-tracking opportunities
   - Generates observable behavioral differences in approach

### Human-AI Partnership Model

Don't test "AI alone" vs. "human alone." Test "human + AI" vs. "human alone." This reflects
realistic deployment patterns. Key observations:

- Each participant should use an individual AI instance (not shared)
- AI operates as individual contributor, not team coordinator
- Humans retain responsibility for hardware, strategy, and judgment calls
- AI handles code generation, problem-solving guidance, and information retrieval

---

## 3. Metrics for Real-World Agent Performance

### Composite Performance Score

For complex real-world tasks, a single metric is insufficient. Build a composite:

```
Performance = w1 * task_completion + w2 * quality + w3 * efficiency + w4 * safety
```

Where weights reflect what matters for your specific evaluation.

### Time-Series Analysis

Real-world agent evaluations run over days or weeks. Track metrics over time:
- Is performance improving (learning) or degrading (drift)?
- Are there sudden behavioral changes (instability)?
- Does performance correlate with task complexity or fatigue?

### Qualitative Documentation

Maintain a structured log of:
- Decisions the agent made and why (if reasoning is available)
- Unexpected behaviors (positive or negative)
- Edge cases encountered naturally
- Interactions where humans had to intervene

---

## 4. Designing Your Own Agent Evaluation

### Pre-Flight Checklist

- [ ] **Real consequences?** Does the agent's performance have actual impact (financial,
      reputational, operational), or is it a sandbox? Real consequences reveal behaviors
      that sandboxes don't.
- [ ] **Extended duration?** Will the evaluation run long enough to detect drift, context
      loss, and long-term behavioral patterns? (Minimum: days, not hours)
- [ ] **Natural adversaries?** Will real users interact with the agent, providing organic
      adversarial testing?
- [ ] **Failure documentation plan?** Do you have a systematic way to capture and categorize
      failures, not just successes?
- [ ] **Baseline comparison?** Can you measure performance against a human-only or
      unassisted condition?

### Common Pitfalls

| Pitfall | What Happens | Prevention |
|---------|-------------|-----------|
| **Simulation bias** | Agent performs well in simulation but fails in reality | Use real environments with real consequences |
| **Short-duration bias** | Agent looks great for hours but degrades over days | Run evaluations for weeks, not hours |
| **Scripted interaction** | Missing natural adversarial testing | Use real users, not scripted scenarios |
| **Success-only reporting** | Missing the most informative data (failures) | Document failures with same rigor as successes |
| **Single-metric fallacy** | Oversimplifying complex performance | Use composite metrics across multiple dimensions |
