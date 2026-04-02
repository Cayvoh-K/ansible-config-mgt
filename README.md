# Ansible Configuration Management - Infrastructure Automation Refactoring(Testing)

This repository contains the configuration management code for automating server setup using **Ansible**. The project demonstrates a full CI/CD workflow where infrastructure changes are developed on feature branches, peer-reviewed via Pull Requests, and automatically deployed/archived via **Jenkins**.

## 🛠 Project Architecture
The infrastructure consists of several EC2 instances playing different roles:
* **Ansible Control Node:** Jenkins-Ansible Server (RHEL 8)
* **NFS Server:** Storage management (RHEL 8)
* **Database Server:** Database backend (RHEL 8)
* **Web Servers:** Two frontend servers (RHEL 8)
* **Load Balancer:** Traffic distribution (Ubuntu 24.04)

<img width="627" height="614" alt="Screen Shot 2026-03-12 at 19 34 47" src="https://github.com/user-attachments/assets/48fff1e8-d225-4cba-afc0-387ed58e1618" />

🚀 Technical Implementation

### 1. SSH Agent Forwarding
To maintain high security, **SSH-Agent Forwarding** was implemented. This allows the Control Node to securely communicate with the target servers using the local workstation's private key without ever storing the `.pem` file on the AWS cloud instance.

### 2. Inventory Management
The project uses a structured inventory system to manage different environments:
* **Location:** `/inventory`
* **Files:** `dev`, `staging`, `uat`, `prod`
* **Format:** INI-style host groupings with specific `ansible_ssh_user` definitions for both RHEL (`ec2-user`) and Ubuntu (`ubuntu`) distributions.

<img width="1227" height="368" alt="Screen Shot 2026-03-12 at 07 09 10" src="https://github.com/user-attachments/assets/c5a5ee23-55ea-4e89-bcc5-3d06e5d2b267" />

<img width="1073" height="471" alt="Screen Shot 2026-03-12 at 08 26 55" src="https://github.com/user-attachments/assets/1c222ccd-c18e-4e31-9920-74ca9370db9c" />


### 3. Playbook Automation
The `playbooks/common.yml` script was developed to handle baseline configurations. 
* **RHEL/CentOS Tasks:** Uses the `yum` module.
* **Debian/Ubuntu Tasks:** Uses the `apt` module.
* **Objective:** Automated the installation of tools like **Wireshark** to ensure environment parity across all tiers.

### 4. CI/CD Workflow
1.  **Version Control:** Git used for branching (`feature/prj-145-lvm`).
2.  **Collaboration:** "Four-Eyes Principle" applied via GitHub Pull Requests.
3.  **Automation:** Jenkins triggers a build upon every merge to `master`, archiving code as build artifacts in `/var/lib/jenkins/jobs/ansible/builds/`.

<img width="1270" height="637" alt="Screen Shot 2026-03-11 at 21 20 00" src="https://github.com/user-attachments/assets/e693af92-f234-416f-9a1d-4c0a2e5a79c7" />

<img width="1271" height="587" alt="Screen Shot 2026-03-12 at 18 59 43" src="https://github.com/user-attachments/assets/f47bf674-5df6-4241-9609-fcc33b617871" />

<img width="1249" height="680" alt="Screen Shot 2026-03-12 at 19 01 05" src="https://github.com/user-attachments/assets/6c3e6fdd-c006-471a-b02f-d6972117d4d0" />


## 📂 Directory Structure
```text
ansible-config-mgt/
├── inventory/
│   ├── dev          # Development environment hosts
│   ├── staging      # Staging environment hosts
│   ├── uat          # User Acceptance Testing hosts
│   └── prod         # Production hosts
├── playbooks/
│   └── common.yml   # Main playbook for infrastructure tasks
└── README.md        # Project documentation

