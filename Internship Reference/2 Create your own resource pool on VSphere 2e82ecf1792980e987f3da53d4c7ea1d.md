# 2. Create your own resource pool on VSphere

# **[ TASK ] To efficiently manage and allocate organization’s resources, we need to create resource clusters for our deployed and upcoming VMs.**

---

**[ OBJECTIVE ]**

Understand how to organize and allocate computing resources for efficient VM management.

```
REQUIREMENTS:
1. Resource Pool for your virtual machines
```

**[ INFORMATION ]**

- Please create your resource pool on vsphere.
- Follow the steps on the [Vsphere resource pool wiki](http://10.1.10.122/?docs=procore-wike/virtualization/how-to-create-resource-pool-in-vsphere).

**[ DONT FORGET!!! ]**

- Proper documentation is mandatory.
- Provide screenshots of the work done.
- Describe the task outlined in every Jira ticket, identify any challenges or roadblocks you encountered, and detail the steps you took to resolve the issue in a video or audio format.

## How to Create a Resource Pool in vSphere

### Creating and Managing vSphere Resource Pools

![https://i0.wp.com/vmtoday.com/wp-content/uploads/sites/11/2012/03/vSphere-Resource-Pool-Shares-CPU-Resource-Allocation.png?ssl=1](https://i0.wp.com/vmtoday.com/wp-content/uploads/sites/11/2012/03/vSphere-Resource-Pool-Shares-CPU-Resource-Allocation.png?ssl=1)

![https://geek-university.com/wp-content/images/vmware-esxi/create_resource_pool.jpg](https://geek-university.com/wp-content/images/vmware-esxi/create_resource_pool.jpg)

4

![https://www.ubackup.com/screenshot/en/acbn/others/vmware-migrate-vm-to-another-vcenter/xvmotion/change-both-compute-resource-and-storage.png](https://www.ubackup.com/screenshot/en/acbn/others/vmware-migrate-vm-to-another-vcenter/xvmotion/change-both-compute-resource-and-storage.png)

VMware vSphere is a powerful virtualization platform that includes products and technologies for building and managing a virtualized IT infrastructure. This guide walks through the steps required to create a vSphere resource pool and migrate virtual machines (VMs) into it for optimized compute resource management.

---

### Step 1: Create a vSphere Resource Pool

1. Open the vSphere Client and navigate to your active **Data Center**.
2. Right-click the **Data Center** object and select **New Resource Pool**.
3. Name the new resource pool using the following naming convention:
    
    ```
    USERNAME-CLUSTER
    
    ```
    
4. Leave the default configuration settings unchanged.
5. Click **OK** to create the resource pool.
6. Verify that the newly created resource pool appears in the left-side navigation pane.

---

### Step 2: Migrate VMs to the Resource Pool

1. Right-click the virtual machine you want to move and select **Migrate**.
2. In the migration wizard, select **Change compute resource only**, then click **Next**.
3. At the top of the next screen, switch the view to **Resource Pools**.
4. Locate and select the newly created resource pool.
5. Click **Next** and continue through the remaining screens using the default options.
6. Click **Finish** to complete the migration.

---

## Steps Performed

1. Followed the instructions listed on the Wiki page:
    
    **How to create a resource pool in vSphere**
    
2. Opened the vSphere Client and highlighted **Data Center 10.1.10.90**.
3. Right-clicked the Data Center and selected **New Resource Pool**.
4. Named the resource pool **Mgherzghir-CLUSTER** and clicked **OK**.
5. Selected the VM created for **Ticket 1**, right-clicked it, and chose **Migrate**.
6. Selected **Change compute resource only** and clicked **Next**.
7. Switched to the **Resource Pools** tab, selected the created resource pool, and clicked **Next**.
8. Selected **YT-Intran-VLAN**, continued with the default options, and clicked **Finish**.

---