# Depoly a new Vmare/Vshpare and Install FreeIPA Client

## How to deploy a VM

How to  **Deploy a new CentOS 9 Virtual Machine on Vsphere**

```bash
REQUIREMENTS:

1. VM Name: dev-app-mg.procore.prod1
2. Host: 10.1.10.90
3. Storage: DS-01
4. CPU: 1 vCPU
5. Memory: 1 GB
6. Hard Disk: 20 GB (Make sure you set up Thin Provisioned)
7. Network Adapter 1: YT-Intran-vlan
8. ISO: /DS-01/ISO Images/CentOS-Stream-9-latest-x86_64-dvd1.iso
9. Asset Inventory Update
```

![image.png](Depoly%20a%20new%20Vmare%20Vshpare%20and%20Install%20FreeIPA%20Cli/image.png)

[20251025-2033-41.3452115.mp4](Depoly%20a%20new%20Vmare%20Vshpare%20and%20Install%20FreeIPA%20Cli/20251025-2033-41.3452115.mp4)

Based on the pic and video this is how you deploy a VM

![image.png](Depoly%20a%20new%20Vmare%20Vshpare%20and%20Install%20FreeIPA%20Cli/image%201.png)

![image.png](Depoly%20a%20new%20Vmare%20Vshpare%20and%20Install%20FreeIPA%20Cli/image%202.png)

**Step 2: Migrate VMs to Your Resource Pool**

- Right-click on the VM you wish to move and select ‘Migrate’.
- On the migration wizard, select the first option: ‘Change compute resource only’, and click ‘Next’.
- At the top of the next screen, switch the view to ‘Resource Pools’.
- Locate and select your newly created resource pool.
- Click ‘Next’ and proceed through the next three screens using the default options.
- Click ‘Finish’ to complete the migration.

## **How Install and Configure FreeIPA client**

### Installing FreeIPA client and host enrollment

The FreeIPA Directory Service is built on the 389 DS LDAP server and serves as the foundational component of a comprehensive Identity Management solution. It supports identity, authentication (Kerberos), authorization services, and additional policy enforcement.

# Steps to Install and Configure the IPA Client

## Step 1: Install the FreeIPA Client Package

Run the following command to install the FreeIPA client on your target server:

```
# yum install ipa-client -y
```

## Step 2: Run the IPA Client Installation Command

```
# ipa-client-install --mkhomedirid 
```

You will be prompted to provide the following information:

- Domain: procore.dev
- Server: ipa.procore.dev

A registered IPA username and password will also be required.

## Step 3: Test the Installation

You can verify that the client is correctly configured using either of the following methods:

- Method 1: Using the id command

```bash
# id <ipa-username>
#Example 

id mgherzghir

#If you receive the error 'kerberos authentication failed: kinit: password incorrect while getting initial credentials', then run:

# kinit <ipa-username>

kinit mgherzghir

username: mgherzghir #Make sure your IPAFree credentails username
password: ********** #Same pasword of your freeipa.

#Then retry the id command.
```

- Method 2: Using ipa user-show command

```bash
# ipa user-show <ipa-username>
#Example 

ipa user-show mgherzghir

#If you receive the error 'kerberos authentication failed: kinit: password incorrect while getting initial credentials', then run:

# kinit <ipa-username>

#Then retry the ipa user-show command.
```

# Troubleshooting IPA Client Installation

If you encounter issues during the IPA client configuration, especially errors related to domain joining or authentication, try forcing the client to join the IPA domain:

```
# ipa-client-install --mkhomedir --force-join
```

For more advanced diagnostics, consider reviewing logs at /var/log/ipaclient-install.log and ensure that time synchronization (NTP) is functioning correctly.

## Once Configure FreeIPA Client, Add user now

**Description**

**[ TASK ] The developer's team recently hired David Duncan. Please create a username dduncan for the new user and add them to the group "webmasters." Additionally, set up a temporary password for the user and ensure this information is documented in the ticket.**

- Created user dduncan by referencing the ipa man page for the correct syntax.
- Added group member webmasters to user dduncan.
- Set the password for user dduncan: dduncan$$.
- Encountered no roadblocks; the ipa man page helped me find the necessary syntax for creating a username, group, and password.
    - Here are both screenshot one is for passwd change and second is profile of dduncan.

![image.png](Depoly%20a%20new%20Vmare%20Vshpare%20and%20Install%20FreeIPA%20Cli/image%203.png)

![image.png](Depoly%20a%20new%20Vmare%20Vshpare%20and%20Install%20FreeIPA%20Cli/image%204.png)

### Add duncan into specific group

![image.png](Depoly%20a%20new%20Vmare%20Vshpare%20and%20Install%20FreeIPA%20Cli/image%205.png)