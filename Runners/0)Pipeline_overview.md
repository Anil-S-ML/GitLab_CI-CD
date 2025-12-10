# Pipeline Stages Overview

![run](./images/Screenshot%202025-12-10%20061613.png)

## Advanced Pipeline – 7 Points

### Run Unit Tests
Ensures code quality and catches early failures.

### Run SAST (Static Application Security Testing)
Scans code for vulnerabilities and enforces DevSecOps.

### Build Docker Image
Creates a standardized, versioned container image.

### Push Image to Docker Registry
Saves the image for deployment across different environments.

### Deploy to DEV Environment
Development team validates the new build in a real environment.

## Advanced Pipeline – 7 Points

### Run Unit Tests
Ensures code quality and catches early failures.

### Run SAST (Static Application Security Testing)
Scans code for vulnerabilities and enforces DevSecOps.

### Build Docker Image
Creates a standardized, versioned container image.

### Push Image to Docker Registry
Saves the image for deployment across different environments.

### Deploy to DEV Environment
Development team validates the new build in a real environment.


## First we will create the basic pipeline :

![run](./images/Screenshot%202025-12-10%20061633.png)

## Basic Pipeline 

### Run Unit Tests
Validates application logic and ensures no breaking changes.

### Build Docker Image
Packages the app with all dependencies into a portable container.

### Push Image to Docker Registry
Stores the image in a central location (Docker Hub/ECR/GitLab Registry).

### Deploy to DEV Environment
Automatically deploys the image to the development environment for testing.

---