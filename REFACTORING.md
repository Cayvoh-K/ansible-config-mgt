#  Ansible Configuration Management & Jenkins CI/CD Integration

This project demonstrates a full-cycle **Infrastructure as Code (IaC)**
solution using **Ansible** and **Jenkins**.
It showcases how to automate configuration management and integrate it
into a CI/CD pipeline for continuous delivery across multiple
environments (Dev and UAT).

<img width="648" height="610" alt="Screen Shot 2026-04-05 at 13 26 07" src="https://github.com/user-attachments/assets/068dd920-a1e9-40bb-998b-07f47052e473" />




##  Architecture

The project utilizes a **multi-tier Jenkins job system** to ensure that
changes pushed to GitHub are automatically reflected on the management
server.

### Workflow:

-   **Upstream Job (`ansible`)**
    -   Monitors the GitHub repository for changes
    -   Archives the codebase
-   **Downstream Job (`save_artifacts`)**
    -   Triggered automatically after a successful upstream build
    -   Copies and organizes artifacts for deployment



##  Jenkins CI/CD Pipeline Setup

### 1. Plugin Configuration

-   **Copy Artifact Plugin**
    -   Enables the downstream job to securely retrieve files from the
        upstream workspace


### 2. Job Configuration: `save_artifacts`

#### Log Rotation

-   Configured to discard old builds
-   Keeps only the last **2 builds** to optimize storage

#### Build Trigger

-   Set to: Build after other projects are built
-   Monitors: `ansible` job

#### Build Step

-   Copy artifacts from another project:
    -   Project name: `ansible`
    -   Artifacts to copy: `**`
    -   Target directory: /home/ubuntu/ansible-config-artifact

<img width="1276" height="570" alt="Screen Shot 2026-03-22 at 12 35 21" src="https://github.com/user-attachments/assets/705993fc-f865-4c62-8cf7-aaf9a4c089e9" />

<img width="1269" height="636" alt="Screen Shot 2026-04-02 at 22 59 33" src="https://github.com/user-attachments/assets/7423cb30-ed52-41e7-888f-deb4366cca79" />


##  Ansible Refactoring (Modular Design)

The project was refactored from a monolithic structure into a modular
architecture to improve reusability, scalability, and maintainability.



### Directory Structure

. ├── inventory │ ├── dev.yml │ └── uat.yml ├── playbooks │ └── site.yml
├── roles │ └── webserver └── static-assignments ├── common.yml ├──
common-del.yml ├── webserver.yml └── uat-webservers.yml

<img width="1122" height="190" alt="Screen Shot 2026-04-05 at 13 32 24" src="https://github.com/user-attachments/assets/66e57103-0e36-4651-9e9e-430d9f81f03d" />


##  Playbook Orchestration (site.yml)

The `site.yml` file acts as the central orchestrator using
import_playbook.



##  Roles and Assignments

### Webserver Role

-   Contains tasks, handlers, and variables for web server setup

### Assignment Example

-   hosts: uat-webservers roles:
    -   webserver



##  Deployment and Verification

### Push Changes

git add . git commit -m "Refactored Ansible structure and added UAT
roles" git push origin `<branch-name>`{=html}

<img width="1276" height="688" alt="Screen Shot 2026-04-03 at 00 03 10" src="https://github.com/user-attachments/assets/60dead2e-d2c8-4771-a9e9-6677e8865174" />

<img width="1252" height="642" alt="Screen Shot 2026-04-03 at 00 18 31" src="https://github.com/user-attachments/assets/c9c62a5d-ba64-4083-8221-fdd1b4914a16" />


### Execute Playbook

ansible-playbook -i inventory/uat.yml playbooks/site.yml



### Verification

Access: http://`<Web-UAT-Server-Public-IP>`{=html}/index.php



## Key Highlights

-   CI/CD pipeline using Jenkins and Ansible
-   Modular architecture using roles
-   Multi-environment deployment (Dev & UAT)
-   Automated artifact management
