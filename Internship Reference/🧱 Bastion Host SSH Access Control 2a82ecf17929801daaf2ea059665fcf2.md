# 🧱 Bastion Host SSH Access Control

### 🎯 Objective

Restrict SSH access on internal servers (`dev-app`, `dev-performance`, `stage-web`) to **only** the **bastion host** IP network, enhancing security and reducing external exposure.

---

### 🧩 Background

A **bastion host** acts as the **secure entry point** into a private network.

Because it’s exposed to the internet, it must be hardened and used as the *only* SSH gateway to internal servers.

> 🧠 Note:
> 
> 
> CentOS 9 uses **OpenSSH 8.7**, which no longer supports `/etc/hosts.allow` or `/etc/hosts.deny`.
> 
> SSH restrictions must now be managed through **`firewalld`**.
> 

---

### ⚙️ Steps Performed

### **1️⃣ Verify Firewall Status**

Check if `firewalld` is active and running:

```bash
sudo systemctl status firewalld

```

---

### **2️⃣ Allow SSH from Bastion Host Subnet**

Allow SSH access **only** from the bastion host’s IP range (`10.1.30.22/23`):

```bash
sudo firewall-cmd --permanent --add-rich-rule='rule family="ipv4" source address="10.1.30.22/23" port port="22" protocol="tcp" accept'

```

---

### **3️⃣ Remove Default SSH Rules**

To ensure no general SSH access exists:

```bash
sudo firewall-cmd --remove-service=ssh --permanent
sudo firewall-cmd --remove-port=22/tcp --permanent

```

---

### **4️⃣ Apply Changes**

Reload `firewalld` so new rules take effect immediately:

```bash
sudo firewall-cmd --reload

```

---

### **5️⃣ Verification**

- Tested SSH access **via the bastion host** — ✅ successful
- Attempted SSH from unauthorized IPs — 🚫 denied
- Confirmed persistent configuration:
    
    ```bash
    sudo firewall-cmd --list-rich-rules
    
    ```
    

---

### 🧠 Key Takeaways

- The `/etc/hosts.allow` and `/etc/hosts.deny` methods are deprecated in CentOS 9.
- `firewalld` **rich rules** offer granular control for security policies.
- Always test changes from a second SSH session to avoid locking yourself out.
- Bastion-controlled access is a **best practice** for production and SOC environments.

---

### ✅ Final Configuration Summary

| Server | Status | Rule Summary |
| --- | --- | --- |
| **dev-app** | ✅ Secured | SSH allowed only from `10.1.30.22/23` |
| **dev-performance** | ✅ Secured | SSH allowed only from `10.1.30.22/23` |
| **stage-web** | ✅ Secured | SSH allowed only from `10.1.30.22/23` |