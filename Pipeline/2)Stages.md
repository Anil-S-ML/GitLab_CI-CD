### Stages 
- we can group multiple jobs into stages that run in a defined order 
- multiple jobs in the same stage are executed parallel
- only if all the jobs in the stages got succeded, the pipeline moves on to the next stage 
- if any job in a stage fails , the next stage is not executed and the pipeline fails
- logically group that belongs together , only when all jobs got executed next stage will be executed 

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
  script:
    - echo "Logging into Docker registry..."
    - echo "Pushing Docker image..."4

deploy_to_dev:
  stage: deploy
  script:
    - echo "Deploying to dev environment..."

```