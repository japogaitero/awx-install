# AWX Deployment Automation on Kubernetes

This project provides a fully automated, modular approach to deploying AWX (Ansible AWX) on a local Kubernetes cluster using Ansible. 

By leveraging Ansible Roles, this infrastructure-as-code solution modularizes the deployment process, ensuring idempotency, readability, and scalability. It is designed to run on development environments like Docker Desktop (WSL2).

## Key Features
* Modular Design: Uses Ansible Roles to separate operator installation from instance configuration.
* Idempotent: Safe to run multiple times; the system checks current state before applying changes.
* Configurable: Centralized variable management (group_vars) to easily adjust namespaces, ports, or versions.
* Kustomize Integration: Uses the official AWX Operator via standard deployment pipelines.

## Prerequisites

Before running the playbook, ensure you have the following installed:

* Docker Desktop (with Kubernetes enabled).
* kubectl (configured to point to your docker-desktop context).
* Python 3.x.
* Ansible (installed in your environment).

## Setup Instructions

### 1. Clone the repository
git clone <your-repo-url>
cd awx-install

### 2. Set up the Python Virtual Environment
It is best practice to keep dependencies isolated:

# Create the virtual environment
python3 -m venv venv

# Activate it
source venv/bin/activate  # On Linux/WSL

# Install Ansible and Kubernetes dependencies
pip install --upgrade pip
pip install ansible kubernetes

### 3. Verify Kubernetes Context
Ensure your terminal is talking to the correct cluster:
kubectl config current-context
# Should return 'docker-desktop'

## Usage

Execute the entire deployment using the main orchestration playbook:

ansible-playbook site.yml

This will run the roles in order:
1. Operator: Deploys the AWX Operator using Kustomize.
2. Instance: Configures the AWX server instance based on your parameters.

## Project Structure

The project follows the standard Ansible directory hierarchy, which promotes maintainability and clean code:

awx-install/
├── site.yml              # Entry point for the orchestration
├── group_vars/
│   └── all.yml           # Global configuration variables
├── roles/
│   ├── operator/         # Logic to deploy the AWX Operator
│   └── instance/         # Logic to deploy the AWX Instance
│       └── templates/    # Jinja2 templates for configurations
└── README.md

## Troubleshooting
* Connection Refused: If you get connectivity errors, ensure your Docker Desktop Kubernetes cluster is running and your kubectl context is set to docker-desktop.
* Deployment Stuck: Check the pods in the namespace: kubectl get pods -n awx.

