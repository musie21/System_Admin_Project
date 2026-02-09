# Urgent! Malicious IP

📌 Overview

---

**Issue Key:** MG-39

**Project:** Musie Gherzghir

**Type:** Task

**Priority:** Medium

**Status:** ✅ Done

**Reporter:** Michael Anthony Lopez

**Assignee:** Musie Gherzghir

**Created:** 15 Oct 2025

**Updated / Resolved:** 10 Nov 2025

---

## 📝 Issue Description

The networking team reported a **malicious IP address** actively attempting to gain unauthorized access to the internal network infrastructure. Immediate action was required to prevent potential compromise.

---

## 🎯 Objective

Strengthen practical Linux networking and security skills by **configuring and securing systems** against malicious access attempts.

---

## 📋 Requirements

- **Affected VMs:**
    - `dev-app`
    - `dev-performance`
    - `stage-web`
- **Action Required:**
    - Block SSH access from the malicious IP address: **174.50.30.12**

---

## ℹ️ Additional Instructions

- Block SSH access **as soon as possible**
- Proper documentation is mandatory
- Provide screenshots of work completed
- Describe:
    - The task performed
    - Any challenges or roadblocks
    - Steps taken to resolve the issue (video or audio format required)

---

## 🛠️ Implementation Details

After reviewing best practices, two mitigation options were considered:

1. Block the malicious IP from accessing **SSH only**
2. Block the malicious IP from accessing the **entire network**

### ✅ Decision

The malicious IP was **blocked at the firewall level for the entire network**, which is considered a stronger and more secure approach than limiting the block to SSH alone.

---

## 🔐 Firewall Configuration (firewalld)

The following **rich rules** were applied on all affected servers:

```bash
sudo firewall-cmd --list-rich-rules

rule family="ipv4" source address="174.50.30.12" drop
rule family="ipv4" source address="10.1.30.22/23" port port="22" protocol="tcp" accept

```

✔ Verified on:

- `dev-app`
- `dev-performance`
- `stage-web`

---

## 🖼️ Evidence / Attachments

Screenshots captured and attached in Jira:

- Firewall rules on `dev-app`
- Firewall rules on `dev-performance`
- Firewall rules on `stage-web`

(Reference images: `image-20251109-191254.png`, `image-20251109-191227.png`, `image-20251109-191202.png`)

---

## 💬 Comments & Feedback

**Musie Gherzghir (09 Nov 2025):**

Outlined the security decision-making process and confirmed firewall-level blocking. Offered rollback if SSH-only blocking was preferred.

**Michael Anthony Lopez (10 Nov 2025):**

> “Thank you for blocking the IP via the firewall. Great job!”
> 

---

## ✅ Outcome

- Malicious IP **successfully blocked**
- No disruption to legitimate SSH traffic
- Security posture improved across all environments
- Task completed and verified

---

## 📚 Key Takeaways

- Firewall-level IP blocking is preferred over service-level blocking when dealing with confirmed malicious sources
- Consistent configuration across environments reduces risk
- Clear documentation and evidence are critical for security-related Jira tasks

---

**Documented by:** Musie Gherzghir

**Role:** Linux System Administrator