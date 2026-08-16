---
putsh:
  specification: PUTSH-0010
  version: 0.1.0
  status: draft
  language: en
  canonical: true

title: Putsh Plugin Standard
short_name: PPS
subtitle: A portable, verifiable and permission-aware extension standard for Putsh
---

1. Abstract

The Putsh Plugin Standard (PPS) defines how external capabilities can be added to the PUTSH protocol without compromising interoperability, sovereignty, security or human control.

A plugin MAY provide:

knowledge
research
computation
software integration
data access
sensors
communication
automation
creative capabilities
specialized intelligence

A plugin MUST NOT automatically acquire authority merely because it has been installed.

The fundamental rule is:

Capability is not authority.

2. Design Objective

PUTSH must be extensible without becoming dependent on a single ecosystem.

A plugin should therefore be portable across:

Codex
Claude Code
Gemini
Mistral
Ollama
local Putsh runtime
cloud runtime
future AI systems

The plugin describes what it can do.

The runtime determines whether it is allowed to do it.

3. Plugin Architecture
   PUTSH PLUGIN
   │
   ┌──────────────┼──────────────┐
   ↓ ↓ ↓
   Manifest Capability Interface
   │ │ │
   └──────────────┼──────────────┘
   ↓
   PIP AUTHORITY
   ↓
   POE RUNTIME
   ↓
   0011 SECURITY
   ↓
   ACTION
4. Plugin Definition

A plugin is a portable package containing:

manifest
implementation
interfaces
capabilities
dependencies
documentation
tests
provenance
version information

A plugin MAY be entirely local.

A plugin MAY be community-developed.

A plugin MAY be distributed through a registry.

A registry MUST NOT be required for operation.

5. Plugin Manifest

Every plugin MUST contain a manifest.

Example:

plugin:
id: putsh.plugin.example
name: Example Plugin
version: 1.0.0

description: >
Example capability.

author:
name: ...

license: ...

runtime:
minimum: 0.1.0

capabilities: - ...

permissions: - ...

interfaces: - ...

dependencies: - ...

security:
sandbox: required

provenance:
source: ...
hash: ...
signature: ... 6. Plugin Identity

Plugin IDs MUST be globally distinguishable.

Recommended form:

putsh.plugin.<namespace>.<name>

Examples:

putsh.plugin.core.filesystem
putsh.plugin.community.weather
putsh.plugin.research.pubmed
putsh.plugin.local.calendar 7. Namespaces

Namespaces prevent collisions.

A plugin MAY belong to:

core
official
community
personal
organization
experimental

Namespace ownership SHOULD be independently verifiable.

8. Capability Declaration

A plugin MUST explicitly declare its capabilities.

Example:

capabilities:

- filesystem.read
- filesystem.write
- web.request

The plugin MUST NOT silently expose additional privileged functionality.

9. Capability ≠ Permission

This distinction is fundamental.

A plugin can declare:

filesystem.write

without being granted:

filesystem.write

The declaration means:

"I can perform this operation."

The permission means:

"You are authorized to perform this operation here."

10. Least Authority

Plugins SHOULD receive the minimum permissions required for their task.

Example:

Plugin capability:
filesystem.read

Granted authority:
./project/data/\*\*

rather than:

filesystem.read
/ 11. Permission Scope

Permissions SHOULD be scoped by:

resource
operation
time
mission
agent
environment
user

Example:

permission:
capability: filesystem.write
resource: ./project/output/\*\*
scope:
mission: mission_001
duration: 30m 12. Capability Composition

Plugins MAY compose capabilities.

Example:

web.fetch
↓
document.parse
↓
knowledge.create
↓
PKF

Each transition SHOULD remain attributable.

13. Plugin Interfaces

A plugin MUST expose one or more defined interfaces.

Interfaces MAY be:

CLI
HTTP
local process
MCP
function interface
library
filesystem contract
Putsh-native interface

The implementation mechanism is not part of the semantic contract.

14. Putsh-Native Interface

PUTSH SHOULD eventually define a canonical interface based on artifacts.

Example:

INPUT ARTIFACT
↓
PLUGIN
↓
OUTPUT ARTIFACT

This is preferable to requiring every AI ecosystem to understand a proprietary API.

15. Artifact-Based Communication

A plugin SHOULD communicate consequential results through Putsh artifacts.

Example:

type: plugin_result

plugin:
id: putsh.plugin.example
version: 1.2.0

input:
artifact: ...

result:
status: success
output: ...

evidence:

- ...

warnings:

- ...

16. No Hidden State

Plugins SHOULD minimize hidden state.

Important state SHOULD be represented through explicit artifacts or registered persistent storage.

This permits:

agent replacement
runtime replacement
plugin replacement
machine migration
audit
recovery 17. Deterministic Plugins

Where possible, plugins SHOULD be deterministic.

For example:

calculation
format conversion
hashing
validation
schema checking

Deterministic plugins are particularly useful for verification.

18. Non-Deterministic Plugins

Plugins MAY use:

AI
randomness
external data
simulation
probabilistic methods

Such behavior MUST be declared where relevant.

19. External Dependencies

Dependencies MUST be declared.

Example:

dependencies:

- name: python
  version: ">=3.12"

- name: package-x
  version: "2.x"

20. Dependency Integrity

A plugin SHOULD identify:

dependency
version
source
hash
license

This becomes particularly important for supply-chain security.

0011 will define the stronger security requirements.

21. Plugin Lifecycle

A plugin follows:

DISCOVER
↓
INSPECT
↓
INSTALL
↓
VERIFY
↓
AUTHORIZE
↓
ACTIVATE
↓
UPDATE
↓
SUSPEND
↓
REVOKE
↓
REMOVE 22. Discovery

A plugin MAY be discovered through:

local directory
Git repository
community registry
organization registry
Putsh registry
direct file transfer

No central marketplace is mandatory.

23. Inspection

Before installation, the runtime SHOULD be able to inspect:

identity
source
capabilities
permissions
dependencies
license
version
security declarations
signature

The user SHOULD be able to understand what the plugin does.

24. Installation

Installation MUST NOT imply authorization.

INSTALL
≠
TRUST
≠
AUTHORITY

This distinction is critical.

25. Verification

Before activation, the runtime SHOULD verify:

package integrity
manifest validity
dependency consistency
signature
provenance
compatibility 26. Activation

A plugin becomes active only after:

installation

- verification
- authorization

27. Suspension

A plugin MAY be suspended without being removed.

Triggers include:

anomaly
security concern
user request
dependency failure
resource exhaustion
policy violation
verification failure 28. Revocation

A plugin authorization MUST be revocable.

Revocation MAY originate from:

user
administrator
organization
runtime
security system
mission authority 29. Runtime Isolation

Plugins SHOULD execute inside an appropriate isolation boundary.

Possible mechanisms:

process isolation
container
sandbox
virtual machine
restricted filesystem
WASM
OS permissions

The exact mechanism is implementation-dependent.

30. Plugin Sandboxing

A plugin SHOULD NOT receive unrestricted access to:

filesystem
network
credentials
secrets
other plugins
agent memory
human data

unless explicitly authorized.

31. Secret Isolation

Secrets MUST NOT be exposed to a plugin merely because the agent possesses them.

Example:

Agent has API key
≠
Plugin automatically has API key

The runtime SHOULD provide scoped secret access.

32. Network Permissions

Network access SHOULD be explicit.

Example:

network:
mode: allowlist

hosts: - api.example.org

A plugin without network authority SHOULD operate offline.

33. Filesystem Permissions

Filesystem access SHOULD be scoped.

Example:

filesystem:
read: - ./knowledge/\*\*

write: - ./artifacts/\*\* 34. Human Data

Plugins processing personal data SHOULD declare:

data types
purpose
storage
retention
external transmission 35. Data Sovereignty

PUTSH SHOULD favor:

local processing
local storage
user-controlled keys
portable data
open formats

Cloud processing MAY exist but SHOULD remain explicit.

36. Offline Capability

A plugin SHOULD declare whether it requires:

offline
network
specific service
cloud API
hardware

Example:

requirements:
network: optional
cloud_service: false 37. Model Independence

A plugin MUST NOT assume a specific AI model unless its capability explicitly requires one.

Bad:

requires: GPT-X

Preferred:

requires:
capability: reasoning.large

The runtime can then select an appropriate model.

38. Capability Negotiation

A runtime and plugin SHOULD negotiate capabilities.

PLUGIN:
"I require filesystem.read."

RUNTIME:
"I can provide filesystem.read on ./data."

PLUGIN:
"Accepted." 39. Version Negotiation

Plugins MUST declare compatible protocol versions.

compatibility:
putsh:
minimum: 0.1.0
maximum: <future-compatible-range> 40. Graceful Degradation

If an optional capability is unavailable, a plugin SHOULD be able to degrade gracefully.

Example:

Internet unavailable
↓
Use local cache
↓
Mark result as incomplete

It MUST NOT fabricate the missing information.

41. Plugin Failure

Failure MUST be explicit.

result:
status: failed

error:
type: dependency_unavailable
message: ...

A plugin MUST NOT report success when execution failed.

42. Plugin Timeout

Long-running plugins MUST support timeout or checkpoint semantics where possible.

This connects directly to:

PTP
POE
PCL 43. Plugin Cancellation

The runtime SHOULD be able to interrupt plugin execution.

This is especially important for:

network operations
automation
computation
external side effects
long-running processes 44. Plugin Idempotency

Plugins performing external actions SHOULD declare whether operations are idempotent.

Example:

operation:
name: send_message
idempotent: false

This prevents unsafe automatic retries.

45. Side Effects

Plugins MUST declare consequential side effects where possible.

Examples:

writes files
sends network request
creates account
publishes content
moves money
changes configuration
controls hardware 46. Dry Run

Plugins SHOULD support:

DRY_RUN

when technically possible.

This allows an agent to inspect intended effects before execution.

47. Preview

A plugin MAY produce:

preview:
intended_actions: - ...
expected_effects: - ...
risks: - ...

before execution.

48. Verification Interface

Plugins SHOULD expose verification information.

Example:

verification:
method:
expected:
observed:
evidence:

This allows PCL to verify plugin actions rather than trusting plugin self-reporting.

49. Plugin Provenance

Every plugin SHOULD identify:

creator
source
version
build
commit
dependencies 50. Reproducible Builds

Where applicable, plugins SHOULD support reproducible builds.

The objective is:

source
↓
build
↓
artifact

with independently verifiable output.

51. Cryptographic Identity

Plugins MAY be cryptographically signed.

The signature SHOULD cover:

manifest
implementation
version
dependencies

The detailed cryptographic protocol belongs to PUTSH-0011.

52. Community Plugins

PUTSH SHOULD explicitly support community-developed plugins.

But:

community
≠
trusted

A community plugin can be useful without being automatically trusted.

53. Trust Levels

PUTSH MAY eventually define:

UNVERIFIED
COMMUNITY
VERIFIED
TRUSTED
CORE

These labels describe provenance/trust, not authority.

54. Personal Plugins

Users SHOULD be able to create private plugins without publishing them.

Example:

putsh.plugin.personal.home
putsh.plugin.personal.photos
putsh.plugin.personal.workflow 55. Organizational Plugins

Organizations MAY maintain private plugin registries.

Organization
↓
Private Registry
↓
Authorized Agents 56. Plugin Registry

A registry is a distribution mechanism, not the protocol itself.

Therefore:

PUTSH MUST remain usable without a central registry.

This is important for digital sovereignty.

57. Decentralized Distribution

Future implementations MAY support:

Git
IPFS
peer-to-peer distribution
local networks
organization mirrors

provided integrity and provenance remain verifiable.

58. Plugin Communication

Plugins SHOULD communicate through typed inputs and outputs.

Example:

input:
type: research_question
value: ...

output:
type: research_artifact
value: ...

This makes plugins composable.

59. Plugin Pipelines

Plugins MAY form pipelines:

FETCH
↓
PARSE
↓
ANALYZE
↓
VALIDATE
↓
STORE

Each step produces an explicit artifact.

60. Plugin Composition

A complex capability can therefore emerge from simple components.

small plugin

- small plugin
- # small plugin
  larger capability

This reduces dependence on monolithic software.

61. Agent Plugins

A plugin MAY itself expose an agent.

For example:

putsh.plugin.researcher
putsh.plugin.translator
putsh.plugin.code-reviewer

Such an agent remains subject to:

PAP
PIP
PTP
PCL
0011 62. Plugins Cannot Rewrite the Protocol

A plugin MUST NOT silently redefine:

PIP
PTP
PAP
POE
PCL
PKF

Protocol extensions MUST be explicitly versioned.

63. Plugin Extensions

A plugin MAY introduce new:

artifact types
capabilities
operations
adapters
domain vocabularies

provided they are namespaced.

Example:

x-example.medical.diagnosis

rather than silently redefining:

diagnosis 64. Experimental Extensions

Experimental capabilities MAY be marked:

status: experimental

Agents SHOULD treat experimental extensions with appropriate caution.

65. Compatibility

A plugin SHOULD declare:

protocol compatibility
runtime compatibility
platform compatibility
dependency compatibility 66. Migration

Plugin upgrades SHOULD support migration when data structures change.

Example:

v1 artifact
↓
migration
↓
v2 artifact

Original data SHOULD remain recoverable where practical.

67. Rollback

A plugin update SHOULD be reversible.

v2
↓
failure
↓
rollback
↓
v1 68. No Silent Updates

Plugins SHOULD NOT update themselves silently.

Updates SHOULD be:

visible
versioned
verifiable
authorizable
reversible 69. Plugin Audit

The runtime SHOULD be able to answer:

Which plugin performed this action?
Which version?
Under whose authority?
With which permissions?
Using which inputs?
Producing which outputs? 70. Plugin-to-Plugin Trust

A plugin MUST NOT automatically trust another plugin.

Trust SHOULD be established through:

identity
provenance
permissions
verification
policy 71. Plugin-to-Agent Trust

Likewise:

Agent trusts plugin

must not mean:

Plugin controls agent

The authority hierarchy remains external to the plugin.

72. Agent Oath Compatibility

Every plugin operating as an autonomous agent MUST respect the Putsh Agent Oath.

The oath is not merely descriptive.

Its enforceable portions SHOULD eventually map to:

permissions
capabilities
interruptibility
delegation
verification
audit

through PIP, POE and 0011.

73. Plugin Security Boundary

0010 defines the security interface.

0011 defines the security guarantees.

Therefore:

0010:
"What security information must a plugin expose?"

0011:
"How is that information enforced?"

This avoids duplication.

74. Plugin Contract

A minimal plugin contract becomes:

IDENTITY
CAPABILITIES
INTERFACES
DEPENDENCIES
INPUTS
OUTPUTS
PERMISSIONS
SIDE EFFECTS
VERSION
PROVENANCE
LIFECYCLE 75. Minimal Plugin

The smallest useful Putsh plugin could theoretically be:

manifest.md
plugin.yaml
run

with:

input artifact
→ operation
→ output artifact

This keeps the barrier to entry extremely low.

76. Human-Created Plugins

A person who is not a professional developer SHOULD eventually be able to create a plugin.

For example:

"Create a plugin that converts my recipes
into a weekly shopping list."

An AI could generate the implementation and manifest.

The resulting plugin would still pass through:

INSPECT
VERIFY
AUTHORIZE

before execution.

77. AI-Created Plugins

Agents MAY create plugins.

But an AI-generated plugin MUST NOT automatically gain the authority required to execute its own generated code.

This distinction is fundamental.

AI creates plugin
↓
plugin exists
↓
verification
↓
human/runtime authorization
↓
execution 78. Self-Extension

PUTSH therefore supports controlled self-extension without uncontrolled self-modification.

An agent may propose:

"I need a new capability."

It may generate a plugin.

But:

Creation does not equal activation.

79. Plugin Lifecycle as a Cognitive Loop

Plugins themselves can participate in PCL:

OBSERVE limitation
↓
UNDERSTAND missing capability
↓
RESEARCH solution
↓
REASON about implementation
↓
PLAN plugin
↓
CREATE
↓
VERIFY
↓
PROPOSE activation

This is a powerful connection between 0010 and 0009.

80. Civilization-Scale Extensibility

This matters particularly for the original Putsh objective.

If Putsh is intended to help solve large human problems, we cannot hard-code every domain.

Instead:

PUTSH CORE
│
├── food
├── health
├── energy
├── education
├── environment
├── mobility
├── science
├── governance
├── humanitarian action
└── future domains

become capability ecosystems, not modifications to the core protocol.

81. Core Principle

The core protocol should remain small.

The ecosystem should become large.

small protocol +
many interoperable capabilities
=
Putsh ecosystem 82. Canonical Plugin Vocabulary

The initial vocabulary SHOULD include:

PLUGIN
MANIFEST
CAPABILITY
PERMISSION
INTERFACE
DEPENDENCY
INPUT
OUTPUT
SIDE_EFFECT
PROVENANCE
SIGNATURE
SANDBOX
REGISTRY
VERSION
INSTALL
VERIFY
AUTHORIZE
ACTIVATE
SUSPEND
REVOKE
REMOVE 83. Plugin Security Invariants

0010 establishes these preliminary invariants:

Invariant 1

Installing a plugin does not authorize it.

Invariant 2

Declaring a capability does not grant the capability.

Invariant 3

A plugin cannot increase its own authority.

Invariant 4

Plugin permissions must be externally controlled.

Invariant 5

Consequential side effects must be explicit where possible.

Invariant 6

Plugin failures must be observable.

Invariant 7

Plugin execution should be interruptible.

Invariant 8

Plugin versions must be identifiable.

Invariant 9

Plugin provenance should be recoverable.

Invariant 10

No central registry is required for protocol operation.

Invariant 11

AI-generated plugins are not automatically trusted.

Invariant 12

Plugins cannot silently redefine the Putsh protocol.

84. Relationship With the Other Bricks

The architecture now becomes particularly coherent:

PKF
│
├── defines artifacts
│
PIP
│
├── defines identity & authority
│
PTP
│
├── defines missions
│
PAP
│
├── defines agents
│
POE
│
├── executes the system
│
PCL
│
├── defines cognition
│
0010
│
├── defines extensions
│
0011
│
└── protects everything

And the important part is that 0010 does not sit above the other bricks.

It plugs into all of them.

85. The Emerging Putsh Stack

We can now visualize the protocol as:

                    HUMANITY
                       │
                ┌──────┴──────┐
                │   MISSIONS  │
                │    PTP      │
                └──────┬──────┘
                       │
                 ┌─────┴─────┐
                 │  COGNITION │
                 │    PCL     │
                 └─────┬─────┘
                       │
                ┌──────┴──────┐
                │    AGENTS   │
                │     PAP     │
                └──────┬──────┘
                       │
          ┌────────────┴────────────┐
          │                         │
       PLUGINS                   RUNTIME
        0010                      POE
          │                         │
          └────────────┬────────────┘
                       │
                  KNOWLEDGE
                     PKF
                       │
                 AUTHORITY
                     PIP
                       │
                  SECURITY
                     0011

This is becoming much more than a framework for running agents.

It is shaping into a portable operating protocol for human–AI–AI collaboration.
