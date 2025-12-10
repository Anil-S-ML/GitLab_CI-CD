# Setup Node.js Application for GitLab CI/CD Demo

## Overview

This guide will help you set up the demo Node.js application that we'll use for testing our GitLab CI/CD runners.



---

## Prerequisites

- Node.js installed on your machine
- npm (comes with Node.js)
- Git installed

---

## Step 1: Clone the Repository

![runner](./images/Screenshot%202025-12-10%20065154.png)

```bash
git clone https://gitlab.com/anil9099/mynodeapp-cicd-project.git
```

---

## Step 2: Navigate to Project Directory

```bash
cd mynodeapp-cicd-project
```

---

## Step 3: Install Dependencies

```bash
npm install
```

This command will:
- Read the `package.json` file
- Download and install all required dependencies
- Create a `node_modules` folder with all packages

---

## Step 4: Start the Application

```bash
npm start
```

This will:
- Start the Node.js application
- Usually runs on `http://localhost:3000` (check console output)

![run](./images/Screenshot%202025-12-10%20065400.png)

- Keep the terminal open while the app is running

---

## Step 5: Run Tests

Open a new terminal window/tab and run:

```bash
npm test
```

This command will:

![run](./images/Screenshot%202025-12-10%20065543.png)

- Execute all test files
- Show test results in the console
- Exit with success/failure status

---

## Verify Setup

After completing these steps, you should have:

1. **Application running** - accessible via browser
2. **Tests passing** - all tests should pass
3. **Dependencies installed** - `node_modules` folder created

---
## Next Steps

Once you have the application running locally:

1. Push changes to trigger GitLab CI/CD pipeline
2. Monitor how jobs run on different runners (local vs AWS)
3. Observe the same commands (`npm install`, `npm test`, `npm start`) executing in CI/CD environment

# Unit Tests with Test Reporting in GitLab CI/CD

## Unit Test Job Configuration

![run](./images/Screenshot%202025-12-10%20083133.png)

```yaml
run_unit_tests:
  image: node:17-alpine3.14
  tags:
    - ec2
    - docker
    - remote
  before_script:
    - cd app
    - npm install
  script:
    - npm run test
  artifacts:
    when: always
    reports:
      junit:
        - app/junit.xml
```

## How Test Reports Work in GitLab

### 1. Test Execution Flow

```
1. Runner pulls Docker image (node:17-alpine3.14)
2. Executes before_script (cd app && npm install)
3. Runs test script (npm run test)
4. Test framework generates junit.xml
5. GitLab collects artifacts (junit.xml)
6. GitLab parses JUnit XML and displays results
```

### 2. GitLab Pipeline Editor Integration

![run](./images/Screenshot%202025-12-10%20083250.png)

-----------------------------------------------------

![run](./images/Screenshot%202025-12-10%20083355.png)

------------------------------------------------------

![run](./images/Screenshot%202025-12-10%20083414.png)

When you use the **GitLab Pipeline Editor**:

1. **Visual Test Results** - Test results appear in the pipeline view
2. **Test Summary** - Shows passed/failed/skipped test counts
3. **Detailed Reports** - Click on job to see individual test results
4. **Failure Analysis** - Failed tests show error messages and stack traces

### 3. Test Report Storage

#### Artifact Storage Location

![run](./images/Screenshot%202025-12-10%20083717.png)

--------------------------------------------------------

![run](./images/Screenshot%202025-12-10%20084102.png)

```
GitLab Project → CI/CD →  Artifacts
```

## 2. GitLab Pipeline Editor Integration

When you use the **GitLab Pipeline Editor**:

![run](./images/Screenshot%202025-12-10%20083533.png)

1. **Visual Test Results** - Test results appear in the pipeline view
2. **Test Summary** - Shows passed/failed/skipped test counts
3. **Detailed Reports** - Click on job to see individual test results
4. **Failure Analysis** - Failed tests show error messages and stack traces