### Needs in gitlab

- `needs` keyword is used to define dependencies between jobs in a pipeline.
- By default, GitLab runs jobs stage by stage build → test → deploy
- But sometimes you want jobs to
- Run faster

- Run in parallel

- Run even if they are in different stages

- This is where needs: is used.

- It tells GitLab which job must finish before this job starts
- It lets jobs run out of stage order
- It makes pipelines much faster

```
stages:
  - test
  - build
  - deploy

run_unit_test:
  stage: test
  before_script:
    - echo "Running before_script for test job..."
  script:
    - echo "Running unit test script for test job..."
  after_script:
    - echo "Running after_script for test job..."

run_lint_test:
  stage: test
  before_script:
    - echo "Running before_script for test job..."
  script:
    - echo "Running lint script for test job..."
  after_script:
    - echo "Running after_script for test job..."


build_image:
  stage: build
  script:
    - echo "Building Docker image..."

push_image:
  stage: build
  needs:
    - build_image
  script:
    - echo "Logging into Docker registry..."
    - echo "Pushing Docker image..."4

deploy_to_dev:
  stage: deploy
  script:
    - echo "Deploying to dev environment..."

```