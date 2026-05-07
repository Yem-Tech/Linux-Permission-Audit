# Linux Permission Audit
### File System Access Control — Numeric and Symbolic Methods

> **Project Summary:** A hands-on Linux permission audit conducted on a Kali Linux environment, demonstrating file and directory access control management using both numeric (octal) and symbolic methods. This project reflects core competencies required of a Cybersecurity Analyst in managing least-privilege principles across a Linux file system.

---

## Project Overview

| Field | Details |
|---|---|
| **Project Type** | Linux File System Permission Audit |
| **Environment** | Kali Linux — `/home/kali` |
| **Methods Used** | Numeric (Octal) and Symbolic (`chmod`) |
| **Core Principle** | Least Privilege Access Control |
| **Analyst** | Olayemi Owoeye |
| **Relevance** | SOC Operations, Linux Hardening, Access Management |

---

## 1. Why Linux Permissions Matter in Cybersecurity

Linux file permissions are one of the foundational layers of operating system security. Misconfigured permissions are a leading cause of privilege escalation, data exposure, and unauthorised access in real-world environments. As a cybersecurity analyst, auditing and correctly configuring file system permissions is a critical daily task across:

- **SOC operations** — investigating suspicious file access events in logs
- **Linux hardening** — applying least-privilege principles to reduce attack surface
- **Incident response** — identifying whether a threat actor modified permissions to maintain persistence
- **Compliance** — ensuring file access aligns with organisational policy and regulatory requirements

---

## 2. Understanding Linux Permission Structure

Every file and directory in Linux carries a 10-character permission string visible when running `ls -l`.

### 2.1 Permission String Anatomy

```
drwxrwxr-x  2  kali  kali  4096  May 7 13:55  Coach
```

| Position | Character | Meaning |
|---|---|---|
| 1 | `d` | Directory (use `-` for a regular file) |
| 2–4 | `rwx` | Owner (user) permissions |
| 5–7 | `rwx` | Group permissions |
| 8–10 | `r-x` | Others permissions |

### 2.2 Permission Types

| Symbol | Name | Meaning on Files | Meaning on Directories |
|---|---|---|---|
| `r` | Read | View file contents | List directory contents |
| `w` | Write | Modify file contents | Create, delete files inside |
| `x` | Execute | Run the file as a program | Enter (traverse) the directory |
| `-` | No permission | Permission denied | Permission denied |

### 2.3 User Categories

| Symbol | Category | Description |
|---|---|---|
| `u` | User (Owner) | The account that owns the file |
| `g` | Group | Users belonging to the file's assigned group |
| `o` | Others | All other users on the system |
| `a` | All | Shorthand for user + group + others combined |

---

## 3. Environment Setup

The audit was conducted in a clean working directory under `/home/kali`. Initial environment confirmed using `ls` and `pwd` to establish baseline directory structure before any permission changes were applied.

**Directories audited:**
`Coach`, `Footballer`, `Templates`, `Downloads`, `Public`, `Desktop`, `Documents`, `Music`, `Pictures`, `Videos`, `simulation_project`, `olayemi_folder`, `XERXES`, `zphisher`

**Files created for testing:**
`textfile.tx` — created using `touch textfile.tx` to provide a file target for permission exercises

**Baseline permission state confirmed using:**
```bash
ls -l
pwd
```

### Screenshot 1 — Baseline directory listing and working directory confirmation
> *Insert screenshot: `01.png` — shows `ls` and `pwd` output confirming working directory at `/home/kali`*

![Baseline directory listing](https://raw.githubusercontent.com/Yem-Tech/Linux-Permission-Audit/4fce969e1f641b8c1fb0fe96ccbe054aa840e21e/Screenshots/01.png)

### Screenshot 2 — Directory and file creation
> *Insert screenshot: `02.png` — shows `mkdir Footballer Coach`, `touch textfile.tx`, and updated `ls` confirming new entries*

![Directory and file creation](https://raw.githubusercontent.com/Yem-Tech/Linux-Permission-Audit/4fce969e1f641b8c1fb0fe96ccbe054aa840e21e/Screenshots/02.png)

---

## 4. Permission Methods

### 4.1 Numeric (Octal) Method

The numeric method assigns permissions using a three-digit octal value, where each digit represents the combined permissions for owner, group, and others respectively.

**Octal Values:**

| Permission | Symbol | Value |
|---|---|---|
| Read | `r` | 4 |
| Write | `w` | 2 |
| Execute | `x` | 1 |
| No permission | `-` | 0 |

**Calculating octal values — add the values for each permission set:**

| Permission Set | Calculation | Octal |
|---|---|---|
| `rwx` | 4 + 2 + 1 | **7** |
| `rw-` | 4 + 2 + 0 | **6** |
| `r-x` | 4 + 0 + 1 | **5** |
| `r--` | 4 + 0 + 0 | **4** |
| `-w-` | 0 + 2 + 0 | **2** |
| `---` | 0 + 0 + 0 | **0** |

**Syntax:**
```bash
chmod [octal] [target]
```

### 4.2 Symbolic Method

The symbolic method modifies permissions using operators and category references, making it more readable and precise for targeted changes.

**Operators:**

| Operator | Action |
|---|---|
| `+` | Add permission |
| `-` | Remove permission |
| `=` | Set exact permission (replaces existing) |

**Syntax:**
```bash
chmod [who][operator][permission] [target]
```

---

## 5. Audit Tasks and Commands

### Task 1 — Numeric: Set Coach to 755
**Objective:** Apply the same permissions as Music (`rwxr-xr-x`) to the Coach directory using the numeric method.

**Analysis of target permissions:**

| Entity | Permissions | Calculation | Octal |
|---|---|---|---|
| Owner | `rwx` | 4+2+1 | 7 |
| Group | `r-x` | 4+0+1 | 5 |
| Others | `r-x` | 4+0+1 | 5 |

```bash
chmod 755 Coach
```

**Before:** `drwxrwxr-x`
**After:** `drwxr-xr-x`

**Result:** Owner retains full access; group and others restricted to read and execute only.

### Screenshot 3 — chmod 755 Coach (numeric method)
> *Insert screenshot: `03-change_mod_numerical.png` — shows `chmod 755 Coach` command and `ls -l` output confirming permission change*

![chmod 755 Coach](https://raw.githubusercontent.com/Yem-Tech/Linux-Permission-Audit/4fce969e1f641b8c1fb0fe96ccbe054aa840e21e/Screenshots/03-change_mod_numerical.png)

---

### Task 2 — Symbolic: Remove execute from owner on Templates
**Objective:** Use symbolic method to remove execute permission from the owner only.

```bash
chmod u-x Templates
```

**Before:** `drwxr-xr-x`
**After:** `drw-r--r--`

**Result:** Owner can no longer execute (traverse) the directory. Group and others unaffected.

### Screenshot 4 — chmod u-x Templates (symbolic remove)
> *Insert screenshot: `04-chmod_symbolic.png` — shows `chmod u-x Templates` command and `ls -l` confirming execute bit removed from owner*

![chmod u-x Templates](https://raw.githubusercontent.com/Yem-Tech/Linux-Permission-Audit/4fce969e1f641b8c1fb0fe96ccbe054aa840e21e/Screenshots/04-chmod_symbolic.png)

---

### Task 3 — Symbolic: Remove execute from all on Templates
**Objective:** Remove execute permission from all users (owner, group, others) simultaneously.

```bash
chmod a-x Templates
```

**Before:** `drw-r--r--`
**After:** `drw-r--r--`

**Result:** Execute bit removed across all user categories.

### Screenshot 5 — chmod a-x Templates (remove execute all)
> *Insert screenshot: `05-chmod-remove_xtemplate.png` — shows `chmod a-x Templates` command and `ls -l` confirming execute removed from all*

![chmod a-x Templates](https://raw.githubusercontent.com/Yem-Tech/Linux-Permission-Audit/4fce969e1f641b8c1fb0fe96ccbe054aa840e21e/Screenshots/05-chmod-remove_xtemplate.png)

---

### Task 4 — Symbolic: Add write for all on Templates
**Objective:** Grant write permission to all users on the Templates directory.

```bash
chmod a+w Templates
```

**Before:** `drw-r--r--`
**After:** `drw-rw-rw-`

**Result:** Write permission added for owner, group, and others simultaneously.

### Screenshot 6 — chmod a+w Templates (add write all)
> *Insert screenshot: `06-chmod_add_wtemplates.png` — shows `chmod a+w Templates` command and `ls -l` confirming write added for all*

![chmod a+w Templates](https://raw.githubusercontent.com/Yem-Tech/Linux-Permission-Audit/4fce969e1f641b8c1fb0fe96ccbe054aa840e21e/Screenshots/06-chmod_add_wtemplates.png)

---

### Task 5 — Symbolic: Set exact permissions rx for all on Templates
**Objective:** Use the `=` operator to set exact permissions — read and execute only for all users, replacing any existing permissions.

```bash
chmod a=rx Templates
```

**Before:** `drw-rw-rw-`
**After:** `dr-xr-xr-x`

**Result:** All previous permissions replaced. Every user category now has read and execute only. Write access removed entirely.

### Screenshot 7 — chmod a=rx Templates (set equal)
> *Insert screenshot: `07-.png` — shows `chmod a=rx Templates` command and `ls -l` confirming exact permissions set*

![chmod a=rx Templates](https://raw.githubusercontent.com/Yem-Tech/Linux-Permission-Audit/4fce969e1f641b8c1fb0fe96ccbe054aa840e21e/Screenshots/07-chmod_equal_rx_templates.png)

---

### Task 6 — Symbolic: Remove read and execute from all on Downloads
**Objective:** Strip read and execute permissions from all users on the Downloads directory.

```bash
chmod a-rx Downloads
```

**Before:** `drwxr-xr-x`
**After:** `d-w-------`

**Result:** Downloads directory becomes inaccessible for listing or traversal by all users. Only owner write permission remains — a highly restrictive state.

### Screenshot 8 — chmod a-rx Downloads (remove rx all)
> *Insert screenshot: `08-chmod_remove-rx_Downloads.png` — shows `chmod a-rx Downloads` command and `ls -l` confirming rx removed from all users*

![chmod a-rx Downloads](https://raw.githubusercontent.com/Yem-Tech/Linux-Permission-Audit/4fce969e1f641b8c1fb0fe96ccbe054aa840e21e/Screenshots/08-chmod_remove-rx_Downloads.png)

---

### Task 7 — Symbolic: Set write only for all on Public
**Objective:** Use the `=` operator to set write-only permissions for all users, stripping all other permissions.

```bash
chmod a=w Public
```

**Before:** `drwxr-xr-x`
**After:** `d-w--w--w-`

**Result:** All users can only write. Read and execute stripped entirely. The directory cannot be listed or traversed — demonstrating how `=` replaces rather than adds.

### Screenshot 9 — chmod a=w Public (set write only)
> *Insert screenshot: `09-chmod-equal_wpublic.png` — shows `chmod a=w Public` command and `ls -l` confirming write-only set for all*

![chmod a=w Public](https://raw.githubusercontent.com/Yem-Tech/Linux-Permission-Audit/4fce969e1f641b8c1fb0fe96ccbe054aa840e21e/Screenshots/09-chmod-equal_wpublic.png)

---

## 6. Permission Change Summary

| Task | Target | Command | Before | After | Method |
|---|---|---|---|---|---|
| 1 | Coach | `chmod 755 Coach` | `rwxrwxr-x` | `rwxr-xr-x` | Numeric |
| 2 | Templates | `chmod u-x Templates` | `rwxr-xr-x` | `rw-r--r--` | Symbolic |
| 3 | Templates | `chmod a-x Templates` | `rw-r--r--` | `rw-r--r--` | Symbolic |
| 4 | Templates | `chmod a+w Templates` | `rw-r--r--` | `rw-rw-rw-` | Symbolic |
| 5 | Templates | `chmod a=rx Templates` | `rw-rw-rw-` | `r-xr-xr-x` | Symbolic |
| 6 | Downloads | `chmod a-rx Downloads` | `rwxr-xr-x` | `-w-------` | Symbolic |
| 7 | Public | `chmod a=w Public` | `rwxr-xr-x` | `-w--w--w-` | Symbolic |

---

## 7. Key Findings and Security Observations

**Finding 1 — Numeric vs Symbolic trade-offs:**
The numeric method is faster for setting complete permission states from scratch. The symbolic method is more precise for targeted modifications without affecting unrelated permission bits.

**Finding 2 — The `=` operator is destructive by design:**
Using `chmod a=w` does not *add* write — it *replaces all existing permissions* with write only. This is a common source of accidental misconfiguration. Analysts must understand the difference between `+`, `-`, and `=` operators before applying them in production environments.

**Finding 3 — Removing `rx` makes directories functionally inaccessible:**
`chmod a-rx Downloads` produced `d-w-------` — a directory that can receive writes but cannot be listed or entered by anyone. This reflects a critical hardening consideration: execute on a directory controls traversal, not just execution of files within it.

**Finding 4 — Least privilege principle in practice:**
Permission 755 (`rwxr-xr-x`) represents the standard least-privilege baseline for most directories — owner has full control, group and others can read and navigate but cannot modify. This is the recommended default for most application and home directories.

**Finding 5 — World-writable permissions are a security risk:**
`chmod a=w` or `chmod 222` creates world-writable directories — a significant misconfiguration risk in production. World-writable directories can be abused for privilege escalation, symlink attacks, and unauthorised file injection.

---

## 8. Security Recommendations

- Apply **755** as the default permission baseline for most user directories — owner full access, group and others read and execute only
- Avoid **world-writable (777 or a=w)** permissions on any production directory or file
- Regularly audit permissions using `ls -l` and `find / -perm -o+w` to identify overly permissive files
- Apply **least privilege** — grant only the minimum permissions necessary for each user category to perform its required function
- Use **numeric method** for consistent, scripted permission application across multiple targets
- Use **symbolic method** for targeted, incremental changes that must not disturb unrelated bits
- Pay close attention to **execute on directories** — it controls traversal, and removing it effectively blocks all access regardless of read/write bits

---

## 9. Commands Reference

```bash
# List files with full permission details
ls -l

# Show current working directory
pwd

# Create a new directory
mkdir [directory_name]

# Create a new empty file
touch [filename]

# Change permissions — numeric method
chmod 755 [target]    # rwxr-xr-x
chmod 775 [target]    # rwxrwxr-x
chmod 644 [target]    # rw-r--r--
chmod 600 [target]    # rw-------

# Change permissions — symbolic method
chmod u-x [target]    # Remove execute from owner
chmod a-x [target]    # Remove execute from all
chmod a+w [target]    # Add write for all
chmod a=rx [target]   # Set read+execute only for all (replaces existing)
chmod a-rx [target]   # Remove read and execute from all
chmod a=w [target]    # Set write only for all (replaces existing)
```

---

## 10. Tools and Environment

| Component | Details |
|---|---|
| **Operating System** | Kali Linux |
| **Shell** | Bash |
| **Primary Command** | `chmod` |
| **Verification Command** | `ls -l` |
| **Working Directory** | `/home/kali` |

---

## Skills Demonstrated

- Linux file system navigation and directory management
- Understanding of Unix permission model — owner, group, others
- Numeric (octal) permission calculation and application
- Symbolic permission modification using `+`, `-`, and `=` operators
- Least privilege access control principles
- File system security auditing and hardening
- Recognition of common permission misconfigurations and their security implications

---




## Author

**Cybersecurity Analyst | HypertechAi**
Certifications: CCNA | Microsoft Azure | AWS | CompTIA Security+ | Python | Cybersecurity
Currently pursuing: Advanced Cybersecurity Specialisation

---

*This project was conducted in an authorised personal lab environment on Kali Linux for professional development and portfolio documentation purposes.*

