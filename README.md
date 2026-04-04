# 🚀 Catalogue CI Pipeline (Jenkins)

## 📌 Overview

This repository contains the **CI pipeline definition** for the `catalogue` microservice using Jenkins.

The pipeline leverages a **Jenkins Shared Library** to execute standardized CI workflows.

---

## 🧠 Architecture

```
Jenkinsfile (Caller)
        ↓
Jenkins Shared Library
        ↓
nodeJSEKSPipeline (CI Execution)
```

---

## 📂 Repository Structure

```id="ci1"
.
├── Jenkinsfile
├── package.json
├── Dockerfile
├── server.js
└── sonar-project.properties
```

---

## ⚙️ Pipeline Trigger Logic

```groovy id="ci2"
if (!env.BRANCH_NAME.equalsIgnoreCase('main')) {
    nodeJSEKSPipeline(configMap)
} else {
    echo "Please follow the CR process"
}
```

### 🔹 Behavior

* Feature branches → CI pipeline runs
* Main branch → blocked (CR process required)

---

## 🧾 Configuration

```groovy id="ci3"
def configMap = [
    project: "roboshop",
    component: "catalogue"
]
```

---

## 🔄 CI Pipeline Stages

Executed via shared library:

### ✅ Build

* Initializes pipeline

### ✅ Read Version

* Reads version from `package.json`

### ✅ Install Dependencies

```bash
npm install
```

### ✅ Unit Testing

```bash
npm test
```

### ✅ Docker Build & Push

* Builds image
* Pushes to AWS ECR

### ✅ Trigger Deployment

* Calls downstream job:

```
catalogue-deploy
```

---

## 🔐 Optional Integrations

* SonarQube (code quality)
* Trivy (image scan)
* Dependabot (dependency security)

---

## 🚀 Workflow

1. Developer pushes code
2. CI pipeline triggered (non-main branches)
3. Build + test + Docker image
4. Image pushed to ECR
5. Deployment pipeline triggered

---

## ⚡ Benefits

* Standardized CI using shared library
* No duplicate pipeline logic
* Secure and scalable

---

## 👨‍💻 Author

Pavan – DevOps Engineer
