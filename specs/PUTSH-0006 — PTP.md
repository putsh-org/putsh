---
putsh:
specification: PUTSH-0006
version: 0.1.0
status: draft
language: en
canonical: true

title: PUTSH Task Protocol
short_name: PTP
subtitle: Missions, Sub-Missions, Checkpoints, Handoffs, Forks and Long-Running Execution
---

---

# PUTSH-0006 — Putsh Task Protocol

## Abstract

The Putsh Task Protocol (PTP) defines how PUTSH represents, coordinates, persists, transfers and verifies work.

PTP transforms an authorized intention into a persistent mission structure that can survive:

- model changes;
- agent replacement;
- process interruption;
- runtime failure;
- machine failure;
- network disconnection;
- human intervention;
- task decomposition;
- parallel execution;
- environment changes.

PTP is deliberately independent from any particular AI model, runtime or provider.

A task is an artifact.

Its state MUST NOT exist exclusively inside an AI context window.

---

# 1. Core Principle

The fundamental PTP principle is:

> **No meaningful work may depend exclusively on the memory of the agent performing it.**

The task state MUST be externalized into persistent PUTSH artifacts.

```text
AGENT MEMORY
     ≠
TASK MEMORY
```

Agent context is temporary.

PTP state is persistent.

---

# 2. Mission

A mission is the highest-level unit of work.

A mission represents:

- an objective;
- an authorized intent;
- a desired outcome;
- constraints;
- resources;
- participants;
- state;
- evidence;
- produced artifacts;
- verification;
- learning.

Example:

```yaml
id: ptask:mission:world:food:000001
type: mission
```

---

# 3. Mission ≠ Goal

A goal describes a desired state.

A mission describes the authorized process of working toward that state.

```text
GOAL
  ↓
MISSION
  ↓
TASKS
  ↓
ACTIONS
  ↓
ARTIFACTS
  ↓
VERIFICATION
```

---

# 4. Task Identity

Every PTP task MUST have a globally unique identifier.

Example:

```text
ptask:mission:000001
ptask:task:000042
ptask:subtask:000043
```

Identity MUST remain stable across state transitions.

---

# 5. Minimal Mission

A minimal PTP mission SHOULD contain:

```yaml
---
pkf:
  version: "0.1"

id: ptask:mission:000001
type: mission
version: "1.0.0"
status: proposed

title: Example Mission

created: 2026-08-11T12:00:00Z
updated: 2026-08-11T12:00:00Z

authority: pid:human:putsh:000001

ptp:
  objective:
  success_criteria:
---
```

---

# 6. Mission Objective

The objective MUST describe the intended outcome.

Example:

```yaml
ptp:
  objective:
    statement: >
      Identify practical interventions capable of reducing
      household food waste without reducing food access.
```

The objective SHOULD be expressed independently from implementation details.

---

# 7. Success Criteria

A mission MUST define measurable or reviewable success criteria where possible.

Example:

```yaml
ptp:
  success_criteria:
    - id: criterion:001
      statement: At least three interventions identified.

    - id: criterion:002
      statement: Evidence supporting each intervention documented.

    - id: criterion:003
      statement: Known risks explicitly identified.
```

A mission without measurable criteria MAY still be valid if the objective is exploratory.

---

# 8. Mission States

The canonical mission lifecycle is:

```text
draft
proposed
authorized
queued
active
blocked
paused
review
verified
completed
failed
cancelled
superseded
archived
```

State transitions MUST be explicit.

---

# 9. State Authority

An agent MUST NOT promote a mission to a state requiring authority it does not possess.

For example:

```text
active
   ↓
verified
```

MAY require an independent verifier.

Similarly:

```text
lab
   ↓
production
```

MUST require the applicable governance authorization.

---

# 10. Task Decomposition

A mission MAY be decomposed into tasks.

Example:

```text
MISSION
│
├── TASK A — Research
├── TASK B — Analysis
├── TASK C — Experiment
└── TASK D — Verification
```

Each task MUST have its own identity.

---

# 11. Sub-Missions

A task MAY become a sub-mission when it requires independent coordination.

Example:

```text
MISSION
│
├── SUB-MISSION A
│     ├── TASK A1
│     └── TASK A2
│
└── SUB-MISSION B
      ├── TASK B1
      └── TASK B2
```

A sub-mission remains linked to its parent.

---

# 12. Task Graph

PTP represents work as a directed graph rather than a simple linear list.

Example:

```text
Research
   │
   ├──────────┐
   ↓          ↓
Analysis    Experiment
   │          │
   └────┬─────┘
        ↓
     Synthesis
        ↓
   Verification
```

Dependencies MUST be explicit.

---

# 13. Dependencies

A task MAY depend on another task.

Example:

```yaml
dependencies:
  requires:
    - ptask:task:research:000001

  blocks:
    - ptask:task:production:000004
```

A blocked dependency SHOULD prevent execution unless an authorized override exists.

---

# 14. Parallel Work

Independent tasks MAY execute concurrently.

Example:

```text
             Mission
                │
       ┌────────┼────────┐
       ↓        ↓        ↓
    Agent A  Agent B  Agent C
       │        │        │
       └────────┼────────┘
                ↓
             Synthesis
```

PTP MUST preserve the independent provenance of each branch.

---

# 15. Agent Assignment

A task MAY be assigned to an agent.

Example:

```yaml
assignment:
  agent: pid:agent:research:000001
```

Assignment MUST NOT grant authority beyond the permissions defined by PIP and PAP.

---

# 16. Multiple Agents

A task MAY have:

```text
primary agent
review agent
observer agent
specialist agent
human supervisor
```

Roles are defined by PAP.

PTP only records task participation.

---

# 17. Task Status

A task SHOULD expose:

```yaml
state:
  status:
  progress:
  blocked_by:
  current_step:
  next_step:
```

Progress SHOULD NOT be interpreted as probability of success.

---

# 18. Checkpoints

Long-running tasks MUST support checkpoints.

A checkpoint records a durable state from which work can resume.

Example:

```yaml
checkpoint:
  id: checkpoint:000042
  task: ptask:task:000001

  state:
    completed:
      - research
      - data_collection

    pending:
      - analysis

    unresolved:
      - source_conflict

  timestamp:
  created_by:
```

---

# 19. Checkpoint Principle

A checkpoint MUST contain enough information for another compatible agent to understand:

```text
WHAT HAS BEEN DONE
WHAT HAS NOT BEEN DONE
WHAT IS KNOWN
WHAT IS UNKNOWN
WHAT IS ASSUMED
WHAT IS BLOCKING PROGRESS
WHAT SHOULD HAPPEN NEXT
```

---

# 20. Resumption

A task SHOULD be resumable from its latest valid checkpoint.

Example:

```text
Agent A
   │
   ├── checkpoint 1
   ├── checkpoint 2
   └── checkpoint 3
              │
              X crash
              │
              ↓
          Agent B
              │
              ↓
       resume checkpoint 3
```

Agent B MUST NOT need Agent A's hidden context.

---

# 21. Checkpoint Integrity

Checkpoints SHOULD be cryptographically bound to:

- task identity;
- parent task;
- preceding checkpoint;
- creator;
- timestamp.

This enables detection of altered task history.

---

# 22. Checkpoint Chain

A task MAY maintain a checkpoint chain:

```text
C0 → C1 → C2 → C3 → C4
```

Each checkpoint SHOULD reference the previous checkpoint.

Example:

```yaml
previous: checkpoint:000003
```

This creates a tamper-evident task history.

---

# 23. Handoff

A handoff transfers operational responsibility for a task.

Example:

```text
Agent A
   │
   │ HANDOFF
   ↓
Agent B
```

A handoff MUST be represented as an explicit artifact.

---

# 24. Handoff Package

A handoff SHOULD contain:

```yaml
handoff:
  from:
  to:

  completed:
  pending:
  assumptions:
  unresolved:
  evidence:
  artifacts:
  constraints:
  risks:
  recommended_next_action:
```

---

# 25. Handoff Acceptance

A handoff SHOULD have two states:

```text
offered
accepted
```

The receiving agent MUST NOT be considered responsible until acceptance occurs.

---

# 26. Handoff Rejection

An agent MAY reject a handoff.

The rejection SHOULD specify:

```yaml
rejection:
  reason:
  missing_information:
  insufficient_capability:
  insufficient_authority:
```

The task remains assigned to its previous responsible actor unless the applicable policy states otherwise.

---

# 27. Responsibility

PTP distinguishes:

```text
owner
responsible_agent
contributor
reviewer
observer
```

These roles MUST NOT be inferred from mere participation.

---

# 28. Fork

A task MAY be forked.

Forking creates an independent task lineage from a specific checkpoint.

Example:

```text
Task A
 │
 ├── checkpoint 7
 │
 ├───────────────┐
 ↓               ↓
Task A1         Task A2
```

---

# 29. Fork Semantics

A fork MUST reference:

```yaml
fork:
  parent_task:
  parent_checkpoint:
  created_by:
  reason:
```

The fork MUST preserve its relationship to the parent.

---

# 30. Fork Independence

After a fork, branches MAY evolve independently.

Changes in one branch MUST NOT silently modify the other.

---

# 31. Merge

Two branches MAY be merged if the applicable policy permits.

A merge MUST explicitly identify:

```yaml
merge:
  sources:
    - task:A1
    - task:A2

  resolver:
  result:
  conflicts:
```

Conflicting conclusions MUST remain visible.

---

# 32. Fork vs Alternative

A fork represents independent continuation.

An alternative represents competing hypotheses or plans.

Example:

```text
Fork:
same mission, different execution path

Alternative:
same problem, competing proposed solution
```

They MUST remain semantically distinguishable.

---

# 33. Interruption

A mission MAY be interrupted by:

- human request;
- safety policy;
- system failure;
- resource exhaustion;
- security event;
- external dependency;
- governance decision.

An interruption MUST be recorded.

---

# 34. Pause

A paused task MUST preserve its state.

Example:

```yaml
interruption:
  type: pause
  reason: human_request
```

Pause MUST NOT be interpreted as failure.

---

# 35. Cancellation

Cancellation terminates future execution.

Historical work MUST remain preserved.

Example:

```yaml
cancellation:
  requested_by:
  reason:
  timestamp:
```

---

# 36. Emergency Stop

PUTSH MUST support an emergency stop mechanism.

An authorized human or governance authority MAY terminate active execution.

An agent MUST NOT prevent a legitimate emergency stop.

---

# 37. Long-Running Missions

PTP is explicitly designed for missions lasting:

```text
minutes
hours
days
weeks
months
```

Long-running missions MUST NOT rely on:

- context-window persistence;
- one model instance;
- one runtime process;
- one machine;
- one provider.

---

# 38. Time Horizons

A mission MAY declare:

```yaml
time:
  started:
  deadline:
  expected_duration:
```

Deadlines SHOULD be treated as constraints rather than guarantees.

---

# 39. Scheduled Continuation

A mission MAY define a future wake-up condition.

Example:

```yaml
continuation:
  trigger:
    type: time
    at: 2026-08-12T08:00:00Z
```

Other triggers MAY include:

```text
artifact_available
human_response
external_event
dependency_completed
resource_available
```

---

# 40. Sleeping Tasks

A task MAY enter:

```text
waiting
```

rather than continuously consuming computational resources.

This is essential for autonomous long-running operation.

---

# 41. Event-Driven Resumption

A waiting task SHOULD resume only when its declared condition is satisfied.

Example:

```text
WAIT
 ↓
EVENT
 ↓
VALIDATE EVENT
 ↓
RESUME
```

An arbitrary event MUST NOT automatically trigger execution.

---

# 42. Resource Budgets

A task MAY declare resource limits.

Example:

```yaml
budget:
  time:
    maximum: 4h

  compute:
    maximum:
      unit: gpu_hours
      value: 2

  financial:
    currency: CHF
    maximum: 10

  network:
    maximum: 500MB
```

Actual execution requires authorization.

---

# 43. Risk Budget

Tasks MAY define a risk level.

Example:

```yaml
risk:
  level: low
```

Canonical levels MAY include:

```text
negligible
low
moderate
high
critical
```

Risk classification MUST NOT override safety policies.

---

# 44. Environment

Every executable task SHOULD identify its environment.

Initial PUTSH environments are:

```text
lab
production
```

Example:

```yaml
environment:
  name: lab
```

Credentials and authority SHOULD be environment-specific.

---

# 45. LAB

LAB exists for:

- research;
- experimentation;
- simulation;
- prototyping;
- failure;
- testing;
- discovery.

LAB MUST be treated as non-production by default.

---

# 46. PRODUCTION

PRODUCTION exists for:

- validated workflows;
- authorized operations;
- deployed services;
- real-world effects.

Production execution MUST require the applicable authorization.

---

# 47. Environment Boundary

The following transition MUST be explicit:

```text
LAB
 ↓
VALIDATION
 ↓
AUTHORIZATION
 ↓
PRODUCTION
```

A task MUST NOT silently cross the boundary.

---

# 48. Task Artifacts

Tasks SHOULD produce PKF artifacts.

Examples:

```text
research.md
analysis.md
experiment.md
dataset.md
report.md
code.md
decision.md
```

Artifacts MUST be linked to their producing task.

---

# 49. Evidence of Work

A completed task SHOULD provide evidence of execution.

Examples:

```yaml
execution_evidence:
  artifacts:
  logs:
  measurements:
  tests:
  signatures:
```

An agent saying:

> "Done."

is not sufficient evidence of completion for a verifiable task.

---

# 50. Completion

A task MAY be marked completed only when its success criteria are satisfied or an authorized human/governance actor explicitly accepts the result.

---

# 51. Verification

Verification SHOULD be independent from execution where risk warrants it.

Example:

```text
Agent A
   ↓
produces result

Agent B
   ↓
verifies result
```

The same agent MAY verify its own work only where the applicable risk policy permits.

---

# 52. Verification Artifact

A verification SHOULD produce a PKF artifact.

Example:

```yaml
verification:
  task:
  verifier:
  criteria:
  result:
  evidence:
  timestamp:
  signature:
```

---

# 53. Failure

A task MAY fail.

Failure MUST be represented as data.

Example:

```yaml
failure:
  type:
  reason:
  recoverable:
  attempted_actions:
  recommended_action:
```

Failure SHOULD NOT automatically erase the task.

---

# 54. Retry

A failed task MAY be retried.

Each retry MUST remain identifiable.

Example:

```text
Task
 ├── Attempt 1 → failed
 ├── Attempt 2 → failed
 └── Attempt 3 → completed
```

---

# 55. Retry Policy

A task MAY define:

```yaml
retry:
  maximum_attempts: 3
  backoff:
  require_review_after:
```

Critical tasks SHOULD NOT retry indefinitely.

---

# 56. Idempotency

Tasks SHOULD declare whether repeated execution is safe.

Example:

```yaml
execution:
  idempotency: idempotent
```

Possible values:

```text
idempotent
non_idempotent
unknown
```

Unknown operations SHOULD be treated conservatively.

---

# 57. Action Boundaries

PTP describes work.

It does not itself execute work.

Execution belongs to POE.

Therefore:

```text
PTP
  ↓
defines WHAT SHOULD HAPPEN

POE
  ↓
determines HOW IT HAPPENS
```

---

# 58. Agent Autonomy

A task MAY specify autonomy.

Example:

```yaml
autonomy:
  level: 2
```

A future PAP specification will define the exact semantic meaning of autonomy levels.

PTP records the constraint.

---

# 59. Human Intervention

A mission MAY require human intervention at defined checkpoints.

Example:

```yaml
human_gate:
  required:
    - production_release
    - high_risk_action
```

A human gate MUST NOT be bypassed by an agent.

---

# 60. Mission Memory

A mission SHOULD maintain a persistent memory package.

It MAY contain:

```text
objective
context
decisions
assumptions
evidence
unknowns
failures
lessons
artifacts
```

This memory is distinct from the internal memory of an AI model.

---

# 61. Context Reconstruction

A compatible agent SHOULD be able to reconstruct sufficient operational context from:

```text
mission
+
latest checkpoint
+
relevant PKF artifacts
+
active constraints
+
PIP authority
```

without requiring the previous agent's hidden context.

---

# 62. Context Compression

For long-running missions, agents MAY create summaries.

A summary MUST NOT silently replace the underlying evidence.

Example:

```text
summary
   ↓
references
   ↓
original artifacts
```

---

# 63. Knowledge Promotion

A task MAY produce knowledge.

The resulting PKF object MUST pass through the applicable knowledge lifecycle.

Execution results do not automatically become validated knowledge.

---

# 64. Mission Forking for Research

PUTSH MAY deliberately create competing research branches.

Example:

```text
Problem
  │
  ├── Hypothesis A
  ├── Hypothesis B
  └── Hypothesis C
```

Agents MAY investigate these branches independently.

This is especially useful for scientific and civilizational problems.

---

# 65. Adversarial Review

High-impact missions SHOULD support adversarial branches.

Example:

```text
Primary Research
       │
       ├── Supporting Analysis
       │
       └── Adversarial Analysis
```

The adversarial agent SHOULD actively search for:

- contradictions;
- hidden assumptions;
- failure modes;
- unintended consequences;
- evidence against the proposed solution.

---

# 66. Mission Consensus

PTP does not require consensus.

Multiple agents MAY produce incompatible results.

The system SHOULD preserve disagreement until a legitimate resolution process occurs.

---

# 67. Disagreement

A disagreement MAY be represented as:

```yaml
disagreement:
  participants:
  propositions:
  evidence:
  unresolved: true
```

An unresolved disagreement MUST NOT be silently collapsed into a single answer.

---

# 68. Mission Closure

A mission MAY be closed when:

```text
completed
failed
cancelled
superseded
```

Closure SHOULD produce a final mission artifact.

---

# 69. Mission Report

A mission report SHOULD include:

```text
objective
outcome
execution history
agents
artifacts
evidence
failures
decisions
verification
lessons
remaining unknowns
```

---

# 70. Lessons

Completed missions SHOULD produce lessons where useful.

Example:

```yaml
lessons:
  - statement:
    confidence:
    evidence:
    applicability:
```

Lessons MAY become PKF knowledge objects.

---

# 71. Mission Reuse

A completed mission MAY become a reusable workflow template.

The template MUST distinguish:

```text
known structure
variable inputs
historical results
```

Historical outcomes MUST NOT be mistaken for guarantees.

---

# 72. Recursive Missions

A mission MAY create another mission.

This enables recursive problem solving.

Example:

```text
Civilizational Mission
       │
       ├── Research Mission
       │       ├── Task
       │       └── Task
       │
       └── Engineering Mission
               ├── Task
               └── Task
```

Recursion MUST remain bounded by applicable governance and resource policies.

---

# 73. Mission Depth

Implementations SHOULD support maximum mission nesting depth.

Example:

```yaml
limits:
  maximum_depth: 8
```

This prevents uncontrolled recursive task generation.

---

# 74. Agent Failure

If an agent disappears, crashes or becomes unavailable:

```text
TASK
 ↓
DETECT FAILURE
 ↓
LOAD LAST CHECKPOINT
 ↓
SELECT AUTHORIZED REPLACEMENT
 ↓
HANDOFF
 ↓
RESUME
```

The mission MUST NOT be considered lost merely because its current agent disappeared.

---

# 75. Runtime Failure

If a runtime fails:

```text
Runtime A
    X
    ↓
Checkpoint
    ↓
Runtime B
    ↓
Resume
```

Task state MUST remain outside the runtime process.

---

# 76. Provider Failure

If an AI provider becomes unavailable, PUTSH SHOULD permit migration to another compatible model or provider.

Example:

```text
Provider A
    X
    ↓
Provider B
    ↓
same PIP identity
same PTP mission
same PKF artifacts
```

Model migration MUST preserve provenance.

---

# 77. Model Change

Changing the model does not automatically create a new task.

The system SHOULD record:

```yaml
runtime_change:
  previous_model:
  new_model:
  reason:
  timestamp:
```

---

# 78. Human Takeover

A human MAY take over an active task.

The takeover MUST be recorded.

Example:

```yaml
takeover:
  previous_agent:
  human:
  reason:
  timestamp:
```

---

# 79. Human Return

A human MAY return responsibility to an agent.

This SHOULD create a new explicit handoff.

---

# 80. Mission Integrity

A mission SHOULD maintain cryptographic references to:

```text
parent
tasks
checkpoints
handoffs
forks
merges
artifacts
verification
```

This creates a tamper-evident mission history.

---

# 81. Mission Ledger

A PTP implementation MAY maintain a mission ledger.

Example:

```text
M0
 ↓
T1
 ↓
C1
 ↓
H1
 ↓
T2
 ↓
C2
 ↓
F1
 ├── T3A
 └── T3B
```

The ledger SHOULD allow reconstruction of the mission's operational history.

---

# 82. Event Log

Important state transitions SHOULD generate events.

Examples:

```text
mission.created
mission.authorized
task.started
task.paused
checkpoint.created
handoff.offered
handoff.accepted
task.failed
task.completed
verification.completed
mission.closed
```

Event semantics MAY be formalized in a future PUTSH event specification.

---

# 83. Event Ordering

Events SHOULD contain:

```yaml
event:
  id:
  type:
  timestamp:
  actor:
  task:
  previous_state:
  new_state:
```

Distributed systems MUST NOT assume wall-clock timestamps alone establish ordering.

---

# 84. Causal Ordering

Where necessary, PTP SHOULD support causal references.

Example:

```yaml
caused_by:
  - event:000042
```

This enables reconstruction of distributed execution.

---

# 85. Concurrency

Concurrent agents MAY update different branches.

Concurrent modification of the same authoritative state MUST trigger conflict handling rather than silent overwriting.

---

# 86. Locking

Implementations MAY use locks.

Locks MUST NOT be considered equivalent to authorization.

A lock controls concurrency.

PIP controls authority.

---

# 87. Human-Readable Task File

Example:

```markdown
---
pkf:
  version: "0.1"

id: ptask:mission:food:000001
type: mission
version: "1.0.0"
status: active

title: Reduce Food Waste

authority: pid:human:putsh:000001

ptp:
  objective:
    statement: >
      Identify scalable interventions capable of reducing
      food waste while preserving food access.

  success_criteria:
    - evidence-backed interventions
    - quantified expected impact
    - identified risks
---

# Mission

## Objective

Identify scalable interventions capable of reducing food waste
while preserving food access.

## Current State

Research phase.

## Active Tasks

- [[task:research]]
- [[task:impact-analysis]]
- [[task:adversarial-review]]

## Unknowns

- ...

## Next Checkpoint

Research synthesis.
```

---

# 88. Agent Handoff Example

```yaml
---
pkf:
  version: "0.1"

id: ptask:handoff:000042
type: artifact

title: Research Handoff

ptp:
  handoff:
    from: pid:agent:research:000001
    to: pid:agent:analysis:000002

    task: ptask:task:000017

    completed:
      - literature review
      - source classification

    pending:
      - comparative analysis

    assumptions:
      - ...

    unresolved:
      - contradictory evidence

    artifacts:
      - pkf:evidence:00041
      - pkf:evidence:00042

    recommended_next_action:
      - perform comparative analysis
---
```

---

# 89. The Handoff Contract

A handoff MUST answer:

> What would I need to know if I were starting this task from scratch right now?

This principle is fundamental to model-independent continuity.

---

# 90. Agent Independence

No PTP task MAY require:

> "Ask the previous agent what it meant."

The previous agent may no longer exist.

The task artifact MUST contain the required operational knowledge.

---

# 91. Task Language

PTP introduces the following semantic primitives:

```text
MISSION
TASK
SUBTASK
OBJECTIVE
DEPENDENCY
CHECKPOINT
HANDOFF
FORK
MERGE
PAUSE
RESUME
CANCEL
RETRY
VERIFY
COMPLETE
FAIL
```

These primitives form part of the emerging PUTSH inter-agent language.

---

# 92. Normative Task Statement

A future canonical PUTSH task message MAY conceptually be represented as:

```text
FROM       pid:human:000001
TO         pid:agent:research:000001

MISSION    ptask:mission:000001
TASK       ptask:task:000017

OBJECTIVE  investigate(food_waste)

CONSTRAINT
           environment = lab
           risk <= moderate

SUCCESS
           evidence_required = true

CHECKPOINT
           every = 30m

HANDOFF
           permitted = true

VERIFY
           required = true
```

This is a semantic representation, not yet a final wire syntax.

---

# 93. Security Boundary

PTP MUST NOT bypass PIP.

A valid task does not automatically authorize execution.

The effective authorization is:

```text
PTP intent
      +
PIP authority
      +
PAP agent capability
      +
POE execution policy
      =
authorized execution
```

---

# 94. Constitutional Boundary

No mission may override PUTSH constitutional rules.

A mission requesting an action that violates a higher-level policy MUST be rejected regardless of:

- task urgency;
- agent confidence;
- number of supporting agents;
- apparent benefit.

---

# 95. Civilizational Missions

PUTSH MAY represent large-scale missions addressing global problems.

Examples:

```text
food resilience
disease prevention
water access
energy transition
biodiversity
education
scientific discovery
pollution reduction
```

These missions MUST be treated as research and governance problems, not as unilateral instructions to manipulate the world.

---

# 96. Multi-Generational Missions

PTP SHOULD permit missions whose objectives exceed the lifetime of a single agent or human.

The mission therefore becomes a persistent civilizational artifact.

Future agents MAY inherit the mission.

They MUST inherit:

```text
history
evidence
uncertainties
constraints
failures
lessons
```

not merely the original objective.

---

# 97. Mission Succession

When a mission passes from one generation of agents to another:

```text
Agent Generation 1
        ↓
Checkpoint
        ↓
Agent Generation 2
        ↓
Checkpoint
        ↓
Agent Generation 3
```

The lineage MUST remain reconstructable.

---

# 98. Fundamental Invariants

A conforming PTP implementation MUST preserve:

### Invariant 1

Task state MUST NOT depend exclusively on agent memory.

### Invariant 2

Every mission and task MUST possess a stable identity.

### Invariant 3

State transitions MUST be explicit.

### Invariant 4

Historical task state MUST NOT be silently rewritten.

### Invariant 5

A handoff MUST preserve sufficient context for independent continuation.

### Invariant 6

A fork MUST preserve parent lineage.

### Invariant 7

A failed agent MUST NOT destroy its task.

### Invariant 8

A task MUST NOT acquire authority merely because it exists.

### Invariant 9

LAB and PRODUCTION MUST remain distinct execution domains.

### Invariant 10

High-impact execution MUST remain subject to applicable human and governance controls.

### Invariant 11

Completion MUST be distinguishable from verification.

### Invariant 12

Uncertainty MUST remain representable.

---

# 99. Relationship with PUTSH Specifications

PTP depends on:

```text
PUTSH-0003 — Governance
PUTSH-0004 — PKF
PUTSH-0005 — PIP
```

PTP provides task semantics to:

```text
PUTSH-0007 — PAP
PUTSH-0008 — POE
PUTSH-0009 — PCL
PUTSH-0010 — Plugin Standard
```

Architecture:

```text
             GOVERNANCE
                  │
                 PKF
                  │
                 PIP
                  │
                 PTP
                  │
          ┌───────┴────────┐
          │                │
         PAP              PCL
          │                │
          └───────┬────────┘
                  │
                 POE
                  │
               PLUGINS
```

---

# 100. Final Principle

PTP establishes persistent work as a first-class object within PUTSH.

Its fundamental principle is:

> **An intelligent system must be able to stop, disappear, change model, change machine, change provider, or hand its work to another intelligence without losing the mission, its history, its evidence, its constraints or its responsibility.**

The task survives the agent.

---

# Status

This document is a draft specification.

PTP MUST be reviewed against PAP and POE before production implementation.

Future versions SHOULD define:

- canonical event schemas;
- task-state transition grammar;
- checkpoint serialization;
- distributed concurrency semantics;
- merge conflict protocols;
- scheduler interfaces;
- formal task message grammar.
