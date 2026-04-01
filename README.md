# GIBB-PSA Infrastructure Automation

This repository provides Ansible playbooks to provision and manage a
standardized learning environment across multiple Linux nodes.

## Infrastructure Overview

The setup follows a \"Control Node -\> Managed Nodes\" architecture.

- **Control Node**: Your local machine (e.g., Kali Linux/WSL) where
  Ansible is installed.
- **Managed Nodes**: Three target servers (VMs) acting as the project
  environment.
- **Automation Flow**: Ansible connects via SSH, installs the required
  software stack (Docker, Git), clones specific repositories for each
  server, and launches a web service using Docker-Compose.

## Prerequisites

- **Ansible**: Must be installed on the control node.
- **SSH-Key Authentication**: Your public key must be authorized on all
  target nodes.
- **Inventory**: A valid `hosts.ini` file must be present in the project
  root.

## Project Structure

- `hosts.ini`: Defines target inventory, IP addresses, and repository
  mapping.
- `setup_env.yml`: Main playbook for provisioning (installs Docker/Git,
  clones repos, starts services).
- `cleanup_env.yml`: Teardown playbook (stops containers and removes
  project directories).

## Usage

### 1. Connectivity Check

Verify that the control node can reach all servers and that the Python
interpreter is functional:

``` bash
ansible nodes -m ping -i hosts.ini
```

### 2. Infrastructure Deployment (Setup)

Execute the rollout to install all dependencies and start the
environment:

``` bash
ansible-playbook -i hosts.ini setup_env.yml
```

### 3. Infrastructure Teardown (Cleanup)

Stop all running services and wipe the project data from target nodes to
restore a clean state:

``` bash
ansible-playbook -i hosts.ini cleanup_env.yml
```

## Inventory Configuration (hosts.ini)

Ensure your `hosts.ini` is configured as follows to map the servers and
their specific repositories:

``` ini
[nodes]
server-1 ansible_host=192.168.110.10 git_repo_url=https://github.com/GIBB-PSA/server-1.git
server-2 ansible_host=192.168.110.11 git_repo_url=https://github.com/GIBB-PSA/server-2.git
server-3 ansible_host=192.168.110.12 git_repo_url=https://github.com/GIBB-PSA/server-3.git

[nodes:vars]
ansible_user=vmadmin