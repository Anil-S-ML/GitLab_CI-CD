#  only: and except:

`only:` and `except:` are rules used to control **WHEN a job should run**.

They allow you to run specific jobs:

- only on certain branches  
- only on merge requests  
- except on certain branches  
- except on tags, schedules, pipelines, etc.

---

## ✔ `only:` — Run the job ONLY when these conditions match

### Example: Run job only on `main` branch

```yaml
deploy_job:
  stage: deploy
  only:
    - main
  script:
    - echo "Deploying only on main branch"
