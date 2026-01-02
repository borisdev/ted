# Evaldoc

Evaldoc allows natural language to define and execute AI evaluations using one file (`ai-task.eval.yaml`).

## Evaluation Language Design Challenge

Our open source grammar design challenge is to maximizes criteria precision while minimizing the loss of general criteria expressiveness.

## Current Highlights

- **Portability:** A single YAML file represents your evaluation workflow (rubric, input schema, output schema, providers).
- **Swapability:** Your evaluation workflow can swap different criteria, AI providers, and evaluator platform providers.
- **Hackability:** The essential core (the YAML file's grammar or internal representation) is one light pydantic open source module.
- **Transparency:** Rubric predicates contain their natural language source.
- **Precision:** Rubric grammar enforces semantic ambiguity and schema.
- **Expressiveness:**
  - Rubric conditional behavioral predicates express more than the `actual-expected` pattern.
  - conditional predicates are evaluated over a panel observations of outcome and side-effect
    observations
  - conditional predicates contain severity levels

## Dialog Illustration

- President: Prove to me why I should buy your AI?
- You: Have all your stakeholder email me their `eval-doc` file, and I will make you a reproducible report.
- The People: All the power must not be in the hands of AI Engineers in making the AI Traffic Police Bots.
- You: Email your representative your personal `eval-doc` file judging the quality of Traffic Police Bots.

## Aims by Phase

- The short term aim is to make it simpler to jump start a spec driven development of a LLM prompt without being tied to any one runtime framework.
- The middle term aim is to make it simpler and more expressive for developers to evaluate AI agents.
- The long term aim is for people with zero knowledge of how AI works to control the criteria of good AI behavior.

## Example CLI usage

```bash
evaldoc --interpret "the task is to discover what PMC manuscript facts we can query to help the user with her problem"
evaldoc --interpret "the task output must contain a field of type list: `audience_memberships`
evaldoc --interpret "clinical trials on animals are never relevant to the biohacker audience"
evaldoc --interpret "run evaluations against these raw input conversation threads in dir `/.ted/nobsmed/test_cases`
evaldoc --interpret "run evaluations against this AI Agent invocation`

#or

evaldoc compose --interpret --input instructions.md --output my-ted-compose.yaml

evaldoc evaluate --format=md >> report.model
#or
evaldoc evaluate -f my-ted-compose.yaml
# console output looks like....
{
    "summary": .....
    "faults": ...
}
```

## Components

A valid`some_task.eval.yaml` requires three main parts:

### what are we looking at? - observations schema

### what is the criteria or rubric for "good behavior"? - predicates

### who we are judging? - provider bindings
