# How to Restrict Access speicfic host

# **Bastion Host Access Control Guide**

A bastion host is a specially configured server designed to provide secure access to a private network from an external network, such as the Internet. Due to its exposure, it is often the most targeted system in a network and must be hardened to reduce the chance of penetration. This guide outlines how to restrict SSH access through a bastion host on CentOS 9 systems using firewalls.

# **Steps to Restrict SSH Access on CentOS 9 (Using Firewall)**

OpenSSH version 7.4 and newer no longer supports TCP Wrappers (***/etc/hosts.allow*** and ***/etc/hosts.deny***). CentOS 9 uses OpenSSH 8.7, so SSH access control must be implemented using firewalld.

## **Step 1: Allow the Bastion Host IP**

Use a rich rule to allow SSH access from the bastion host’s IP address (10.1.30.22/23):

```
sudo firewall-cmd --permanent --add-rich-rule='rule family="ipv4" source address="10.1.30.22/23" port port="22" protocol="tcp" accept'
```

## **Step 2: Remove Default SSH Access Rules**

Remove general access to port 22 and SSH service:

```
sudo firewall-cmd --remove-port=22/tcp --permanent

sudo firewall-cmd --remove-service=ssh --permanent
```

**Step 3: Reload Firewall**

Apply the changes by reloading the firewall:

```
sudo firewall-cmd --reload
```

## **Best Practices**

- Always test configuration changes in a separate SSH session to avoid locking yourself out.
- Use strong authentication methods (e.g., SSH keys) and disable password-based login where possible.
- Regularly monitor SSH logs for unauthorized access attempts (`/var/log/secure` or `/var/log/auth.log`).
- Maintain an up-to-date inventory of all IP addresses with SSH access.

[firewalld-rich-rule-explained.md](How%20to%20Restrict%20Access%20speicfic%20host/firewalld-rich-rule-explained.md)