## Demo Overview

In this demo, we will:

![runner](./images/Screenshot%202025-12-09%20151218.png)

- Set up 2 specific self-managed GitLab Runners
- One running on a local machine
- One running on an AWS EC2 instance
- Execute CI/CD jobs on these runners
- Jobs in `.gitlab-ci.yml` will be configured to run specifically on these runners instead of shared runners

---

## GitLab.com as the CI/CD Orchestrator

**GitLab.com serves as the central coordinator:**

- Receives code pushes and triggers pipelines
- Assigns jobs to registered runners
- **Does NOT execute jobs itself**
- Sends job definitions to registered runners
- Collects results and updates pipeline status

---

## Self-Managed Runners in the Demo

### A. GitLab Runner on Local Machine

**Installation:**
- Installed manually using the GitLab Runner installer
- Registered to the GitLab project using a registration token

### B. GitLab Runner on AWS EC2 Instance

**Installation:**
- Installed on an EC2 virtual machine (Linux)
- Registered as a specific runner for the project

**Best for:**
- Larger compute power
- Scalable jobs
- Cloud-based workloads
- Access to cloud resources (S3, RDS, VPC, etc.)

**Important:** Both runners are **specific runners**, meaning they are dedicated to one project and not shared with others.

---

## How the Demo Pipeline Works

### Step 1: Code Push
Developer pushes code to GitLab repository

### Step 2: Pipeline Trigger
GitLab triggers the CI/CD pipeline defined in `.gitlab-ci.yml`

### Step 3: Runner Selection
GitLab selects the correct runner based on:
- **Tags** (e.g., `tags: [local]` or `tags: [aws]`)
- **Runner scope** (specific)
- **Job rules** or `only:` conditions

### Step 4: Job Execution
```yaml
# Example job distribution
test_local:
  tags:
    - local
  script:
    - echo "Running on local machine"

deploy_aws:
  tags:
    - aws
  script:
    - echo "Running on AWS EC2"
```

### Step 5: Results Collection
- Logs and artifacts return to GitLab.com
- GitLab updates job and pipeline status (passed/failed)
- Results visible in GitLab UI

---

## Demo Architecture Diagram

```
┌─────────────────────────┐
│      GitLab.com         │
│   (CI/CD Orchestrator)  │
│                         │
│ • Receives code pushes  │
│ • Triggers pipelines    │
│ • Assigns jobs          │
│ • Collects results      │
└──────────┬──────────────┘
           │
    Assigns Jobs
           │
    ┌──────┴──────┐
    │             │
┌───▼────────┐ ┌──▼──────────┐
│Local Runner│ │AWS EC2 Runner│
│            │ │             │
│• Fast tests│ │• Heavy builds│
│• Debugging │ │• Cloud access│
│• Lightweight│ │• Scalability│
└────────────┘ └─────────────┘
```

---

## Demo Setup Steps

### 1. Prepare GitLab Project
- Create or use existing GitLab project
- Get registration token from **Settings → CI/CD → Runners**

### 2. Setup Local Runner
```bash
# Install GitLab Runner on local machine
curl -L "https://packages.gitlab.com/install/repositories/runner/gitlab-runner/script.deb.sh" | sudo bash
sudo apt-get install gitlab-runner

# Register runner with 'local' tag
sudo gitlab-runner register
```

### 3. Setup AWS EC2 Runner
```bash
# SSH into EC2 instance
# Install GitLab Runner
curl -L "https://packages.gitlab.com/install/repositories/runner/gitlab-runner/script.deb.sh" | sudo bash
sudo apt-get install gitlab-runner

# Register runner with 'aws' tag
sudo gitlab-runner register
```

### 4. Configure `.gitlab-ci.yml`
```yaml
stages:
  - test
  - build
  - deploy

test_local:
  stage: test
  tags:
    - local
  script:
    - echo "Running tests on local machine"
    - hostname
    - pwd

build_aws:
  stage: build
  tags:
    - aws
  script:
    - echo "Building on AWS EC2"
    - hostname
    - df -h

deploy_aws:
  stage: deploy
  tags:
    - aws
  only:
    - main
  script:
    - echo "Deploying from AWS EC2"
    - aws --version
```

### 5. Push and Monitor
- Push code to trigger pipeline
- Monitor job execution in GitLab UI
- Verify jobs run on correct runners

---

## Key Takeaways 

### Flexibility
- GitLab runners can be installed **anywhere**: laptop, cloud, on-prem, etc.
- Multiple runners can be attached to the same GitLab project

### Job Distribution
- Pipelines can distribute jobs across different environments
- Use **tags** to control which runner executes which job

### Full Control
Self-managed runners provide complete control over:
- **Environment** configuration
- **Installed tools** and dependencies
- **Security** settings and access
- **Scaling** and resource allocation

### When to Use Self-Managed Runners
- Shared runners are not sufficient for your needs
- You need custom configurations or special tools
- You require access to private networks or resources
- You want dedicated compute resources
- You need specific security or compliance requirements

---

## Benefits

| Aspect | Local Runner | AWS EC2 Runner |
|--------|-------------|----------------|
| **Speed** | Fast for small jobs | Scalable for large jobs |
| **Cost** | Free (your machine) | Pay for EC2 usage |
| **Access** | Local resources | Cloud services (S3, RDS) |
| **Use Case** | Development, testing | Production builds, deployment |
