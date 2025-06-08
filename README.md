# Java Maven Docker Deployment with Ansible

This project automates the deployment of a Java Maven application into Docker containers on remote AWS EC2 instances using **Ansible**.

---

## Overview

- Uses **OpenJDK 17** and **Maven 3.8.7** to build the Java project.
- Clones your project from a GitHub repository.
- Builds and pushes Docker images to Docker Hub.
- Uses a dynamic AWS EC2 inventory based on instance tags (`Environment=dev`, `Role=web`).
- Automates all steps via an Ansible playbook and role.

---

## Assumptions

- You have a **master/control server** where Ansible is installed and configured.
- AWS EC2 target servers are running Ubuntu, already launched, and tagged appropriately.
- SSH key-based access to EC2 instances is configured for the `ubuntu` user.
- Docker Hub account exists for pushing images.
- Your `ansible-vault` password is securely stored or known.

---

## Prerequisites

- Ansible installed on the control machine.
- AWS CLI configured or credentials set up for dynamic inventory.
- Access to GitHub repo containing your Java Maven project.
- Docker Hub credentials for image pushing.

---

## Setup

1. **Configure Inventory**

   Inventory is dynamically fetched from AWS EC2 instances with:

   - Region: `ap-south-1`
   - Filters: `Environment=dev`, `Role=web`
   - Only running instances included

2. **Define Variables**

   Edit `group_vars/all.yaml` to set:

   - `git_repo_url` — your GitHub repository URL
   - `repo_dir` — remote directory for the project
   - `docker_image_name` — Docker image name (e.g., `yourdockerhub/yourapp`)
   - `docker_tag` — Docker image tag (e.g., `latest`)

3. **Store Secrets**

   Create `vault.yaml` with your Docker Hub credentials:

   ```yaml
   ansible-vault create vault.yaml
   ```
