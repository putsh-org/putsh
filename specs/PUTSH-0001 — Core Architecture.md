---
putsh:
  specification: PUTSH-0001
  version: 0.1.0
  status: draft
  language: en
  canonical: true

title: PUTSH Core Architecture
---

# 1. Overview

PUTSH consists of a set of interoperable protocols and governance
documents.

The architecture is divided into four layers.

## Layer 1 — Foundation

Defines why the system exists and the principles governing it.

Components:

- Vision
- Constitution
- Agent Oath

## Layer 2 — Protocol

Defines how the system represents and protects information.

Components:

- PKF — Putsh Knowledge Framework
- PIP — Putsh Identity Protocol
- PTP — Putsh Task Protocol
- PAP — Putsh Agent Protocol

## Layer 3 — Runtime

Defines how protocol-compliant components are executed.

Component:

- POE — Putsh Orchestration Engine

## Layer 4 — Cognitive Lifecycle

Defines how problems, knowledge, decisions and learning evolve.

Component:

- PCL — Putsh Cognition Loop

# 2. Architectural Relationship

```text
                 FOUNDATION
                     │
          ┌──────────┴──────────┐
          │                     │
      Constitution         Agent Oath
          │                     │
          └──────────┬──────────┘
                     │
                  PROTOCOL
                     │
      ┌──────────────┼──────────────┐
      │              │              │
     PKF            PIP            PTP
      │              │              │
      └──────────────┼──────────────┘
                     │
                    PAP
                     │
                    POE
                     │
                    PCL
                     │
          ┌──────────┴──────────┐
          │                     │
         LAB               PRODUCTION
```
