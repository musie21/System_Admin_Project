# How to add clients in Check-mk

### How to add clients in Check-mk

Checkmk is an advanced IT infrastructure monitoring system written in Python and C++. It provides comprehensive monitoring capabilities for servers, applications, networks, cloud environments, containers, databases, and more. This guide walks you through the process of adding a new client (host) to Checkmk.

# Prerequisites

Ensure the following steps are completed on the client-host before proceeding:

- Install required tools and the Checkmk agent:

```
sudo yum install wget
wget http://10.1.30.37/procore/check_mk/agents/check-mk-agent-2.3.0p2-1.noarch.rpm

sudo yum -y localinstall check-mk-agent-2.3.0p2-1.noarch.rpm
```

- Open port 6556 for Checkmk agent communication:

```
sudo firewall-cmd --zone public --add-port 6556/tcp --permanent
sudo firewall-cmd --reload
```

- Ensure that Checkmk agent socket systemd service is up and running

```
systemctl status check-mk-agent.socket
```

# Configuration Steps

- Access Checkmk Web Interface
    - Open a browser and navigate to: http://10.1.30.37/procore/check_mk
    - Login Credentials:
        - Username: procore-intran
        - Password: Welcome2VITI
- Navigate to the Hosts Section
    - Click on ‘Status’ then select ‘Hosts’.
    - Open the DEV Directory
    - Inside the Hosts view, select the ‘DEV’ directory
- Add New Host by clicking on ‘Add Host’
- 

![image.png](How%20to%20add%20clients%20in%20Check-mk/image.png)

- Configure Host Settings
    - In ‘BASIC SETTINGS’, enter the hostname.
    - In ‘NETWORK ADDRESS’, enter the IP address of the host.
    - In ‘MANAGEMENT BOARD’, keep default values

![](http://10.1.10.122/wp-content/uploads/2025/06/check_mk_add_host-1024x751.png)

- Click ‘Save & Go to Services’.
- Discover Services
    - Click ‘FULL SCAN’ to detect available services

![](http://10.1.10.122/wp-content/uploads/2025/06/check_mk_discover_services-1024x134.png)

- Activate Changes
    - Return to the main menu, click ‘Changes’, then ‘Activate Changes’.

![](http://10.1.10.122/wp-content/uploads/2025/06/check_mk_changes-1024x626.png)

![](http://10.1.10.122/wp-content/uploads/2025/06/chaeck_mk_activate_changes-1024x268.png)

- Verify Host
    - Navigate to ‘Views’ > ‘All hosts’ to confirm the host is listed and being monitored.

For further details and advanced configuration, visit the official