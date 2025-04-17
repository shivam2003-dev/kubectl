

## 🗂️ Objective

You need to complete a shell script at `q2/runscript.sh` to:

1. **Extract** a `.tar.gz` archive
2. **Update permissions**:
   - Files → `0664`
   - Directories → `0775`
3. **Change ownership** to:
   - Owner: `anonymous`
   - Group: `no-team`
4. **Re-archive** everything into `/tmp/fixed-archive.tar.gz`
5. **Run it all with a single command**: `sudo activate`

---

## ✅ Complete Script: `q2/runscript.sh`

```bash
#!/bin/bash

cd /q2 || exit 1

# Extract the archive
tar -xzf backup.tar.gz

# Set file permissions to 0664
find . -type f -exec chmod 0664 {} \;

# Set directory permissions to 0775
find . -type d -exec chmod 0775 {} \;

# Change owner and group
chown -R anonymous:no-team .

# Create new archive
tar -czf /tmp/fixed-archive.tar.gz ./*
```

---

## 🧠 Step-by-Step Explanation

| Step | Command | Explanation |
|------|---------|-------------|
| `cd /q2` | Navigate to the working directory |
| `tar -xzf backup.tar.gz` | Extracts the contents of the `.tar.gz` archive |
| `find . -type f -exec chmod 0664 {} \;` | Sets file permissions to allow owner & group to read/write, others to read |
| `find . -type d -exec chmod 0775 {} \;` | Sets directory permissions: full access for owner & group, read/execute for others |
| `chown -R anonymous:no-team .` | Changes ownership of all extracted files and folders recursively |
| `tar -czf /tmp/fixed-archive.tar.gz ./*` | Compresses the modified files into a new archive |

---

## ⚙️ How to Run It

Once you've saved the file to `q2/runscript.sh`, run the following:

```bash
cd q2
sudo activate
```

> Just like `execute`, `activate` is likely an alias or script that does something like:

```bash
bash runscript.sh
```

With elevated (sudo) privileges.


# 🧠 Quick Notes: Shell Scripting Essentials

## 1. `cd /q2 || exit 1`

✅ **Purpose**:  
Ensure the script only continues **if the directory change is successful**.

📌 **Breakdown**:
- `cd /q2`: Try to go to the `/q2` directory.
- `||`: OR operator — only runs the next command **if the previous one fails**.
- `exit 1`: Stop the script with an error code.

🔐 **Why?**  
Prevents running commands in the **wrong location** if `/q2` doesn’t exist.

---

## 2. `find . -type f -exec chmod 0664 {} \;`

✅ **Purpose**:  
**Find all files** in the current directory and change their permissions.

📌 **Breakdown**:
- `find .`: Start searching from the current directory.
- `-type f`: Match **files only**.
- `-exec chmod 0664 {} \;`: For each file found, run `chmod 0664`.

🔑 **Symbols**:
- `{}` → placeholder for each matched file.
- `\;` → ends the `-exec` command (must be escaped).

---

## 📝 TL;DR Summary

| Snippet | Purpose |
|--------|---------|
| `cd DIR || exit 1` | Fail fast if directory doesn't exist |
| `find . -type f -exec chmod ...` | Change permissions for all files recursively |

---


