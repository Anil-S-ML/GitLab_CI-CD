# GitLab Runner and Executor Workflow

## The 5-Step Process

![Runner Executor Workflow](./images/Screenshot%202025-12-09%20131800.png)

### 🔹 1. Runner requests a job from GitLab

The Runner continuously checks with the GitLab instance to see if there are any new jobs available to execute.

---

### 🔹 2. Runner sends job payload to the Executor

When a job is assigned, the Runner prepares and forwards the job's configuration, scripts, and variables to the Executor.

---

### 🔹 3. Executor clones the repository and executes the job

The Executor fetches the source code or artifacts from GitLab, sets up the environment, and runs the job's script.

---

### 🔹 4. Executor returns the job output

After completing execution, the Executor sends logs, artifacts, and job status back to the Runner.

---

### 🔹 5. Runner reports job status to GitLab

Finally, the Runner updates the job result (success/failure) in the GitLab UI, where users can view the job logs.

---

## Workflow Diagram

```
GitLab Server  ←──────────── GitLab Runner ──────────→ Executor
     │                           │                        │
     │ 1. Job Request            │                        │
     │ ←─────────────────────────│                        │
     │                           │                        │
     │ 2. Job Assignment         │ 2. Send Payload        │
     │ ─────────────────────────→│ ──────────────────────→│
     │                           │                        │
     │                           │                        │ 3. Clone & Execute
     │                           │                        │ ─────────────────
     │                           │                        │
     │                           │ 4. Return Output       │
     │                           │ ←──────────────────────│
     │                           │                        │
     │ 5. Report Status          │                        │
     │ ←─────────────────────────│                        │
```

---

## Key Components

| Component | Role |
|-----------|------|
| **GitLab Server** | Manages pipelines and assigns jobs |
| **GitLab Runner** | Communicates between GitLab and Executor |
| **Executor** | Actually runs the job commands |

---

## Summary

This 5-step workflow ensures that:
- Jobs are efficiently distributed to available runners
- Code is properly fetched and executed in isolated environments  
- Results are accurately reported back to GitLab
- Users can monitor job progress and view logs in real-time

The Runner acts as the **coordinator** while the Executor does the **actual work**.