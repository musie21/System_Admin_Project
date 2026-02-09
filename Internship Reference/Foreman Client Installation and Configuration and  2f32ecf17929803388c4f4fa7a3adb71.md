# Foreman Client Installation and Configuration and Run remote command

## Foreman Client Installation and Configuration

# Foreman Client Installation and Configuration

**Author:** Musie Gherzghir

**Role:** Linux System Administrator

**Purpose:** Standardized guide for registering Linux servers with Foreman

---

## 📌 Overview

This document provides a step-by-step guide for installing, configuring, and registering Linux client servers with the **Foreman Configuration Management Server**. Proper registration ensures centralized visibility, patching, and compliance with security requirements.

This guide is intended for **Dev, Stage, and Production Linux environments**.

---

## 🎯 Objectives

- Install Foreman client packages on Linux servers
- Securely register hosts with the Foreman server
- Validate successful registration
- Restore system hardening after setup

---

## 🧰 Supported Environments

- RHEL / CentOS / Rocky / AlmaLinux (8/9)
- Systemd-based distributions

---

## 🛠️ Prerequisites

- Network connectivity to the Foreman server
- DNS resolution between client and Foreman server
- SSH access from Foreman to client hosts
- Sudo or root privileges

---

## 🔐 Pre-Configuration (Security Preparation)

### 1️⃣ Temporarily Enable Root SSH Access

Foreman requires SSH access during registration.

Edit SSH configuration:

```bash
vi /etc/ssh/sshd_config

```

Ensure the following is set:

```
PermitRootLogin yes

```

Restart SSH service:

```bash
systemctl restart sshd

```

> ⚠️ Root SSH access must be disabled again after registration.
> 

---

### 2️⃣ Adjust Firewall Rules

Temporarily allow Foreman server access.

Example (if firewalld is enabled):

```bash
systemctl stop firewalld

```

Or open SSH explicitly:

```bash
firewall-cmd --add-service=ssh --permanent
firewall-cmd --reload

```

---

## 🔑 SSH Key Exchange

From the **Foreman server**, copy the SSH key to the client host:

```bash
ssh-copy-id root@<client-ip>

```

Verify passwordless access:

```bash
ssh root@<client-ip>

```

---

## 📦 Install Foreman Client Packages

On the **client server**, install the Foreman client:

```bash
yum install -y foreman-client

```

If required, enable Foreman repositories before installation.

---

## 📝 Register Client with Foreman

Run the Foreman registration command provided by the Foreman UI or administrator.

Example:

```bash
foreman-client register \
--foreman-hostname foreman.example.local \
--organization "Default Organization" \
--location "Default Location"

```

Registration will:

- Create the host entry in Foreman
- Associate it with the correct organization/location
- Enable future configuration and patch management

---

## ✅ Verification

### Check Foreman Dashboard

- Log into Foreman Web UI
- Navigate to **Hosts → All Hosts**
- Confirm the client appears with correct hostname and status

### Verify Client Status

```bash
foreman-client status

```

---

## 🔒 Post-Registration Hardening

### Disable Root SSH Access

```bash
vi /etc/ssh/sshd_config

```

Set:

```
PermitRootLogin no

```

Restart SSH:

```bash
systemctl restart sshd

```

---

### Restore Firewall Rules

```bash
systemctl start firewalld

```

Verify firewall status:

```bash
firewall-cmd --list-all

```

---

## 🧠 Common Issues & Troubleshooting

### ❌ SSH Connection Fails

- Verify firewall rules
- Confirm SSH key copied correctly
- Check DNS resolution

### ❌ Host Not Appearing in Foreman

- Re-run registration command
- Verify organization and location parameters
- Check Foreman server logs

---

## 📸 Documentation & Evidence Requirements

- Screenshots of Foreman host registration
- Terminal output of successful registration
- Jira ticket reference

---

## 📌 Best Practices

- Never leave root SSH enabled after setup
- Always document configuration changes
- Use consistent host naming conventions
- Validate registration immediatel
    
    ![image-20251107-070100.png](Foreman%20Client%20Installation%20and%20Configuration%20and%20/image-20251107-070100.png)
    

## Run Remote Commands from Foreman

Run Remote Commands from Foreman

**Author:** Musie Gherzghir

**Role:** Linux System Administrator

**Ticket:** PROCORE-34

**Status:** Completed

---

## 📌 Overview

The Security Team required the creation of a **local user account** across all Linux virtual machines in the infrastructure using **Foreman Remote Execution**. This task ensures consistent user provisioning, centralized management, and compliance with security standards.

The user to be created:

- **Full Name:** Reuben Camilo
- **Username:** `rcamilo`

All actions were executed centrally using Foreman’s **Run Remote Command** functionality.

---

## 🎯 Objective

- Strengthen Linux system administration skills through automation
- Use Foreman to run remote commands across multiple hosts
- Ensure consistent user creation on all servers
- Properly document the workflow, challenges, and resolutions

---

## 📋 Requirements

- User `rcamilo` added to **all Linux servers** using Foreman
- Follow Foreman Wiki guidance for remote command execution
- Capture screenshots as evidence
- Document challenges and resolutions

---

## 🧰 Environment Scope

- Linux VMs registered with Foreman
- Dev / Stage infrastructure
- Systemd-based Linux distributions

---

## 🛠️ Implementation Steps

### 1️⃣ Verify Host Registration in Foreman

- Logged into the Foreman Web UI
- Navigated to **Hosts → All Hosts**
- Confirmed all target VMs were online and reachable

---

### 2️⃣ Prepare SSH Access for Remote Execution

Foreman requires SSH access to execute remote commands.

**Issue Identified:**

- Remote command execution failed due to authentication error:
    
    `Permission denied for root@<IP-address>`
    

**Resolution:**

- Temporarily enabled root SSH access on affected client servers

```bash
vi /etc/ssh/sshd_config

```

Updated setting:

```
PermitRootLogin yes

```

Restarted SSH service:

```bash
systemctl restart sshd

```

> ⚠️ Root SSH was re-disabled after task completion.
> 

---

### 3️⃣ Run Remote Command from Foreman

From the **Foreman UI**:

- Navigated to **Hosts → All Hosts**
- Selected all target servers
- Clicked **Run Job / Run Remote Command**
- Chose the appropriate job template

Executed command to create the user:

```bash
useradd rcamilo

```

(Optional validation):

```bash
id rcamilo

```

---

### 4️⃣ Verify User Creation

Verification steps:

- Confirmed successful job execution in Foreman
- Logged into selected servers
- Validated user existence using:

```bash
id rcamilo

```

---

### 5️⃣ Post-Execution Hardening

After successful execution:

- Restored SSH configuration to its previous secure state

```bash
PermitRootLogin no

```

- Restarted SSH service
- Ensured firewall and access controls remained intact

---

## ✅ Validation

- User `rcamilo` successfully created on all target servers
- Foreman job execution status confirmed
- Screenshots attached in Jira as evidence
- Task verified and approved by Security Team

---

## 🧠 Challenges & Resolutions

### ❌ Remote Command Authentication Failure

**Issue:**

- Foreman could not authenticate to client servers via root SSH

**Resolution:**

- Temporarily enabled root SSH access
- Re-ran remote execution successfully
- Reverted SSH configuration immediately after

---

## 📸 Evidence

- Foreman job execution screenshots
- Terminal verification screenshots
- SSH configuration confirmation screenshots

(All attached in Jira ticket PROCORE-34)

---

## 📌 Best Practices & Lessons Learned

- Always validate SSH connectivity before remote execution
- Never leave elevated access enabled after task completion
- Centralized automation reduces configuration drift
- Clear documentation prevents audit gaps

---

## 🔗 Reference

- Internal Foreman Wiki: *Running Remote Commands*

---

✅ **Task successfully completed and verified by Security Team**