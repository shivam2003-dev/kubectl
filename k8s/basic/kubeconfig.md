

# 📘 Kubernetes Notes: Understanding `kubectl config view` and Output Flags

---

## 🧾 Command: `kubectl config view`

This command displays your **Kubernetes configuration** (`~/.kube/config`), which includes:
- **Clusters** (where Kubernetes is running)
- **Users** (who is accessing it)
- **Contexts** (which user is talking to which cluster)

---

## 🔹 Option: `--minify`

### ✅ Purpose:
> Filters the output to show **only the current context** (cluster, user, namespace you are using).

### 🧠 Why use it?
When you have **many clusters or users** defined, this gives you a **clean, focused output**.

### 🧪 Example:
```bash
kubectl config view --minify
```

---

## 🔹 Option: `-o` (Output Format)

This tells `kubectl` how to **format the output**.

### Common values:

| Option            | Description                              |
|-------------------|------------------------------------------|
| `-o yaml`         | Output as easy-to-read YAML              |
| `-o json`         | Output as JSON                           |
| `-o jsonpath=...` | Extract specific values from JSON output |

---

## 🔹 Option: `-o yaml`

### ✅ Purpose:
> Shows the kubeconfig in a human-friendly format (like a config file).

### 🧪 Example:
```bash
kubectl config view -o yaml
```

---

## 🔹 Option: `-o jsonpath='{.field}'`

### ✅ Purpose:
> Extracts **specific values** from a large JSON object — like a laser pointer! 🎯

### 💡 Why use it?
To **get only the data you need**, such as a cluster name or server URL.

---

## 🔍 Example: Getting the Cluster Name

### ✅ Command:
```bash
kubectl config view --minify -o jsonpath='{.clusters[0].name}'
```

### 🧠 What it does:
- `--minify`: only use current context
- `-o jsonpath=...`: look inside the config for the **first cluster** and show its **name**

### 🧪 Example output:
```
my-cluster
```

---

## 🧩 Visual Reference

```json
{
  "clusters": [
    {
      "name": "my-cluster",
      "cluster": {
        "server": "https://my-cluster-endpoint"
      }
    }
  ]
}
```

🧠 To get the name:  
`-o jsonpath='{.clusters[0].name}'` → `"my-cluster"`

🧠 To get the server URL:  
`-o jsonpath='{.clusters[0].cluster.server}'` → `"https://my-cluster-endpoint"`

---

## ✅ Summary Table

| Option                        | Use Case                                |
|------------------------------|------------------------------------------|
| `--minify`                   | Show only current context info           |
| `-o yaml`                    | View output in human-readable format     |
| `-o json`                    | View raw JSON output                     |
| `-o jsonpath='{...}'`        | Extract only specific values from JSON   |

---
---
# Exercise 

## 📝 What are we doing?

We need to modify the Kubernetes **kubeconfig file** (which is like a config file that tells `kubectl` who you are and where to connect).

You have:
- An SSL certificate: `security.crt`
- A private key: `secrecy.key`

These are used to **authenticate as a user** named `operator`.

---

### 🎯 Goal:
1. **Task 1:** Add a new **user** called `operator` that uses `security.crt` and `secrecy.key` to prove identity.
2. **Task 2:** Create a new **context** also called `operator`, which links the current Kubernetes cluster to the `operator` user.

A "context" tells `kubectl`:  
➡️ *Which user* to act as,  
➡️ *Which cluster* to connect to.

---

## ✅ Full Script with Notes (Comments)

Put this in `/home/ubuntu/tech-vault-q4/taskrunner.sh`:

```bash
#!/bin/bash

# ---------------------------------------
# taskrunner.sh - Setup user and context
# ---------------------------------------

# Define the paths to your certificate and key files
CRT_PATH="/home/ubuntu/tech-vault-q4/security.crt"
KEY_PATH="/home/ubuntu/tech-vault-q4/secrecy.key"

# --------------------------
# Task 1: Create a user
# --------------------------

# We're adding a user named 'operator' to the kubeconfig
# The user will authenticate using the client certificate and key
kubectl config set-credentials operator \
  --client-certificate="${CRT_PATH}" \
  --client-key="${KEY_PATH}"

# --------------------------
# Task 2: Create a context
# --------------------------

# We need to find the name of the current cluster
# This command gets the current cluster name from the config
CURRENT_CLUSTER=$(kubectl config view --minify -o jsonpath='{.clusters[0].name}')

# Now we create a new context named 'operator'
# This context connects the 'operator' user to the current cluster
kubectl config set-context operator \
  --cluster="${CURRENT_CLUSTER}" \
  --user=operator

# (Optional) You can activate the context using this line:
# kubectl config use-context operator
```

---

## 📦 How to Use
After saving the file:
1. Make sure it’s **executable**:
   ```bash
   chmod +x /home/ubuntu/tech-vault-q4/taskrunner.sh
   ```

2. Run the test:
   ```bash
   sudo execute
   ```

