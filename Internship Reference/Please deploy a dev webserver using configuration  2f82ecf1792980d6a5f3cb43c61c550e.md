# Please deploy a dev webserver using configuration Template.

**Description**

**[ TASK ] The web development team requires you to deploy a development web server using the NEW-YT-DEV-WEBSERVER-TEMPLATE . Please update the necessary information on the newly deployed server.**
**[ OBJECTIVE ]** 
Enhance Linux system administration skills through virtualization using vSphere.`REQUIREMENTS:
1. VM Name: dev-performance-mg.procore.prod1
2. Network: Static connection
3. Package: ipa-client
4. DNS and Mount Configurations
5. Local user "procore" created and added to wheel group 
6. Asset Inventory Update`

**[ INFORMATION ]**

- Create New Virtual Machine using the above mentioned Template.
- Please change the host name to **dev-performance-[initials].procore.prod1**
- Create a static IP connection using the [IPAM sheet](https://docs.google.com/spreadsheets/d/1HXReAl4tGZah4yVf_5S1xeHo4HydeCRJ/edit#gid=1769532830)
- Configure IPA Client
- Configure your DNS and mount points configurations
    - Refer to Tickets 8,9, and 10
- Add local user “procore” with password “procoreplus” in your dev-performance
- Add user “procore” to the wheel group.
- Ensure VM is added to asset inventory tool.

### **Create new VM for app performance using the provided template**

1. Click on the host environment 10.1.30.90 and search for the template name.
2. Click on the template name action and select “new VM from this template.”
3. Create a local user, set the password requirements, and add the user to the wheel group.
4. Change the VM hostname and configure the static network.
5. Add the DNS local host from the previous ticket to ping one of the hostnames and configure the mount point.
6. Update the asset inventory.

![image.png](Please%20deploy%20a%20dev%20webserver%20using%20configuration%20/image.png)

![image.png](Please%20deploy%20a%20dev%20webserver%20using%20configuration%20/image%201.png)