# 🚀 Ansible Configuration Management  
## Structural Refactoring & Dynamic Assignments

This project demonstrates the transformation of a basic Ansible setup into a modular, scalable, and environment-aware Infrastructure-as-Code (IaC) solution.

---

## 🏗️ Project Evolution

### 1️⃣ Branching Strategy & Environment Setup
- Feature branches used: dynamic-assignments, roles-feature
- Changes tested before merging into main

---

### 2️⃣ Dynamic Variable Management

```yaml
include_vars: "{{ item }}"
with_first_found:
  - files: [dev.yml, stage.yml, uat.yml, prod.yml]
    paths: ["../env-vars"]
```
<img width="960" height="342" alt="Screen Shot 2026-04-12 at 12 57 56" src="https://github.com/user-attachments/assets/2b664760-197d-4d9b-a02d-2fdc6453f939" />


### 3️⃣ Environment-Specific Configuration

```
env-vars/
├── dev.yml
├── stage.yml
├── uat.yml
└── prod.yml
```
<img width="1209" height="240" alt="Screen Shot 2026-04-12 at 12 59 04" src="https://github.com/user-attachments/assets/2f872ffe-eda7-471c-8d9d-42f96c501592" />


### 4️⃣ Modularization with Ansible Roles

```
roles/
├── mysql
├── nginx
└── apache
```

---

### 5️⃣ Conditional Load Balancer Deployment

```yaml
- { role: nginx, when: enable_nginx_lb and load_balancer_is_required }
- { role: apache, when: enable_apache_lb and load_balancer_is_required }
```

---

### 6️⃣ Central Orchestration

```
playbooks/site.yml
```

---

## 📂 Final Structure

```
.
├── dynamic-assignments/
├── env-vars/
├── inventory/
├── playbooks/
├── roles/
└── static-assignments/
```
<img width="627" height="88" alt="Screen Shot 2026-04-12 at 18 48 24" src="https://github.com/user-attachments/assets/b4d06fba-c136-4981-9384-9378235cddc6" />


## 🚀 Usage

```bash
ansible-playbook -i inventory/uat playbooks/site.yml
```

---

## 🌟 Key Takeaways

- Dynamic configuration
- Role-based architecture
- Environment-aware deployments
