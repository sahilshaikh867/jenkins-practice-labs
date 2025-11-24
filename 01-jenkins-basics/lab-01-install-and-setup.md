# Lab 01 – Jenkins Install & First-Time Setup (Ultra Detailed)

---


## 1. Objective

In this lab, you will:

- Install Jenkins on a Linux server (Ubuntu-based).
- Start and enable the Jenkins service.
- Access the Jenkins web interface from a browser.
- Complete the initial setup:
  - Unlock Jenkins
  - Install plugins
  - Create the first admin user
  - Confirm Jenkins is ready for use

---

## 2. Lab Environment

### 2.1 Recommended Setup

You can use any of the following:

- **Cloud VM**: AWS EC2, GCP VM, Azure VM  
- **Local VM**: VirtualBox / VMware with Ubuntu  
- **Bare Metal**: Physical machine with Ubuntu

### 2.2 Supported OS (for this lab)

Prefer one of:

- Ubuntu 20.04 LTS
- Ubuntu 22.04 LTS
- Ubuntu 24.04 LTS

> Note: Commands here are tested with Ubuntu-based systems using `apt`.

---

## 3. Prerequisites

Before starting, ensure you have:

- `sudo` access to the server.
- Stable internet connection (for downloading packages).
- Ability to SSH into the server.
- A web browser (Chrome/Firefox/Edge) on your local machine.

If using **AWS EC2**:
- Security Group rule allowing inbound traffic on **port 8080** (for Jenkins UI).
- SSH key pair to connect to the instance.

---

## 4. Step-by-Step Installation

### 4.1 Connect to the Server

If using a remote server (like AWS EC2), connect via SSH:

```bash
ssh -i /path/to/your-key.pem ubuntu@your-server-ip
````

Replace:

* `/path/to/your-key.pem` → Your SSH key path
* `your-server-ip` → Public IP of your server

Check you are logged in:

```bash
whoami
hostname
```

You should see something like:

* `whoami` → `ubuntu` (or your username)
* `hostname` → some host name like `ip-172-31-7-105`

---

### 4.2 Update System Packages

Always update your package list before installing anything:

```bash
sudo apt update
```

Optionally, upgrade existing packages:

```bash
sudo apt upgrade -y
```

> Why?
>
> * Ensures you have the latest package metadata.
> * Reduces conflicts during installation.

---

### 4.3 Install Java (Jenkins Dependency)

Jenkins requires Java. We will install **OpenJDK 17**.

Run:

```bash
sudo apt install openjdk-17-jdk -y
```

Verify Java installation:

```bash
java -version
```

Expected output (example):

```text
openjdk version "17.x.x" ...
```

If `java -version` fails or shows “command not found”, Java is not installed correctly. Fix this before continuing.

---

## 5. Add Jenkins Repository (Official Source)

Jenkins is not available in the default Ubuntu repositories in a usable form for production.
We will add the **official Jenkins repository**.

### 5.1 Add Jenkins GPG Key

Run:

```bash
curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key | sudo tee \
  /usr/share/keyrings/jenkins-keyring.asc > /dev/null
```

What this does:

* Downloads the Jenkins signing key.
* Stores it in `/usr/share/keyrings/jenkins-keyring.asc`.
* This key is used by `apt` to verify Jenkins packages.

You can verify the file exists:

```bash
ls -l /usr/share/keyrings/jenkins-keyring.asc
```

---

### 5.2 Add Jenkins Repository to APT Sources

Run:

```bash
echo "deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian-stable binary/" | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
```

What this does:

* Adds a new file: `/etc/apt/sources.list.d/jenkins.list`
* Tells `apt` that Jenkins packages are available from `https://pkg.jenkins.io/debian-stable`.

Verify the file:

```bash
cat /etc/apt/sources.list.d/jenkins.list
```

You should see a line starting with:

```text
deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] https://pkg.jenkins.io/debian-stable binary/
```

---

### 5.3 Update Package List Again

Because we added a new repository, run:

```bash
sudo apt update
```

Look for a line that mentions `pkg.jenkins.io` in the output.
If you see errors about the Jenkins repo, fix them before proceeding.

---

## 6. Install Jenkins

Now install Jenkins package:

```bash
sudo apt install jenkins -y
```

This will:

* Install Jenkins binaries
* Create `jenkins` user
* Create a systemd service `jenkins.service`
* Configure default data directory at `/var/lib/jenkins`

Verify installation:

```bash
jenkins --version || systemctl status jenkins
```

> Note: `jenkins --version` may or may not be available depending on path.
> `systemctl status jenkins` is more reliable to check the service state.

---

## 7. Manage Jenkins Service

### 7.1 Enable Jenkins at Boot

Ensure Jenkins starts automatically on system reboot:

```bash
sudo systemctl enable jenkins
```

### 7.2 Start Jenkins Service

Start Jenkins:

```bash
sudo systemctl start jenkins
```

### 7.3 Check Jenkins Service Status

Check if Jenkins is running:

```bash
sudo systemctl status jenkins
```

You should see something like:

* `Active: active (running)`

If it shows `failed` or `inactive`, note the error and check the log:

```bash
journalctl -u jenkins -xe
```

---

## 8. Networking & Port Configuration (Port 8080)

Jenkins by default listens on **port 8080**.

### 8.1 Check if Jenkins is Listening on 8080

Run:

```bash
sudo ss -tulnp | grep 8080
```

Expected output:

* A line showing `java` process bound to `*:8080` or `0.0.0.0:8080`

Example:

```text
LISTEN 0 50 *:8080 *:* users:(("java",pid=xxxx,fd=xxx))
```

---

### 8.2 Firewall Configuration (If UFW is used)

If `ufw` firewall is enabled, allow port 8080:

```bash
sudo ufw allow 8080/tcp
sudo ufw status
```

Look for a line:

```text
8080/tcp ALLOW Anywhere
```

If `ufw` is not active, you may see:

```text
Status: inactive
```

Then you do not need to change anything on UFW.

---

### 8.3 Cloud Provider Security Group (e.g., AWS EC2)

If you are on AWS EC2:

1. Go to **EC2 Console → Instances**.
2. Select your instance.
3. Scroll to **Security** section.
4. Click on the **Security Group** linked to your instance.
5. Go to **Inbound rules**.
6. Add rule:

   * Type: `Custom TCP`
   * Port: `8080`
   * Source: `0.0.0.0/0` (for testing) or your IP (for security).

Save the rule.

---

## 9. Access Jenkins Web Interface

Now, from your **local machine browser**, open:

```text
http://<your-server-ip>:8080
```

Examples:

* `http://54.123.45.67:8080`
* `http://ec2-54-123-45-67.ap-south-1.compute.amazonaws.com:8080`

You should see the **"Unlock Jenkins"** screen.

If you cannot access:

* Re-check security group rules.
* Verify Jenkins is running (`systemctl status jenkins`).
* Check firewall settings.

---

## 10. Unlock Jenkins (Initial Admin Password)

On the "Unlock Jenkins" page, it asks for an **Administrator password**.

On the server, run:

```bash
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

This outputs a long random string like:

```text
a1b2c3d4e5f6g7h8i9j0...
```

Copy the entire string **exactly**.

In the browser:

1. Paste the password in the **"Administrator password"** field.
2. Click **"Continue"** or **"Unlock"**.

If the password is rejected:

* Ensure you did not copy extra spaces.
* Retrieve the password again from the file.
* Check Jenkins logs for errors.

---

## 11. Plugin Installation

After unlocking, you will see a screen:

> "Customize Jenkins"

With two options:

* **Install suggested plugins**
* **Select plugins to install**

For this lab, choose:

✅ **Install suggested plugins**

What happens:

* Jenkins will download and install a set of commonly used plugins:

  * Git
  * Pipeline
  * Matrix Authorization
  * Credentials
  * And more…

You will see a progress screen:

* Some plugins may show “Failed” if network issues occur.
* You can retry later from **Manage Jenkins → Manage Plugins**.

Wait until the installation completes.

---

## 12. Create First Admin User

After plugins are installed, Jenkins will ask to:

> "Create First Admin User"

Fill the form:

* **Username**: Example: `admin` or your name
* **Password**: Strong password
* **Full name**: Your full name
* **E-mail address**: Your email (can be dummy for local testing)

Click **"Save and Continue"**.

This creates a non-default admin user instead of using only the initial password.

---

## 13. Instance Configuration (Jenkins URL)

Next screen:

> "Instance Configuration"

It will show a **Jenkins URL**, e.g.:

```text
http://your-server-ip:8080/
```

Verify that:

* The IP/hostname is correct.
* Protocol is `http` (unless you configured HTTPS).

If you are just testing, you can accept the default and click:

✅ **"Save and Finish"**

---

## 14. Jenkins is Ready Screen

You will see:

> "Jenkins is ready!"

Click:

✅ **"Start using Jenkins"**

Now you will be redirected to the **Jenkins Dashboard**.

---

## 15. Verify Dashboard & Basic Health

On the Jenkins Dashboard, verify:

* Top-left: Jenkins logo + version
* Center: "Welcome to Jenkins!" if no jobs exist
* Left side menu:

  * **New Item**
  * **People**
  * **Build History**
  * **Manage Jenkins**
  * **My Views**, etc.

Click on:

* **Manage Jenkins**
* Scroll to see important sections:

  * System Configuration
  * Security
  * Tools
  * Plugins
  * Nodes

This confirms Jenkins is fully functional.

---

## 16. (Optional) Create a Basic Test Job

To confirm Jenkins is working end-to-end:

1. Click **"New Item"** on the left sidebar.

2. Enter an item name: `test-job`.

3. Select **"Freestyle project"**.

4. Click **"OK"**.

5. On the next page, scroll down to **"Build"** section.

6. Click **"Add build step" → "Execute shell"**.

7. In the command box, type:

   ```bash
   echo "Jenkins is working!"
   ```

8. Click **"Save"**.

9. On the job page, click **"Build Now"**.

10. Check the **Build History** (bottom-left) → click on the build number `#1`.

11. Click **"Console Output"**.

You should see:

```text
Jenkins is working!
Finished: SUCCESS
```

This confirms that:

* Jenkins can execute builds.
* The workspace and shell execution are functioning.

---

## 17. Summary of What You Achieved

By the end of this lab, you have:

* Installed Java and Jenkins.
* Added the official Jenkins repository.
* Installed and started the Jenkins service.
* Configured port access and networking.
* Unlocked Jenkins using the initial admin password.
* Installed suggested plugins.
* Created the first admin account.
* Verified Jenkins dashboard access.
* (Optionally) Created and ran your first test job.

You now have a **fully working Jenkins instance**, ready for:

* Freestyle jobs
* Pipelines
* Git integration
* CI/CD workflows
