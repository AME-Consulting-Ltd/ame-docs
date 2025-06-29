# AWS Infrastructure (Preliminary Overview)

> ⚠ This page may be out of date or incomplete. If you're familiar with our AWS setup, please help improve it by submitting an update or pull request!

## Overview

AME Timesheets uses several AWS services to host and deploy its application stack:

- **Amazon EC2** – Hosts the Docker containers for the Django backend and React frontend
- **Amazon ECR** – Stores Docker images built and pushed from GitHub Actions
- **Amazon RDS (PostgreSQL)** – Handles persistent database storage for the backend
- **Amazon S3** – (Assumed) Stores uploaded files and serves static/media assets
- **IAM Roles and Secrets Manager** – Provides secure credentials and scoped access
- **Route 53 / Elastic IPs** – (Assumed) Used for DNS or static IP mapping for frontend/backend
- **AWS CLI** – Used to authenticate, pull/push images, and deploy containers in EC2 instances

## Current Architecture

```
Developer Pushes to GitHub → GitHub Actions Build & Push Docker Images → ECR

Main Branch Push → GitHub Actions Deploy to EC2:
  - Pulls Docker Images
  - Stops old containers
  - Runs new containers (backend + frontend)
  - Runs DB Migrations
```

## EC2 Details

- Two EC2 instances (assumed to be Amazon Linux 2)
- Docker installed
- SSH access via GitHub Actions + injected secrets (`SSH_KEY`)
- Pulls latest images from ECR and runs them

### Backend

- Django runs on port `8000`
- Migrations are applied after container starts
- `.env` file is loaded inside container
- `.pgpass` is used to authenticate with RDS

### Frontend

- React app (Vite) runs on port `4173`
- Login credentials and server URL passed via build-time env vars

## Deployment Workflow

- Build + Push on **Pull Request** to `main`:
  - Triggers `build-and-push.yml`
  - Builds both backend and frontend images
  - Pushes to ECR

- Deploy on **Push to main**:
  - Triggers `deploy-prod.yml`
  - Deploys to both EC2 instances

## To Be Confirmed / Updated

- [ ] Are we using S3 for media/static files?
- [ ] How are EIPs / domain names managed (Route53)?
- [ ] Do we use a load balancer or DNS round-robin between EC2 instances?
- [ ] Are logs collected anywhere (e.g., CloudWatch)?
- [ ] Are there VPC or security group customizations?
- [ ] Is HTTPS termination handled via Nginx or another service?

## Related Files

- `.github/workflows/build-and-push.yml`
- `.github/workflows/deploy-prod.yml`
- `backend.env`
- Dockerfiles for backend and frontend

---

> 🛠 **Contribute:** Please help fill in the gaps by editing this file or opening a pull request.