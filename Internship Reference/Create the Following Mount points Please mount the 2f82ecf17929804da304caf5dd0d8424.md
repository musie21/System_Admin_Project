# Create the Following Mount points/ Please mount the following NFS shares permanently

## Create Mount point

# **[ TASK ] Please create the following mount points for the upcoming nfs share on the dev-app server.**

---

**[ OBJECTIVE ]**

Build expertise in Linux storage operations by managing mounts and shares with Linux.

**[ INFORMATION ]**

```bash
REQUIREMENTS:
1. VM: dev-app server
2. Mount points:
      /nfs/incoming/home
      /nfs/incoming/vhosts
      /nfs/incoming/scripts
      
      
#Create the parent Directory

mkdir -p /nfs/incoming/home

mkdir -p /nfs/incoming/vhosts

mkdir -p /nfs/incoming/scripts					
```

![image.png](Create%20the%20Following%20Mount%20points%20Please%20mount%20the/image.png)

## Mount NFS shared Dir

**Description**

**[ TASK ] Please ensure the following NFS shares are mounted permanently on the dev-app server.**
**[ OBJECTIVE ]** 
Build expertise in Linux storage operations by managing mounts and shares with Linux.
**[ INFORMATION ]** `REQUIREMENTS:
1. VM: dev-app server
2. Mount info:
      10.1.30.148:/nfs/share/vhosts/ /nfs/incoming/vhosts    
      10.1.30.148:/nfs/share/home/   /nfs/incoming/home 
      10.1.30.148:/nfs/share/scripts/ /nfs/incoming/scripts`

- start nfs-server
- I used this syntax to temporally look into my mountpoint type mount -t 10.1.30.148:/nfs/share/home /nfs/incoming/home in order to type in the correct file system in fstab.
- lastly verfly with using mount -a after apply all mountpoint on fstab and no error has been found.
- Here are both screenshot for this ticket.

![image.png](Create%20the%20Following%20Mount%20points%20Please%20mount%20the/image%201.png)

To Verify if its mounted on 

![image.png](Create%20the%20Following%20Mount%20points%20Please%20mount%20the/image%202.png)

![image.png](Create%20the%20Following%20Mount%20points%20Please%20mount%20the/image%203.png)