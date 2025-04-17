
# 🐳 Kubernetes Deployment Notes – Beginner Guide

## 📘 Scenario Overview

We need to set up a Kubernetes environment for a company to run a Redis web service. Tasks include:

- Creating a custom namespace `CyberCo`
- Deploying a Redis container from DockerHub (`redis:buster`)
- Creating a deployment named `cache-db`
- Running 2 replicas (containers)
- Exposing port `6379` for Redis

---

## 📁 Namespace

A **Namespace** in Kubernetes is used to isolate and organize resources.

```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: CyberCo
```

This creates a new logical space named `CyberCo` to deploy our resources in.

---

## 📦 Deployment

A **Deployment** manages a set of Pods and ensures the desired state (e.g., number of replicas) is maintained.

### Key Parts:
- **name**: `cache-db`
- **namespace**: `CyberCo`
- **replicas**: 2
- **image**: `redis:buster` (from DockerHub)
- **port**: 6379 (Redis default)

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: cache-db
  namespace: CyberCo
spec:
  replicas: 2
  selector:
    matchLabels:
      app: redis
  template:
    metadata:
      labels:
        app: redis
    spec:
      containers:
        - name: redis
          image: redis:buster
          ports:
            - containerPort: 6379
```

---

## 🚀 How to Deploy

1. Save the full YAML into:
   ```
   /home/ubuntu/tech-vault-q1/special-definition.yml
   ```

2. Run the deployment with a single command:

   ```bash
   cd /home/ubuntu/tech-vault-q1
   sudo execute
   ```

   > ☑️ `execute` is an alias for something like:  
   > `kubectl apply -f special-definition.yml`

---

## 🔍 Optional: Verify Resources

Check everything was created:

```bash
kubectl get namespaces
kubectl get deployments -n CyberCo
kubectl get pods -n CyberCo
```

---

## ✅ Summary

| Resource       | Purpose                          |
|----------------|----------------------------------|
| Namespace      | Isolates resources (`CyberCo`)   |
| Deployment     | Manages containers (`cache-db`)  |
| Image          | Runs Redis (`redis:buster`)      |
| Replicas       | Ensures 2 pods are running       |
| Port 6379      | Required for Redis communication |

---

