# Network setup request

**Description**

**[ TASK ] Please refer to the IP Address Management  ([IPAM sheet](https://docs.google.com/spreadsheets/d/1HXReAl4tGZah4yVf_5S1xeHo4HydeCRJ/edit#gid=1769532830)) to locate the necessary network information. Use this information to establish a static connection for your dev-app server.**
**[ OBJECTIVE ]** 
Strengthen networking fundamentals by configuring connectivity for Linux servers.
**[ INFORMATION ]** `REQUIREMENTS:
1. VM: dev-app server
2. IP: 10.1.3x.xxx
3. Gateway: 10.1.30.1
4. Netmask: 255.255.254.0
5. DNS1: 10.1.15.13
6. Local user -> procore
7. Password -> procoreplus
8. procore -> member of wheel group
9. Asset Inventory Update`
• Please look for your IP addresses assigned to your servers.
    ◦ [IPAM sheet](https://docs.google.com/spreadsheets/d/1HXReAl4tGZah4yVf_5S1xeHo4HydeCRJ/edit#gid=1769532830)
• Please add your IP address and hostname to the ticket. 
• Create user procore with password procoreplus
• Add procore to the wheel secondary group
• Update Asset Inventory Tool

```bash
#Added procore to local system
useradd procore

#Add the user procore to wheel group

usermod -G wheel procore

#Setting the network is first check the network device

nmcli con show 

#once i identitfied the network device name
#create static network
nmcli con add con-name static ifname ensp1 type ethernet autoconnect yes ip4 10.1.30.150/23 gw4 10.1.30.1 ipv4.dns 10.1.15.13

```

![image.png](Network%20setup%20request/image.png)