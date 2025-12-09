# GitLab Architecture

## 1. GitLab Server

Also called:
- GitLab Server
- GitLab Instance
- GitLab Installation

This is the main system where:
- Repositories are hosted
- Issues, merge requests, CI/CD pipelines are managed
- Jobs are assigned to runners

**Important:** GitLab server does **NOT** execute CI/CD jobs itself — it only coordinates.

---

## 2. GitLab Runner

A separate program that you install on a machine (VM, bare metal, Kubernetes, Docker).

**Responsibilities:**
- Running CI/CD jobs
- Pulling job configuration from `.gitlab-ci.yml`
- Executing the defined scripts

**How it works:**
- GitLab Server assigns jobs to available runners
- Runners pull the job configuration and execute them

---

## Purpose

| Component | Role |
|-----------|------|
| **GitLab Server** | Orchestration |
| **GitLab Runner** | Execution |

---

## GitLab.com SaaS Architecture

GitLab hosts everything for you:

```
GitLab.com (SaaS)
     |
     |---> Job 1 ----> Runner (GitLab-provided shared runner)
     |---> Job 2 ----> Runner (GitLab-provided shared runner)
```

**Key Points:**
- When using `gitlab.com`, GitLab provides runners
- You can also register your own self-managed runner

---

## Shared Runner

### What is a shared runner?

A runner that is available to **ALL projects** within a GitLab instance.

On GitLab.com, shared runners are available to all users.

### Key Properties

- Offered automatically on `gitlab.com` (SaaS)
- Useful for teams who don't want to manage their own runners
- Multiple jobs from multiple projects can use them
- Good for simple workloads or general use

---

## Architecture Diagram

```
            ┌────────────────────────┐
            │      GitLab Server     │
            │ (Instance/Installation)│
            └──────────┬─────────────┘
                       │
          Assigns CI/CD Jobs
                       │
        ┌──────────────┴──────────────┐
        │                             │
┌───────────────┐              ┌─────────────────┐
│ Shared Runner │              │ Self-Managed     │
│ (GitLab.com or│              │ GitLab Runner    │
│ Instance-wide)│              │ Installed by user│
└───────────────┘              └─────────────────┘
```

---

## Key 

| Component | Description |
|-----------|-------------|
| **GitLab Server** | Orchestrator - manages and coordinates |
| **GitLab Runner** | Executor - runs the actual jobs |
| **Shared Runner** | Available to all projects |
| **Self-managed Runner** | Installed on your own machine |
| **GitLab.com SaaS** | GitLab provides runners for you |

---

![alt text](image.png)