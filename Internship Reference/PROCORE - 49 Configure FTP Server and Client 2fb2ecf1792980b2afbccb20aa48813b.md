# PROCORE - 49. Configure FTP Server and Client

**Description**

**[ TASK ] Please configure the FTP server on dev-app-[initials].procore.prod1-IP and  the FTP client on stage-web-[initials].procore.prod1-IP**
**[ OBJECTIVE ]** 
Build practical networking skills by configuring and troubleshooting with FTP.`REQUIREMENTS:
1. FTP server: dev-app
2. FTP client: stage-web
3. Successful FTP download`
**[ INFORMATION ]** 
• Copy the file **/nfs/incoming/vhosts/ftp-files/ftp-prod.config** to your home directory on  **dev-app-[initials].procore.prod1-IP.**
• Then transfer the file using ftp to **stage-web-[initials].procore.prod1-IP**

### My Work:

## **Dev-app-mg (server FTP)**

- Install vsftpd on the server side (dev-app-mg)
- enable port 21 and add service ftp on the firewall
- enable and run the services vsftpd
- cp **ftp-prod.config** to /home/mgherzghir/

## **Stage-web-mg (FTP client)**

- installed ftp package
- connect ftp tp server side
    - ftp 10.1.30.150
    - typed in my username and password
- lcd to /home/mgherzghir/ and download **ftp-prod.config** from dev-app-mg server.
- exit

![image.png](PROCORE%20-%2049%20Configure%20FTP%20Server%20and%20Client/image.png)

![image.png](PROCORE%20-%2049%20Configure%20FTP%20Server%20and%20Client/image%201.png)