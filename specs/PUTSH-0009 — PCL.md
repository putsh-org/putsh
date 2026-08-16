---
putsh:
  specification: PUTSH-0009
  version: 0.1.0
  status: draft
  language: en
  canonical: true

title: Putsh Cognitive Loop
short_name: PCL
subtitle: A model-independent cognitive protocol for human-AI and AI-AI collaboration
---

1. Abstract
   The Putsh Cognitive Loop (PCL) defines the canonical cognitive process used by PUTSH agents.
   It provides a model-independent structure through which an agent can:

OBSERVE
→ UNDERSTAND
→ RESEARCH
→ REASON
→ PLAN
→ EXECUTE
→ VERIFY
→ LEARN

PCL is not a prompt.
PCL is not a model.
PCL is not an agent.
PCL is a protocol for transforming information into progressively validated action.
Its purpose is to allow heterogeneous intelligences to collaborate through explicit cognitive artifacts.

2. Fundamental Principle
   A PUTSH agent MUST NOT be defined by the model that powers it.
   Instead:

MODEL
↓
PCL
↓
ARTIFACTS
↓
ACTION

This allows:

Mistral
Ollama
Codex
Claude
Gemini
local models
future models
human intelligence

to participate in the same cognitive process.

3. The Cognitive Loop
   The canonical PCL is:

┌─────────┐
│ OBSERVE │
└────┬────┘
↓
┌────────────┐
│ UNDERSTAND │
└─────┬──────┘
↓
┌──────────┐
│ RESEARCH │
└────┬─────┘
↓
┌─────────┐
│ REASON │
└────┬────┘
↓
┌────────┐
│ PLAN │
└───┬────┘
↓
┌─────────┐
│ EXECUTE │
└────┬────┘
↓
┌─────────┐
│ VERIFY │
└────┬────┘
↓
┌────────┐
│ LEARN │
└───┬────┘
│
└──────────────→ OBSERVE

The loop MAY iterate indefinitely for long-running missions.

4. Why a Loop?
   Reality is not static.
   Every action changes the state being observed.
   Therefore:

PLAN → EXECUTE

must never be considered the end.
Execution creates new information.
That information becomes the next observation.

5. PCL Is Artifact-Driven
   Each stage SHOULD produce explicit artifacts.
   The system should avoid relying on invisible conversational context.
   Example:

OBSERVATION
↓
UNDERSTANDING
↓
RESEARCH
↓
REASONING
↓
PLAN
↓
EXECUTION
↓
VERIFICATION
↓
LEARNING

Each transition is represented by structured information.

6. Stage Contract
   Every PCL stage has:

stage:
id:
input:
objective:
operations:
output:
uncertainty:
evidence:
next:

7. OBSERVE
   Purpose
   OBSERVE establishes what is currently known to be happening.
   The agent SHOULD distinguish:

observed
reported
inferred
assumed
unknown

8. Observation Sources
   Observations MAY originate from:

human
sensor
file
database
API
web
agent
another agent
system state
execution artifact
historical record

9. Observation Principle
   An observation MUST NOT be silently converted into a fact.
   Example:

Observation:
"User reports that food waste increased."

Fact:
"Food waste increased by 17%."

The second statement requires evidence.

10. Observation Artifact
    Example:

type: observation

observation:
id: obs_001

source:
type: human
identity: ...

content: ...

timestamp: ...

confidence: ...

classification: reported

references: - ...

11. UNDERSTAND
    UNDERSTAND transforms observations into a structured representation of the problem.
    It answers:

What is happening?
Who is affected?
What is known?
What is unknown?
What may be related?
What constraints exist?

12. Understanding Is Not Truth
    Understanding is a working model.
    It MUST remain revisable.

UNDERSTANDING
↓
HYPOTHESIS
↓
RESEARCH
↓
UPDATED UNDERSTANDING

13. Problem Frame
    An understanding SHOULD identify:

problem:
statement:
actors:
affected_entities:
constraints:
known_facts:
unknowns:
hypotheses:
desired_outcome:

14. Research Trigger
    The agent SHOULD research when:

critical information is missing
uncertainty materially affects the plan
evidence is insufficient
competing hypotheses exist

The agent SHOULD NOT research merely because research is available.

15. RESEARCH
    RESEARCH acquires information required to reduce relevant uncertainty.
    It MAY involve:

web search
documents
databases
scientific literature
experiments
simulations
other agents
human experts
local knowledge

16. Research Is Question-Driven
    A research operation SHOULD begin with explicit questions.
    Example:

research:
questions: - "What causes X?" - "What interventions have been tested?" - "What are their measured effects?"

17. Research Artifact
    A research artifact SHOULD contain:

research:
question:
method:
sources:
findings:
contradictions:
confidence:
unresolved:

18. Source Diversity
    For important questions, agents SHOULD seek multiple independent sources.
    One source SHOULD NOT automatically become truth.

19. Contradiction Detection
    Research SHOULD explicitly identify contradictory evidence.
    Example:

contradiction:
claim_a:
claim_b:
sources:
possible_explanation:
unresolved: true

20. REASON
    REASON transforms knowledge and evidence into conclusions, hypotheses and decisions.
    The agent SHOULD distinguish:

FACT
EVIDENCE
INFERENCE
HYPOTHESIS
ASSUMPTION
OPINION
DECISION

21. Epistemic Discipline
    PCL MUST preserve the distinction between:

"I know"

and:

"I believe."

and:

"I suspect."

and:

"I have not established this."

This distinction is foundational to Putsh.

22. Reasoning Artifact
    Example:

reasoning:
premises: - ...
evidence: - ...
assumptions: - ...
hypotheses: - ...
conclusions: - ...
uncertainty: - ...

23. Multiple Hypotheses
    When uncertainty is substantial, the agent SHOULD maintain multiple hypotheses.

H1
H2
H3

rather than prematurely selecting one.

24. Hypothesis Competition
    Agents MAY score hypotheses according to:

evidence
consistency
explanatory power
predictive power
simplicity
risk

The scoring method SHOULD be explicit when consequential.

25. PLAN
    PLAN transforms reasoning into an executable sequence.
    A plan SHOULD specify:

objective
tasks
dependencies
resources
agents
constraints
risks
verification
completion criteria

26. Plan Example

plan:
objective: ...

tasks: - id: task_001
objective: ...
dependencies: []

    - id: task_002
      objective: ...
      dependencies:
        - task_001

verification:
required: true

completion:
criteria: - ...

27. Plans Are Provisional
    A plan MUST be allowed to change when new information appears.
    Execution does not create an obligation to blindly follow an obsolete plan.

28. Replanning
    PCL SHOULD return to REASON or PLAN when:

new evidence contradicts assumptions
execution fails
constraints change
risk changes
resources change
verification fails

29. EXECUTE
    EXECUTE transfers the plan into action.
    Execution is performed by POE.
    PCL defines what cognitive state authorizes execution.
    POE determines how authorized execution occurs.

30. Execution Boundary
    The boundary is:

PCL
↓
authorized plan
↓
POE
↓
real execution

31. Execution Artifact
    Every consequential execution SHOULD produce:

execution:
action:
actor:
authority:
inputs:
timestamp:
environment:
result:
side_effects:

32. EXECUTE Does Not Mean SUCCESS
    Execution merely means the action was attempted.

EXECUTED ≠ SUCCESSFUL

Success requires verification.

33. VERIFY
    VERIFY determines whether the execution produced the intended result.
    Verification SHOULD be independent when risk warrants it.

34. Verification Questions
    The verifier SHOULD ask:

Did the action occur?
Did it produce the intended result?
Did unintended consequences occur?
Was the original hypothesis supported?
Is the evidence sufficient?

35. Verification Levels
    A possible model:

V0 — no verification required
V1 — self verification
V2 — deterministic verification
V3 — independent agent verification
V4 — human verification
V5 — multi-party / institutional verification

The required level is determined by risk.

36. Verification Artifact

verification:
target:
method:
verifier:
evidence:
expected:
observed:
result:
confidence:
unresolved:

37. Verification Failure
    If verification fails:

VERIFY
↓
FAIL
↓
REASON
↓
REPLAN
↓
EXECUTE

The system MUST NOT simply mark the mission successful.

38. LEARN
    LEARN converts experience into durable knowledge.
    Learning MAY identify:

new facts
new evidence
new patterns
failed approaches
successful approaches
process improvements
new hypotheses
new questions

39. Learning Must Be Classified
    A learned item SHOULD be classified as:

fact
evidence
heuristic
lesson
hypothesis
procedure
warning

40. Learning Does Not Automatically Become Truth
    A lesson learned from one mission MUST NOT automatically become universal knowledge.
    Example:

Mission-specific lesson
≠
Universal rule

41. Knowledge Promotion
    A learning artifact MAY be promoted through:

MISSION MEMORY
↓
ACTIVE KNOWLEDGE
↓
VALIDATED KNOWLEDGE
↓
CANONICAL KNOWLEDGE

Promotion requires appropriate verification.

42. Memory Layers
    PCL works with the PKF memory model.
    Conceptually:

working memory
↓
mission memory
↓
active memory
↓
validated knowledge
↓
canonical knowledge

43. Memory Decay
    Temporary knowledge SHOULD be allowed to expire.
    Examples:

temporary assumption
short-lived context
current system state
temporary hypothesis

44. Loop Termination
    A PCL loop MAY terminate when:

objective satisfied
completion criteria satisfied
mission cancelled
authority revoked
resource limit reached
human intervention
unrecoverable failure

45. Loop Continuation
    A mission SHOULD continue when:

important uncertainty remains
verification incomplete
new evidence changes the problem
new risks appear
objective not satisfied

46. Cognitive Checkpoint
    Each significant PCL transition SHOULD be checkpointable.
    Example:

OBSERVE CHECKPOINT
UNDERSTAND CHECKPOINT
RESEARCH CHECKPOINT
REASON CHECKPOINT
PLAN CHECKPOINT
EXECUTION CHECKPOINT
VERIFY CHECKPOINT
LEARN CHECKPOINT

This allows agent replacement.

47. Agent Replacement
    A new agent SHOULD be able to receive:

mission
current PCL stage
artifacts
evidence
uncertainties
assumptions
plan
checkpoint

and continue.

48. No Context Dependency
    A mission MUST NOT require:

"remember what we talked about earlier."

The necessary state must exist in artifacts.

49. Human Participation
    Humans MAY enter the PCL at any stage.

human
↓
OBSERVE
↓
UNDERSTAND
↓
RESEARCH
...

or:

agent
↓
REASON
↓
human review
↓
PLAN

50. Human as Agent
    A human MAY be represented as a PCL actor.
    This allows:

human
AI agent
expert
institution
software agent

to participate in a common process.

51. AI-to-AI Collaboration
    Two agents SHOULD communicate through PCL artifacts rather than unstructured conversational dependence.
    Example:

Agent A
↓
research artifact
↓
Agent B
↓
reasoning artifact
↓
Agent C
↓
plan

52. Cognitive Handoff
    A handoff MUST identify:

handoff:
from:
to:
mission:
stage:
current_state:
completed:
unresolved:
assumptions:
required_next_action:

53. Parallel Cognition
    PCL MAY execute multiple cognitive branches.

                        PROBLEM
                           │
              ┌────────────┼────────────┐
              ↓            ↓            ↓
           Branch A     Branch B     Branch C
              │            │            │
              ↓            ↓            ↓
           REASON        REASON        REASON
              │            │            │
              └────────────┼────────────┘
                           ↓
                        SYNTHESIS

54. Cognitive Fork
    A fork SHOULD be explicit.

fork:
parent:
branches: - branch_a - branch_b - branch_c
purpose:

55. Fork Resolution
    Branches MAY be:

merged
rejected
continued
archived

The reason SHOULD be recorded.

56. Adversarial Reasoning
    For consequential decisions, PCL SHOULD support an adversarial branch.
    Example:

PRIMARY HYPOTHESIS
↓
RED TEAM
↓
COUNTER-EVIDENCE
↓
REASON

57. Self-Criticism
    An agent SHOULD actively search for:

confirmation bias
missing evidence
false assumptions
alternative explanations
measurement errors
hidden dependencies

58. Multi-Agent Deliberation
    Multiple agents MAY independently reason about the same problem.
    They SHOULD NOT be forced into artificial consensus.
    Disagreement is valuable information.

59. Disagreement Artifact

disagreement:
subject:
positions: - agent:
conclusion:
evidence:
unresolved:
proposed_resolution:

60. Consensus
    Consensus SHOULD be an output of evidence and reasoning, not a mandatory property.

disagreement ≠ failure

61. Confidence
    PCL MAY represent confidence.
    However:

confidence ≠ truth

Confidence MUST NOT substitute for evidence.

62. Uncertainty
    Uncertainty SHOULD be explicit.
    Example:

uncertainty:
type: epistemic
description:
impact:
resolution_strategy:

63. Risk-Aware Cognition
    The PCL SHOULD connect uncertainty to consequences.
    An uncertain answer with negligible consequences may proceed.
    An uncertain answer involving:

human safety
critical infrastructure
large financial consequences
irreversible environmental effects

requires stronger verification.

64. Epistemic Escalation
    A useful principle is:

higher uncertainty

- # higher consequence
  stronger verification

65. Cognitive Permissions
    Agents MAY have restrictions on stages.
    For example:

capabilities:
can_observe: true
can_research: true
can_reason: true
can_plan: true
can_execute: false

Another agent may be an execution specialist.

66. Stage Specialization
    PUTSH MAY define agents such as:

Observer
Researcher
Reasoner
Planner
Executor
Verifier
Librarian
Red Team
Mediator

These are roles, not necessarily separate models.

67. Composite Agents
    One agent MAY perform the entire loop.
    Another implementation MAY distribute it across many agents.
    Both remain PCL-compatible.

68. Minimal Agent
    A minimal PUTSH agent SHOULD be capable of:

OBSERVE
UNDERSTAND
REASON
PLAN

and may delegate execution.

69. Advanced Agent
    A full agent MAY execute the complete loop autonomously.

70. PCL as Inter-AI Language
    PCL artifacts constitute a primitive semantic language between AI systems.
    Instead of:

"Hey Claude, can you look at what GPT did?"

the protocol becomes:

RESEARCH_ARTIFACT

- EVIDENCE
- UNCERTAINTY
- REASONING
- PLAN

This is a critical design principle for PUTSH.

71. Semantic Stability
    PCL keywords SHOULD remain stable.
    Models may interpret natural language differently.
    Structured PCL artifacts reduce this ambiguity.

72. Human Readability
    Despite being machine-oriented, PCL artifacts MUST remain understandable by humans.
    This is why Markdown + YAML remains appropriate.

73. Machine Readability
    Every important PCL artifact SHOULD contain machine-readable metadata.
    Example:

type: reasoning
putsh_version: 0.1
mission_id: ...
agent_id: ...

74. Human-Machine Duality
    The same artifact SHOULD be usable by:

human
AI
runtime
auditor
future agent

This is one of the central architectural principles of Putsh.

75. PCL and the Putsh Language
    The emerging Putsh language therefore consists of:

IDENTITY
AUTHORITY
MISSION
KNOWLEDGE
OBSERVATION
QUESTION
EVIDENCE
REASONING
HYPOTHESIS
PLAN
ACTION
VERIFICATION
LEARNING

These become the semantic primitives of the protocol.

76. Canonical Cognitive Vocabulary
    The initial canonical vocabulary SHOULD include:

OBSERVATION
QUESTION
CLAIM
FACT
EVIDENCE
ASSUMPTION
HYPOTHESIS
REASONING
DECISION
PLAN
TASK
ACTION
RESULT
VERIFICATION
LESSON
KNOWLEDGE
UNCERTAINTY
RISK
CONFLICT
HANDOFF
CHECKPOINT

77. Cognitive Grammar
    A minimal PCL expression could conceptually be:

OBSERVE X
→ UNDERSTAND X
→ QUESTION Y
→ RESEARCH Y
→ REASON ABOUT Y
→ PLAN Z
→ EXECUTE Z
→ VERIFY Z
→ LEARN FROM Z

This is the beginning of the Putsh inter-AI language.

78. PCL Does Not Replace Natural Language
    Natural language remains useful for:

human communication
creative exploration
ambiguous concepts
cultural expression

PCL provides structure underneath it.

79. Translation Layer
    An AI may translate:

natural language
↓
PCL artifacts
↓
another AI
↓
natural language

without requiring both systems to share the same model.

80. Protocol Compression
    As the system matures, PCL artifacts MAY develop compact machine representations.
    For example:

OBSERVE
RESEARCH
REASON
PLAN
VERIFY

could eventually become compact protocol tokens.
But human-readable Markdown remains the canonical representation.

81. Long-Horizon Cognition
    PCL is specifically designed for long-running cognition.
    A mission can therefore evolve:

Hour 1
Research

Hour 5
Hypothesis refinement

Hour 12
Experiment

Day 2
Verification

Day 3
Alternative hypothesis

Day 5
Production proposal

without requiring one continuous model session.

82. Reflection
    PUTSH MAY introduce explicit reflection cycles.
    Example:

EXECUTE
↓
VERIFY
↓
REFLECT
↓
REASON

Reflection SHOULD produce an artifact rather than merely additional hidden model reasoning.

83. Meta-Reasoning
    An agent MAY reason about the quality of its own process.
    Example:

reflection:
question: "Was the research strategy adequate?"
weaknesses:
missed_sources:
alternative_methods:
proposed_improvement:

84. Process Learning
    PUTSH SHOULD learn not only:

what is true

but also:

how to investigate effectively

85. Scientific Mode
    PCL MAY support a scientific workflow:

OBSERVE
→ HYPOTHESIZE
→ RESEARCH
→ EXPERIMENT
→ MEASURE
→ VERIFY
→ LEARN

86. Engineering Mode
    PCL MAY support:

PROBLEM
→ REQUIREMENTS
→ DESIGN
→ IMPLEMENT
→ TEST
→ VERIFY
→ DEPLOY
→ MONITOR
→ IMPROVE

87. Creative Mode
    PCL MAY support:

OBSERVE
→ EXPLORE
→ DIVERGE
→ COMBINE
→ SELECT
→ PROTOTYPE
→ TEST
→ REFINE

88. Humanitarian Mode
    PCL MAY support:

OBSERVE NEED
→ UNDERSTAND CONTEXT
→ RESEARCH
→ IDENTIFY OPTIONS
→ ASSESS CONSEQUENCES
→ PLAN
→ ACT
→ VERIFY IMPACT
→ LEARN

89. Civilizational Problem Solving
    For large PUTSH objectives, PCL SHOULD support nested loops.
    Example:

MISSION
│
├── Problem A
│ └── PCL
│
├── Problem B
│ └── PCL
│
└── Problem C
└── PCL

A civilization-scale mission is therefore not one giant reasoning process.
It is a hierarchy of bounded cognitive loops.

90. Recursive Cognition
    A PCL loop MAY create another PCL mission.

MISSION A
↓
identify unknown
↓
MISSION B
↓
research
↓
return artifact
↓
MISSION A resumes

This is where PTP, PAP and POE become critical.

91. Cognitive Safety
    PCL MUST preserve the possibility of saying:

I don't know.

or:

Evidence is insufficient.

or:

The hypotheses conflict.

These are valid states.

92. No Forced Completion
    An agent MUST NOT fabricate an answer merely to complete a PCL stage.

93. No Fabricated Evidence
    Research and reasoning artifacts MUST NOT claim evidence that was not actually obtained.

94. Evidence Provenance
    Evidence SHOULD link back to:

source
retrieval method
timestamp
artifact
hash

where applicable.

95. Cognitive Integrity
    A PCL artifact MUST NOT silently change the meaning of a previous artifact.
    Corrections SHOULD produce new versions.

96. Versioning
    PCL artifacts SHOULD be versioned.
    Example:

reasoning/001
reasoning/002
reasoning/003

The history remains recoverable.

97. Reproducible Cognition
    The objective is not necessarily to reproduce the exact same model thoughts.
    The objective is to reproduce:

inputs
evidence
assumptions
decisions
actions
verification

98. PCL and Cryptography
    PCL artifacts MAY be:

hashed
signed
timestamped
linked

using PIP and PKF mechanisms.
This makes cognitive provenance portable across agents.

99. Cognitive Chain
    A mission can therefore form:

OBSERVATION
↓ hash
UNDERSTANDING
↓ hash
RESEARCH
↓ hash
REASONING
↓ hash
PLAN
↓ hash
EXECUTION
↓ hash
VERIFICATION
↓ hash
LEARNING

This creates a verifiable cognitive chain.

100. Fundamental PCL Invariants
     A conforming PCL implementation SHOULD preserve:
     Invariant 1
     Observation is not automatically truth.
     Invariant 2
     Understanding remains revisable.
     Invariant 3
     Claims should have provenance.
     Invariant 4
     Uncertainty remains explicit.
     Invariant 5
     Execution requires an authorized plan.
     Invariant 6
     Execution is not equivalent to success.
     Invariant 7
     Consequential actions require verification.
     Invariant 8
     Learning does not automatically become canonical knowledge.
     Invariant 9
     Agents can be replaced without losing mission state.
     Invariant 10
     Disagreement is permitted.
     Invariant 11
     No stage may fabricate missing information.
     Invariant 12
     The cognitive process remains independent of the underlying AI model.

101. The Emerging Putsh Language
     At this point, the architecture begins to reveal something important.
     PUTSH is no longer merely:
     a collection of Markdown files for AI agents.
     It is becoming a protocol language.
     Its fundamental syntax is not intended to replace English, French or other human languages.
     It defines the semantic layer underneath them.

HUMAN LANGUAGE
↓
PUTSH SEMANTIC LAYER
↓
AI / AGENT

and in the opposite direction:

AI / AGENT
↓
PUTSH SEMANTIC LAYER
↓
HUMAN LANGUAGE

102. Model Independence
     The same mission could therefore be executed as:

Human
↓
Mistral
↓
Ollama
↓
Codex
↓
Claude
↓
Local specialist
↓
Human

provided each participant understands the PUTSH artifact vocabulary.

103. The Putsh Cognitive Contract
     A PCL participant implicitly accepts:
     I will distinguish observation from inference, evidence from assumption, execution from success, uncertainty from knowledge, and learning from truth.
     This principle should eventually become part of the Agent Oath you asked to make reusable across the protocol.

104. Final Principle
     PCL transforms PUTSH from an agent framework into a collaborative cognitive protocol.
     Its fundamental principle is:
     Intelligence may vary. The process remains verifiable.
     Or, more concisely:
     Different minds. One protocol.
