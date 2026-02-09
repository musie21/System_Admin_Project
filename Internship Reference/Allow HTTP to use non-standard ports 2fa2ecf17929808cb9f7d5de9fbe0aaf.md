# Allow HTTP  to use non-standard ports.

**Description**

**[ TASK ] The network and security teams are requesting that your dev-performance web server listens on a non-standard port (8001). Please configure your server to meet this requirement and provide the link for testing.**
**[ OBJECTIVE ]** 
Build practical networking skills by configuring and troubleshooting with Linux.`REQUIREMENTS:
1. VM: dev-performance server
2. Link for testing 8001 port`

**SELinux** enforces policies that restrict services to run only on a predefined set of well-known ports. For the `httpd` service, these ports include **80, 443, 488, 8008, 8009**, and **8443**. If you need to configure `httpd` to listen on a non-standard port, you must update the **SELinux** policy accordingly by following the proper procedure.

Make sure to provide link for testing the 8001 port.

### My work:

1. Verify Apache installation.
2. Start and enable the service.
3. Update the Apache config file to listen on port 8001:
    - `sudo vi /etc/httpd/conf/httpd.conf`
    - Replace “Listen 80” with “Listen 8001”.
4. Check the SELinux policy:
    - `sudo semanage port --list | grep http`
    - Notice that port 8001 is not added; add port 8001:
        - `semanage -p -a -t http_port_t -p tcp 8001`.
5. Open Chrome and enter the link to verify functionality:
    - `http://10.1.30.152:8001`
    - Confirm it works.
    - 
    
    ![image.png](Allow%20HTTP%20to%20use%20non-standard%20ports/image.png)
    

![image.png](Allow%20HTTP%20to%20use%20non-standard%20ports/image%201.png)

![image.png](Allow%20HTTP%20to%20use%20non-standard%20ports/image%202.png)