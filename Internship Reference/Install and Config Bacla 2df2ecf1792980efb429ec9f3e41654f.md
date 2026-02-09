# Install and Config Bacla

**Description**

**[ TASK ] The infrastructure team needs to back up your VMs. Please install the Bacula Client and Libraries on both of your servers.**
**[ OBJECTIVE ]** 
Build confidence in data protection by setting up and restoring backups using Bacula`REQUIREMENTS:
1. Bacula Clients: dev-app ; stage-web ; dev-performance
2. Perform back-up Jobs in Bacula Server`

### 1. Building Bacula client on CentOS 9

**Bacula Client Build on CentOS 9 (Version 9.6.6)**

This guide provides a step-by-step approach to downgrading and building the Bacula File Daemon (Client) version 9.6.6 from source on CentOS 9. This is necessary due to compatibility issues between Bacula Director 9.6.6 and newer client versions such as 11.0.1, which are incompatible with the older protocol and configuration structure.

## **Why Downgrade Bacula Client on CentOS 9?**

By default, CentOS 9 installs Bacula Client 11.0.1, which introduces backward-incompatible changes such as:

- New directives not supported by Bacula Director 9.6.6
- Incompatible handshake protocols
- Updated job execution logic that the older Director can’t interpret

To ensure stable communication, both the client and Director must run the same version: 9.6.6.

## **Why Build from Source?**

CentOS 9 does not offer Bacula 9.6.6 in its official or EPEL repositories. Hence, the only reliable option is to compile the client manually from the source with client-only mode enabled.

# **Steps to Build Bacula 9.6.6 Client from Source**

**Step 1: Install Required Development Tools and Libraries**

```
sudo dnf groupinstall "Development Tools" -y

sudo dnf install gcc gcc-c++ make cmake autoconf libtool openssl-devel readline-devel ncurses-devel zlib-devel lzo lzo-devel sqlite-devel libacl-devel -y
```

**Step 2: Download Bacula 9.6.6 Source Code**

```
curl -LO https://sourceforge.net/projects/bacula/files/bacula/9.6.6/bacula-9.6.6.tar.gz

tar -xvzf bacula-9.6.6.tar.gz

cd bacula-9.6.6
```

**Step 3: Configure Build for Client Only**

```
./configure --enable-client-only
```

**Step 4: Compile and Install the Client**

```
make -j$(nproc)

sudo make install
```

**Step 5: Rebuild the bacula-fd systemd Service**

```
sudo vim /etc/systemd/system/bacula-fd.service
```

Insert the following service unit configuration:

```
[Unit]
Description=Bacula File Daemon (Client)
After=network.target

[Service]
Type=simple
ExecStart=/usr/sbin/bacula-fd -f
Restart=on-failure
User=root
Group=root

[Install]
WantedBy=multi-user.target
```

**Step 6: Enable and Start the Bacula File Daemon**

```
sudo systemctl daemon-reexec
sudo systemctl daemon-reload
sudo systemctl start bacula-fd
sudo systemctl enable --now bacula-fd

```

![image.png](Install%20and%20Config%20Bacla/image.png)

![image.png](Install%20and%20Config%20Bacla/image%201.png)

### 2. Perform back-up Jobs in Bacula Server

### Bacula client configuration

# **Bacula Client Configuration and Backup Job Setup Guide**

This guide provides a step-by-step procedure for configuring a Bacula client on Linux, connecting it to the Bacula server, and creating backup jobs using the Baculum web interface. Ensure that Bacula is already installed on the client before proceeding.

## **Prerequisite**

Bacula client must already be installed on the host.

## **Step 1: Update /etc/hosts with Bacula Server**

Edit the /etc/hosts file on the client and add the following entry:

```
10.1.30.43 prod-bacula.procore.prod1 prod-bacula
```

## **Step 2: Open Firewall for Bacula Client Service**

Run the following commands to allow Bacula client traffic:

```
firewall-cmd --add-service=bacula-client --permanent
firewall-cmd --add-port=9102/tcp --permanent
firewall-cmd --reload
```

## **Step 3: Configure bacula-fd and bconsole**

Edit Bacula file daemon configuration:

```
sudo vi /etc/bacula/bacula-fd.conf

→ Replace `<server director name>` with: prod-bacula
→ Set Password: "Welcome2VITI"
```

![image.png](Install%20and%20Config%20Bacla/image%202.png)

![image.png](Install%20and%20Config%20Bacla/image%203.png)

![image.png](Install%20and%20Config%20Bacla/image%204.png)

## **Step 4: Enable and Start bacula-fd Service**

Enable and start the Bacula File Daemon:

```
sudo systemctl restart bacula-fd
```

## **Step 5: Register the Client in Baculum**

- Open a web browser and navigate to: **http://10.1.30.43:9095**
- Log in using:
    - Username: admin
    - Password: Welcome2VITI
- From the right-side menu, click on ‘Clients’.

![image.png](Install%20and%20Config%20Bacla/image%205.png)

• Click ‘Add Client’ and fill out the required fields.

![image.png](Install%20and%20Config%20Bacla/image%206.png)

- Under ‘Pruning’, set JobRetention and FileRetention to 30 days.
- Click ‘Create’ at the bottom.
- After creation, click the ‘Details’ button for the new client.

![image.png](Install%20and%20Config%20Bacla/image%207.png)

## **Step 6: Create a Backup Job**

- In the Baculum menu, click ‘Jobs’.

![image.png](Install%20and%20Config%20Bacla/image%208.png)

- Click ‘New Job Wizard’.
    - Step 1: Configure the following:
        - Job Type: Backup
        - Job Name: [use hostname]
        - JobDefs: DefaultJob
    - Step 2: Select your client and use FileSet: Config-files.
    - Step 3 and 4: Leave defaults.
    - Step 5: Choose ‘Weekly Cycle’.
    - Step 6: Review the summary and click ‘Create Job’.

![image.png](Install%20and%20Config%20Bacla/image%209.png)

![image.png](Install%20and%20Config%20Bacla/image%2010.png)

![image.png](Install%20and%20Config%20Bacla/image%2011.png)

- To run the job:
    - Go to the ‘Jobs’ menu
    - Click ‘Run Job’
    - Select your job, client, and FileSet: Config-files
    - Click to execute

![image.png](Install%20and%20Config%20Bacla/image%2012.png)

![image.png](Install%20and%20Config%20Bacla/image%2013.png)