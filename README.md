## 🧱 Part 1: Repo ka Design (sirf structure abhi)

GitHub pe repo ka naam kuch aisa rakh:

> `jenkins-zero-to-hero`
> ya
> `jenkins-practice-labs`

Folder structure (isey hum aage bharte jayenge):

```text
jenkins-zero-to-hero/
├── README.md
├── 01-jenkins-basics/
│   ├── notes.md
|   ├──JENKINS-PLUGINS.md
│   └── lab-01-install-and-setup.md
├── 02-freestyle-jobs/
│   ├── notes.md
│   └── lab-01-hello-world-job.md
├── 03-git-integration/
│   ├── notes.md
│   └── lab-01-build-from-github.md
...
```

Aaj hum sirf is pe kaam karenge:

* `README.md` (basic intro + roadmap skeleton)
* `01-jenkins-basics/notes.md`
* `01-jenkins-basics/lab-01-install-and-setup.md`

---

## 🧾 Step 1: README.md ka starting version

GitHub repo me `README.md` bna aur ye daal:

# Jenkins Zero to Hero (Practice Repo)

Ye repo Jenkins ko bilkul basic se leke advanced tak practice karne ke liye hai.
Saare examples step-by-step aur real commands ke sath honge.

## Roadmap

1. Jenkins Basics
2. Freestyle Jobs
3. Git Integration
4. Build Triggers
5. Jenkins Agents
6. Pipelines (Scripted & Declarative)
7. CI/CD with GitHub & Jenkins
8. Jenkins with Docker / AWS
9. Advanced Topics (Security, Backup, etc.)

Har folder me:
- `notes.md` → theory + concepts
- `lab-xx-...md` → pure hands-on steps
---

## 📚 Step 2: Topic 1 – Jenkins Basics (Theory)

Ab `01-jenkins-basics/notes.md` file bna aur ye content daal:

# 1. Jenkins Basics

## 1.1 What is CI/CD?

- **CI (Continuous Integration)**:
  - Developers apna code frequently (roz ya din me multiple times) ek shared repo me push karte hain.
  - Har push ke baad:
    - Code automatically build hota hai
    - Tests run hote hain
    - Issues jaldi pakad me aate hain

- **CD (Continuous Delivery/Deployment)**:
  - Delivery: Code hamesha deployable state me rehta hai.
  - Deployment: Code automatically staging/production pe deploy ho jata hai (minimal manual steps ke sath).

### CI/CD kyun important hai?

- Early bug detection
- Fast feedback
- Manual kaam kam, automation zyada
- Stable releases

---

## 1.2 What is Jenkins?

- Open-source **automation server**
- Mainly **CI/CD tools** me sabse purana & popular.
- Aap Jenkins se:
  - Code build kar sakte ho
  - Tests run kar sakte ho
  - Artifacts generate kar sakte ho (jar, war, docker image, etc.)
  - Deployments trigger kar sakte ho (AWS, Docker, Kubernetes, etc.)

### Jenkins Key Features

- Web UI se manage sab kuch
- Huge plugin ecosystem
- Git, Maven, Docker, Kubernetes, AWS, etc. support
- Freestyle jobs + Pipelines

---

## 1.3 Jenkins Architecture (High Level)

- **Jenkins Controller (Master)**:
  - Web UI yahi pe hota hai
  - Jobs config yahi stored hoti hai
  - Build ko schedule karta hai

- **Jenkins Agent (Slave/Node)**:
  - Jaha actual build run hote hain
  - Controller in agents ko control karta hai

Simple line me:
> Controller dimag hai, agents haath-pair hain.

---

## 1.4 Important Jenkins Terminology

- **Job / Project** → Jo kaam Jenkins karega (build, test, deploy).
- **Build** → Job ka ek particular run.
- **Workspace** → Directory jaha Jenkins job ka code checkout + build files rakhta hai.
- **Plugin** → Extra feature (Git, Docker, Slack, etc.)
- **Node** → Machine jaha build run hota hai (Controller ya Agent).
- **Pipeline** → Code ke form me CI/CD flow (Jenkinsfile).
- **Stage** → Pipeline ka logical part (Build, Test, Deploy).
- **Step** → Ek ek command jo run hoti hai stage ke andar.

---

## 1.5 Jenkins Web UI Basic Tour (Concept)

Jab tum Jenkins khologe:

- **Dashboard**:
  - Saare jobs ki list
- **Left Sidebar**:
  - New Item (naya job)
  - People
  - Build History
  - Manage Jenkins
- **Manage Jenkins**:
  - Global settings
  - Plugin management
  - Security
  - System config

 **theory section**

---

## 🧪 Step 3: Lab 1 – Install & First Login (Hands-on)

`01-jenkins-basics/lab-01-install-and-setup.md` file bna aur ye daal:


# Lab 01 – Jenkins Install & First Login

## Objective

- Jenkins ko server pe install karna
- Web UI access karna
- Initial admin setup complete karna

## Prerequisites

- Ubuntu server (20.04/22.04/24.04)
- `sudo` access
- Internet working

---

## Step 1: System Update

```bash
sudo apt update
sudo apt upgrade -y
````

---

## Step 2: Java Install (Jenkins Dependency)

```bash
sudo apt install openjdk-17-jdk -y
java -version
```

Expected output: Java 17 wali version line.

---

## Step 3: Jenkins Repository Add karna

```bash
curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key | sudo tee \
  /usr/share/keyrings/jenkins-keyring.asc > /dev/null
```

```bash
echo "deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian-stable binary/" | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
```

Repo add hone ke baad:

```bash
sudo apt update
```

---

## Step 4: Jenkins Install

```bash
sudo apt install jenkins -y
```

---

## Step 5: Jenkins Service Start + Enable

```bash
sudo systemctl enable jenkins
sudo systemctl start jenkins
sudo systemctl status jenkins
```

Status me `active (running)` aana chahiye.

---

## Step 6: Firewall / Security Group (Port 8080)

* Agar AWS EC2 use ho raha hai:

  * Security Group me **Inbound Rule** add karo:

    * Type: Custom TCP
    * Port: `8080`
    * Source: `0.0.0.0/0` (ya restricted IP)

---

## Step 7: Jenkins Web UI Access

Browser me open:

```text
http://<your-server-ip>:8080
```

---

## Step 8: Initial Admin Password

Server pe:

```bash
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

Jo password aaya, wo UI me paste karo.

---

## Step 9: Plugin Installation

* “Install suggested plugins” select karo
* Wait until plugins install ho jaye

---

## Step 10: First Admin User Create

* Username, password, full name, email fill karo
* “Save and Continue”

---

## Step 11: Instance URL Confirm

* Default URL accept kar sakte ho (like `http://<your-ip>:8080/`)
* “Save and Finish”

---

## Step 12: Dashboard par Arrival

* Tumhe **Jenkins Dashboard** dikhna chahiye
* Ye screenshot note ke liye le sakte ho (optional)

**Result:** Jenkins ready for creating first Job ✅

