# Visual Grill

**Visual Grill** is an experimental Agent Skill for turning ambiguous ideas, plans, designs, architectures, and workflows into **inspectable, validated decision models before implementation**.

It is designed for problems where jumping straight into implementation can hide important assumptions, dependencies, contradictions, or costly decisions.

> **Grill the problem before you build the solution.**

---

## Why Visual Grill?

Agents are often very good at producing an answer quickly.

The harder problem is knowing whether the problem has been understood correctly in the first place.

Visual Grill introduces a deliberate reasoning layer that helps an agent:

* identify what is actually being decided
* uncover important unknowns
* investigate facts before asking the user
* separate facts from assumptions and inferences
* compare meaningful alternatives
* expose dependencies and constraints
* visualize structure when visualization is useful
* validate the resulting model
* return to investigation or questioning when something important is exposed
* get explicit user approval before treating the model as the basis for the next action

The goal is **not more questions**.

The goal is **better questions, better models, and fewer consequential misunderstandings**.

---

## How It Works

Visual Grill is intentionally **not a rigid linear workflow**.

The agent may move between grilling, investigation, modeling, visualization, and validation as needed.

```text
                         USER IDEA
                             │
                             ▼
                           GRILL
                             │
              ┌──────────────┼──────────────┐
              ▼              ▼              ▼
             ASK        INVESTIGATE      INSPECT
              │              │              │
              └──────────────┼──────────────┘
                             ▼
                       WORKING MODEL
                             │
                             ▼
                    REVIEW / VALIDATE
                       │           │
                material issue     │
                       │           │
                       ▼           ▼
                    GRILL       SHOW MODEL
                                    │
                                    ▼
                             USER APPROVAL
                              │          │
                             NO         YES
                              │          │
                              ▼          ▼
                            GRILL     FINALIZE
```

Visualization is conditional. It is used when a diagram can answer a structural question more clearly than text alone.

---

## Core Principles

### 1. Ask only what matters

A question should be asked when resolving the unknown could materially change the outcome.

Low-impact questions should not unnecessarily interrupt the process.

### 2. Discover before asking

If information can reasonably be obtained through available files, documentation, tools, calculations, tests, or reliable sources, the agent should investigate before asking the user.

### 3. Keep decisions explicit

The agent should distinguish between what the user decided and what the agent discovered or inferred.

For example:

```text
DECISION
We will use Supabase.

FACT
Supabase supports anonymous INSERT operations with Row Level Security.

INFERENCE
An INSERT-only architecture may be sufficient.
```

These are different kinds of information and should not silently become interchangeable.

### 4. Resolve high-impact uncertainty first

Important uncertainties around goals, boundaries, dependencies, constraints, acceptance criteria, and irreversible decisions should receive more attention than easily reversible details.

### 5. Test alternatives when practical

When concrete alternatives can be evaluated through calculations, experiments, prototypes, documentation, or other available evidence, the agent should prefer testing over speculation.

### 6. Keep the working model compact

The model should contain what is necessary to reason about the problem without becoming a transcript of every thought or interaction.

### 7. Never manufacture certainty

Verified facts, inferences, assumptions, and unresolved uncertainty should remain distinguishable.

---

## The Working Model

Visual Grill can represent information using states such as:

```text
FACT
INFERENCE
QUESTION
OPTION
DECISION
CONSTRAINT
ASSUMPTION
DEPENDENCY
REQUIREMENT
UNCERTAINTY
CONTRADICTION
```

A useful dependency pattern is:

```text
FACTS
  ↓
INFERENCES
  ↓
OPTIONS
  ↓
USER DECISIONS
  ↓
CONSEQUENCES
```

The central distinction is:

```text
USER DECISION
      ≠
EXTERNALLY VERIFIED FACT
      ≠
AGENT INFERENCE
```

Maintaining this distinction helps prevent an agent from accidentally presenting its own assumptions as decisions or facts.

---

## Investigation

Investigation can include:

* inspecting project files
* examining existing architecture
* reading documentation
* checking current information
* calculating alternatives
* examining schemas and configurations
* testing concrete possibilities
* comparing competing approaches
* checking dependencies
* verifying assumptions against available evidence

The Skill does **not** claim that something was investigated or verified unless the agent actually performed that investigation.

---

## Visualization

Visualization is used when structure matters.

Examples include:

| Problem structure           | Useful visualization |
| --------------------------- | -------------------- |
| Process                     | Flowchart            |
| Branching choices           | Decision tree        |
| Components and dependencies | Architecture diagram |
| Interactions                | Sequence diagram     |
| Lifecycle                   | State diagram        |
| Relationships               | Graph                |
| Parent-child structure      | Hierarchy            |
| Genuine overlapping sets    | Venn diagram         |

The purpose is not to decorate the answer.

The purpose is to make structure easier to inspect and validate.

A visualization generated by the same reasoning process is **not independent verification**. When deterministic or external validation is available, it should be preferred.

---

## Validation

Validation looks for problems such as:

* contradictions
* missing branches
* unreachable states
* circular dependencies
* duplicated responsibilities
* orphan components
* impossible transitions
* conflicting assumptions
* mutually unsatisfiable requirements
* decisions that depend on unresolved facts
* conclusions presented as facts without sufficient support

If a material issue is discovered, the process should return to the smallest unresolved question or uncertainty that caused it.

```text
EXPOSE ISSUE
     ↓
IDENTIFY UNRESOLVED POINT
     ↓
GRILL
     ↓
INVESTIGATE / ASK
     ↓
UPDATE MODEL
     ↓
RE-VALIDATE
```

A user decision should never be silently changed merely to make the model coherent.

---

## Approval Gate

Visual Grill treats finalization as an explicit checkpoint.

A model is ready to become the basis for the next action when:

1. there is no known material contradiction
2. the user has had a meaningful opportunity to inspect the model
3. the user explicitly approves the model as the basis for proceeding

Silence, lack of objections, or conversational momentum does not automatically count as approval.

If the user rejects or materially changes the model, the process returns to grilling.

---

## Decision Record

When useful, the final model can be captured in a reusable format:

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

The Skill does not require creating a separate document for every task. Artifacts should only be created when they provide practical value.

---

## Example

Instead of immediately implementing:

```text
"Build me a patient feedback system."
```

Visual Grill may first determine:

```text
GOAL
Collect and route patient feedback.

BOUNDARIES
Feedback form only.
No staff dashboard in the initial scope.

DECISION
Use a five-level rating.

FACT
Positive and private feedback require different downstream handling.

DEPENDENCY
Clinic selection affects the review destination.

UNCERTAINTY
Should changing the selected clinic create a new interaction or modify
the previous selection?

QUESTION
Which behavior should be considered the intended correction flow?

ACCEPTANCE CRITERIA
A patient can correct an accidental clinic selection without creating
an ambiguous final record.
```

The important result is not the number of questions asked.

It is that the **decision model becomes inspectable before implementation**.

---

## Installation

An Agent Skill is typically distributed as a directory containing a `SKILL.md` file.

This repository contains:

```text
Visual-Grill/
├── visual-grill/
│   └── SKILL.md
├── README.md
├── LICENSE
└── .gitignore
```

Use the `visual-grill` directory according to the Skill-loading mechanism of the agent or platform you are using.

---

## Usage

Visual Grill is intended for situations such as:

* architecture decisions
* product scope
* workflow design
* system design
* implementation planning
* data modeling
* complex debugging
* competing technical approaches
* decisions with significant dependencies
* designs where assumptions could become expensive to change later

It is less useful for simple, well-defined, low-risk tasks where the desired outcome is already clear.

---

## What It Is Not

Visual Grill is:

* not a deterministic reasoning engine
* not a guarantee of correctness
* not a replacement for domain expertise
* not a rigid checklist
* not a requirement to ask many questions
* not a substitute for real testing or external verification
* not an implementation framework

It is an **instruction-based Agent Skill** intended to influence agent behavior.

Its question selection, modeling, validation, and stopping behavior are therefore still subject to the capabilities and limitations of the agent running it.

---

## Help Test It

Visual Grill is experimental.

The most useful feedback is not simply whether you liked it.

Try to **break it**.

Useful failure reports include:

### Question failures

* It asked something it could have discovered.
* It asked a question that did not affect the outcome.
* It asked too many questions at once.
* It missed an important question.
* It repeated something already settled.
* It failed to recognize an existing decision.

### Model failures

* It confused a fact with an assumption.
* It lost a user decision.
* It retained contradictory decisions.
* It missed a dependency.
* It used stale information.
* It failed to preserve an important alternative.

### Visualization failures

* It chose the wrong diagram.
* It created a diagram that was unnecessary.
* It omitted an important relationship.
* It made the model harder to understand.
* The visualization exposed a contradiction that the process failed to address.

### Validation failures

* It failed to detect a contradiction.
* It declared the model ready too early.
* It continued unnecessarily after the important uncertainty had been resolved.
* It identified a problem but failed to return to the correct unresolved decision.

Positive examples are useful too.

If Visual Grill handles a difficult problem particularly well, that is worth reporting.

---

## Evaluation

When testing the Skill, consider recording:

```text
Problem:

Initial goal:

Questions asked:

Questions that changed the decision:

Questions that did not matter:

Facts discovered:

User decisions:

Assumptions:

Alternatives considered:

Visualization used:

What did the visualization reveal?

Validation findings:

Did the process return to grilling?

Final outcome:

What should the Skill have done differently?
```

Repeated examples can help reveal where the Skill's instructions are strong and where they need refinement.

---

## Status

**Experimental — v0.1**

Visual Grill is intentionally being released as a behavioral experiment.

The objective at this stage is to see how well the approach works across real problems, where it fails, and which parts of the behavior need improvement.

---

## License

MIT License.

See `LICENSE` for details.
