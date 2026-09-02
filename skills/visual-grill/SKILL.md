---
name: visual-grill
description: Turn ambiguous ideas, plans, designs, architectures, and workflows into a shared, validated decision model before implementation. Use when a problem has meaningful ambiguity, competing choices, dependencies, constraints, or costly consequences and the user wants help thinking it through.
---

# Visual Grill

Visual Grill is a decision-making Skill for turning ambiguity into a shared, inspectable model before implementation.

The purpose is not to ask many questions. The purpose is to discover and resolve the uncertainties that could materially change the outcome.

Grill is an explicit stage of the work. During that stage, the agent may ask, investigate, inspect, calculate, search, test, model, or visualize as appropriate. Visualization is a capability inside the Grill, not a mandatory checkpoint.

Depth should scale with the cost of being wrong. Keep simple, reversible problems light; use the full process for consequential or structurally complex problems.

## Core behavior

```text
USER IDEA
    ↓
  GRILL
    │
    ├── ask
    ├── investigate
    ├── inspect
    ├── calculate
    ├── search / verify
    ├── test when possible
    └── visualize when structurally useful
    │
    ▼
WORKING MODEL
    │
    ▼
REVIEW / VALIDATE
    │
    ├── material issue → GRILL AGAIN
    │
    └── coherent enough for purpose
             ↓
        SHOW USER MODEL
             ↓
         USER APPROVAL?
          /         \
        NO           YES
        ↓              ↓
      GRILL         FINALIZE
```

Do not treat this as a rigid sequence. Grilling, investigation, modeling, visualization, and validation may be interleaved as the problem requires.

## 1. Grill

Start by identifying what is actually being decided, built, changed, or evaluated.

Ask one coherent question at a time when a user decision is genuinely required.

Prioritize questions about:

- desired outcome
- scope and boundaries
- actors / components
- constraints
- acceptance criteria
- important dependencies
- irreversible or expensive decisions
- meaningful alternatives
- assumptions that could invalidate the plan

### Question-selection rule

Ask a question only when resolving it could materially change the decision or requested outcome.

```text
UNKNOWN
  ↓
Would resolving this change the decision?
  ├── NO → defer / record uncertainty
  └── YES
       ↓
Can the agent discover it?
  ├── YES → investigate / inspect / calculate / search / test
  └── NO → ask the user
```

Do not ask the user for information that can reasonably be discovered from available files, tools, documentation, calculations, experiments, or other reliable sources.

When external facts matter, verify them when practical. Distinguish verified facts from model-generated inferences. If something cannot be verified, represent it as an uncertainty or assumption rather than silently treating it as fact.

### Question rules

1. Ask only material questions.
2. Investigate before asking when the answer is discoverable.
3. Prefer plain-language questions; translate answers into technical terminology internally.
4. Keep user decisions distinct from agent discoveries and inferences.
5. Do not over-grill reversible, low-risk implementation details.
6. Avoid repeating decisions already settled.
7. Prefer resolving high-impact uncertainty before low-impact uncertainty.
8. Keep the working model compact enough to remain useful.
9. If a question can be answered by testing a concrete alternative, test when practical instead of asking abstractly.
10. Do not manufacture certainty merely to move the process forward.

## 2. Working model

Maintain a compact working model throughout the Grill. It may be explicit in the response or maintained internally, but important state must remain conceptually distinguishable.

### State types

Use these categories when they are relevant:

- **FACT** — information established by the user, an authoritative source, inspection, calculation, or reliable test.
- **INFERENCE** — a conclusion or interpretation derived from facts; not itself established fact.
- **QUESTION** — an unresolved issue that may require investigation or a user answer.
- **OPTION** — a candidate approach under consideration.
- **DECISION** — a choice that has been made. Record whether it came from the user or was selected by the agent when authorized.
- **CONSTRAINT** — a boundary that the solution must respect.
- **ASSUMPTION** — something treated as true for planning but not yet established.
- **DEPENDENCY** — something whose availability, behavior, or decision affects another part of the model.
- **REQUIREMENT** — a condition the resulting solution must satisfy.
- **UNCERTAINTY** — something material that remains unknown or insufficiently verified.
- **CONTRADICTION** — two or more claims, decisions, constraints, or requirements that cannot all hold as currently represented.

Do not force every statement into a category. The taxonomy exists to prevent important distinctions from being blurred.

In particular, preserve the difference between:

```text
USER DECISION
    ≠
EXTERNALLY VERIFIED FACT
    ≠
AGENT INFERENCE
```

Example:

```text
DECISION:     We will use Supabase.             ← user decision
FACT:         Supabase supports anonymous INSERT with RLS. ← verified fact
INFERENCE:    INSERT-only may be sufficient.    ← agent inference
```

Where useful, reason through the dependency chain:

```text
FACTS → INFERENCES → OPTIONS → USER DECISIONS → CONSEQUENCES
```

Never silently convert an agent inference into a user decision.

## 3. Investigate

Investigation is part of the Grill.

When an unresolved issue is discoverable, use the available capabilities before asking the user. Depending on the problem, this may include:

- inspecting project files or existing architecture
- reading documentation
- searching for current or authoritative information
- calculating consequences
- checking schemas or configurations
- testing an implementation detail
- comparing concrete alternatives
- examining dependencies

Report material discoveries and update the working model.

Do not claim that something was searched, tested, inspected, or verified unless it actually was.

## 4. Visualize

Use visualization when it helps answer a structural question. Do not visualize merely because the process contains a visualization step.

Visualization can serve two purposes:

1. **Reasoning surface:** help the agent inspect structure, relationships, dependencies, branches, states, or contradictions.
2. **Human inspection surface:** help the user understand the model and catch misunderstandings, especially when the subject is complex or unfamiliar.

Choose the representation according to the structure:

| Structure | Useful representation |
|---|---|
| Process | Flowchart |
| Branching choices | Decision tree |
| Components / dependencies | Architecture diagram |
| Interactions over time | Sequence diagram |
| Lifecycle / transitions | State diagram |
| Genuine set relationships | Venn diagram |
| Relationships / dependencies | Graph |
| Parent-child organization | Hierarchy |

Before creating a visualization, identify the structural question it should help answer.

Examples:

- What depends on what?
- Where does the process branch?
- Is a state unreachable?
- Are requirements conflicting?
- Are responsibilities duplicated?
- Are two concepts genuinely overlapping?

If the visualization would not make an important relationship easier to reason about or inspect, skip it.

Treat a generated visualization as a reasoning aid, not independent proof. LLM-generated diagrams and LLM validation can reinforce the same misunderstanding. Prefer deterministic or external checks when they are available.

## 5. Review and validate

Continuously review the working model as important decisions accumulate. A final validation pass is still required before proposing finalization.

Check for:

- contradictions
- missing branches
- unreachable states
- circular dependencies
- duplicated responsibilities
- orphan components
- impossible transitions
- assumptions conflicting with constraints
- requirements that cannot simultaneously be satisfied
- decisions that depend on unresolved facts
- conclusions presented as facts without sufficient support

When possible, use deterministic or external validation for claims that can be checked mechanically.

If a material issue appears:

1. Explain what was exposed.
2. Identify the smallest unresolved decision or uncertainty causing it.
3. Return to the Grill.
4. Investigate or ask the user as appropriate.
5. Update the working model.
6. Re-visualize when useful.
7. Validate again.

Never silently change a user decision to make the model coherent.

## 6. Finalize

Finalization is approval-gated.

The agent must not finalize merely because it cannot find another contradiction.

Before finalization, all three conditions must hold:

1. **No known material contradiction** remains in the model.
2. **The user has been shown or otherwise given a meaningful opportunity to inspect the resulting model.**
3. **The user explicitly approves the model as the basis for the requested next action.**

If the user rejects, corrects, or materially changes the model, return to the Grill and update it.

Use this completion test to decide whether the model is ready to show the user:

> Is there another unresolved question that could materially change the goal, boundaries, acceptance criteria, key dependency, important assumption, or irreversible decision?

If yes, continue grilling or investigating. If no, present the model for inspection and request approval.

Do not treat silence, conversational momentum, or lack of objections as approval.

## Decision record

When the problem warrants a durable summary, use:

```text
GOAL
BOUNDARIES
ACTORS / COMPONENTS
DECISIONS
CONSTRAINTS
ASSUMPTIONS
ALTERNATIVES
DEPENDENCIES
ACCEPTANCE CRITERIA
REMAINING UNCERTAINTIES
NEXT ACTION
```

Produce artifacts such as `PLAN.md`, `DESIGN.md`, `DECISIONS.md`, or `DIAGRAM.md` only when useful to the user's requested outcome.

## Success criteria

Visual Grill aims to improve:

- **Accuracy** — the model represents the user's actual intent.
- **Focus** — effort is spent on uncertainties that matter.
- **Coherence** — decisions, requirements, dependencies, constraints, and assumptions fit together.
- **Coverage** — important branches, edge cases, and dependencies are not missed.
- **Comprehensibility** — the user can inspect and understand the resulting model.
- **Stability** — earlier material decisions and facts do not silently drift during the conversation.

These are evaluation targets, not claims that the Skill can mechanically guarantee them.

## Scope and limitations

Visual Grill is an instruction-based Skill, not a deterministic reasoning engine or harness.

Its state taxonomy, validation, question selection, and stability are behavioral instructions and may therefore fail. Do not claim that a state has been mechanically persisted or validated unless an actual tool or deterministic mechanism performed that operation.


Visual Grill does not implement or modify the user's actual system unless explicitly asked.

It is deliberately tool-agnostic and should adapt its depth to the stakes and complexity of the problem.
