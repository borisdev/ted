# Oracle Spec: A Standard Language for Non-Coders to Define Good AI Behavior

Oracle-spec is a controlled natural language (CNL) and schema for declaring quality-control predicates that compile into executable checks.

Oracle-spec enables non-coders to author verifiable definitions of “good behavior”—rules, constraints, and rubrics—without writing custom evaluator code. The oracle-spec runtime enforces unambiguous syntax and defined semantics, so authors can safely use LLM-assisted tools (e.g., Codex, Claude Code) to draft and refine rules while preserving determinism.

After execution, oracle-spec produces human-readable feedback from (1) schema/grammar validation and (2) predicate evaluation results. This feedback helps authors clarify intent, resolve ambiguity, and iteratively converge on a precise behavioral contract within the controlled grammar.

## Design Principles

1. Declarative Spec for Grammar Feedback

By authoring an oracle using a schema-validated declarative specification (ie., a
controlled natural language) a non-coder can unambiguously express normative
behavioral assertions on changes in key observable states in his natural language
since they can get feedback to fix any of their ambiguous directives.

2. Executable for Assertion Feedback

By compiling an oracle into an evaluator, we can execute one or more
AI agents against the full oracle or the proposed changes in the oracle
to give fine-tuning feedback.

3. Multi-state, multi-time

Impact is computed from changes between pre- and post-run states. An oracle may
observe multiple states across multiple times. For example, what was the impact
of an agent pre-intervention, post-intervention, and follow-up evaluation?

4. Predicates to express behavioral expectations

Evaluation is based on predicates, which are more general than the deterministic `actual vs expected` form.
Predicate assertion may carry severity levels.

## Motivation

### Principles

- do not need to code
- can swap different competing AI agent candidates
- can steer the fine-tuning of AI agents
- can get feedback on their directives
- can formulate wholistic evaluations based on a variety of competing concerns
- can formulate evaluation based on many pre and post conditions (states)
- can read and share an AI oracle
- can familiarize themselves with a standard

## Core Innovation

This addresses the fundamental **"alignment specification" problem** - how do domain experts encode their knowledge and values into AI systems without needing to be ML engineers.

## Key Strengths

**1. Separation of Concerns**

```yaml
# Subject matter experts write the "what"
contract.yaml:
  behavior: classify_biohacking_relevance
  criteria: intervention + measurable_outcome

# ML engineers handle the "how"
model_config.py:
  architecture: transformer
  training: fine_tune_on_oracle_spec
```

```yaml
# oracle-spec.yaml
apiVersion: oracle/v1
kind: BehaviorSpec
metadata:
  domain: biohacking_relevance
  version: 2.1.0

spec:
```

## Governance Benefits

**Auditability**: "Why did the AI make this decision?" → "It followed oracle-spec v2.1, section 3.4"
**Accountability**: Clear ownership chain from domain expert → oracle spec → AI behavior
**Transparency**: Stakeholders can read and critique the behavioral specification
**Iteration**: Update specs based on real-world performance without retraining models
