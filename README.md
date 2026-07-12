# Tax_Calculator_Project

This repository contains the Tekton CI/CD pipeline configuration used to build, test, and deploy the **Tax Calculator** application as part of the IBM Skills Network Cloud Native, DevOps, Agile, and NoSQL final project.

## Overview

The Tax Calculator app is containerized with Docker and deployed to **IBM Cloud Code Engine**. This pipeline automates the process using **Tekton**, ensuring unit tests pass before the application image is built and pushed to the IBM Container Registry.

## Pipeline Stages

1. **Clone** — Pulls the application source code from the [zntwk-tax_calculator](https://github.com/ibm-developer-skills-network/zntwk-tax_calculator) repository.
2. **npminstall** — Installs Node.js dependencies using `npm install`.
3. **tests** — Runs unit tests with the **Jasmine** testing framework (`npx jasmine`).
4. **build** — Builds and pushes the Docker image to IBM Container Registry using **Buildah**, but only if all tests pass.

## Files

| File | Description |
|------|-------------|
| `Dockerfile` | Defines the Docker image, using `nginx` as the base image and copying app files into `/usr/share/nginx/html/`. |
| `tasks.yaml` | Defines the custom Tekton Tasks: `npm` (installs dependencies) and `jasmine` (runs unit tests). |
| `pipeline-tasks-snippet.yaml` | Contains the `npminstall`, `tests`, and `build` Task references that were added into the main `pipeline.yaml`. |
| `run.yaml` | Defines the `PipelineRun` resource used to trigger and execute the pipeline. |

## Deployment

The final application image is deployed to **IBM Cloud Code Engine**:

```bash
ibmcloud ce application create --name tax-calculator \
  --image us.icr.io/${SN_ICR_NAMESPACE}/tax-calculator \
  --registry-secret icr-secret \
  --port 80
```

## Running the Pipeline

```bash
kubectl apply -f tasks.yaml
kubectl apply -f pipeline.yaml
kubectl create -f run.yaml

# Monitor the pipeline run
tkn pipelinerun logs --last
```

## Author

Prashant Pal

---
*Final Project for the IBM Skills Network Cloud Native, DevOps, Agile, and NoSQL course.*
