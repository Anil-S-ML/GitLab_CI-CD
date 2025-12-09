# GitLab CI/CD – Executors

## Why do we need Executors?

**GitLab Runner** = program that pulls and runs jobs.

But Runner alone cannot execute jobs → it needs an **executor**.

**Executor** = the component that actually runs the job commands

![GitLab Architecture](./images/Screenshot%202025-12-09%20124319.png)

---

## Types of Executors

### 1. Shell Executor

Commands run directly on the operating system (host machine)

**Characteristics:**
- Simplest executor → uses the shell of the server
- No isolation between jobs

**Disadvantages:**
- You must manually install all required tools (Docker, Python, Node, Java, etc.)
- Jobs share the same OS → no isolation
- Leads to tool conflicts, version conflicts, and higher maintenance


![Runner Configuration](./images/Screenshot%202025-12-09%20124755.png)

---

### 2. Docker Executor

Commands run inside Docker containers.

Each job runs in a separate isolated container.

**Advantages:**
- All tools come pre-packaged in images
- No need to install software on the server except Docker itself
- Isolation → no version conflicts
- Clean environment for every job

**Note:** When registering a runner, you can choose `docker` as executor.

---

### 3. Virtual Machine (VM) Executor

Runs each job inside a newly created virtual machine.

**Characteristics:**
- Provides strong isolation
- Slower and requires more resources

---

### 4. Kubernetes Executor

Runs CI/CD jobs as Kubernetes pods.

**Best for:**
- Large-scale pipelines
- Cloud-native workloads
- Auto-scaling CI pipelines

---

### 5. Docker Machine Executor

Special version of Docker executor.

**Features:**
- Supports auto-scaling by creating new Docker hosts on demand (cloud + local)
- Automatically:
  - Creates servers
  - Installs Docker
  - Configures Docker client

**Note:** GitLab shared runners use the Docker Machine executor.

---

## Which Executor to Choose?

![Executor Comparison](./images/Screenshot%202025-12-09%20124746.png)

| Executor | Best For |
|----------|----------|
| **Docker Executor** | Most use cases, best balance, simple, isolated |
| **Kubernetes Executor** | Cloud-native workloads, large distributed pipelines |
| **Shell Executor** | Quick tests, very simple setups |
| **Docker Machine Executor** | Auto-scaling runners |
| **VM Executor** | Max isolation, security-sensitive workloads |

**Recommended default:** Docker Executor

**Special use case:** Kubernetes Executor

---

## How to Use Multiple Executors on the Same Server?

**Answer:** YES, you can use multiple executors on the same server.

### How?

Register multiple runners on the same machine, each with a different executor.

**Example:**
- Runner #1 → shell executor
- Runner #2 → docker executor
- Runner #3 → docker-machine executor
- Runner #4 → kubernetes executor

All running on the same host.

### Commands:

```bash
gitlab-runner register
gitlab-runner register
gitlab-runner register
```

Each registration creates:
- A new runner entry in GitLab
- A new configuration block in: `/etc/gitlab-runner/config.toml`

### Each runner can have a different:
- Name
- Tags
- Executor
- Configuration

This is how you use multiple executors on one server.

---

## Summary

| Concept | Description |
|---------|-------------|
| **Runner** | Needs executor to run jobs |
| **Shell executor** | Simplest but not isolated |
| **Docker executor** | Best, isolated, widely used |
| **Kubernetes executor** | For large-scale cloud-native CI |
| **Docker Machine executor** | Auto-scaling |
| **Multiple executors** | Register multiple runners on same server |

---

