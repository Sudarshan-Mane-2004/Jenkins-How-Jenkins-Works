# How Jenkins Works

## Introduction

Jenkins is an **automation server** used to automate the **software development process** such as building, testing, and deploying applications.

It is mainly used in **DevOps CI/CD pipelines** to make software delivery faster and more reliable.

---

# Basic Working of Jenkins

Jenkins works by automatically running tasks whenever code changes occur in a repository.

The general working flow of Jenkins is:

1. Developer writes code.
2. Code is pushed to a repository.
3. Jenkins detects the change.
4. Jenkins pulls the latest code.
5. Jenkins builds the application.
6. Jenkins runs automated tests.
7. Jenkins deploys the application.

---

# Jenkins Working Flow

```text
Developer
   │
   ▼
Git Repository (GitHub / GitLab)
   │
   ▼
Webhook Trigger
   │
   ▼
Jenkins Server
   │
   ▼
Build Application
   │
   ▼
Run Tests
   │
   ▼
Package Application
   │
   ▼
Deploy Application
```

---

# Step-by-Step Jenkins Workflow

## Step 1: Developer Pushes Code

A developer writes code and pushes it to a repository such as:

* GitHub
* GitLab
* Bitbucket

Example:

```bash
git push origin main
```

---

## Step 2: Webhook Triggers Jenkins

The repository sends a **webhook notification** to Jenkins when new code is pushed.

This automatically starts the Jenkins pipeline.

---

## Step 3: Jenkins Pulls the Code

Jenkins downloads the latest source code from the repository into its **workspace**.

---

## Step 4: Build Stage

Jenkins compiles the source code using build tools like:

* Maven
* Gradle
* npm

Example:

```bash
mvn clean install
```

This generates build artifacts such as:

* `.jar`
* `.war`

---

## Step 5: Test Stage

Jenkins runs automated tests to verify the application.

Example tools:

* JUnit
* Selenium
* TestNG

If tests fail, Jenkins stops the pipeline.

---

## Step 6: Package Stage

The application is packaged into a deployable format.

Example:

* JAR file
* WAR file
* Docker image

---

## Step 7: Deployment Stage

Finally Jenkins deploys the application to servers such as:

* AWS EC2
* Docker containers
* Kubernetes clusters
* Application servers (Tomcat)

---

# Jenkins Architecture

Jenkins mainly works with two components.

## Jenkins Controller (Master)

The controller manages:

* Jenkins jobs
* Pipeline execution
* Plugin management
* Agent communication

---

## Jenkins Agent

Agents are worker machines that execute build tasks.

They help distribute workload and run builds on different environments.

---

# Advantages of Jenkins Working Model

* Automates repetitive tasks
* Detects errors early
* Speeds up development
* Supports continuous integration and deployment

---

# Short Interview Answer

**How does Jenkins work?**

Jenkins works by automatically building, testing, and deploying applications when developers push code to a repository. Jenkins detects code changes using webhooks, pulls the latest code, runs build and test stages, and finally deploys the application to servers.

---

# Conclusion

Jenkins automates the entire software development lifecycle by integrating with source code repositories, build tools, testing frameworks, and deployment platforms. This helps teams deliver software faster and more reliably.

# Give Sudo Permission to Jenkins User (Ubuntu)

## Introduction

When Jenkins runs on an Ubuntu server, it runs under a system user called **jenkins**.
Sometimes Jenkins needs **sudo privileges** to execute commands like:

* Installing packages
* Running Docker commands
* Deploying applications
* Restarting services

In such cases we give **sudo permissions to the Jenkins user**.

---

# Step 1: Check Jenkins User

First verify that the Jenkins user exists.

```bash
id jenkins
```

Example output:

```text
uid=114(jenkins) gid=118(jenkins) groups=118(jenkins)
```

---

# Step 2: Open Sudoers File

To give sudo permission, edit the **sudoers file**.

Run:

```bash
sudo visudo
```

This safely opens the sudo configuration file.

---

# Step 3: Add Jenkins User to Sudoers

Scroll to the bottom of the file and add:

```bash
jenkins ALL=(ALL) NOPASSWD: ALL
```

### Meaning of this line

| Part     | Description          |
| -------- | -------------------- |
| jenkins  | Jenkins system user  |
| ALL      | All hosts            |
| (ALL)    | All users            |
| NOPASSWD | No password required |
| ALL      | Allow all commands   |

Now Jenkins can run **any command with sudo without password**.

---

# Step 4: Save and Exit

If using `visudo`:

* Press **CTRL + X**
* Press **Y**
* Press **Enter**

---

# Step 5: Restart Jenkins Service

After updating permissions restart Jenkins.

```bash
sudo systemctl restart jenkins
```

---

# Step 6: Verify Sudo Permission

Switch to Jenkins user:

```bash
sudo su - jenkins
```

Run a sudo command:

```bash
sudo ls /root
```

If it works without asking for password, sudo permission is configured successfully.

---

# Alternative Method (Add Jenkins to Sudo Group)

Another way is to add Jenkins user to the **sudo group**.

```bash
sudo usermod -aG sudo jenkins
```

Restart Jenkins:

```bash
sudo systemctl restart jenkins
```

---

# Example Use Case in Jenkins Pipeline

Now Jenkins can run commands like:

```bash
sudo docker build -t myapp .
```

or

```bash
sudo systemctl restart nginx
```

---

# Short Interview Answer

**How do you give sudo permission to Jenkins user?**

To give sudo permission to Jenkins user, edit the sudoers file using `visudo` and add the line `jenkins ALL=(ALL) NOPASSWD: ALL`. This allows Jenkins to execute sudo commands without requiring a password.

---

# Conclusion

Giving sudo permission to the Jenkins user allows Jenkins pipelines to run administrative commands needed for building, deploying, and managing applications on the server.
