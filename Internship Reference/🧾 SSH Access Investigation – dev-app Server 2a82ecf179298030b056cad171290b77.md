# 🧾 SSH Access Investigation – dev-app Server

**Date:** Nov 8, 2025

**Author:** Musie Gherzghir

**Environment:** CentOS Stream 9

**System:** `dev-app-mg`

**User Involved:** `apprentice`

**Source IP:** `10.1.30.150`

---

## 🎯 **Task Description**

Developers reported multiple issues with SSH access to the **dev-app** server for user **`apprentice`**.

The objective was to **investigate** and determine the **root cause** — not to fix the issue.

**Requirements:**

1. Check SSH access issue for user `apprentice`.
2. Collect and attach the last 20 lines of the SSH log.
3. Identify the root cause only.

---

## 🧠 **Understanding the Objective**

SSH failures can occur due to several common reasons:

- Invalid credentials or account lockout.
- Incorrect SSH key or authentication method.
- Permission issues on `/home/user/.ssh`.
- Service or SSSD communication delays.

Our goal is to **verify which condition applies** using log inspection.

---

## ⚙️ **Investigation Steps**

### Step 1: Logged into the Server

```bash
ssh mgherzghir@dev-app-mg

```

---

### Step 2: Located SSH Log File

SSH logs are stored in:

```bash
/var/log/secure

```

---

### Step 3: Reviewed the Last 20 SSH Entries

Used the following command:

```bash
sudo grep sshd /var/log/secure | tail -20

```

---

### Step 4: Log Output (Captured)

```
Nov  8 18:24:06 dev-app-mg sshd-session[7345]: Accepted keyboard-interactive/pam for apprentice from 10.1.30.150 port 47376 ssh2
Nov  8 18:24:06 dev-app-mg sshd-session[7345]: pam_unix(sshd:session): session opened for user apprentice(uid=770000490) by apprentice(uid=0)
Nov  8 18:38:40 dev-app-mg sshd-session[7534]: pam_sss(sshd:auth): authentication success; logname= uid=0 euid=0 tty=ssh ruser= rhost=10.1.30.150 user=apprentice
Nov  8 18:38:40 dev-app-mg sshd-session[7530]: Accepted keyboard-interactive/pam for apprentice from 10.1.30.150 port 54066 ssh2
Nov  8 18:38:40 dev-app-mg sshd-session[7530]: pam_unix(sshd:session): session opened for user apprentice(uid=770000490) by apprentice(uid=0)
Nov  8 18:46:14 dev-app-mg sshd-session[7538]: Received disconnect from 10.1.30.150 port 54066:11: disconnected by user
Nov  8 18:46:14 dev-app-mg sshd-session[7538]: Disconnected from user apprentice 10.1.30.150 port 54066
Nov  8 18:46:14 dev-app-mg sshd-session[7530]: pam_unix(sshd:session): session closed for user apprentice
Nov  8 18:46:18 dev-app-mg sshd-session[7367]: Received disconnect from 10.1.30.150 port 47376:11: disconnected by user
Nov  8 18:46:18 dev-app-mg sshd-session[7367]: Disconnected from user apprentice 10.1.30.150 port 47376
Nov  8 18:46:18 dev-app-mg sshd-session[7345]: pam_unix(sshd:session): session closed for user apprentice
Nov  8 18:55:03 dev-app-mg sshd-session[7709]: pam_sss(sshd:auth): authentication failure; logname= uid=0 euid=0 tty=ssh ruser= rhost=10.1.30.150 user=apprentice
Nov  8 18:55:03 dev-app-mg sshd-session[7709]: pam_sss(sshd:auth): received for user apprentice: 7 (Authentication failure)
Nov  8 18:55:05 dev-app-mg sshd-session[7704]: error: PAM: Authentication failure for apprentice from 10.1.30.150
Nov  8 18:55:11 dev-app-mg sshd-session[7712]: pam_sss(sshd:auth): authentication success; logname= uid=0 euid=0 tty=ssh ruser= rhost=10.1.30.150 user=apprentice
Nov  8 18:55:11 dev-app-mg sshd-session[7704]: Accepted keyboard-interactive/pam for apprentice from 10.1.30.150 port 55070 ssh2
Nov  8 18:55:12 dev-app-mg sshd-session[7704]: pam_unix(sshd:session): session opened for user apprentice(uid=770000490) by apprentice(uid=0)
Nov  8 18:55:59 dev-app-mg sshd-session[7733]: Received disconnect from 10.1.30.150 port 55070:11: disconnected by user
Nov  8 18:55:59 dev-app-mg sshd-session[7733]: Disconnected from user apprentice 10.1.30.150 port 55070
Nov  8 18:55:59 dev-app-mg sshd-session[7704]: pam_unix(sshd:session): session closed for user apprentice

```

---

## 🧩 **Analysis**

From the log timeline:

| Time | Event | Description |
| --- | --- | --- |
| 18:24 | ✅ Successful login | User authenticated successfully |
| 18:38 | ✅ Successful login | Second successful session opened |
| 18:46 | 🔚 Normal logout | User disconnected manually |
| 18:55 | ❌ Authentication failure | PAM/SSSD rejected login (invalid credentials or temporary SSSD delay) |
| 18:55 | ✅ Successful login again | Authentication succeeded after retry |

---

## 🧠 **Root Cause**

The **SSH service and account configuration are functioning correctly**.

The logs show **intermittent authentication failure** for the user `apprentice` caused by either:

- Incorrect password entry, **or**
- Temporary **SSSD authentication delay** during PAM verification.

The issue is **not related** to SSH service malfunction, configuration errors, or firewall blocking.

---

## 🧾 **Conclusion**

| Category | Details |
| --- | --- |
| **Status** | Investigated – no system fault found |
| **Root Cause** | Temporary authentication failure via PAM/SSSD (invalid credentials or delay) |
| **Impact** | User `apprentice` experienced one failed login attempt |
| **Next Steps** | None required; system working as expected |

---

## 📘 **Reference Commands Used**

```bash
# View recent SSH logs
sudo grep sshd /var/log/secure | tail -20

# Save log excerpt for documentation
sudo tail -20 /var/log/secure > ~/ssh_issue.log

# Optional: view live SSH events
sudo journalctl -u sshd -f

```