# CI/CD Pipeline

This page outlines the continuous integration and continuous deployment (CI/CD) process for the AME Timesheets platform.

## Full CI/CD Pipeline Diagram

```mermaid
flowchart TD
  A[Pull Request to main] --> B[GitHub Actions: build-and-push.yml]
  B --> C[Configure AWS Credentials]
  C --> D[Login to ECR]
  D --> E[Build Backend Image with Build Args]
  E --> F[Push Backend to ECR]
  D --> G[Build Frontend Image with Build Args]
  G --> H[Push Frontend to ECR]

  I[Push to main] --> J[GitHub Actions: deploy-prod.yml]
  J --> K[Run for each EC2 IP]
  K --> L[SSH into EC2]
  L --> M[Configure AWS CLI and pgpass]
  M --> N[Login to ECR from EC2]
  N --> O[Pull Backend Image]
  O --> P[Stop & Remove Old Backend]
  P --> Q[Run New Backend Container]
  Q --> R[Copy pgpass into Container]
  R --> S[Restart Backend]
  S --> T[Run Migrations]

  N --> U[Pull Frontend Image]
  U --> V[Stop & Remove Old Frontend]
  V --> W[Run New Frontend Container]
```

## Overview

Our CI/CD system uses GitHub Actions to automate the building, testing, and deployment of our backend (Django) and frontend (React) services. The process involves two main workflows:

1. **build-and-push.yml** – Builds Docker images and pushes them to AWS ECR when a pull request is created or updated.
2. **deploy-prod.yml** – Deploys the latest images to our EC2 instances on every push to the `main` branch.

---

## Step-by-Step Breakdown

### 1. Pull Request Trigger (build-and-push.yml)

- Trigger: Any PR targeting `main`
- Steps:
  - Configure AWS credentials
  - Log in to Amazon ECR
  - Build the backend Docker image (injecting secrets and environment variables)
  - Build the frontend Docker image (injecting Vite-specific env vars)
  - Push both images to the appropriate AWS ECR repositories

### 2. Main Branch Push Trigger (deploy-prod.yml)

- Trigger: Push to `main`
- Strategy: Runs per EC2 IP
- Steps:
  - SSH into each EC2 instance
  - Configure AWS CLI and PostgreSQL passwordless authentication
  - Pull latest backend and frontend images from ECR
  - Clean up old containers and volumes
  - Run new containers
  - Run Django migrations for backend

---

If you notice anything outdated or missing, please open a PR or message in the internal dev channel.