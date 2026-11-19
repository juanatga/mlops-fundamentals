# MLOps Fundamentals

MLOps means **Machine Learning Operations**: the practices that help a team build, deliver, and maintain machine learning systems reliably. Training a model is one step. A useful system also needs repeatable experiments, checks before release, and monitoring after people start using it.

This introduction is adapted from [Core Principles of MLOps](microlesson-01/README.md). It is designed for students who know a little Python and are new to operating machine learning systems. You do not need previous cloud or deployment experience.

## What you will learn

By the end of this lesson, you should be able to:

- Explain why a trained model needs ongoing care.
- Recognize the core principles of MLOps: reproducibility, automation, scalability, monitoring, and governance.
- Record a training experiment with MLflow and explain what was saved.
- Describe the different roles of tools used in an ML workflow.

## Why MLOps matters

Imagine training a model to predict the dominant tree type in a forest patch. You try several settings and get a promising result. A week later, a teammate asks which settings and data produced that model. Later still, someone wants to use it in a different region.

MLOps helps you answer practical questions:

- Can someone repeat the experiment using the same code, data, and environment?
- What evidence shows that this model is ready to use?
- How will predictions reach the people or applications that need them?
- How will you notice when the model becomes less useful?

**Production** means the environment where a model serves real users or business processes. Good notebook results are useful evidence, but production also requires reliable operation.

## The model lifecycle

```text
Prepare data → Train → Evaluate → Deploy → Monitor
                  ↑                          |
                  └── Review and improve ────┘
```

To **deploy** a model is to make it available for use, such as through a prediction service or a scheduled job. Monitoring provides evidence for deciding whether to investigate, retrain, or replace it.

## Core principles

| Principle | What it means | A practical first step |
|---|---|---|
| Reproducibility | Keep enough information to repeat and understand an experiment. | Record the code version, data version, dependencies, training settings, and results. |
| Automation | Turn repeated steps into a consistent, checked workflow. | Run tests and model evaluation automatically when making changes. |
| Scalability and performance | Handle the expected amount of work within time and resource limits. | Measure prediction time and memory use before increasing demand. |
| Monitoring | Observe the running system and check whether its predictions remain useful. | Track failures, response time, input changes, and prediction quality when outcomes become available. |
| Security and governance | Control access and keep a record of how data and models are used. | Limit access to sensitive data and record which model version was released. |

You will also encounter these terms:

- **Continuous integration (CI):** automatically checking changes as they are added to shared code.
- **Continuous delivery (CD):** keeping tested changes ready for release through a repeatable process; release can still require approval.
- **Continuous training (CT):** automating retraining when defined conditions are met, with validation before a new model is released.
- **Data drift:** a change in the inputs the model receives compared with earlier data. Drift is a reason to investigate; it does not automatically mean prediction quality has fallen.

## Hands-on: track a forest classification experiment

Start with [the student notebook](microlesson-02/microlesson-02-student.ipynb). Use [the live demonstration notebook](microlesson-02/microlesson-02-live.ipynb) to review the MLflow implementation after trying the exercise.

The notebooks use a random forest classifier to predict forest cover type from terrain and soil information. They download the dataset on the first run, select 50,000 samples, and split that subset into training and test data. An internet connection is needed for the initial download.

### Getting started

1. Open this folder in your notebook editor, such as VS Code with Jupyter support.
2. Activate your Python virtual environment in the terminal and install the packages listed in [requirements.txt](requirements.txt) using either **pip** or **uv**. Choose one of these commands:

   With pip:

   ```sh
   python -m pip install -r requirements.txt
   ```

   Or with uv (if installed):

   ```sh
   uv pip install -r requirements.txt
   ```

3. Select that same environment as the notebook's **kernel**, the Python process that executes its cells.
4. Open the student notebook and run its cells from top to bottom. Training may take a few minutes, depending on your computer.

### Your task

Complete the final section of the student notebook so it records an experiment with MLflow:

1. Wrap training and evaluation in `with mlflow.start_run():`.
2. Log training settings such as `n_estimators`, `max_depth`, and `min_samples_leaf`.
3. Log the accuracy and save the trained model.
4. Check the live notebook if you need an example of model logging.

In MLflow, a **run** is a record of one experiment. A **parameter** is a setting you choose, such as the number of trees. A **metric** is a measured result, such as accuracy. An **artifact** is a saved output, such as a model or chart.

For a follow-up experiment, change one setting at a time and compare results using a validation split taken from the training data. Keep the test set for the final assessment. Accuracy is the fraction of correct predictions; also inspect performance for each class so that a good overall score does not hide weak results for less common classes.

This exercise introduces experiment tracking. Deployment, automated release checks, and production monitoring are further parts of the lifecycle to explore.

## How the tools fit together

Different tools support different parts of the workflow:

- **Git** tracks changes to code; **DVC** can help version datasets and model files.
- **MLflow** records experiments, metrics, and models.
- **Docker** packages an application and its dependencies into a container image.
- **GitHub Actions or Jenkins** automates steps such as testing, building, and releasing.
- **Kubernetes** coordinates running containers when deployment needs become more complex.

Read [MLOps Tools Compared](MLOps_Tools_Comparison.md) for examples, trade-offs, and a suggested learning order for Docker, Kubernetes, Jenkins, and GitHub Actions. Start by understanding the problem each tool solves; you do not need to install every tool to complete this lesson.

## Discussion: improve this workflow

A data scientist trains a model locally without recording the code version or settings. Someone copies the model to a server by hand. There are no automated tests and nobody monitors the predictions.

Discuss:

1. What could go wrong, and how would the team discover it?
2. What information would you save to make the experiment reproducible?
3. Which check would you automate first, and why?
4. What would you monitor after deployment?

## Further reference material

Read these in order, starting with the broad introduction:

1. [NVIDIA: What Is MLOps?](https://blogs.nvidia.com/blog/what-is-mlops/) — an accessible overview of the field, the teams involved, and why organizations need practices for managing the ML lifecycle.
2. [Google Cloud: MLOps — Continuous delivery and automation pipelines in machine learning](https://docs.cloud.google.com/architecture/mlops-continuous-delivery-and-automation-pipelines-in-machine-learning) — a deeper explanation of how workflows progress from manual processes to automated training and delivery. Use it to connect CI, CD, and CT to the wider lifecycle.
3. [MLOps Tools Compared](MLOps_Tools_Comparison.md) — the local reference for comparing infrastructure and automation tools discussed in this lesson.
4. [Original microlesson: Core Principles of MLOps](microlesson-01/README.md) — the source lesson on which this introduction is based.
