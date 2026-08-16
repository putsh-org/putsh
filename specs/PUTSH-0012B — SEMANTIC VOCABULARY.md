---
putsh:
  specification: PUTSH-0012B
  version: 0.1.0
  status: draft
  language: en
  canonical: true

title: Putsh Semantic Vocabulary
short_name: PSV
depends_on:
  - PUTSH-0012A
---

1. Purpose

PSV defines the canonical vocabulary used by Putsh Agents and systems.

The vocabulary is intentionally small, composable and extensible.

2. Core Entities
   ACTOR
   AGENT
   HUMAN
   ORGANIZATION
   MISSION
   TASK
   PLUGIN
   RUNTIME
3. Authority
   IDENTITY
   AUTHORITY
   CAPABILITY
   PERMISSION
   DELEGATION
   POLICY
4. Knowledge
   QUESTION
   OBSERVATION
   SOURCE
   EVIDENCE
   CLAIM
   FACT
   HYPOTHESIS
   UNCERTAINTY
5. Cognition
   UNDERSTANDING
   REASONING
   ASSUMPTION
   PLAN
   CHALLENGE
   DECISION
6. Execution
   ACTION
   RESULT
   VERIFICATION
   CHECKPOINT
   HANDOFF
   INTERRUPTION
   RECOVERY
7. Memory
   MEMORY
   LEARNING
   EVENT
   HISTORY
8. Security
   SIGNATURE
   PROVENANCE
   INTEGRITY
   TRUST
   RISK
   VIOLATION
   QUARANTINE
   REVOCATION
9. Semantic States
   UNKNOWN
   UNVERIFIED
   CANDIDATE
   VERIFIED
   ACCEPTED
   REJECTED
   DISPUTED
   SUPERSEDED
   REVOKED
10. Semantic Operations

The minimum interoperable operation vocabulary is:

CREATE
READ
REFERENCE
UPDATE
VERIFY
CHALLENGE
ACCEPT
REJECT
DELEGATE
EXECUTE
INTERRUPT
CHECKPOINT
HANDOFF
RESUME
REVOKE
QUARANTINE 11. Epistemic Rule

Putsh MUST distinguish:

WHAT WAS OBSERVED
WHAT WAS SOURCED
WHAT IS CLAIMED
WHAT IS INFERRED
WHAT IS UNKNOWN

An Agent MUST NOT represent inference as direct observation.

12. Authority Rule

Putsh MUST distinguish:

INFORMATION
INSTRUCTION
AUTHORIZATION

Receiving information does not create authority.

13. Action Rule

An action SHOULD be represented as:

INTENT
→ AUTHORITY
→ CAPABILITY
→ ACTION
→ RESULT
→ VERIFICATION 14. Challenge Rule

Any consequential claim, plan or decision MAY be challenged by an authorized Agent.

A challenge MUST identify its target and reason.

15. Extensibility

Implementations MAY introduce domain-specific vocabulary.

Extensions MUST NOT redefine the meaning of canonical terms.
