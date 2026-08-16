---
putsh:
  specification: PUTSH-0012A
  version: 0.1.0
  status: draft
  language: en
  canonical: true

title: Putsh Artifact Model
short_name: PAM
depends_on:
  - PUTSH-0000
  - PUTSH-0001
  - PUTSH-0002
  - PUTSH-0004
  - PUTSH-0005
  - PUTSH-0006
  - PUTSH-0007
  - PUTSH-0008
  - PUTSH-0009
  - PUTSH-0010
  - PUTSH-0011
---

1. Purpose

The Putsh Artifact Model defines the canonical unit of information exchanged, stored, verified, referenced and transformed within a Putsh system.

An Artifact MUST be:

identifiable
typed
versioned
provenance-aware
integrity-protected
machine-readable
human-readable where applicable 2. Universal Model

Every Putsh Artifact consists of:

IDENTITY
TYPE
METADATA
CONTENT
REFERENCES
PROVENANCE
AUTHORITY
INTEGRITY
STATUS
LIFECYCLE 3. Canonical Structure

---

putsh:
type: claim
id: claim-0042
version: 1
status: candidate

identity:
author: agent.research.01
mission: mission-017

authority:
issuer: human.owner
scope: research

provenance:
created: 2026-08-15T12:00:00Z
sources: - artifact://source/001

integrity:
algorithm: ...
hash: ...
signature: ...

references:
evidence: - artifact://evidence/001

---

# Content

Artifact content. 4. Artifact Identity

id MUST uniquely identify the logical Artifact.

version MUST identify its revision.

Changing consequential content MUST create a new version.

5. Artifact Types

An implementation MUST support extensible Artifact types.

Core types include:

mission
task
agent
instruction
question
observation
source
evidence
claim
hypothesis
reasoning
plan
action
result
verification
challenge
decision
memory
checkpoint
handoff
event
policy
plugin 6. References

Artifacts SHOULD reference other Artifacts by stable identifiers.

References MUST NOT require a particular storage system.

artifact://claim/0042
artifact://evidence/0091

is a logical reference, not necessarily a network location.

7. Lifecycle

Artifacts MAY transition through:

draft
candidate
verified
accepted
rejected
superseded
revoked
archived
quarantined

Transitions SHOULD be recorded as events.

8. Immutability

Consequential historical Artifacts SHOULD be append-only.

Modification MUST NOT silently destroy historical state.

9. Trust

Artifact authenticity, integrity and truth MUST remain separate concepts.

AUTHENTIC ≠ TRUE
SIGNED ≠ VERIFIED
VERIFIED ≠ CERTAIN 10. Security

Security requirements defined by PUTSH-0011 apply to all security-relevant Artifacts.

11. Conformance

A Putsh implementation conforms to PAM when it can:

create
identify
parse
reference
version
verify
store
retrieve
and audit

Putsh Artifacts according to this specification.
