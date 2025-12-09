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

---

## How to Add Variables in GitLab (Settings → CI/CD)

### Step 1: Go to CI/CD Settings

1. Open your GitLab project
2. Go to **Settings → CI/CD**
3. Expand **Variables**

### Step 2: Add Normal Variables (type = "Variable")

Add the following variables:

| Key | Value | Type |
|-----|-------|------|
| `MICROSERVICE_NAME` | `SHOPPING CART` | Variable |
| `DEPLOYMENT_ENVIRONMENT` | `test2-myapp.com` | Variable |

### Step 3: Create a FILE-TYPE Variable

You will see two types under "Type":

- **Variable** → normal key-value text
- **File** → stores the value as a file your pipeline can read

Create a file-type variable:

| Key | Type | Content (file value) |
|-----|------|---------------------|
| `PROPERTIES_FILE` | File | `environment=test2-myapp.com`<br>`microservicename=shopping cart` |

### Step 4: Use Variables in Your Pipeline

**Using normal variables:**

```yaml
test_job:
  stage: test
  script:
    - echo "Microservice: $MICROSERVICE_NAME"
    - echo "Environment: $DEPLOYMENT_ENVIRONMENT"
```

**Using file-type variable:**

```yaml
read_properties:
  stage: test
  script:
    - cat $PROPERTIES_FILE
    - echo "Reading properties from file..."
```

**Output:**
```
environment=test2-myapp.com
microservicename=shopping cart
Reading properties from file...
```

---

## Complete Example with GitLab UI Variables

```yaml
workflow:
  rules:
    - if: $CI_COMMIT_BRANCH != "main" && $CI_PIPELINE_SOURCE != "merge_request_event"
      when: never
    - when: always

stages:
  - test
  - build
  - deploy

print_variables:
  stage: test
  script:
    - echo "Microservice Name: $MICROSERVICE_NAME"
    - echo "Deployment Environment: $DEPLOYMENT_ENVIRONMENT"
    - echo "Properties file content:"
    - cat $PROPERTIES_FILE

build_with_vars:
  stage: build
  only:
    - main
  script:
    - echo "Building $MICROSERVICE_NAME for $DEPLOYMENT_ENVIRONMENT"

deploy_with_properties:
  stage: deploy
  only:
    - main
  script:
    - echo "Deploying using properties:"
    - cat $PROPERTIES_FILE
    - echo "Deployment complete!"
```

---

