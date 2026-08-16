---
putsh:
  specification: PUTSH-0012C
  version: 0.1.0
  status: draft
  language: en
  canonical: true

title: Putsh Canonical Serialization
short_name: PCS
depends_on:
  - PUTSH-0012A
  - PUTSH-0012B
---

1. Purpose

PCS defines how Putsh Artifacts are represented across files, systems, runtimes and AI models.

2. Canonical Human Format

The preferred human-readable representation is:

Markdown

- YAML Front Matter
- structured Markdown content

Recommended extension:

.md 3. Canonical Structure

---

putsh:
type: claim
id: claim-0042
version: 1
status: candidate

identity:
author: agent.research.01

provenance:
created: 2026-08-15T12:00:00Z

integrity:
hash: ...
signature: ...

references:
evidence: - artifact://evidence/0091

---

# Claim

The claim content.

## Uncertainty

...

## Reasoning

...

## Verification

... 4. Parsing Rule

A conforming parser MUST process:

YAML front matter
↓
artifact metadata
↓
Markdown body
↓
semantic structure 5. Unknown Fields

Unknown extension fields MUST NOT cause an otherwise valid Artifact to become unreadable.

Implementations SHOULD preserve unknown fields when rewriting an Artifact.

6. Canonical Serialization

For cryptographic purposes, implementations MUST use a deterministic canonical representation.

Whitespace, field ordering and serialization differences MUST NOT produce different cryptographic identities for semantically identical canonical metadata.

7. Alternate Representations

PUTSH MAY be serialized as:

Markdown
YAML
JSON
database records
API payloads
binary formats

provided that the resulting representation preserves the canonical Artifact semantics.

8. Interoperability

Two independent implementations MUST be able to exchange a conforming Artifact without requiring:

the same model
the same vendor
the same runtime
the same operating system
the same programming language 9. Human Compatibility

The canonical .md representation SHOULD remain understandable and editable by a human without specialized software.

10. Machine Compatibility

The same Artifact MUST be parsable by an automated runtime.

This establishes the core Putsh principle:

ONE ARTIFACT
↓
HUMAN +
AI +
RUNTIME 11. Integrity

Serialization MUST preserve the integrity mechanisms defined by PUTSH-0011.

Changing a signed consequential Artifact MUST invalidate its previous integrity state unless a new valid version is produced.

12. Conformance

A Putsh serializer conforms to PCS when it can:

serialize
deserialize
validate
canonicalize
preserve
and verify

Putsh Artifacts without semantic loss.

Les trois briques forment maintenant un ensemble
0012A — WHAT IS AN ARTIFACT?
↓
0012B — WHAT DOES IT MEAN?
↓
0012C — HOW IS IT WRITTEN / TRANSMITTED?
↓
PUTSH LANGUAGE
↓
PUTSH RUNTIME
↓
Codex / Ollama / Mistral / OpenClaw / etc.
