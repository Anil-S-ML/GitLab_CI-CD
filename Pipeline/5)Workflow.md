## workflow:

`workflow:` controls **when an entire pipeline should run**.

It is used to:
- Allow or block the whole pipeline
- Run pipelines only on certain branches
- Prevent pipelines from running on specific conditions
- Apply global logic before jobs run

---

## Basic Structure

```yaml
workflow:
  rules:
    - if : $CI_COMMIT_BRANCH != "main" && $CI_PIPELINE_SOURCE != "merge_request_event"
      when : never
    - when: always

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
  only:
   - main
  stage: build
  script:
    - echo "Building Docker image..."

push_image:
  only:
   - main
  stage: build
  needs:
    - build_image
  script:
    - echo "Logging into Docker registry..."
    - echo "Pushing Docker image..."

deploy_to_dev:
  only:
   - main
  stage: deploy
  script:
    - echo "Deploying to dev environment..."


```
