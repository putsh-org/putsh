---
putsh:
  specification: PUTSH-0008
  version: 0.1.0
  status: draft
  language: en
  canonical: true

title: PUTSH Orchestration Engine
short_name: POE
subtitle: Runtime, Scheduling, Execution, Isolation and Autonomous Coordination
---

# PUTSH-0008 — Putsh Orchestration Engine

## Abstract

The Putsh Orchestration Engine (POE) is the runtime responsible for executing PUTSH missions and coordinating PUTSH agents.

POE is deliberately not an intelligence layer.

POE does not decide what humanity should want.

POE does not define governance.

POE does not define agent authority.

POE executes authorized work according to:

- Governance;
- PKF;
- PIP;
- PTP;
- PAP;
- PCL;
- Plugin Standard.

POE is therefore the operational layer between PUTSH specifications and real computation.

Its fundamental principle is:

> Execute authority. Never manufacture authority.

---

# 1. Runtime Architecture

The conceptual architecture is:

```text
                         HUMAN / GOVERNANCE
                                │
                                ▼
                             PIP
                                │
                                ▼
                             PAP
                                │
                                ▼
                             PTP
                                │
                                ▼
                             PCL
                                │
                                ▼
                         ┌──────────────┐
                         │     POE      │
                         │   RUNTIME    │
                         └──────┬───────┘
                                │
              ┌─────────────────┼─────────────────┐
              ▼                 ▼                 ▼
           MODELS            PLUGINS          SYSTEMS
              │                 │                 │
              └─────────────────┼─────────────────┘
                                ▼
                            ARTIFACTS
                                │
                                ▼
                               PKF
```
