# SSH Issue solution

# 🧩 SSH Host Key Mismatch — SSSD / FreeIPA Environment Fix

### **Document Owner:** M. Gherzghir

**Last Updated:** November 2025

**Applies To:**

- Bastion hosts and managed Linux clients integrated with **FreeIPA + SSSD**
- SSH host key mismatch or “REMOTE HOST IDENTIFICATION HAS CHANGED!” errors

---

## 🔎 Problem Description

When attempting to SSH into a managed server (e.g., `10.1.30.150` or `10.1.30.157`) from a FreeIPA-integrated bastion, the following error appears:

```
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@    WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!     @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
IT IS POSSIBLE THAT SOMEONE IS DOING SOMETHING NASTY!
Offending RSA key in /var/lib/sss/pubconf/known_hosts:<line>
Host key verification failed.

```

### **Root Cause**

- The server’s SSH host key changed (commonly after an OS reinstall or rekeying).
- SSSD stores cached fingerprints under `/var/lib/sss/pubconf/known_hosts`.
- When the new fingerprint differs from the cached one, SSSD blocks SSH for security reasons.

---

## 🧭 Step-by-Step Resolution

### **1️⃣ Clean Up Old Entries**

Run these on the **bastion host** (the system initiating the SSH connection):

```bash
sudo ssh-keygen -R <IP_ADDRESS> -f /var/lib/sss/pubconf/known_hosts
ssh-keygen -R <IP_ADDRESS>
sudo sss_cache -E

```

> If you see “not found,” it simply means the old entry was already removed.
> 

---

### **2️⃣ Retrieve and Register the New Host Key**

Fetch the updated SSH host key directly from the target system and append it to the SSSD known_hosts file:

```bash
sudo ssh-keyscan -H <IP_ADDRESS> | sudo tee -a /var/lib/sss/pubconf/known_hosts

```

✅ Example:

```bash
sudo ssh-keyscan -H 10.1.30.157 | sudo tee -a /var/lib/sss/pubconf/known_hosts

```

This command:

- Uses `ssh-keyscan` to query the host’s current SSH key.
- Hashes the IP for privacy (`H`).
- Appends it into SSSD’s central host trust file (`/var/lib/sss/pubconf/known_hosts`).

---

### **3️⃣ Optional: Verify Entry**

To confirm the key was added:

```bash
sudo ssh-keygen -F <IP_ADDRESS> -f /var/lib/sss/pubconf/known_hosts

```

---

### **4️⃣ Test Connection**

Once updated, reattempt the SSH connection:

```bash
ssh mgherzghir@<IP_ADDRESS>

```

You should connect **without any fingerprint mismatch warnings**.

---

## ✅ Validation

| Test | Command | Expected Result |
| --- | --- | --- |
| Network Reachable | `ping <IP_ADDRESS>` | Host replies |
| SSH Works | `ssh mgherzghir@<IP_ADDRESS>` | Connection successful |
| Hostname Works | `ssh mgherzghir@<HOSTNAME>` | Connection successful |
| SSSD Cache Clean | `sudo sss_cache -E` | No output (OK) |

---

## 🧠 Notes & Best Practices

- Always verify that the IP or hostname points to the **correct system** before accepting a new key.
- FreeIPA/SSSD systems rely on `/var/lib/sss/pubconf/known_hosts` instead of each user’s local `~/.ssh/known_hosts`.
- If multiple users or automation systems rely on the bastion, updating this file ensures consistent trust for everyone.
- After major VM rebuilds, re-run `ssh-keyscan` to refresh fingerprints automatically.

---

---

## 📘 Summary

| Step | Action | Purpose |
| --- | --- | --- |
| 1 | Remove old host key | Eliminate outdated fingerprint |
| 2 | Scan and add new key | Register the correct key with SSSD |
| 3 | Verify and test | Confirm smooth SSH login |
| 4 | (Optional) Automate | Standardize for future rebuilds |

---
