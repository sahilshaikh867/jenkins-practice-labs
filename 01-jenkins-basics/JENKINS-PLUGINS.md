# Jenkins Plugin Stack – Full Professional Guide

This document lists all the recommended Jenkins plugins required to run a complete DevOps workflow, including:

- Git + GitHub
- Docker
- Terraform
- AWS Services
- CI/CD Pipelines
- Security + Credentials
- Agents/Nodes
- Testing + Reporting
- Notifications

The list is categorized, professionally explained, and aligned with real-world enterprise Jenkins setups.

---

# 1. Source Control Plugins (Git + GitHub)

| Plugin | Purpose |
|--------|---------|
| **Git Plugin** | Core plugin required to pull code from Git repositories. |
| **GitHub Plugin** | Integrates Jenkins with GitHub APIs and webhooks. |
| **GitHub Branch Source Plugin** | Enables Multibranch Pipelines and GitHub Organization projects. |
| **SCM API Plugin** | Required by multiple Git-related integrations. |
| **SSH Credentials Plugin** | Stores SSH keys used for Git operations. |

**Why Needed?**  
Every CI/CD pipeline starts with pulling source code — these plugins make GitHub integration seamless and secure.

---

# 2. Pipeline Plugins (Backbone of CI/CD)

| Plugin | Purpose |
|--------|---------|
| **Pipeline Plugin** | Adds Jenkinsfile support — foundation of modern CI/CD. |
| **Pipeline: Declarative** | Declarative pipeline syntax (recommended for enterprise). |
| **Pipeline: Stage View** | Provides visual stages, logs, and pipeline graphs. |
| **Pipeline: GitHub Integration** | Triggers pipelines on GitHub events. |
| **Pipeline: Input Step** | Adds approval and manual input steps inside pipelines. |

**Why Needed?**  
Modern Jenkins = Pipeline-first automation.  
Declarative + scripted pipelines handle build → test → deploy flows.

---

# 3. AWS Plugins (For EC2, S3, ECR, ECS, Lambda)

| Plugin | Purpose |
|--------|---------|
| **AWS Steps Plugin** | Upload to S3, run Lambda, manage EC2 — AWS native steps. |
| **Amazon EC2 Plugin** | Use EC2 instances as Jenkins agents (autoscaling). |
| **AWS Credentials Plugin** | Securely stores IAM access keys. |
| **CloudBees AWS Credential Provider** | Supports IAM Role Assumption. |
| **Amazon ECR Plugin** | Push/pull Docker images to Amazon ECR. |
| **Amazon ECS Plugin** | Deploy containers to ECS clusters. |

**Why Needed?**  
These plugins allow Jenkins to fully automate AWS-based deployments — EC2, ECS, S3, and more.

---

# 4. Docker Integration Plugins

| Plugin | Purpose |
|--------|---------|
| **Docker Plugin** | Creates Docker-based build agents. |
| **Docker Pipeline Plugin** | Enables `docker.build()` and `docker.image()` in Jenkinsfile. |
| **Docker API Plugin** | Communicates with Docker Engine. |
| **Docker Commons Plugin** | Shared Docker utility library used by above plugins. |

**Why Needed?**  
For building Docker images, scanning them, and deploying containers.

---

# 5. Terraform Plugins

| Plugin | Purpose |
|--------|---------|
| **Terraform Plugin (HashiCorp)** | Install and run Terraform directly through Jenkins. |
| **Credentials Binding Plugin** | Inject AWS/Terraform variables securely into pipelines. |
| **AnsiColor Plugin** | Provides color-coded Terraform output in Jenkins logs. |

**Why Needed?**  
To build infrastructure pipelines that run `plan`, `apply`, `destroy`, and generate reports.

---

# 6. Security & Credentials Plugins

| Plugin | Purpose |
|--------|---------|
| **Credentials Plugin** | Master plugin for storing secrets securely. |
| **Credentials Binding Plugin** | Inject credentials into environment variables. |
| **Matrix Authorization Strategy** | Role-based access control for Jenkins users. |
| **SSH Credentials Plugin** | Store SSH private keys (for Git/servers). |
| **Audit Trail Plugin** | Tracks who changed what in Jenkins. |

**Why Needed?**  
For safe CI/CD setups where credentials, roles, and audit logs matter.

---

# 7. Agent / Node Management Plugins

| Plugin | Purpose |
|--------|---------|
| **SSH Build Agents Plugin** | Connect remote Linux servers as Jenkins agents. |
| **Amazon EC2 Agent Plugin** | Auto-create EC2-based Jenkins agents. |
| **Kubernetes Plugin** | Spin up Kubernetes pods as on-demand Jenkins agents. |

**Why Needed?**  
Scalability — separate build machines based on language/needs.

---

# 8. Build Tool Plugins

| Plugin | Purpose |
|--------|---------|
| **Maven Integration Plugin** | For Java-based projects. |
| **Gradle Plugin** | For Gradle builds. |
| **NodeJS Plugin** | For Node.js + NPM workflows. |
| **Python Plugin (ShiningPanda)** | For Python CI pipelines. |

**Why Needed?**  
Every project uses different build tools — these plugins integrate them easily.

---

# 9. Testing & Reporting Plugins

| Plugin | Purpose |
|--------|---------|
| **JUnit Plugin** | Publish JUnit/XML test results. |
| **HTML Publisher Plugin** | Publish custom HTML reports (Selenium, Cypress). |
| **JaCoCo Plugin** | Code coverage reports for Java. |

---

# 10. Notifications & Communication Plugins

| Plugin | Purpose |
|--------|---------|
| **Slack Notification Plugin** | Send build notifications to Slack channels. |
| **Email Extension Plugin** | Advanced email alerts on job success/failure. |

**Why Needed?**  
Teams need real-time build/deploy status updates.

---

# 11. Utility / Productivity Plugins

| Plugin | Purpose |
|--------|---------|
| **AnsiColor Plugin** | Colored logs (Terraform/Docker outputs look clean). |
| **Timestamper Plugin** | Adds timestamps to console logs (debugging). |
| **Workspace Cleanup Plugin** | Cleans workspace before/after builds. |
| **Build Timeout Plugin** | Automatically stops jobs taking too long. |

**Why Needed?**  
Stability. Debugging ease. Faster builds.

---

# 🔥 Final Recommended Plugin Set (Copy-Paste)

```

Git Plugin
GitHub Plugin
GitHub Branch Source Plugin
SCM API Plugin
SSH Credentials Plugin
Credentials Plugin
Credentials Binding Plugin

Pipeline Plugin
Pipeline: Declarative
Pipeline: Stage View
Pipeline: GitHub Integration
Pipeline: Input Step

AWS Steps Plugin
AWS Credentials Plugin
CloudBees AWS Credential Provider
Amazon ECR Plugin
Amazon EC2 Plugin
Amazon ECS Plugin

Docker Plugin
Docker Pipeline Plugin
Docker API Plugin
Docker Commons Plugin

Terraform Plugin (HashiCorp)
AnsiColor Plugin

SSH Build Agents Plugin
Kubernetes Plugin (optional)

Maven Plugin
Gradle Plugin
NodeJS Plugin

JUnit Plugin
HTML Publisher Plugin
JaCoCo Plugin

Slack Notification Plugin
Email Extension Plugin

Timestamper Plugin
Workspace Cleanup Plugin
Build Timeout Plugin

---

# 🏁 Conclusion

This complete plugin set gives you:

✔ AWS Deployments  
✔ Docker Builds  
✔ Terraform IaC automation  
✔ GitHub CI/CD  
✔ Secure credentials  
✔ Scalable agents  
✔ Test automation  
✔ Notifications  
✔ Enterprise-grade Jenkins setup  

