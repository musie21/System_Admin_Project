# 📄 Configure NFS Server (Musie G.)

---

## **📝 Overview**

This task was to configure an NFS server on **dev-app-mgherzghir** and test mounting from **stage-web-mgherzghir**.

A Java JDK package was downloaded to the NFS share and verified over the network.

---

## **📁 NFS Share Path**

**`/nfs-mgherzghir`**

---

## **🖥️ 1. Configure NFS Server (dev-app)**

### **Create NFS Directory**

```bash
sudo mkdir /nfs-mgherzghir
sudo chown -R mgherzghir: /nfs-mgherzghir

```

---

### **Install & Enable NFS Services**

```bash
sudo dnf install nfs-utils -y
sudo systemctl enable --now nfs-server

```

---

### **Configure /etc/exports**

Edit the file:

```bash
sudo vi /etc/exports

```

Add:

```
/nfs-mgherzghir 10.1.30.157(rw,sync,no_root_squash)

```

Apply:

```bash
sudo exportfs -r
sudo exportfs -v

```

---

### **Open Firewall for NFS**

```bash
sudo firewall-cmd --zone=public --add-service=nfs --permanent
sudo firewall-cmd --reload

```

---

## **🖥️ 2. Set Up NFS Client (stage-web)**

### **Create Mount Directory**

```bash
sudo mkdir -pv /mnt/nfs-mgherzghir

```

### **Mount NFS Share**

```bash
sudo mount -t nfs 10.1.30.150:/nfs-mgherzghir /mnt/nfs-mgherzghir

```

---

## **📥 3. Download Test File**

### **On dev-app (NFS Server)**

Download Java JDK into NFS share:

```bash
wget https://download.java.net/java/GA/jdk18.0.1.1/65ae32619e2f40f3a9af3af1851d6e19/2/GPL/openjdk-18.0.1.1_linux-x64_bin.tar.gz \
     -P /nfs-mgherzghir/

```

---

### **On stage-web (NFS Client)**

Verify the file is visible:

```bash
ls -lh /mnt/nfs-mgherzghir

```

✔️ JDK file was successfully visible — NFS working.

---

## **⚠️ Roadblocks & Resolutions**

### **1️⃣ Error: “No route to host”**

**Cause:** NFS firewall service wasn’t enabled

**Fix:**

```bash
sudo firewall-cmd --add-service=nfs --zone=public --permanent
sudo firewall-cmd --reload

```

---

### **2️⃣ Error: “access denied by server”**

**Cause:** Wrong IP in `/etc/exports`

- Export used `10.1.30.150`
- Actual stage-web IP was `10.1.30.157`

**Fix:** Updated `/etc/exports` to correct IP and reloaded exports.

---

## **✅ Final Result**

- NFS share created
- Correct IP exported
- NFS mounted successfully from stage-web
- JDK file downloaded and verified
- Task fully completed

---

## **📸 Screenshots Required**

- `/etc/exports`
- `exportfs -v`
- Firewall services
- Mount point on stage-web
- JDK file visible in `/mnt/nfs-mgherzghir`

---

If you want, I can also create:

📌 A shorter version

📌 A version with emojis removed

📌 A version formatted like a corporate SOP

📌 A version in bullet-only layout

Just tell me!