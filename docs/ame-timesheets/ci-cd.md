# CI/CD Pipeline

This page describes how the AME Timesheets project is built and deployed using GitHub Actions, Docker, Amazon ECR, and EC2.

---

## Workflow Overview

### Step 1: Pull Request Workflow (`build-and-push.yml`)

Triggered on: **Pull Request to `main`**

1. **Checkout code**  
2. **Configure AWS credentials** using GitHub secrets  
3. **Login to Amazon ECR**
4. **Build Docker image for backend**  
   - Injects Django and AWS secrets via `--build-arg`
5. **Push backend image to ECR**
6. **Build Docker image for frontend**  
   - Injects Vite secrets via `--build-arg`
7. **Push frontend image to ECR**

---

### Step 2: Production Deployment (`deploy-prod.yml`)

Triggered on: **Push to `main`**

1. **Matrix deploys to multiple EC2 instances**
2. **SSH into each instance using GitHub Secrets**
3. **Export AWS + RDS environment variables**
4. **Authenticate with ECR from EC2**
5. **Clean up old Docker containers, images, and volumes**
6. **Pull latest backend image from ECR**
7. **Stop and remove existing backend container**
8. **Run new backend container with `.env` and `.pgpass` setup**
9. **Run database migrations**
10. **Pull latest frontend image**
11. **Stop and remove existing frontend container**
12. **Run new frontend container with Vite environment variables**

---

## Summary

- On **PRs**, Docker images for backend and frontend are built and pushed to **Amazon ECR**.
- On **merge to `main`**, EC2 instances pull the images and run them.
- Django migrations are run automatically during deploy.

---

## AWS Services Used

- **ECR** – Docker image registry
- **EC2** – Hosts backend and frontend containers
- **RDS (PostgreSQL)** – Backend database

---

Deployment is fully automated and can scale to additional EC2 instances via the matrix strategy.
