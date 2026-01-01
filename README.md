# NoBS Compose

NoBS Compose uses a single YAML file to define and run an evaluation of an AI task.

## Highlights

1. Standardization: Declarative YAML (portable, transparent).
2. Precision: The schema of the YAML allows a SWE Agent (ie., Claude Code, Codex) to interpret a human's natural language evaluation authoring instructions with less hallucination and feedback when there is ambiguity.
3. Expressiveness: Test Cases and Invariants with Predicate assertions (general)
   Evaluation is based on predicates, which are more general than the deterministic `actual vs expected` form.
   Predicate assertion may carry severity levels.
   Multi-state, multi-time (general)
   Impact is computed from changes between pre- and post-run states. An oracle may
   observe multiple states across multiple times. For example, what was the impact
   of an agent pre-intervention, post-intervention, and follow-up evaluation?
4. Simpler workflow
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
