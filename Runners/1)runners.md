## Runners 

## 1) Scope & Types of Runners

### Shared Runners
- **Available to:** All projects/groups in the GitLab instance
- **Useful when:** General-purpose builds where isolation and exact environment are not strict requirements

### Group Runners
- **Available to:** All projects inside that group
- **Useful when:** Many projects in a group share similar build needs

### Specific (Project) Runners
- **Registered and attached to:** A single project
- **Typically:** Self-managed, dedicated to one repo (e.g., `payment-svc` → `runner-111`)
- **Use when:** You need custom executors, special tools, privileged access, or resource isolation

### Self-managed vs GitLab.com Shared Runners
- **Self-managed:** You host runners (hosted on your infra/VM/k8s)

![Runner Types](./images/Screenshot%202025-12-09%20142723.png)
---

## 2) How Runners Execute a Job — Execution Flow (5 Steps)

###  Step 1: Runner polls GitLab for new jobs
Runner requests job payload from GitLab server.

###  Step 2: Runner forwards job payload to executor
Runner sends job configuration to the chosen executor (docker, shell, docker-machine, etc.).

###  Step 3: Executor clones and executes
Executor clones source/downloads artifacts from GitLab and runs the job scripts.

###  Step 4: Executor returns job output
Executor sends logs, artifacts, and status back to the Runner.

###  Step 5: Runner updates GitLab
Runner updates job status in GitLab (visible in the pipeline/job UI).

---

## 3) Setting a Default Image for Jobs (`image:`)

You can set a global `image:` at the top of `.gitlab-ci.yml`:

![Runner Types](./images/Screenshot%202025-12-09%20142723.png)

- in the above example we are using node:17-alpine as the default image for all jobs

```yaml
image: node:17-alpine

stages:
  - test
  - build

test_job:
  stage: test
  script:
    - npm test
```

**Key Points:**
- This image becomes the default container image for all jobs
- Individual jobs can override with their own `image:`
- Docker executors will pull that image and run job scripts inside it
- Ensure the image contains required tools for your jobs

---
