---
putsh:
specification: PUTSH-0005
version: 0.1.0
status: draft
language: en
canonical: true

title: PUTSH Identity Protocol
short_name: PIP
subtitle: Identity, Cryptographic Trust, Sessions and Delegated Authority
---

---

# PUTSH-0005 — Putsh Identity Protocol

## Abstract

The Putsh Identity Protocol (PIP) defines identity, authentication, cryptographic trust, sessions, permissions and delegation within PUTSH.

PIP allows humans, AI agents, software components, plugins and PUTSH runtimes to identify themselves and establish authenticated relationships without requiring a centralized identity provider.

PIP is designed for sovereign, interoperable and model-independent operation.

Its primary objective is not merely to identify an actor.

Its objective is to establish:

```text
WHO
    +
WHAT
    +
UNDER WHICH AUTHORITY
    +
WITH WHICH CAPABILITIES
    +
FOR HOW LONG
    +
PROVEN BY WHAT
```

PIP therefore forms the identity and trust layer of the PUTSH inter-intelligence protocol.

---

# 1. Design Principles

PIP MUST follow these principles:

1. Identity MUST be cryptographically verifiable.
2. Authority MUST be distinct from identity.
3. Capability MUST be distinct from authority.
4. Authentication MUST be distinct from authorization.
5. Delegation MUST be explicit.
6. Credentials MUST be revocable.
7. Sessions MUST be bounded.
8. Identity MUST remain portable.
9. A model MUST NOT constitute an identity.
10. A provider MUST NOT be the owner of an agent identity by default.
11. Local operation MUST be supported.
12. Human sovereignty MUST remain superior to agent authority.

---

# 2. Identity Model

PIP defines an identity as a cryptographically bound actor.

The primary identity classes are:

```text
human
agent
runtime
plugin
organization
device
service
key
```

An identity MAY represent a physical, digital or organizational actor.

---

# 3. Identity Identifier

Every PIP identity MUST have a globally unique identifier.

Example:

```text
pid:human:putsh:000001
pid:agent:research:000001
pid:runtime:local:000001
pid:plugin:example:000001
```

The identifier is an identity reference.

It is NOT itself proof of identity.

Proof requires cryptographic authentication.

---

# 4. Identity Document

A PIP identity SHOULD be represented by a PKF-compatible document.

Example:

```yaml
---
pkf:
  version: "0.1"

id: pid:agent:research:000001
type: agent
version: "1.0.0"
status: active

title: Research Agent

identity:
  class: agent

cryptography:
  public_keys:
    - key: key:agent:research:000001
      algorithm: ed25519

authority:
  issuer: pid:human:putsh:000001

capabilities:
  - research
  - knowledge.read
  - artifact.create
---
```

The identity document describes the identity.

The associated private key proves control of the identity.

---

# 5. Identity ≠ Model

An AI model MUST NOT automatically constitute a PIP identity.

For example:

```text
GPT-X
Claude-X
Mistral-X
Llama-X
```

are model identifiers.

They are not identities.

A PUTSH agent MAY use any compatible model while retaining the same PIP identity.

```text
                 PIP IDENTITY
                      │
          ┌───────────┼───────────┐
          │           │           │
        Model A     Model B     Model C
```

This permits model replacement without destroying the agent's identity history.

---

# 6. Identity ≠ Provider

An AI provider MAY be recorded as provenance.

It MUST NOT automatically become the identity authority of a sovereign PUTSH agent.

Example:

```yaml
runtime:
  provider: optional
  model: optional
```

Provider information is metadata.

Cryptographic identity remains independently verifiable.

---

# 7. Cryptographic Keys

A PIP identity MAY possess one or more cryptographic keys.

Keys SHOULD be separated by purpose.

Recommended purposes include:

```text
identity
signing
encryption
authentication
recovery
```

A key MUST NOT be assumed to be valid for every purpose.

---

# 8. Key Identifier

Every managed key SHOULD possess a unique identifier.

Example:

```text
key:agent:research:signing:000001
```

A key record SHOULD contain:

```yaml
key:
  id:
  owner:
  purpose:
  algorithm:
  public_key:
  created:
  expires:
  status:
```

Private keys MUST NOT be stored in public PKF documents.

---

# 9. Key Ownership

Control of a private key establishes cryptographic control.

However:

> **Cryptographic control does not automatically establish constitutional authority.**

Possession of a key MUST NOT grant authority beyond the permissions associated with the identity.

---

# 10. Recommended Cryptography

PIP MUST support modern cryptographic algorithms through an algorithm registry.

An initial implementation MAY use:

```text
Ed25519
SHA-256
X25519
```

for:

```text
signatures
hashing
key agreement
```

Algorithm agility MUST be preserved.

PIP MUST NOT permanently hard-code a single cryptographic algorithm.

---

# 11. Signatures

A signature binds an artifact or message to a signing identity.

A signed object SHOULD contain:

```yaml
signature:
  algorithm: Ed25519
  key: key:agent:research:signing:000001
  signer: pid:agent:research:000001
  timestamp: 2026-08-11T12:00:00Z
  value: "..."
```

The exact canonical signing format is defined by the applicable cryptographic profile.

---

# 12. What a Signature Proves

A valid signature proves, subject to the security of the cryptographic system:

1. the content has not changed since signing;
2. the corresponding private key was used;
3. the signer controlled that key at signing time.

A signature does NOT automatically prove:

- that the content is true;
- that the signer was authorized;
- that the signer was acting correctly;
- that the underlying evidence is reliable.

Therefore:

```text
SIGNATURE
    ≠
TRUTH
    ≠
AUTHORITY
```

---

# 13. Authentication

Authentication establishes that an actor controls the identity it claims.

Authentication SHOULD involve a cryptographic challenge.

Example:

```text
A → B
    challenge

B → A
    signed(challenge)

A
    verifies signature
```

A replayed response MUST NOT be accepted as a new authentication event.

---

# 14. Nonces

Authentication challenges SHOULD contain cryptographically secure nonces.

A nonce MUST be sufficiently unpredictable and MUST NOT be reused within a security context.

Example:

```yaml
challenge:
  nonce: "..."
  issued:
  expires:
```

---

# 15. Authentication Context

A successful authentication SHOULD establish:

```text
identity
key
timestamp
session
authentication method
security context
```

The authentication result MUST be auditable where auditability is required.

---

# 16. Authorization

Authentication answers:

> Who are you?

Authorization answers:

> What are you allowed to do?

PIP MUST keep these concepts separate.

```text
AUTHENTICATION
       ↓
IDENTITY
       ↓
AUTHORIZATION
       ↓
CAPABILITY
```

---

# 17. Capabilities

Capabilities describe operations an identity MAY perform.

Examples:

```text
knowledge.read
knowledge.write
knowledge.review
task.create
task.execute
artifact.create
artifact.promote
agent.delegate
plugin.install
production.release
```

Capabilities MUST be explicit.

---

# 18. Capability Scope

A capability MAY be constrained by:

```text
resource
operation
environment
time
location
risk level
authority
```

Example:

```yaml
capability:
  id: capability:000001

  operation:
    - knowledge.read

  scope:
    namespace: world.health

  environment:
    - lab

  expires: 2026-08-15T00:00:00Z
```

---

# 19. Capability ≠ Authority

A capability is an executable permission.

Authority determines whether that capability may legitimately exist.

Therefore:

```text
Authority
    ↓
Permission
    ↓
Capability
    ↓
Operation
```

An implementation MUST NOT treat an arbitrary capability declaration as proof of authority.

---

# 20. Delegation

An identity MAY delegate authority to another identity.

Delegation MUST be explicit and cryptographically attributable.

Example:

```yaml
delegation:
  id: delegation:000001

  issuer: pid:human:putsh:000001

  subject: pid:agent:research:000001

  capabilities:
    - knowledge.read
    - knowledge.write

  scope:
    namespace: world.health

  environment:
    - lab

  issued: 2026-08-11T10:00:00Z

  expires: 2026-08-12T10:00:00Z
```

---

# 21. Delegation Rule

A delegate MUST NOT delegate more authority than it possesses.

Formally:

```text
Authority(delegate) ⊆ Authority(issuer)
```

unless the applicable governance policy explicitly permits controlled sub-delegation.

---

# 22. Delegation Chains

Delegation MAY form a chain.

Example:

```text
Human
  ↓
Research Organization
  ↓
Research Agent
  ↓
Sub-Agent
```

The effective authority MUST be the intersection of all applicable constraints.

```text
Effective Authority
=
A₁ ∩ A₂ ∩ A₃ ...
```

A single restrictive delegation MUST constrain all downstream delegation.

---

# 23. Delegation Depth

Implementations SHOULD support a maximum delegation depth.

Example:

```yaml
delegation:
  max_depth: 2
```

This reduces uncontrolled authority propagation.

---

# 24. Revocation

Credentials, identities, keys and delegations MUST be revocable.

A revocation SHOULD specify:

```yaml
revocation:
  target:
  reason:
  issuer:
  timestamp:
  effective:
  signature:
```

Revocation records MUST be preserved.

---

# 25. Key Rotation

A PIP identity SHOULD support key rotation without changing its identity.

Example:

```text
pid:agent:research:000001
        │
        ├── key:000001
        ├── key:000002
        └── key:000003
```

Historical signatures MUST remain verifiable where the historical key remains available and trusted.

---

# 26. Key Compromise

If a private key is suspected to be compromised:

1. the key MUST be revoked;
2. affected credentials SHOULD be reviewed;
3. affected delegations SHOULD be evaluated;
4. replacement keys SHOULD be issued;
5. the incident SHOULD be recorded.

A compromised key MUST NOT silently remain trusted.

---

# 27. Recovery

PIP SHOULD support identity recovery.

Recovery MAY use:

- multiple trusted human authorities;
- hardware-backed keys;
- offline recovery keys;
- threshold cryptography;
- organizational recovery policies.

A recovery mechanism MUST NOT create an undocumented privileged backdoor.

---

# 28. Human Identity

Human identities MAY be represented by PIP.

A human identity SHOULD allow multiple authentication methods.

For example:

```text
Human
 ├── hardware key
 ├── local device
 └── recovery credential
```

A human MUST NOT be forced to expose personal information beyond what the applicable trust relationship requires.

---

# 29. Agent Identity

An agent identity SHOULD contain:

```yaml
agent:
  identity:
  creator:
  runtime:
  model:
  capabilities:
  constraints:
  lifecycle:
```

The model MAY change during the agent lifecycle.

The identity remains stable unless explicitly replaced.

---

# 30. Agent Replacement

An agent MAY be replaced by another implementation.

Example:

```text
Agent A
   │
   ├── model X
   │
   ▼
Agent A
   │
   └── model Y
```

The identity history MUST preserve the transition.

If the replacement represents a fundamentally different actor, a new identity SHOULD be created.

---

# 31. Agent Lineage

PUTSH SHOULD preserve agent lineage.

Example:

```yaml
lineage:
  parent:
    - pid:agent:research:000001

  created_by:
    - pid:human:putsh:000001

  derived_from:
    - pid:agent:research:000000
```

Lineage MUST NOT imply authority.

A child agent does not automatically inherit the parent's privileges.

---

# 32. Session

A session is a bounded authenticated interaction between actors.

A session SHOULD contain:

```yaml
session:
  id:
  initiator:
  responder:
  created:
  expires:
  authentication:
  security_context:
```

Sessions MUST have explicit lifecycle boundaries.

---

# 33. Session Establishment

A session SHOULD follow:

```text
IDENTIFY
   ↓
AUTHENTICATE
   ↓
NEGOTIATE
   ↓
AUTHORIZE
   ↓
ESTABLISH SESSION
```

No operational command SHOULD be accepted before the required authorization state exists.

---

# 34. Session Expiration

Sessions MUST expire.

Long-running agents SHOULD renew sessions explicitly.

A session MUST NOT remain valid indefinitely by default.

---

# 35. Session Binding

A session SHOULD be bound to:

- authenticated identities;
- negotiated cryptographic parameters;
- authorization context;
- session identifier.

A command from another session MUST NOT be accepted merely because it carries a valid identity signature.

---

# 36. Inter-Agent Communication

PUTSH agents SHOULD communicate using authenticated PKF-compatible messages.

A message MAY contain:

```yaml
message:
  id:
  sender:
  recipient:
  session:
  type:
  timestamp:
  payload:
  nonce:
  signature:
```

The payload SHOULD reference PKF objects where practical.

---

# 37. Message Semantics

PIP provides identity and security.

PIP does NOT define the semantic meaning of tasks or workflows.

For example:

```text
PIP → Who sent this?
PTP → What work is being requested?
PAP → What kind of agent is acting?
POE → How is it executed?
PKF → What does the information mean?
```

This separation is mandatory.

---

# 38. Trust

PIP deliberately avoids a single global numerical trust score.

A PIP identity SHOULD instead be evaluated through explicit evidence:

```text
identity
  +
key validity
  +
delegation
  +
provenance
  +
authorization
  +
history
```

Implementations MAY calculate local trust assessments.

Such assessments MUST NOT alter the canonical identity itself.

---

# 39. Trust Roots

A PUTSH installation MAY define one or more trust roots.

Examples:

```text
human
organization
foundation
local authority
community
```

Trust roots MUST be explicit.

A trust root MUST NOT silently acquire authority over unrelated sovereign installations.

---

# 40. Sovereign Identity

A user SHOULD be able to operate a PIP identity without depending on a centralized PUTSH identity service.

A sovereign identity MAY be stored locally.

Example:

```text
~/.putsh/identity/
    identity.md
    keys/
    credentials/
    revocations/
```

The exact local filesystem layout is implementation-specific.

---

# 41. Offline Verification

A verifier SHOULD be able to validate locally available signatures without contacting a central server.

Where revocation or freshness requires network information, the verifier SHOULD explicitly indicate that online verification was required.

---

# 42. Cross-Domain Identity

Two independent PUTSH installations MAY establish a trust relationship.

Example:

```text
PUTSH A
   │
   │ authenticated federation
   │
   ▼
PUTSH B
```

Neither installation automatically becomes subordinate to the other.

Trust MUST be explicitly established.

---

# 43. Federation

Federated identities SHOULD preserve their original issuer.

Example:

```yaml
federation:
  local_identity: pid:agent:a:000001

  external_identity: pid:agent:b:000042

  trust:
    established_by: pid:human:a:000001
```

Federation MUST NOT erase identity provenance.

---

# 44. Identity Discovery

An implementation MAY discover another PIP identity through:

- local files;
- QR codes;
- Git repositories;
- network protocols;
- secure directories;
- PKF references.

Discovery MUST NOT imply trust.

```text
DISCOVERY
    ≠
AUTHENTICATION
    ≠
AUTHORIZATION
```

---

# 45. Identity Suspension

An identity MAY be temporarily suspended.

Suspension SHOULD preserve historical provenance while preventing new authorized actions.

---

# 46. Identity Destruction

Destroying an identity MUST NOT destroy its historical records.

Historical signatures, tasks and artifacts SHOULD remain attributable to the identity identifier.

The identity MAY become:

```text
revoked
retired
destroyed
```

while its historical references remain intact.

---

# 47. Privacy

PIP SHOULD minimize unnecessary disclosure.

An actor SHOULD disclose only the identity information necessary for the operation.

Where possible, implementations MAY support selective disclosure.

---

# 48. Zero-Knowledge Extensions

Future PIP profiles MAY support zero-knowledge proofs.

Possible applications include proving:

- possession of a capability;
- membership in a governance group;
- authorization within a scope;
- compliance with a policy;

without revealing unnecessary identity information.

Such extensions MUST preserve auditability where legally and operationally required.

---

# 49. Identity Negotiation

Agents MAY negotiate supported protocol capabilities.

Example:

```yaml
hello:
  pip:
    versions:
      - "0.1"

  cryptography:
    signatures:
      - Ed25519

    key_agreement:
      - X25519

  pkf:
    versions:
      - "0.1"
```

Negotiation MUST NOT weaken mandatory security requirements.

---

# 50. Protocol Downgrade Protection

An implementation MUST NOT silently downgrade to an insecure protocol version.

If no mutually acceptable secure protocol exists:

```text
FAIL CLOSED
```

The failure SHOULD be explicit and auditable.

---

# 51. Secure Defaults

PIP implementations SHOULD default to:

```text
authentication required
authorization required
encryption preferred
short-lived sessions
explicit delegation
revocation supported
unknown identity denied
unknown capability denied
```

---

# 52. Emergency Revocation

A governance-authorized emergency mechanism SHOULD allow immediate revocation of:

- agent credentials;
- plugin identities;
- runtime identities;
- production authorization.

Emergency revocation MUST NOT require cooperation from the target agent.

---

# 53. Human Override

A human authority with sufficient authorization MUST be able to:

- revoke an agent;
- revoke a key;
- terminate a session;
- remove a delegation;
- freeze a production environment.

An autonomous agent MUST NOT prevent a legitimate override.

---

# 54. Cryptographic Separation of Environments

LAB and PRODUCTION SHOULD use distinct trust domains.

Example:

```text
LAB TRUST ROOT
      │
      └── experimental agents


PRODUCTION TRUST ROOT
      │
      └── validated agents
```

A LAB credential MUST NOT automatically grant PRODUCTION authority.

---

# 55. Production Promotion

Promotion from LAB to PRODUCTION SHOULD require:

```text
LAB IDENTITY
     ↓
VALIDATION
     ↓
HUMAN / GOVERNANCE AUTHORIZATION
     ↓
PRODUCTION CREDENTIAL
```

The same cryptographic credential SHOULD NOT be reused blindly across environments.

---

# 56. Agent-to-Agent Trust

Two agents MUST NOT trust one another merely because:

- they use the same model;
- they belong to the same provider;
- they share a prompt;
- they claim the same objective.

Trust SHOULD be established through:

```text
identity
authentication
authority
capability
policy
provenance
```

---

# 57. Message Integrity

A message SHOULD be cryptographically bound to:

```text
sender
recipient
session
timestamp
nonce
payload
```

This reduces:

- replay;
- impersonation;
- message substitution;
- session confusion.

---

# 58. Replay Protection

An implementation MUST reject messages that violate the applicable replay-protection policy.

Replay protection MAY use:

- nonces;
- sequence numbers;
- timestamps;
- session identifiers;
- expiration windows.

---

# 59. Identity Collision

Two independent actors MUST NOT intentionally use the same canonical PIP identifier.

If a collision is detected, the identities MUST be treated as distinct until governance resolves the namespace conflict.

---

# 60. Identity Portability

A PIP identity SHOULD be exportable.

Export MUST preserve:

- identity identifier;
- public keys;
- key history;
- credentials;
- delegations;
- revocations;
- provenance.

Private keys SHOULD remain under the owner's control.

---

# 61. Local-First Security

A local PUTSH installation SHOULD remain operational even if external identity infrastructure is unavailable.

Core operations SHOULD NOT depend on:

- a central authentication server;
- a proprietary cloud;
- a single AI provider.

---

# 62. Security Failure Model

When identity or authorization cannot be established, the default behavior MUST be:

```text
DENY
PRESERVE
REPORT
```

The system MUST NOT guess identity.

It MUST NOT infer authority from context alone.

---

# 63. Identity Lifecycle

The recommended identity lifecycle is:

```text
GENERATE
   ↓
REGISTER
   ↓
ACTIVATE
   ↓
AUTHENTICATE
   ↓
DELEGATE
   ↓
ROTATE
   ↓
SUSPEND / REVOKE
   ↓
RETIRE
```

Historical identity information SHOULD remain preserved.

---

# 64. PIP and the PUTSH Language

PIP introduces identity semantics into the PUTSH language.

A future canonical PUTSH message may therefore conceptually be expressed as:

```text
FROM      pid:agent:research:000001
TO        pid:agent:analysis:000002

UNDER     delegation:000042
SESSION   session:000091

REQUEST   task:000117

PROOF     signature:...

CONSTRAINT
          environment = lab
          capability  = knowledge.read
```

This is not a new programming language yet.

It is the semantic direction toward one.

---

# 65. Semantic Separation

The following distinctions MUST remain explicit:

```text
IDENTITY
    Who am I?

AUTHENTICATION
    Can I prove I am this identity?

AUTHORITY
    Who authorized me?

CAPABILITY
    What may I do?

SESSION
    In which authenticated context am I acting?

MESSAGE
    What am I communicating?

TASK
    What work is requested?

EXECUTION
    What actually happened?
```

These concepts MUST NOT be collapsed into a single token or prompt.

---

# 66. Fundamental Invariants

A conforming PIP implementation MUST preserve:

### Invariant 1

Possessing a key does not automatically grant constitutional authority.

### Invariant 2

Authentication does not automatically imply authorization.

### Invariant 3

Authorization does not automatically imply unrestricted capability.

### Invariant 4

An agent identity is independent from its underlying AI model.

### Invariant 5

Delegation cannot increase the authority of the delegator.

### Invariant 6

Revoked credentials MUST NOT remain valid for new operations.

### Invariant 7

Historical identity records MUST remain attributable.

### Invariant 8

Unknown identities MUST NOT be trusted by default.

### Invariant 9

A valid signature MUST NOT be interpreted as proof of truth.

### Invariant 10

A PUTSH installation MUST be capable of maintaining sovereign local identities.

---

# 67. Relationship with Other PUTSH Specifications

PIP depends conceptually on:

```text
PUTSH-0002  Foundation
PUTSH-0003  Governance
PUTSH-0004  PKF
```

PIP provides identity and authorization services to:

```text
PUTSH-0006  PTP
PUTSH-0007  PAP
PUTSH-0008  POE
PUTSH-0010  Plugin Standard
PUTSH-0011  Security & Cryptography
```

The relationship is:

```text
              FOUNDATION
                   │
              GOVERNANCE
                   │
                 PKF
                   │
                 PIP
                   │
       ┌───────────┼───────────┐
       │           │           │
      PTP         PAP         Plugins
       │           │           │
       └───────────┼───────────┘
                   │
                  POE
```

---

# 68. Final Principle

PIP establishes the identity layer of PUTSH.

Its fundamental principle is:

> **Every meaningful action must be attributable to an identity, authorized by an explicit authority, constrained by a defined capability, and verifiable through evidence.**

This is the foundation required for trustworthy communication between humans and artificial intelligences.

---

# Status

This document is a draft specification.

Cryptographic algorithms, canonical serialization, certificate formats, secure transport profiles and key-management details MUST be finalized through PUTSH-0011 before PIP is considered production-ready.
