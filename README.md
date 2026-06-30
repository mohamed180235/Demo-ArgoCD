# GitOps Setup with KinD and ArgoCD

This repository provides a step-by-step guide to setting up a local Kubernetes cluster using **KinD (Kubernetes in Docker)** on an AWS EC2 instance and deploying **ArgoCD** for GitOps continuous delivery.

---

## Infrastructure Prerequisites

Before running the installation scripts, provision an AWS EC2 instance with the following minimum specifications to prevent Kubernetes or Docker from crashing:

* **AMI:** Ubuntu Server 24.04 LTS (64-bit)
* **Instance Type:** `t3.medium` (Minimum 2 vCPUs and 4 GiB RAM required)
* **Storage:** 30 GB gp3 SSD
* **Security Group Rules:**
  * **SSH (Port 22):** Restrict to your public IP.
  * **Custom TCP (Port 8080):** Open to `0.0.0.0/0` (Anywhere) for accessing the ArgoCD UI dashboard.

---

## Installation & Setup Steps

SSH into your EC2 instance and execute the following phases sequentially.

### Phase 1: Update and Prepare System Dependencies
Update your local package indexes and install fundamental networking/certificate utilities.

```bash
sudo apt-get update && sudo apt-get upgrade -y
sudo apt-get install -y curl apt-transport-https ca-certificates gnupg

---

### Phase 2: Install and Configure Docker
KinD requires Docker to spin up Kubernetes nodes as containers.

```bash
# 1. Install Docker components
sudo apt-get update
sudo apt-get install -y docker.io

# 2. Start and enable the Docker daemon
sudo systemctl start docker
sudo systemctl enable docker

# 3. Grant your current 'ubuntu' user permission to use Docker without sudo
sudo usermod -aG docker $USER
newgrp docker