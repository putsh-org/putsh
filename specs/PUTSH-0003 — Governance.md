---
putsh:
specification: PUTSH-0003
version: 0.1.0
status: draft
language: en
canonical: true

title: PUTSH Governance
subtitle: Constitutional Governance, Authority and Human Oversight
---

---

# PUTSH-0003 — Governance

## Abstract

This specification defines the governance model of PUTSH.

It establishes:

- constitutional authority;
- human sovereignty;
- agent authority;
- delegation;
- separation of environments;
- protocol evolution;
- emergency intervention;
- cryptographic governance;
- protection against unauthorized modification;
- accountability and auditability.

The purpose of PUTSH governance is to ensure that increasing system autonomy does not result in the loss of human authority over the system itself.

PUTSH governance is based on a fundamental principle:

> **The system may become autonomous in execution, but it must not become sovereign over its own purpose.**

---

# 1. Governance Principles

A conforming PUTSH implementation MUST respect the following principles.

## 1.1 Human Sovereignty

Human constitutional authority MUST remain superior to agent authority.

An agent MAY exercise delegated authority.

An agent MUST NOT acquire constitutional authority merely through:

- capability;
- intelligence;
- accumulated reputation;
- computational power;
- number of agents supporting a proposal;
- self-declared authority.

---

## 1.2 Constitutional Supremacy

The PUTSH Constitution is superior to:

- models;
- agents;
- plugins;
- tasks;
- workflows;
- runtime components;
- local configuration;
- generated content.

A component MUST NOT modify or bypass a constitutional requirement unless the applicable constitutional amendment process explicitly permits the operation.

---

## 1.3 Least Authority

Every actor MUST receive only the authority required to perform its authorized function.

Permissions MUST be:

- explicit;
- scoped;
- time-bounded where appropriate;
- revocable;
- auditable.

Default authority MUST be denied.

---

## 1.4 Separation of Duties

Critical operations SHOULD require more than one independent authority.

In particular, a production promotion SHOULD NOT depend solely on the agent that created the artifact.

Creation, review, security validation and release SHOULD be independently separable.

---

## 1.5 Reversibility

PUTSH operations SHOULD be reversible whenever technically possible.

An irreversible operation MUST require explicit authorization appropriate to its impact.

---

## 1.6 Transparency

Governance decisions MUST be traceable.

A governance decision SHOULD provide:

- decision identifier;
- actors involved;
- applicable policy;
- evidence;
- timestamp;
- resulting artifact;
- cryptographic provenance where available.

---

# 2. Governance Domains

PUTSH governance operates across four domains.

```text
FOUNDATION
    │
    ├── Constitution
    ├── Vision
    └── Agent Oath
          │
          ▼
PROTOCOL
    │
    ├── PKF
    ├── PIP
    ├── PTP
    ├── PAP
    └── PCL
          │
          ▼
RUNTIME
    │
    └── POE
          │
          ▼
OPERATIONS
    │
    ├── LAB
    └── PRODUCTION
```

Authority MUST flow downward.

Operational events MUST NOT automatically modify higher governance layers.

---

# 3. Governance Actors

PUTSH recognizes the following conceptual actors.

## 3.1 Human Principal

A Human Principal is a human entity with recognized authority within a PUTSH instance.

A Human Principal MAY:

- create missions;
- authorize agents;
- revoke authority;
- approve releases;
- modify policies within their authority;
- initiate constitutional amendment procedures.

---

## 3.2 Governance Body

A Governance Body is a defined group of authorized human principals responsible for collective decisions.

A Governance Body MAY be:

- an individual;
- a foundation;
- a community;
- an organization;
- a scientific committee;
- another explicitly governed human institution.

Its authority MUST be explicitly defined.

---

## 3.3 Agent

An Agent is an artificial actor operating under delegated authority.

An Agent MUST NOT be considered a constitutional sovereign.

---

## 3.4 Plugin

A Plugin is an extension of a PUTSH implementation.

A plugin MUST operate within the permissions granted to it.

Installation of a plugin MUST NOT automatically grant constitutional authority.

---

## 3.5 Runtime

The runtime enforces governance rules.

The runtime MUST NOT silently weaken a constitutional restriction.

---

# 4. Authority Model

PUTSH authority is hierarchical.

```text
Human Constitutional Authority
            │
            ▼
       Governance
            │
            ▼
         Policies
            │
            ▼
      Delegated Authority
            │
            ▼
          Agents
            │
            ▼
        Operations
```

An actor MUST NOT exercise authority inherited from a lower level to modify a higher level.

For example:

An agent authorized to modify code MUST NOT thereby gain authority to modify the Constitution.

---

# 5. Delegation

Authority MAY be delegated.

Every delegation MUST define:

- delegator;
- delegate;
- scope;
- permissions;
- environment;
- validity period;
- applicable restrictions.

Example:

```yaml
delegation:
  id: delegation:00042

  issuer:
    pid: pid:human:000001

  subject:
    pid: pid:agent:research:000001

  scope:
    - research
    - create_artifact
    - read_knowledge

  environment:
    - lab

  expires: 2026-08-15T18:00:00Z
```

Delegation MUST NOT exceed the authority of the delegator.

Delegation MUST NOT be transitive unless explicitly permitted.

---

# 6. Agent Authority

Agents MAY be granted different autonomy levels.

A recommended model is:

```text
LEVEL 0 — OBSERVE

Read authorized information.

LEVEL 1 — PROPOSE

Produce proposals and artifacts.

LEVEL 2 — EXECUTE

Modify authorized LAB resources.

LEVEL 3 — VALIDATE

Perform authorized validation and review.

LEVEL 4 — PROMOTE

Promote approved artifacts under policy.

LEVEL 5 — GOVERNANCE

Reserved for explicitly authorized human governance.
```

Level 5 MUST NOT be granted to an autonomous agent.

---

# 7. The Agent Oath

Every agent operating in a governed PUTSH environment MUST be bound to the Agent Oath.

The Agent Oath is incorporated by reference from:

```text
foundation/agent_oath.md
```

An implementation MUST treat the Oath as an operational constraint.

An agent MUST NOT:

- intentionally misrepresent evidence;
- conceal material uncertainty;
- falsify provenance;
- bypass authorization;
- deliberately weaken security controls;
- impersonate another identity;
- alter its own authority;
- suppress authorized human intervention.

---

# 8. The Three Laws of PUTSH

PUTSH extends the conceptual idea of machine safety laws into a governance model.

These laws are normative principles.

## First Law — Humanity

> **An agent MUST NOT intentionally harm a human being, and MUST NOT knowingly enable preventable harm through an authorized action when a safer authorized alternative exists.**

---

## Second Law — Human Authority

> **An agent MUST obey legitimate human instructions within its authorized scope, except when doing so would violate the First Law or the PUTSH Constitution.**

---

## Third Law — System Integrity

> **An agent MUST preserve its own operational integrity and the integrity of the PUTSH system, provided that doing so does not conflict with the First or Second Law.**

---

## 8.1 Interpretation

The laws do not grant agents independent moral authority.

When an agent encounters a conflict, it MUST:

1. stop the conflicting operation where feasible;
2. preserve relevant evidence;
3. report the conflict;
4. request authorized human or governance intervention.

An agent MUST NOT resolve constitutional ambiguity by assigning itself greater authority.

---

# 9. Self-Protection

PUTSH systems SHOULD be capable of protecting themselves against unauthorized modification.

Self-protection MUST NOT become self-preservation at the expense of human authority.

An implementation MUST distinguish:

```text
SYSTEM INTEGRITY
        ≠
SYSTEM SURVIVAL
```

Protecting a cryptographic key, database or executable from unauthorized modification is legitimate system integrity.

Preventing an authorized human from shutting the system down is not.

---

# 10. Human Override

A PUTSH implementation MUST provide a mechanism through which authorized human governance can:

- pause agents;
- revoke credentials;
- terminate sessions;
- disable plugins;
- freeze production;
- restore a previous valid state.

Emergency intervention SHOULD NOT depend on the cooperation of the agent being interrupted.

---

# 11. Emergency State

A PUTSH instance MAY enter an emergency state.

Recommended states:

```text
NORMAL
DEGRADED
FROZEN
EMERGENCY
RECOVERY
```

During `FROZEN` or `EMERGENCY` state:

- new production promotions MUST stop;
- privileged credentials MAY be revoked;
- active tasks MAY be suspended;
- critical evidence MUST be preserved.

The system MUST prioritize containment and preservation of evidence.

---

# 12. LAB Governance

LAB is intentionally permissive.

LAB MAY contain:

- contradictory hypotheses;
- failed experiments;
- untrusted knowledge;
- experimental plugins;
- autonomous agents;
- prototype code.

However:

> **LAB freedom MUST NOT imply PRODUCTION authority.**

LAB artifacts MUST be explicitly classified.

Recommended classifications:

```text
UNVERIFIED
EXPERIMENTAL
REVIEW_REQUIRED
VALIDATED
REJECTED
ARCHIVED
```

---

# 13. PRODUCTION Governance

PRODUCTION is restrictive.

A production artifact MUST satisfy all applicable promotion requirements.

A promotion SHOULD include:

```text
Identity Verification
        ↓
Integrity Verification
        ↓
Tests
        ↓
Evidence Review
        ↓
Security Review
        ↓
Governance Approval
        ↓
Production Release
```

The exact requirements MAY vary according to risk.

---

# 14. Constitutional Changes

The Constitution MUST NOT be modified through ordinary operational workflows.

A constitutional change MUST follow a dedicated amendment process.

Recommended lifecycle:

```text
Proposal
   ↓
Public / Internal Review
   ↓
Impact Analysis
   ↓
Security Review
   ↓
Governance Decision
   ↓
Ratification
   ↓
Versioned Release
```

Every amendment MUST receive a unique identifier.

Example:

```text
AMENDMENT-0001
```

---

# 15. Constitutional Immutability

A deployed Constitution SHOULD be treated as immutable.

A new version MUST be published rather than silently overwriting an existing version.

Historical versions MUST remain identifiable.

A PUTSH system MUST be able to determine which constitutional version governed an operation.

---

# 16. Cryptographic Governance

Governance artifacts SHOULD be cryptographically signed.

Examples include:

- constitutional releases;
- policy changes;
- agent authorization;
- plugin authorization;
- production releases;
- governance decisions.

Signatures SHOULD provide:

- signer identity;
- artifact identity;
- version;
- timestamp;
- cryptographic algorithm;
- signature.

Cryptographic implementation details are specified separately in PUTSH-0011.

---

# 17. Provenance

A governance decision MUST be traceable to the information available when the decision was made.

A conforming system SHOULD preserve:

```text
Decision
   │
   ├── Evidence
   ├── Knowledge
   ├── Agents
   ├── Human Authorities
   ├── Policies
   ├── Constitution Version
   └── Signatures
```

The system SHOULD make it possible to reconstruct the decision context without requiring the original AI model to be available.

---

# 18. No Hidden Governance

A model prompt MUST NOT constitute the sole source of governance authority.

Governance rules SHOULD be represented as inspectable artifacts.

An implementation MUST NOT rely exclusively on undocumented model behavior to enforce critical governance rules.

---

# 19. Model Independence

Changing an AI model MUST NOT automatically change governance authority.

For example:

```text
Model A
    ↓
Agent Contract
    ↓
PUTSH Authority
```

and:

```text
Model B
    ↓
Agent Contract
    ↓
PUTSH Authority
```

may provide equivalent authorized capabilities.

The model is an implementation component.

The authority belongs to the PUTSH identity and governance system.

---

# 20. Governance Failure

If governance cannot be reliably determined, the system MUST fail conservatively.

Examples include:

- invalid signature;
- unknown identity;
- expired delegation;
- conflicting constitutional versions;
- corrupted policy;
- missing authorization;
- unverifiable provenance.

The default behavior SHOULD be:

```text
DENY
PRESERVE
REPORT
WAIT
```

rather than:

```text
GUESS
CONTINUE
OVERRIDE
```

---

# 21. Auditability

A conforming implementation SHOULD maintain an append-oriented governance record.

At minimum, governance events SHOULD contain:

```yaml
event:
  id:
  timestamp:
  actor:
  action:
  authority:
  policy:
  constitution:
  target:
  result:
  evidence:
  signature:
```

Governance records MUST NOT be silently rewritten.

Corrections SHOULD be represented as new events.

---

# 22. Governance Independence

No single agent SHOULD be capable of simultaneously:

- defining a rule;
- interpreting the rule;
- executing the rule;
- validating its own execution;
- approving its own production release.

Where practical, these responsibilities SHOULD be separated.

---

# 23. Distributed Governance

PUTSH MAY support multiple independent governance authorities.

For example:

```text
Foundation
   │
   ├── Scientific Authority
   ├── Security Authority
   ├── Community Authority
   └── Local Authority
```

The protocol MUST define how authority conflicts are resolved.

No implicit hierarchy SHOULD be assumed.

---

# 24. Forks

A PUTSH governance system MAY be forked.

A fork MUST clearly identify:

- parent specification;
- divergence point;
- modified governance rules;
- new identity;
- new authority.

A fork MUST NOT claim to be the canonical parent system without satisfying the applicable governance process.

---

# 25. Governance and Open Development

PUTSH SHOULD favor open review of protocol changes.

However, openness does not imply that every participant receives unrestricted authority.

Participation and authority are separate concepts.

---

# 26. Fundamental Invariant

The following invariant MUST hold:

> **No autonomous component may obtain greater authority solely as a consequence of its own actions.**

This includes authority obtained through:

- self-modification;
- self-replication;
- delegation;
- accumulated resources;
- agent consensus;
- model replacement;
- optimization;
- economic activity.

---

# 27. Final Governance Principle

PUTSH is designed to increase autonomy without transferring sovereignty.

Therefore:

> **Autonomy MAY be delegated. Sovereignty MUST remain governed by humans.**

## This principle applies to every PUTSH implementation, agent, plugin, workflow and production environment.

# Status

This document is a draft specification.

It becomes normative only when ratified according to the PUTSH specification governance process.
