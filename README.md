# PUTSH

### An open protocol for trustworthy human–AI collaboration.

PUTSH is an open protocol for trustworthy collaboration between humans and artificial intelligence.

It provides a common foundation for:

* structured knowledge;
* autonomous and collaborative AI agents;
* task orchestration;
* provenance and verification;
* digital sovereignty;
* human governance;
* experimental and production environments.

PUTSH is designed to remain independent from any particular AI model, provider, programming language, operating system, or cloud platform.

A PUTSH system may run locally, remotely, or as a hybrid system.

## Core Principle

**The protocol is the source of truth. Implementations are not.**

PUTSH separates its specifications from their implementations so that the system can evolve independently from individual AI models, vendors, frameworks, and technologies.

## Architecture

PUTSH is built around several complementary protocols:

* **PKF** — Putsh Knowledge Framework
* **PIP** — Putsh Identity Protocol
* **PTP** — Putsh Task Protocol
* **PAP** — Putsh Agent Protocol
* **POE** — Putsh Orchestration Engine
* **PCL** — Putsh Cognition Loop

These protocols operate under the PUTSH Foundation and Constitution.

## Environments

PUTSH separates experimentation from validated operation.

### LAB

A protected environment for:

* research;
* experimentation;
* hypotheses;
* prototypes;
* simulations;
* competing solutions.

### PRODUCTION

A controlled environment for:

* validated knowledge;
* approved agents;
* tested workflows;
* released artifacts.

No LAB artifact becomes a PRODUCTION artifact without passing the applicable validation and promotion process.

## Sovereignty

PUTSH is designed for local-first operation.

Users SHOULD be able to retain control over their:

* knowledge;
* identities;
* cryptographic keys;
* agents;
* execution history;
* configuration.

PUTSH MUST NOT require dependence on a single AI provider.

## Specifications

The normative architecture of PUTSH is defined in the `specification/` directory.

The canonical specifications use English as their normative language.

Translations MAY be provided as non-canonical derived documents.

## Status

PUTSH is currently in the **Specification Phase**.

The protocol is being designed before the development of its reference implementation.

## License

PUTSH is intended to be released as open infrastructure.

See `LICENSE` for licensing terms.
