# Documentation: Adding Hosts to Nagios + Troubleshooting Duplicate Host & Missing File Errors

## **1. Objective**

Add two CentOS 9 servers (`dev-app-mg` and `stage-web-mg`) to Nagios monitoring using NRPE, and resolve configuration issues encountered during setup.

---

# 📌 **2. Installing NRPE & Plugins on Each Remote Host**

(Performed on dev-app-mg and stage-web-mg)

```bash
sudo yum install epel-release -y
sudo yum install nrpe -y
sudo yum install nagios-plugins-{load,http,users,procs,disk,swap,nrpe,uptime} -y

```

---

# 📌 **3. Configure NRPE to Allow Nagios Server**

Edit:

```bash
sudo vi /etc/nagios/nrpe.cfg

```

Update:

```
allowed_hosts=127.0.0.1,10.1.10.37

```

Start & enable NRPE:

```bash
sudo systemctl start nrpe
sudo systemctl enable nrpe

```

---

# 📌 **4. Host File Creation on Nagios Server**

Location:

```
/usr/local/nagios/etc/objects/

```

## **Host: dev-app-mg**

File: `dev-app-mg.cfg`

```
define host {
    use                     linux-server
    host_name               dev-app-mg
    alias                   dev-app-mg
    address                 10.1.30.150
    max_check_attempts      5
    check_period            24x7
    notification_interval   30
    notification_period     24x7
}

define service {
    use                     generic-service
    host_name               dev-app-mg
    service_description     PING
    check_command           check_ping!100.0,20%!500.0,60%
}

define service {
    use                     generic-service
    host_name               dev-app-mg
    service_description     SSH
    check_command           check_ssh
}

```

---

## **Host: stage-web-mg**

File: `stage-web-mg.cfg`

```
define host {
    use                     linux-server
    host_name               stage-web-mg
    alias                   Staging Web Server
    address                 10.1.30.157
    max_check_attempts      5
    check_period            24x7
    notification_interval   30
    notification_period     24x7
}

define service {
    use                     generic-service
    host_name               stage-web-mg
    service_description     PING
    check_command           check_ping!100.0,20%!500.0,60%
}

define service {
    use                     generic-service
    host_name               stage-web-mg
    service_description     SSH
    check_command           check_ssh
}

```

---

# 📌 **5. Link Host Files to Main Nagios Config**

Edit:

```bash
sudo vi /usr/local/nagios/etc/nagios.cfg

```

Add:

```
cfg_file=/usr/local/nagios/etc/objects/dev-app-mg.cfg
cfg_file=/usr/local/nagios/etc/objects/stage-web-mg.cfg

```

---

# 🚨 **6. Troubleshooting Issues Encountered**

You handled two major real-world Nagios problems:

---

## **Issue #1: Duplicate Host Definition (FQDN vs Short Name)**

**Error:**

```
Duplicate definition found for host 'dev-app-mg.procore.prod1'

```

**Cause:**

A second host file existed:

```
dev-app-mg.procore.prod1.cfg

```

and Nagios was loading both FQDN and short name.

**Fix:**

Rename (disable) the incorrect file:

```bash
sudo mv dev-app-mg.procore.prod1.cfg dev-app-mg.procore.prod1.cfg.disabled

```

Then remove reference from `nagios.cfg` using search in vi:

```
/dev-app-mg.procore.prod1
dd
:wq

```

---

## **Issue #2: Missing Host File After Removal**

**Error:**

```
Cannot open config file '/usr/local/nagios/etc/objects/dev-app-mg.cfg'

```

**Cause:**

The correct file name didn’t exist yet OR was created later.

**Fix:**

Create the correct file:

```
dev-app-mg.cfg

```

Then validate again.

---

# 📌 **7. Validate Nagios Config**

Always validate before restart:

```bash
sudo /usr/local/nagios/bin/nagios -v /usr/local/nagios/etc/nagios.cfg

```

Expected output:

✔ Read main config file okay

✔ Read object config files okay

✔ Total Warnings: 0

✔ Total Errors: 0

---

# 📌 **8. Reload Nagios**

```bash
sudo systemctl reload nagios

```

---

# 📌 **9. Verification in Web UI**

Visit:

```
http://<nagios-server>/nagios/

```

Check:

✔ Hosts » dev-app-mg

✔ Hosts » stage-web-mg

✔ Services: PING and SSH showing green (OK)

---

# 🎯 **What I Learned / Skills Gained**

### ✔ Correct procedure to add hosts to Nagios

### ✔ How to configure NRPE on CentOS 9

### ✔ How Nagios loads config files (`cfg_file=`)

### ✔ Troubleshooting duplicate host definitions

### ✔ Fixing missing file errors

### ✔ Using vi search (`/pattern`) and line delete (`dd`)

### ✔ Validating Nagios config like a professional admin

---

If you want, I can also turn this into:

✅ A Markdown file

✅ A PDF

✅ A Notion-ready template

✅ A Jira ticket summary version

# 📘 **Notion Template — Nagios Host & Service Configuration Cheat Sheet**

---

## **🖥️ Nagios: Adding a Host + Service Checks (Study Notes)**

*A clear breakdown of every line inside a Nagios config file.*

---

## **📍 Host Definition**

```
define host {
    use                     linux-server
    host_name               dev-app-mg
    alias                   dev-app-mg
    address                 10.1.30.150
    max_check_attempts      5
    check_period            24x7
    notification_interval   30
    notification_period     24x7
}

```

---

### **🔍 Breakdown of Host Directives**

| Directive | Meaning |
| --- | --- |
| **use linux-server** | Inherit default settings from the *linux-server* template. |
| **host_name** | Unique internal name used by Nagios (must be unique). |
| **alias** | Human-friendly name shown in the Nagios UI. |
| **address** | The IP address Nagios uses to monitor the host. |
| **max_check_attempts 5** | Host must fail 5 checks before being marked DOWN (prevents false positives). |
| **check_period 24x7** | Monitor this host continuously (all day, every day). |
| **notification_interval 30** | If still down, send reminder alerts every 30 minutes. |
| **notification_period 24x7** | Alerts can be sent at any time (24/7). |

---

## **⚙️ Service Definition (PING)**

```
define service {
    use                     generic-service
    host_name               dev-app-mg
    service_description     PING
    check_command           check_ping!100.0,20%!500.0,60%
}

```

### **🔍 Breakdown of PING Check**

| Directive | Meaning |
| --- | --- |
| **use generic-service** | Inherit default service template settings. |
| **host_name dev-app-mg** | Attach this check to *this* host. |
| **service_description PING** | Label shown in Nagios UI. |
| **check_command check_ping!100.0,20%!500.0,60%** | Ping with WARNING and CRITICAL thresholds. |

**Threshold meaning:**

- **WARNING:** >100ms latency OR >20% packet loss
- **CRITICAL:** >500ms latency OR >60% packet loss

---

## **🔐 Service Definition (SSH)**

```
define service {
    use                     generic-service
    host_name               dev-app-mg
    service_description     SSH
    check_command           check_ssh
}

```

### **🔍 Breakdown of SSH Check**

| Directive | Meaning |
| --- | --- |
| **service_description SSH** | Label shown in the UI. |
| **check_command check_ssh** | Built-in SSH check: OK if port 22 is open, CRITICAL if not. |