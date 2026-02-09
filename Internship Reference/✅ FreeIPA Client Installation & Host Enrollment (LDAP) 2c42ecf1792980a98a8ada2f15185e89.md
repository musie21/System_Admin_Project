<details>
<summary>📘 How to deploy a VM </summary>

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

![image.png](attachment:e5f80119-0b02-4f93-a781-13674f594a8d:image.png)

[20251025-2033-41.3452115.mp4](attachment:ec389310-043f-4a31-ac69-3cdd526b3695:20251025-2033-41.3452115.mp4)

Based on the pic and video this is how you deploy a VM

![image.png](attachment:df14d5b2-329a-4180-8f59-8391c5baf94c:image.png)

![image.png](attachment:b6a6ee8a-e7b9-459c-837c-83b92e37f48d:image.png)

**Step 2: Migrate VMs to Your Resource Pool**

- Right-click on the VM you wish to move and select ‘Migrate’.
- On the migration wizard, select the first option: ‘Change compute resource only’, and click ‘Next’.
- At the top of the next screen, switch the view to ‘Resource Pools’.
- Locate and select your newly created resource pool.
- Click ‘Next’ and proceed through the next three screens using the default options.
- Click ‘Finish’ to complete the migration.

</details>