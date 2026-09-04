# 🚀 Production CI/CD Platform

A production-style DevOps project demonstrating an end-to-end CI/CD pipeline for deploying a containerized Flask application to an Azure virtual machine.

The project combines application development, automated testing, code quality, security scanning, Docker, GitHub Actions, GitHub Container Registry, Terraform, Azure, Ansible, SSH, and automated deployments.

---

## 🏗️ Architecture

```text
Developer
    │
    │ git push
    ▼
GitHub Repository
    │
    ▼
GitHub Actions CI
    │
    ├── Ruff
    ├── pip-audit
    ├── Pytest
    ├── Docker Build
    │
    ▼
GitHub Container Registry (GHCR)
    │
    │ Immutable SHA-tagged image
    ▼
GitHub Actions CD
    │
    ├── Automatic deployment
    │     └── workflow_run
    │
    └── Manual deployment
          └── workflow_dispatch
    │
    ▼
Ansible
    │
    │ SSH
    ▼
Azure Virtual Machine
    │
    ▼
Docker Container
    │
    ▼
Flask Application
    │
    ├── /
    └── /health


🎯 Project Goals

This project was built to demonstrate practical DevOps and production-support engineering skills:

Automate application testing and validation
Build containerized applications
Implement CI/CD with GitHub Actions
Publish Docker images to GHCR
Use immutable Docker image tags
Provision Azure infrastructure using Terraform
Configure and deploy applications using Ansible
Deploy securely over SSH
Support both automatic and manual deployments
Verify application health after deployment
🛠️ Technology Stack
Category	Technology
Application	Python, Flask
Testing	Pytest
Code Quality	Ruff
Security	pip-audit
Containerization	Docker
Container Registry	GitHub Container Registry
CI/CD	GitHub Actions
Infrastructure as Code	Terraform
Cloud	Microsoft Azure
Configuration Management	Ansible
Remote Access	SSH
Operating System	Ubuntu Linux
Version Control	Git, GitHub


📁 Project Structure

production-cicd-platform/
│
├── app/
│   ├── app.py
│   ├── requirements.txt
│   └── tests/
│       └── test_app.py
│
├── ansible/
│   ├── site.yml
│   └── inventory.ini
│
├── terraform/
│   ├── main.tf
│   ├── variables.tf
│   ├── outputs.tf
│   └── ...
│
├── .github/
│   └── workflows/
│       ├── ci.yml
│       ├── deploy.yml
│       └── ssh-test.yml
│
├── Dockerfile
├── docker-compose.yml
├── requirements-dev.txt
├── .gitignore
└── README.md


🐍 Flask Application

The project contains a lightweight Flask REST API.

Application endpoint
GET /

Example response:

{
  "message": "Production CI/CD Platform",
  "status": "running",
  "version": "2.0"
}
Health endpoint
GET /health

Example response:

{
  "status": "healthy"
}

The /health endpoint is used to verify that the deployed application is responding correctly.


🐳 Docker

The Flask application is containerized using Docker.

Build locally
docker build -t production-cicd-platform .
Run locally
docker run -d \
  --name production-cicd-app \
  -p 5000:5000 \
  production-cicd-platform

Test:

curl http://localhost:5000/

Health check:

curl http://localhost:5000/health


🐳 Docker Compose

The project also includes Docker Compose configuration.

Start the application:

docker compose up -d

Check containers:

docker compose ps

View logs:

docker compose logs

Stop the application:

docker compose down

The Compose configuration includes a Docker health check for the Flask /health endpoint.


🔄 CI Pipeline

GitHub Actions automatically runs the CI pipeline whenever changes are pushed to the main branch.

CI stages
Checkout
   ↓
Setup Python
   ↓
Install Dependencies
   ↓
Ruff
   ↓
pip-audit
   ↓
Pytest
   ↓
Docker Build
   ↓
Push Docker Image

The pipeline performs:

1. Code quality

Ruff checks the Python code:

ruff check .
2. Security audit

Python dependencies are scanned using:

pip-audit
3. Automated testing

Tests are executed with:

pytest
4. Docker build

The application image is built automatically.



📦 GitHub Container Registry

Docker images are published to GitHub Container Registry:

ghcr.io/aftablone98/production-cicd-platform

Each successful CI build publishes:

latest

and an immutable commit-based tag:

<git-commit-sha>

Example:

ghcr.io/aftablone98/production-cicd-platform:d4c886f7e257906f2dc24397c0ed1f64f6a5df92
Why SHA tags?

Using Git commit SHA tags makes deployments reproducible.

Instead of depending only on:

latest

the deployment can explicitly identify the exact image associated with a Git commit.

This helps with:

Reproducibility
Debugging
Auditing
Rollbacks
Deployment traceability


☁️ Azure Infrastructure

Azure infrastructure is provisioned using Terraform.

The infrastructure includes:

Resource Group
Virtual Network
Subnet
Network Security Group
Network security rules
Public IP
Network Interface
Linux Virtual Machine

Terraform provides Infrastructure as Code so the Azure environment can be recreated consistently.



🏗️ Terraform

Terraform is used to provision the Azure infrastructure.

Typical workflow:

terraform init
terraform plan
terraform apply

To destroy infrastructure:

terraform destroy

Terraform state files are excluded from Git using .gitignore.


⚙️ Ansible Deployment

Ansible is used to configure the Azure VM and deploy the application.

The deployment performs:

SSH to Azure VM
      ↓
Ensure Docker is running
      ↓
Authenticate with GHCR
      ↓
Pull application image
      ↓
Remove existing container
      ↓
Start new application container

The application is exposed through:

Azure VM port 80
        ↓
Docker port 5000
        ↓
Flask application


🔐 Ansible + GHCR Authentication

GHCR credentials are supplied through environment variables rather than hard-coded credentials.

Example variables:

GHCR_USERNAME
GHCR_TOKEN
APP_IMAGE

Secrets are managed through GitHub Actions Secrets for CI/CD execution.

Sensitive files such as:

.env
ansible/vault.yml

are excluded from Git.


🚀 Automatic Deployment

After a successful CI workflow, GitHub Actions automatically triggers the deployment workflow.

The process is:

git push
   ↓
CI
   ↓
Tests
   ↓
Security checks
   ↓
Docker build
   ↓
GHCR
   ↓
Successful CI
   ↓
workflow_run
   ↓
Ansible
   ↓
Azure VM

The deployment uses the SHA of the successful CI workflow to select the exact Docker image.


🖱️ Manual Deployment

The deployment workflow also supports manual execution using:

workflow_dispatch

A Docker image tag can be supplied manually.

Example:

image_tag:
d4c886f7e257906f2dc24397c0ed1f64f6a5df92

This allows a specific image version to be deployed without rebuilding the application.

This can be useful for:

Rollbacks
Testing a previous build
Controlled deployments
Troubleshooting


🔍 Deployment Verification

After deployment, the application can be verified using:

curl http://<AZURE_VM_IP>/

Expected:

{
  "message": "Production CI/CD Platform",
  "status": "running",
  "version": "2.0"
}

Health check:

curl http://<AZURE_VM_IP>/health

Expected:

{
  "status": "healthy"
}


🔑 Key DevOps Concepts Demonstrated

This project demonstrates practical experience with:

Linux administration
Git and GitHub
Python automation
Flask
REST APIs
Automated testing
Code quality
Dependency security scanning
Docker
Docker Compose
GitHub Actions
CI/CD
Container registries
Immutable image tagging
Infrastructure as Code
Terraform
Microsoft Azure
SSH
Ansible
Configuration management
Automated deployments
Application health checks
Deployment troubleshooting
Production-style troubleshooting


🧪 Example CI/CD Flow

A typical deployment looks like:

1. Developer changes application
          ↓
2. git commit
          ↓
3. git push
          ↓
4. GitHub Actions starts CI
          ↓
5. Ruff checks code
          ↓
6. pip-audit scans dependencies
          ↓
7. Pytest executes tests
          ↓
8. Docker image is built
          ↓
9. Image pushed to GHCR
          ↓
10. Successful CI triggers deployment
          ↓
11. Ansible connects to Azure VM
          ↓
12. Exact SHA image is pulled
          ↓
13. Existing container is replaced
          ↓
14. New container starts
          ↓
15. Application health verified


🛡️ Security Practices

The project incorporates several security practices:

GitHub Secrets for sensitive CI/CD credentials
SSH key authentication
No secrets committed to Git
.gitignore protection for sensitive files
Dependency vulnerability scanning with pip-audit
Docker image scanning during development
Immutable image tagging
Restricted Azure network access through NSG rules


📈 Future Improvements

Planned enhancements include:

Jenkins CI/CD integration
Kubernetes deployment
Monitoring and observability
Prometheus
Grafana
Centralized logging
Automated rollback
Blue/green deployments
Infrastructure monitoring
Production alerting
End-to-end DevOps platform


💼 Portfolio Highlights

This project demonstrates the ability to build and operate a complete application delivery pipeline rather than only writing application code.

Highlights
Python + Flask
       +
Docker
       +
GitHub Actions
       +
GHCR
       +
Terraform
       +
Azure
       +
Ansible
       +
SSH
       =
End-to-End CI/CD Platform


👨‍💻 Author

Aftab Lone

Production Support Engineer | DevOps | Linux | Python | Azure

GitHub:

https://github.com/AftabLone98

LinkedIn:

https://linkedin.com/in/aftablone
