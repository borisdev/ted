# Eval Compose

Eval Compose uses a single YAML configuration file to define expectations of AI task behavior and run multiple AI candidates against those expectations.

Using a declarative language over custom coding

- Standardization
- Simpler to compare then swap AI products, to read, edit, share, and compare expectations
- Paves the way to swap out different evaluator run times
- Paves the way for AI control by non-coders through a natural language interpretor.

Using predicates is more expressive to represent expectations than the classical `actual-expected` ML pattern.
A test oracle is a predicate/mechanism that decides whether observed behavior is acceptable by comparing actual results to expected results.
a test oracle is a mechanism that determines whether a system’s observed output/behavior is correct for a given test input (i.e., assigns pass/fail against expected behavior).

Oracle Compose is a single YAML spec for declaring expected AI task behavior and running the same evaluation across multiple candidates

## Evaluation Oracle has Two Roles

1. Elicitor that points out ambiguity

- Human oracle analogy: Socrates asks "...what is piety, and what is impiety?”
- AI Test-Oracle example: "...how do we formulate a 'good behavior' measure with these available variables?"
- Depends on: grammar rules to enforce natural language precision

2. Advisor that guides action

- Human oracle analogy: Context: On the eve of the Kurukshetra battle (the climactic war in the Mahābhārata), Arjuna rides out between the armies with Krishna as charioteer and sees his own relatives and revered teachers on both sides....Arjuna asks Krishna, “I am confused about my duty… Please instruct me for certain what is best for me [Encyclopedia Britannica](https://www.britannica.com/topic/Mahabharata)".
- AI Test-Oracle example: "Should I sacrifice the precision of GPT 5.1 and go with GPT 4.1 to save money?"
- Depends on: asserting normative rules based on observations from simulated action

# Oracle-Compose to define Good AI Behavior

Docker Compose is a tool for defining and running multi-container Docker applications. It uses a single YAML configuration file (typically named compose.yaml or docker-compose.yml) to manage an entire application stack, including services, networks, and volumes, with a single command.

Key Features and Benefits
Simplified Configuration: Instead of using complex, long docker run commands for each container, you use a declarative YAML file to specify all configurations, making the setup readable and version-controllable.
Multi-Container Management: It orchestrates multiple containers as a single unit, allowing different services like a web app, database, and cache to run and communicate seamlessly.
Reproducible Environments: By defining the entire environment in a single file, Docker Compose ensures consistency across different environments (development, testing, staging, and production), allowing anyone to spin up the entire application stack with just a few commands.
Lifecycle Management: It provides commands to manage the whole lifecycle of the application, including starting (docker compose up), stopping (docker compose down), viewing status, and streaming logs for all services.
Automated Networking: By default, Compose automatically creates a dedicated network for all the services defined in the file, allowing them to discover and communicate with each other using their service names as hostnames.

eval-compose is an open-source tool that lets you define an AI task evaluation policy in a declarative way.
The aim here is to have developers use a machine-readable YAML file to define a
task evaluation policy that can be run against competing AI agents.

How it Works (Three Steps)
Define the environment with a Dockerfile for each service to ensure it can be reproduced anywhere.
Define the services that make up your application in a compose.yaml file so they can run together in an isolated environment.
Run docker compose up in your project directory to create and start all the services.

```console
policy-compose validate -f biohack-extract-policy.yaml
policy-compose evaluate -f biohack-extract-policy.yaml
policy-compose nl_update -f biohack-extract-policy.yaml --text "never use bad words"
```

Execution produces two types of feedback:

1. schema and ambiguity validation and (2) predicate evaluation results. This feedback
   helps authors clarify intent, resolve ambiguity, and iteratively compose an AI behavioral contract using natural language.

## Highlights

1. Declarative YAML Artifact (portable, transparent)
2. Schema validation (natural language expression with precision via feedback)
   By authoring an oracle using a schema-validated declarative specification (ie., a
   controlled natural language) a non-coder can unambiguously express normative
   behavioral assertions on changes in key observable states in his natural language
   since they can get feedback to fix any of their ambiguous directives.
3. Test Cases and Invariants with Predicate assertions (general)
   Evaluation is based on predicates, which are more general than the deterministic `actual vs expected` form.
   Predicate assertion may carry severity levels.
4. Multi-state, multi-time (general)
   Impact is computed from changes between pre- and post-run states. An oracle may
   observe multiple states across multiple times. For example, what was the impact
   of an agent pre-intervention, post-intervention, and follow-up evaluation?

https://raw.githubusercontent.com/replicate/cog/refs/heads/main/README.md
How it works

Define the Docker environment your model runs in with cog.yaml:

build:
gpu: true
system_packages: - "libgl1-mesa-glx" - "libglib2.0-0"
python_version: "3.12"
python_packages: - "torch==2.3"
predict: "predict.py:Predictor"

Define how predictions are run on your model with predict.py:

from cog import BasePredictor, Input, Path
import torch

class Predictor(BasePredictor):
def setup(self):
"""Load the model into memory to make running multiple predictions efficient"""
self.model = torch.load("./weights.pth")

    # The arguments and types the model takes as input
    def predict(self,
          image: Path = Input(description="Grayscale input image")
    ) -> Path:
        """Run a single prediction on the model"""
        processed_image = preprocess(image)
        output = self.model(processed_image)
        return postprocess(output)

## Illustration

Extraction AI prompt

Why are we building this?
It’s really hard for researchers to ship machine learning models to production.

Part of the solution is Docker, but it is so complex to get it to work: Dockerfiles, pre-/post-processing, Flask servers, CUDA versions. More often than not the researcher has to sit down with an engineer to get the damn thing deployed.

Andreas and Ben created Cog. Andreas used to work at Spotify, where he built tools for building and deploying ML models with Docker. Ben worked at Docker, where he created Docker Compose.

We realized that, in addition to Spotify, other companies were also using Docker to build and deploy machine learning models. Uber and others have built similar systems. So, we’re making an open source version so other people can do this too.

Hit us up if you’re interested in using it or want to collaborate with us. We’re on Discord or email us at team@replicate.com.
