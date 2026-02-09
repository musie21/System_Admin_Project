# Add the following info to the local DNS file.

**Description**

**[ TASK ] Please add the following information to the local DNS file on the dev-app server.**
**[ OBJECTIVE ]** 
Build practical networking skills by configuring with Linux.`REQUIREMENTS:
1. VM: dev-app server
2. DNS Info:
    10.1.15.5 vcenter.sandbox.prod
    10.1.15.13 ipa.procore.dev
    10.1.10.37 dev-nagios.procore.prod1
    10.1.30.24 stage-foreman.procore.prod
    10.1.30.43 stage-bacula.procore.prod1
    10.1.30.41 dev-ansible.procore.prod1 dev-ansible
    10.1.30.22 stage-bastion.procore.prod1 stage-bastion
    10.1.30.148 nfs-dev.procore.prod1
    10.1.30.51 stage-graylog.procore.prod`
**[ INFORMATION ]**
• Make sure to add these DNS info on all your future servers moving forward.

```bash
sudo vi /etc/hosts
```

![image.png](Add%20the%20following%20info%20to%20the%20local%20DNS%20file/image.png)