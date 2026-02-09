# ✅ Systemd Service Creation — Quick Documentation

## **1. What is systemd?**

Systemd is the service manager used in RHEL/CentOS/Rocky Linux.

It controls background services (daemons): start, stop, restart, enable on boot.

---

# **2. Steps to Create a New Systemd Service**

## **Step 1 — Place the script or binary**

All executable service files should go under:

```
/usr/bin/

```

Example:

```
sudo cp /nfs/incoming/scripts/procored.sh /usr/bin/
sudo chmod +x /usr/bin/procored.sh

```

---

## **Step 2 — Create the service file**

All service definition files live under:

```
/etc/systemd/system/

```

Create a new one:

```
sudo vi /etc/systemd/system/procored.service

```

Add content:

```
[Unit]
Description=ProcoreD Daemon
After=network.target

[Service]
Type=simple
ExecStart=/usr/bin/procored.sh
Restart=always

[Install]
WantedBy=multi-user.target

```

---

# **3. Meaning of Each Section**

## ⭐ **[Unit] — Service metadata**

- `Description=` → Friendly name for the service
- `After=` → When to start the service (e.g., after networking is up)

Example:

```
After=network.target

```

---

## ⭐ **[Service] — How the service behaves**

### **Type=simple**

- Systemd assumes the script runs in the foreground.
- Most custom scripts use this.

### **ExecStart=**

- Full path to the script you want systemd to run.

Example:

```
ExecStart=/usr/bin/procored.sh

```

### **Restart=always**

- Automatically restarts the service if it crashes.

---

## ⭐ **[Install] — Autostart settings**

Defines when the service should start at boot.

```
WantedBy=multi-user.target

```

`multi-user.target` = normal system startup for servers.

---

# **4. Enable and Start the Service**

Reload systemd (required whenever you create or edit a service):

```
sudo systemctl daemon-reload

```

Enable the service at boot **and start it immediately**:

```
sudo systemctl enable --now procored.service

```

Check service status:

```
systemctl status procored.service

```

---

# **5. Common Troubleshooting**

### ❌ `status=203/EXEC`

Means systemd **cannot execute the file**.

Usually caused by:

- Wrong path in ExecStart
- Missing execute permission
- Missing shebang (`#!/bin/bash`)
- Script not found

### Fix:

- Ensure the correct path in service file
- Ensure file is executable (`chmod +x`)
- Run `daemon-reload`

---

# **6. Final Summary**

To create a systemd service:

1. Copy script to `/usr/bin`
2. Make executable
3. Create `procored.service` under `/etc/systemd/system`
4. Add `[Unit]`, `[Service]`, `[Install]` sections
5. Run `systemctl daemon-reload`
6. Enable + start service
7. Verify status