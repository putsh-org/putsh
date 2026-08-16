---
putsh:
specification: PUTSH-0004
version: 0.1.0
status: draft
language: en
canonical: true

title: PUTSH Knowledge Framework
short_name: PKF
---

---

# PUTSH-0004 — Putsh Knowledge Framework

## Abstract

The Putsh Knowledge Framework (PKF) defines the canonical representation of knowledge, instructions, evidence, decisions, tasks, memories, identities, policies, and other machine-readable artifacts within a PUTSH system.

PKF uses Markdown as its human-readable document layer and structured metadata as its machine-readable semantic layer.

PKF is designed to function as a common language layer between:

- humans;
- AI models;
- autonomous agents;
- software;
- plugins;
- local runtimes;
- remote runtimes;
- independent PUTSH implementations.

A PKF document MUST remain understandable to a human without requiring a proprietary application.

A PKF document MUST contain sufficient structured information for a conforming implementation to identify, validate, classify and process the document.

---

# 1. Design Objective

PKF is not intended to be merely a file format.

PKF defines a semantic language.

The language consists of:

1. identity;
2. type;
3. state;
4. authority;
5. content;
6. relationships;
7. evidence;
8. provenance;
9. constraints;
10. lifecycle;
11. integrity information.

The same semantic object SHOULD be representable independently of the AI model processing it.

---

# 2. Canonical Representation

The canonical PKF representation is a UTF-8 encoded Markdown document containing:

1. a YAML front matter block;
2. a Markdown body;
3. optional structured PKF blocks.

Canonical structure:

```text
┌──────────────────────────────┐
│ YAML FRONT MATTER            │
│ identity + semantics         │
├──────────────────────────────┤
│ MARKDOWN BODY                │
│ human-readable content       │
├──────────────────────────────┤
│ STRUCTURED BLOCKS            │
│ optional machine semantics   │
└──────────────────────────────┘
```

The Markdown layer MUST NOT contradict the authoritative structured metadata.

---

# 3. Document Identity

Every PKF document MUST possess a globally unique identifier.

Example:

```yaml
id: pkf:knowledge:world:food:000001
```

The identifier MUST remain stable throughout the document lifecycle.

A new conceptual object MUST receive a new identifier.

A modified version of the same object MUST retain its identity and receive a new version.

---

# 4. Required Front Matter

Every canonical PKF document MUST contain:

```yaml
---
pkf:
  version: "0.1"

id:
type:
version:
status:

title:

created:
updated:

authority:
---
```

Example:

```yaml
---
pkf:
  version: "0.1"

id: pkf:knowledge:world:food:000001
type: knowledge
version: "1.0.0"
status: active

title: Global Food Resilience

created: 2026-08-11T10:00:00Z
updated: 2026-08-11T10:00:00Z

authority: putsh:foundation
---
```

---

# 5. PKF Types

PKF defines a common vocabulary of object types.

The initial core types are:

```text
knowledge
problem
question
hypothesis
evidence
claim
decision
mission
task
agent
human
plugin
policy
constitution
protocol
artifact
memory
event
experiment
workflow
release
package
relationship
```

Implementations MAY define additional types.

Custom types MUST declare their parent type.

Example:

```yaml
type: knowledge.medical
extends: knowledge
```

---

# 6. Semantic Separation

PKF MUST distinguish between:

```text
FACT
CLAIM
HYPOTHESIS
OPINION
INFERENCE
DECISION
INSTRUCTION
```

These categories MUST NOT be silently interchangeable.

Example:

```yaml
statement:
  kind: hypothesis
  confidence: 0.62
```

is fundamentally different from:

```yaml
statement:
  kind: fact
```

An AI MUST NOT convert a hypothesis into a fact merely through repetition.

---

# 7. Knowledge State

A PKF object MAY have the following lifecycle:

```text
draft
proposed
experimental
review
validated
active
deprecated
superseded
rejected
archived
```

State transitions MUST be governed by the applicable protocol.

An agent MUST NOT declare an artifact `validated` unless it possesses the authority required by the applicable policy.

---

# 8. Confidence

PKF MAY represent confidence.

Confidence MUST be associated with a specific assertion.

Example:

```yaml
claim:
  id: claim:00042
  confidence: 0.87
```

A confidence value MUST NOT be interpreted as truth probability unless the methodology defining the value explicitly states this.

Therefore:

```yaml
confidence: 0.87
```

means:

> confidence according to the declared methodology.

It does NOT automatically mean:

> 87% probability that the statement is true.

---

# 9. Evidence

Knowledge claims SHOULD reference evidence.

Example:

```yaml
evidence:
  - id: evidence:00017
    type: scientific-study
    source: ...
    accessed: ...
```

Evidence SHOULD include:

- identifier;
- source;
- provenance;
- acquisition date;
- author where known;
- integrity information;
- relevance;
- limitations.

---

# 10. Evidence Hierarchy

PKF does not define one universal hierarchy of truth.

Instead, evidence MUST declare its nature.

Examples:

```text
direct-observation
measurement
experiment
scientific-publication
official-record
primary-source
secondary-source
expert-analysis
agent-generated
human-report
simulation
inference
```

The applicable domain policy MAY define how evidence types are weighted.

---

# 11. Provenance

Every significant PKF artifact SHOULD maintain provenance.

Provenance answers:

```text
WHO
WHAT
WHEN
WHY
FROM WHERE
UNDER WHICH AUTHORITY
```

Example:

```yaml
provenance:
  created_by: pid:agent:research:000001

  based_on:
    - pkf:evidence:00042
    - pkf:knowledge:00017

  method:
    - literature_review
    - comparative_analysis

  timestamp: 2026-08-11T11:32:00Z
```

---

# 12. Relationships

PKF documents MAY reference other PKF objects.

Relationships MUST be explicit.

Example:

```yaml
relationships:
  supports:
    - pkf:evidence:00042

  contradicts:
    - pkf:claim:00031

  derived_from:
    - pkf:knowledge:00017

  supersedes:
    - pkf:knowledge:00011

  depends_on:
    - pkf:protocol:00003
```

This allows PKF to form a knowledge graph without requiring a separate proprietary graph database.

---

# 13. Relationship Vocabulary

The initial relationship vocabulary includes:

```text
supports
contradicts
derived_from
depends_on
references
supersedes
implements
validates
invalidates
requires
contains
part_of
created_by
reviewed_by
approved_by
assigned_to
delegated_to
```

Implementations MAY extend this vocabulary.

Extensions MUST define their semantics.

---

# 14. Active Memory

PKF distinguishes between historical knowledge and active operational memory.

An object MAY declare:

```yaml
memory:
  class: active
```

Active memory SHOULD contain information required for ongoing work.

Examples:

- current task state;
- unresolved questions;
- current assumptions;
- pending decisions;
- active constraints;
- next actions.

Active memory MUST NOT silently overwrite historical knowledge.

---

# 15. Historical Memory

Historical memory represents information that has occurred previously.

Example:

```yaml
memory:
  class: historical
```

Historical information MUST remain distinguishable from current state.

Corrections SHOULD create new events rather than silently rewriting historical records.

---

# 16. World Knowledge

PUTSH MAY maintain a global knowledge namespace.

The canonical conceptual object is:

```text
world
```

Example:

```text
world.md
```

The world knowledge base SHOULD be divided into domains.

Example:

```text
world/
├── health/
├── food/
├── water/
├── energy/
├── climate/
├── biodiversity/
├── education/
├── science/
├── technology/
└── society/
```

`world.md` MAY act as a root index.

It SHOULD NOT contain the entire knowledge base.

---

# 17. Problems

PUTSH treats problems as first-class objects.

Example:

```yaml
---
pkf:
  version: "0.1"

id: pkf:problem:world:000001
type: problem
version: "1.0.0"
status: active

title: Global Food Waste

authority: putsh:foundation
---
```

A problem SHOULD define:

```yaml
problem:
  description:
  affected_systems:
  known_causes:
  constraints:
  desired_outcomes:
  current_state:
  unknowns:
```

A problem is not a task.

A problem describes a condition requiring understanding or intervention.

---

# 18. Questions

Questions MUST be representable independently from their answers.

Example:

```yaml
id: pkf:question:000001
type: question

question:
  text: "How can food waste be reduced without reducing food access?"
```

Questions MAY remain unresolved indefinitely.

An unresolved question MUST NOT be treated as a failure of the system.

---

# 19. Hypotheses

A hypothesis MUST explicitly declare itself as such.

Example:

```yaml
id: pkf:hypothesis:000001
type: hypothesis

hypothesis:
  statement: ...
  assumptions:
    - ...
  predicted_observations:
    - ...
```

A hypothesis MAY become:

```text
supported
weakly-supported
refuted
inconclusive
superseded
```

---

# 20. Claims

Claims represent assertions that may be evaluated.

Example:

```yaml
claim:
  statement: ...
  evidence:
    - evidence:00017
  status: under_review
```

Claims SHOULD NOT be promoted to validated knowledge without the applicable review process.

---

# 21. Decisions

A decision records an authorized choice.

A decision SHOULD contain:

```yaml
decision:
  question:
  options:
  selected:
  rationale:
  evidence:
  authority:
  consequences:
  reversibility:
```

A decision is not automatically a fact.

It records an action or choice made under specific conditions.

---

# 22. Assumptions

Assumptions MUST be explicit where they materially influence reasoning.

Example:

```yaml
assumptions:
  - id: assumption:00001
    statement: ...
    status: unverified
```

Agents SHOULD NOT silently introduce critical assumptions.

---

# 23. Constraints

Constraints MAY be:

```text
technical
legal
ethical
physical
economic
environmental
temporal
constitutional
operational
```

Example:

```yaml
constraints:
  - type: constitutional
    rule: human_override_required

  - type: operational
    rule: no_external_network
```

---

# 24. Instructions

Instructions are executable intentions.

They MUST be distinguishable from knowledge.

Example:

```yaml
instruction:
  action: review
  target: pkf:knowledge:00042
```

An instruction MUST NOT acquire authority solely because it appears in a knowledge document.

The applicable PIP/PTP rules determine whether the instruction is authorized.

---

# 25. Artifacts

An artifact is a produced object.

Examples:

- source code;
- document;
- dataset;
- image;
- model;
- report;
- experiment result;
- configuration.

Artifacts SHOULD include:

```yaml
artifact:
  media_type:
  hash:
  size:
  location:
```

---

# 26. Integrity

A PKF artifact SHOULD contain integrity information.

Example:

```yaml
integrity:
  algorithm: SHA-256
  hash: "..."
```

Cryptographic signatures are defined by PIP and PUTSH-0011.

A hash establishes content integrity.

A signature establishes integrity plus an associated signer identity.

---

# 27. Versioning

PKF objects MUST be versioned.

Example:

```yaml
version: "2.1.0"
```

Semantic versioning SHOULD be used where applicable.

A new version MUST preserve the object's identity.

---

# 28. Immutable History

A version MUST NOT silently replace another version.

Example:

```text
knowledge:0001
    │
    ├── v1.0
    ├── v1.1
    ├── v1.2
    └── v2.0
```

The lineage MUST remain reconstructable.

---

# 29. Content Addressability

Implementations SHOULD support content-addressable references.

Example:

```yaml
content:
  hash:
    algorithm: sha256
    value: ...
```

This enables independent systems to verify that they possess identical content.

---

# 30. Human Readability

PKF documents MUST remain readable using ordinary text tools.

A user SHOULD be able to inspect a PKF document with:

```text
cat
less
vim
VS Code
GitHub
any Markdown viewer
```

without requiring a PUTSH runtime.

---

# 31. Machine Readability

A conforming implementation MUST be able to parse:

- PKF version;
- object identifier;
- object type;
- object version;
- status;
- authority;
- relationships;
- applicable integrity metadata.

Unknown optional fields MUST NOT cause parsing failure.

---

# 32. Forward Compatibility

A PKF implementation MUST ignore unknown optional fields unless the applicable specification declares them mandatory.

This allows future PKF versions to introduce additional semantics.

---

# 33. Canonical Serialization

Implementations SHOULD use deterministic serialization when computing hashes or signatures.

Equivalent semantic objects SHOULD produce equivalent canonical representations before cryptographic processing.

Whitespace and presentation formatting MUST NOT accidentally alter semantic identity.

The canonical serialization algorithm will be defined in a future PKF serialization profile.

---

# 34. Human and Machine Layers

PKF intentionally maintains two simultaneous representations.

```text
                PKF OBJECT
                    │
        ┌───────────┴───────────┐
        │                       │
 HUMAN SEMANTICS          MACHINE SEMANTICS
        │                       │
    Markdown                 YAML / PKF
        │                       │
        └───────────┬───────────┘
                    │
                 Meaning
```

Neither layer is intended to replace the other.

---

# 35. Agent Communication

Agents SHOULD communicate through PKF artifacts rather than relying exclusively on conversational context.

A handoff MAY therefore contain:

```yaml
type: package

package:
  summary:
  completed:
  unresolved:
  assumptions:
  evidence:
  artifacts:
  next_action:
```

This allows an agent based on one model to continue work started by another model.

---

# 36. Model Independence

A PKF document MUST NOT require a particular AI model to interpret its fundamental semantics.

A conforming implementation MAY add model-specific hints.

Such hints MUST be clearly identified as non-canonical.

Example:

```yaml
model_hints:
  provider: optional
  model: optional
```

These fields MUST NOT alter the underlying meaning of the object.

---

# 37. Prompt Independence

PUTSH knowledge MUST NOT depend exclusively on hidden prompts.

Critical knowledge, constraints and authority MUST exist in inspectable PKF artifacts.

Prompts MAY provide implementation-specific instructions.

They MUST NOT be the only representation of constitutional or security-critical rules.

---

# 38. Language Interoperability

PKF's canonical protocol language is English.

Human-readable content MAY exist in any language.

A document MAY provide translations.

Example:

```yaml
language:
  canonical: en

translations:
  - fr
  - de
  - es
```

The canonical semantic fields MUST remain machine-stable.

---

# 39. Translation Integrity

Translations MUST NOT silently modify normative meaning.

Where a translation differs from the canonical version, the canonical version takes precedence.

---

# 40. Namespaces

PKF identifiers SHOULD use namespaces.

Examples:

```text
putsh:
pkf:
pid:
task:
agent:
plugin:
event:
```

Namespaces allow multiple object families to coexist without identifier collisions.

---

# 41. Local Sovereignty

A PKF repository SHOULD be portable.

A user SHOULD be able to copy the complete knowledge repository to another compatible implementation without losing its semantic structure.

PUTSH MUST NOT require a proprietary database format for fundamental knowledge preservation.

---

# 42. Offline Operation

PKF SHOULD support offline operation.

A local implementation MAY continue to:

- read knowledge;
- create knowledge;
- perform research;
- execute tasks;
- create artifacts;
- maintain local provenance.

Synchronization MAY occur later.

---

# 43. Conflict Resolution

Distributed PKF repositories MAY diverge.

PUTSH MUST NOT silently merge conflicting knowledge.

Conflicts SHOULD be represented explicitly.

Example:

```yaml
conflict:
  objects:
    - pkf:claim:0001
    - pkf:claim:0002

  status: unresolved
```

Resolution MUST preserve the conflicting histories.

---

# 44. Knowledge Promotion

Knowledge MAY progress through:

```text
RAW
    ↓
STRUCTURED
    ↓
REVIEWED
    ↓
VALIDATED
    ↓
PUBLISHED
```

Promotion rules are governed by the applicable policy.

An agent MUST NOT promote knowledge beyond its authority.

---

# 45. Knowledge Deprecation

Knowledge MAY become obsolete.

Deprecation SHOULD specify:

```yaml
deprecation:
  reason:
  replaced_by:
  effective:
```

Deprecated knowledge SHOULD remain available for historical analysis.

---

# 46. Knowledge Graph

The complete PKF repository forms a distributed knowledge graph.

```text
                  WORLD
                    │
        ┌───────────┼───────────┐
        │           │           │
      PROBLEM    KNOWLEDGE    QUESTION
        │           │           │
        │       ┌───┴───┐       │
        │       │       │       │
     HYPOTHESIS CLAIM  EVIDENCE  │
        │       │       │       │
        └───────┴───────┴───────┘
                    │
                 DECISION
                    │
                  TASK
                    │
                 ARTIFACT
```

The graph MAY be materialized in:

- files;
- databases;
- indexes;
- graphs;
- distributed stores.

The underlying semantics remain PKF.

---

# 47. The World Model

A PUTSH instance MAY maintain a `world.md` document.

`world.md` SHOULD function as a navigational and semantic root rather than a monolithic database.

Example:

```markdown
# World

## Active Civilizational Problems

- [[problem:food]]
- [[problem:health]]
- [[problem:energy]]
- [[problem:water]]
- [[problem:biodiversity]]

## Knowledge Domains

- [[world:health]]
- [[world:food]]
- [[world:energy]]
- [[world:climate]]
```

The World Model MUST remain open to revision.

No agent may unilaterally define the authoritative state of humanity.

---

# 48. PKF as Inter-AI Language

PKF is designed to allow different AI systems to exchange structured semantic objects.

For example:

```text
Human
  ↓
Claude
  ↓
PKF Artifact
  ↓
Codex
  ↓
PKF Artifact
  ↓
Mistral
  ↓
PKF Artifact
  ↓
Ollama
  ↓
Human
```

The models may differ.

The semantic protocol does not.

---

# 49. Semantic Minimalism

PKF SHOULD prefer explicit, composable primitives over large implicit abstractions.

A small number of reliable primitives is preferable to a large number of ambiguous fields.

---

# 50. Extensibility

PKF MAY be extended.

Extensions MUST:

1. declare their namespace;
2. define their semantics;
3. specify compatibility;
4. avoid redefining existing mandatory semantics.

---

# 51. Security Boundary

PKF is a data and semantic layer.

A PKF document MUST NOT be considered executable merely because it contains instructions.

Execution requires authorization through PIP, PTP, PAP and POE.

This separation is fundamental.

```text
KNOWLEDGE
   ≠
AUTHORITY
   ≠
EXECUTION
```

---

# 52. Fundamental Invariants

A conforming PUTSH implementation MUST preserve the following invariants:

### Invariant 1

A fact MUST NOT be inferred solely from an instruction.

### Invariant 2

An instruction MUST NOT acquire authority solely from its textual presence.

### Invariant 3

A hypothesis MUST remain distinguishable from validated knowledge.

### Invariant 4

Historical records MUST NOT be silently rewritten.

### Invariant 5

An artifact MUST remain traceable to its provenance where provenance is available.

### Invariant 6

A PKF document MUST remain portable between conforming implementations.

### Invariant 7

Unknown optional fields MUST NOT invalidate a document.

### Invariant 8

Cryptographic identity MUST NOT be inferred from document content alone.

---

# 53. Minimal Valid PKF Document

The smallest valid PKF document is:

```markdown
---
pkf:
  version: "0.1"

id: pkf:knowledge:example:000001
type: knowledge
version: "1.0.0"
status: draft

title: Example

created: 2026-08-11T00:00:00Z
updated: 2026-08-11T00:00:00Z

authority: unknown
---

# Example

Knowledge content.
```

---

# 54. Example — Research Knowledge Object

```markdown
---
pkf:
  version: "0.1"

id: pkf:knowledge:mosquito:malaria:000001
type: knowledge
version: "1.0.0"
status: experimental

title: Mosquito-borne disease transmission

created: 2026-08-11T10:00:00Z
updated: 2026-08-11T12:00:00Z

authority: putsh:lab

provenance:
  created_by: pid:agent:research:000001

relationships:
  derived_from:
    - pkf:evidence:000001
    - pkf:evidence:000002
---

# Statement

[Human-readable research content.]

## Classification

- Type: hypothesis
- Status: experimental

## Evidence

- [[evidence:000001]]
- [[evidence:000002]]

## Unknowns

- ...

## Next Research

- ...
```

---

# 55. Compatibility

PKF is designed to operate with:

- local AI systems;
- cloud AI systems;
- command-line agents;
- IDE agents;
- autonomous runtimes;
- human editors;
- distributed repositories.

No particular model is required.

---

# 56. Future Language Layer

PKF is intentionally designed as the foundation for a future formal PUTSH language.

Future specifications MAY define:

- a formal grammar;
- canonical serialization;
- semantic validation;
- a PKF parser;
- a PKF compiler;
- semantic type checking;
- capability declarations;
- machine-to-machine negotiation.

Such developments MUST preserve Markdown compatibility where practical.

---

# 57. Design Direction

PKF is therefore not merely:

> Markdown + YAML.

It is a semantic language whose initial human-readable syntax is Markdown plus structured metadata.

The long-term architecture is:

```text
                  PUTSH LANGUAGE
                        │
              ┌─────────┴─────────┐
              │                   │
          Human Syntax       Machine Syntax
              │                   │
          Markdown              PKF
              │                   │
              └─────────┬─────────┘
                        │
                     Semantics
                        │
                Protocol Runtime
                        │
                     Agents
```

---

# 58. Normative Principle

The central principle of PKF is:

> **Knowledge must remain understandable, portable, attributable, verifiable and usable across different intelligences.**

This includes human intelligences and artificial intelligences.

---

# Status

This document is a draft specification.

PUTSH-0004 MUST be reviewed together with PUTSH-0003 before ratification because governance determines which PKF states and transitions an actor is authorized to perform.
