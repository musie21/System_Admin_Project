# Please create a shared directory using your username.

**Description**

**[ TASK ] Since you will be using SSH in accessing multiple servers, please create a shared directory using your username.** 
**[ OBJECTIVE ]** 
Build expertise in Linux storage operations by managing mounts and shares with Linux.
**[ INFORMATION ]** `REQUIREMENTS:
1. Shared Directory Path: /nfs/incoming/home/{freeipa_username} 
      Example: /nfs/incoming/home/tlouis
2. Permission: 700`

- Created home dir under /nfs/icoming/home/mgherzghir
- changed the permission level for user to have read,write and execute
    - sudo chmod -R 700 /nfs/icoming/home/mgherzghir
- Next change the owner and group to the user mgherzghir
    - sudo chown -R mgherzghir:mgherzghir /nfs/incoming/home/mgherzghir

![image.png](Please%20create%20a%20shared%20directory%20using%20your%20userna/image.png)