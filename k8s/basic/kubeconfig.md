

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

