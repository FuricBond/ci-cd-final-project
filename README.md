# CI/CD Final Project

## Project Overview

This project demonstrates the implementation of a Continuous Integration and Continuous Deployment (CI/CD) pipeline for a Python application. It uses GitHub Actions for automated testing and linting, and Tekton Pipelines with OpenShift to automate application validation, container image building, and deployment.

## Objectives

- Automate code quality checks and unit testing.
- Define a repeatable CI/CD workflow.
- Build and push a container image to IBM Cloud Container Registry.
- Deploy the application to an OpenShift environment.
- Verify successful execution of the pipeline and application deployment.

## Technologies Used

- **Python** — application and test execution
- **Flake8** — Python code linting
- **Nose** — unit testing
- **Git and GitHub** — version control and source code hosting
- **GitHub Actions** — automated CI workflow
- **Tekton Pipelines** — CI/CD pipeline orchestration
- **OpenShift** — container orchestration and application deployment
- **Buildah** — container image building
- **IBM Cloud Container Registry (ICR)** — container image storage

## Project Structure

```text
ci-cd-final-project/
├── .github/
│   └── workflows/
│       └── workflow.yml
├── .tekton/
│   ├── tasks.yml
│   ├── pipeline.yml
│   └── pipelinerun.yml
├── service/
├── tests/
├── Dockerfile
├── requirements.txt
├── pvc.yaml
└── README.md
```

*Note: The structure above highlights the main project components. Additional files may be present in the repository.*

## CI/CD Workflow

The Tekton pipeline executes the following stages in sequence:

1. **Cleanup:** Prepares the shared workspace for a fresh pipeline run.
2. **Fetch Repository:** Clones the project repository from GitHub.
3. **Lint:** Runs Flake8 to check the Python source code for errors and style issues.
4. **Unit Tests:** Runs the project's test suite using Nose.
5. **Build Image:** Uses Buildah to build the container image and push it to IBM Cloud Container Registry.
6. **Deploy:** Uses the OpenShift client task to create the application resources from the container image.

The pipeline proceeds to the next stage only after the preceding stage succeeds.

## Prerequisites

Before running the pipeline, ensure that you have:

- Access to the project GitHub repository.
- Access to an OpenShift cluster and the appropriate project namespace.
- Tekton Pipelines and the required tasks installed in the cluster.
- An IBM Cloud Container Registry image repository.
- A registry authentication secret configured for the pipeline.
- A persistent volume claim for the shared workspace.
- The required permissions to build images and deploy applications.

## Running the Pipeline

### 1. Clone the Repository

```bash
git clone https://github.com/FuricBond/ci-cd-final-project.git
cd ci-cd-final-project
```

### 2. Configure OpenShift

Log in to the OpenShift environment and select the appropriate project namespace.

Ensure that the required Tekton tasks, registry authentication secret, and persistent volume claim are configured.

### 3. Apply the Pipeline Definition

```bash
oc apply -f .tekton/tasks.yml
oc apply -f .tekton/pipeline.yml
```

Ensure that the referenced tasks, including the shared OpenShift Pipelines tasks, are available in the namespace.

### 4. Create the Pipeline Run

```bash
oc create -f .tekton/pipelinerun.yml
```

### 5. Monitor Execution

```bash
oc get pipelineruns
oc get taskruns
```

A successful run should report `True` under the `SUCCEEDED` column.

## Deployment Verification

Check the application deployment, pods, and service:

```bash
oc get deployments
oc get pods
oc get services
```

Verify that the application deployment has available replicas and that its pod is running.

The pipeline creates a Kubernetes service for the application. The service must be exposed or accessed through an appropriate route or ingress configuration if external access is required.

## GitHub Actions

The GitHub Actions workflow automates linting and testing whenever the configured workflow triggers occur. This provides an additional validation step before changes are deployed through the Tekton pipeline.

## Expected Outcome

The completed workflow demonstrates automated code validation, unit testing, container image creation, image publishing, and deployment to OpenShift.

## Author

**GitHub:** [FuricBond](https://github.com/FuricBond)

## Repository

[CI/CD Final Project](https://github.com/FuricBond/ci-cd-final-project)
