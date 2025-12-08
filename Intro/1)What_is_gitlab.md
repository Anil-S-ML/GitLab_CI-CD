# Introduction to Gitlab CI/CD

## What is CI/CD
**CI/CD (Continuous Integration and Continuous Delivery/Deployment)** is a DevOps practice that automates:

- Building the application  
- Running tests  
- Packaging artifacts  
- Deploying to development/staging/production  
- Ensuring fast feedback and safe releases  

GitLab provides a fully integrated CI/CD platform directly inside your repository.

---


##  What is GitLab CI/CD?

GitLab CI/CD is GitLab’s built-in automation tool that allows you to:

- Build code  
- Run tests  
- Deploy to servers, Docker, Kubernetes  
- Use pipelines to automate workflows  
- Integrate security scans (DevSecOps)

The entire pipeline is controlled by one file:

**`.gitlab-ci.yml`**

### Pipeline  

is a set of stages and jobs that run automatically when code is pushed into the repository 4

### Stages 

```yaml
stages:
  - build
  - test
  - deploy



