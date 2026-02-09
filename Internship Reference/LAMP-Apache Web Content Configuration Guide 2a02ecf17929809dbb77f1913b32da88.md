# LAMP-Apache Web Content Configuration Guide

## 🧱 Apache Web Content Configuration Guide (CentOS Stream 9)

### 🧩 Objective

Host a web application from a **GitLab repository** on a CentOS 9 server (`stage-web`), using Apache (`httpd`) to serve the content publicly through a defined hostname or IP.

---

## 🧭 Step-by-Step Process

### 1. **Install and start Apache**

Apache is the web service that listens on port 80 (HTTP) and serves web pages to users.

```bash
sudo dnf install httpd -y
sudo systemctl enable --now httpd
sudo systemctl status httpd

```

**Explanation:**

- `dnf install httpd` → installs Apache
- `systemctl enable --now httpd` → enables and starts the service
- `status` → verifies Apache is running

---

### 2. **Clone your GitLab repository**

Your web files live inside a GitLab repository. To pull them down:

```bash
cd /var/www/html/
git clone git@gitlab.com:procoreplusmd/ariclaw.git

```

**If you get a “Permission denied (publickey)” error:**

- Generate an SSH key on your VM:
    
    ```bash
    ssh-keygen -t ed25519 -C "mgherzghir@procore.prod1"
    cat ~/.ssh/id_ed25519.pub
    
    ```
    
- Add that public key to your GitLab profile (under **Preferences → SSH Keys**).
- Test the connection:
    
    ```bash
    ssh -T git@gitlab.com
    
    ```
    
    You should see:
    
    `Welcome to GitLab, @Milkias-312!`
    

---

### 3. **Fix folder permissions (if needed)**

If your user cannot write to `/var/www/html`, change ownership temporarily:

```bash
sudo chown -R mgherzghir /var/www/html/

```

Then re-clone the repo.

After cloning:

```bash
sudo chown -R apache:apache /var/www/html/ariclaw

```

This gives Apache permission to read the files.

---

### 4. **Understand how Apache serves content**

- The **default web root** is `/var/www/html/`
- Apache reads the **main config file**: `/etc/httpd/conf/httpd.conf`
- At the bottom of that file, you’ll find:
    
    ```
    IncludeOptional conf.d/*.conf
    
    ```
    
    This tells Apache to load **any `.conf` files** in `/etc/httpd/conf.d/`
    

That’s where we’ll define your website.

---

### 5. **Create a VirtualHost configuration**

Each website gets its own `.conf` file under `/etc/httpd/conf.d/`.

```bash
sudo vim /etc/httpd/conf.d/ariclaw.conf

```

Add the following:

```
<VirtualHost *:80>
    ServerName stage-web-mg.procore.prod1
    DocumentRoot /var/www/html/ariclaw

    <Directory /var/www/html/ariclaw>
        AllowOverride All
        Require all granted
    </Directory>

    ErrorLog /var/log/httpd/ariclaw_error.log
    CustomLog /var/log/httpd/ariclaw_access.log combined
</VirtualHost>

```

**Explanation:**

- `<VirtualHost *:80>` → listens on port 80 (HTTP)
- `ServerName` → hostname to match requests (e.g., `stage-web-mg.procore.prod1`)
- `DocumentRoot` → path to website files
- `<Directory>` block → grants access permissions for that folder
- `ErrorLog` & `CustomLog` → where logs for this specific site are stored

---

### 6. **Restart Apache and verify**

Restart the service so it reads your new configuration:

```bash
sudo systemctl restart httpd
sudo systemctl status httpd

```

If it shows “active (running),” open your browser:

```
http://stage-web-mg.procore.prod1

```

or use the server’s IP:

```
http://<your_server_IP>

```

---

### 7. **(Optional) Adjust SELinux and firewall**

If the page doesn’t load:

### Allow HTTP traffic through the firewall:

```bash
sudo firewall-cmd --permanent --add-service=http
sudo firewall-cmd --reload

```

### Adjust SELinux for non-default directories:

```bash
sudo chcon -R -t httpd_sys_content_t /var/www/html/ariclaw

```

---

## 🧠 Summary of Key Concepts

| Concept | Description |
| --- | --- |
| **Apache (httpd)** | The web server software that delivers HTML pages over HTTP. |
| **DocumentRoot** | Directory Apache serves content from. Default is `/var/www/html`. |
| **VirtualHost** | Configuration block that defines a website (hostname, port, and directory). |
| **conf.d/** | Folder that holds custom `.conf` files Apache automatically loads. |
| **SELinux context** | Security labels controlling Apache’s access to files. |
| **FirewallD** | Manages network access and must allow port 80 for web access. |

---

### ✅ End Result

- You can access your cloned GitLab website from the browser.
- You understand how Apache reads `.conf` files and serves directories.
- You can now host multiple sites easily by creating more `.conf` files under `/etc/httpd/conf.d/`.

---

Would you like me to turn this guide into a downloadable `.pdf` or `.docx` version so you can keep it as your personal reference document?