---
putsh:
  specification: PUTSH-0012D
  version: 0.1.0
  status: draft
  language: en
  canonical: true

title: Putsh Canonicalization Profile
short_name: PCP
depends_on:
  - PUTSH-0012A
  - PUTSH-0012B
  - PUTSH-0012C
  - PUTSH-0011
---

# 1. Purpose

PUTSH-0012D defines the deterministic canonical representation of a PUTSH Artifact.

Its purpose is to ensure that independent implementations produce identical canonical bytes for the same canonical Artifact.

Canonicalization is required before cryptographic hashing or signing.

# 2. Core Principle

The canonical representation MUST be:

- deterministic;
- unambiguous;
- UTF-8 encoded;
- independent of implementation;
- stable across compatible implementations;
- suitable as cryptographic input.

Semantic equivalence MUST NOT be assumed to imply canonical equivalence unless the Artifact can be normalized according to this specification.

# 3. Canonical Encoding

Canonical Artifact bytes MUST use:

- UTF-8 encoding;
- Unicode normalization form NFC;
- LF (`\n`) line endings;
- exactly one final LF;
- no UTF-8 byte-order mark (BOM).

Implementations MUST NOT use locale-dependent encoding or normalization.

# 4. Canonical Artifact Representation

The canonical serialized Artifact consists of:

1. YAML front matter;
2. one LF separator;
3. the normalized Markdown body;
4. one final LF.

The opening front-matter delimiter MUST be:

```text
---
```

The closing delimiter MUST be:

```text
---
```

No content may precede the opening delimiter.

# 5. Canonical Metadata Sections

The canonical top-level metadata order is:

```text
putsh
identity
authority
provenance
references
integrity
lifecycle
```

Absent sections MUST NOT be emitted.

Extension sections MUST follow all canonical sections and MUST be ordered by their exact UTF-8 field name in ascending lexicographic order.

# 6. Canonical `putsh` Fields

The canonical order inside `putsh` is:

```text
type
id
version
status
```

Additional `putsh` extension fields MUST follow the canonical fields and be ordered lexicographically.

Canonical field names are case-sensitive and MUST NOT be silently renamed.

# 7. Nested Mappings

Within a canonical mapping, fields defined by the relevant PUTSH specification MUST appear in their specification-defined order.

Where no specification-defined order exists, keys MUST be ordered by their Unicode-normalized UTF-8 field names in ascending lexicographic order.

This rule applies recursively to nested mappings.

# 8. Lists

Lists are ordered collections.

Canonicalization MUST preserve list order.

Implementations MUST NOT sort lists unless the relevant PUTSH specification explicitly declares the list to be an unordered set.

# 9. Scalars

Canonical scalar rules are:

- strings MUST remain strings;
- booleans MUST use `true` or `false`;
- null MUST use `null`;
- integers MUST use their decimal representation without leading zeroes, except `0`;
- floating-point values MUST NOT be used in canonical security-sensitive metadata unless a future PUTSH specification defines their exact canonical representation;
- timestamps MUST be represented as strings unless a future PUTSH specification defines a canonical timestamp type.

Implementations MUST NOT implicitly convert strings into other scalar types during canonicalization.

# 10. YAML Restrictions

The canonical PUTSH profile does not require support for the full YAML language.

Canonical PUTSH YAML MUST NOT rely on:

- aliases;
- anchors;
- custom tags;
- duplicate keys;
- implementation-specific YAML types;
- implicit type conversions;
- flow-style constructs where their representation could create ambiguity;
- executable or application-specific YAML extensions.

Duplicate keys MUST be rejected.

Unknown fields MUST be preserved as data, not interpreted as protocol semantics.

# 11. Comments

YAML comments are not part of the canonical semantic representation.

Comments MUST NOT affect canonical bytes.

Markdown comments or other non-semantic formatting MUST NOT affect canonical metadata.

# 12. Markdown Body

The Markdown body is part of the Artifact representation.

Before canonicalization:

- CRLF and CR MUST be normalized to LF;
- Unicode MUST be normalized to NFC;
- trailing whitespace at line ends MUST be removed;
- the body MUST terminate with exactly one LF.

The semantic Markdown content MUST otherwise be preserved.

PUTSH implementations MUST NOT reflow, rewrite, or otherwise reinterpret Markdown prose during canonicalization.

# 13. Integrity Scope

The canonical Artifact representation used as the cryptographic input MUST exclude the mutable cryptographic values that would otherwise create self-reference.

At minimum, the following fields MUST be excluded from the hash/signature preimage:

```text
integrity.hash
integrity.signature
```

Other exclusions MUST be explicitly defined by PUTSH-0011 or a later security specification.

The canonical representation used for verification MUST therefore be reconstructible without knowing the final hash or signature value.

# 14. Previous Integrity Links

A predecessor reference such as:

```yaml
integrity:
  previous: ...
```

is data and MAY be included in the cryptographic preimage.

This allows immutable revision chains.

The predecessor reference MUST NOT contain the current Artifact's own digest.

# 15. Extensions

Unknown extension fields MUST be preserved.

Unless explicitly excluded by a higher-level specification, extension fields MUST be included in canonicalization and therefore covered by cryptographic integrity.

An extension MUST NOT alter the meaning of a canonical PUTSH field.

Extension semantics MUST be defined by the relevant extension specification or plugin.

# 16. Identity and Revisions

The following concepts are distinct:

```text
Artifact ID
Revision
Content Digest
```

`putsh.id` identifies the logical Artifact.

`putsh.version` identifies its revision.

The cryptographic digest identifies the canonical content represented by the selected integrity scope.

Changing consequential content MUST produce a new revision and therefore a new digest.

# 17. Determinism Requirement

Given the same valid Artifact semantics and the same canonicalization profile:

```text
implementation A
        +
implementation B
        +
implementation C
```

MUST produce identical canonical bytes.

Therefore:

```text
canonicalize(A) == canonicalize(B)
```

MUST be true for interoperable implementations.

# 18. Validation Before Canonicalization

An implementation MUST validate an Artifact before treating it as canonical.

Invalid constructs MUST NOT be silently repaired.

When canonicalization encounters unsupported or ambiguous input, the implementation MUST reject it rather than inventing semantics.

# 19. Cryptographic Boundary

PUTSH-0012D defines canonicalization only.

It does NOT define:

- a mandatory hash algorithm;
- a mandatory signature algorithm;
- key generation;
- key storage;
- key rotation;
- signature verification policy.

Those requirements belong to PUTSH-0011 and PUTSH-0005.

# 20. Conformance

A canonicalization implementation conforms to PUTSH-0012D when it can:

- parse a conforming Artifact;
- reject prohibited or ambiguous constructs;
- normalize the Artifact deterministically;
- preserve permitted extension fields;
- produce canonical UTF-8 bytes;
- reproduce identical canonical bytes across repeated runs.

# 21. Future Compatibility

Changes to canonicalization rules MUST produce a new version of the canonicalization profile.

Implementations MUST identify the profile used for cryptographic operations.

A future profile MUST NOT silently change the canonical interpretation of an existing profile.

# 22. Canonicalization Pipeline

The normative pipeline is:

```text
INPUT
  ↓
Parse
  ↓
Validate
  ↓
Normalize Unicode
  ↓
Normalize line endings
  ↓
Normalize scalar representation
  ↓
Order canonical mappings
  ↓
Preserve list order
  ↓
Normalize Markdown body
  ↓
Apply integrity exclusions
  ↓
Serialize
  ↓
UTF-8 encode
  ↓
CANONICAL BYTES
```
