# Exercise 4: Create Session Hosts

### **Task 1: Deploy 2 Windows 10 Enterprise multi-session Session Hosts to ‘Pooled’ Host Pool**

1. Navigate to Azure portal, then search for **Windows** in search bar and select **Windows Virtual Desktop**.

   ![ws name.](media/y.png)
     
2. Click on **Host pools** and select **WVD-HP-01** to open it.

   ![ws name.](media/a5.png)
     
3. Now click on **Session hosts** present under **Manage** blade and then click on **+Add**.

   ![ws name.](media/a6.png)
    
4. In the Session host creation page, leave the value at default and click on **Next: Virtual Macines**.

   ![ws name.](media/a7.png)
  
5. Configure the session hosts with following configuration.

   **A**. Session Host Specifications

     In this section, we provide the details of the VMs to be created as session Hosts.    

     - Resource Group: Choose the exiting Resource Group that is **WVD-RG**.

     - Virtual machine location: Choose the location of the exiting Resource Group.

     - Virtual machine size: **Standard DS1_V2**. [Click on **Change Size**, then select **DS1_V2** and click on **Select** as shown below]
   
     ![ws name.](media/wvd35.png)

     - Number of VMs: **2**
   
     - Name prefix: **WVD-HP01-SH** 

     - Image type: **Gallery**

     - Image: **Windows 10 Enterprise multi-session, version 1909 + Office 365 ProPlus**(choose from dropdown) 

     - OS disk type: **Standard SSD**

     - Use managed disks: **Yes**
   
     ![ws name.](media/a8.png)
     
   
  **B**. Network and Security 
   - Subnet: **sessionhosts-subnet (10.0.1.0/24)**
     
   - Leave all other values on default.
 
 **C**. Domain and Administrator account 
 
   - Specify Domain or Unit: **No**

   - AD domain join UPN: Paste username of **DomainJoinAdminUser**,for example: DomainJoinAdminUser@azurehol1055.onmicrosoft.com.

   - Password: **Azure1234567**

   - Confirm Password: **Azure1234567**
   
   - Click on **Review + create**

   ![ws name.](media/a9.png)
   
   
6. Click on **Create**.

   ![ws name.](media/a10.png)
  
### **Task 2: Deploy a Windows 10 Enterprise Session Hosts to ‘Personal’ Host Pool**

1. Now go back to **Host pools** and select **WVD-HP-02** to open it.

   ![ws name.](media/a11.png)
  
2. Now click on **Session hosts** present under **Manage** blade and then click on **+Add**.

   ![ws name.](media/a12.png)
    
3. In the Session host creation page, leave the value at default and click on **Next: Virtual Macines**.

   ![ws name.](media/a13.png)
 
4. Configure the session hosts with following configuration.

   **A**. Session Host Specifications

    In this section, we provide the details of the VMs to be created as session Hosts. 
   
     - Resource Group: Choose the exiting Resource Group that is **WVD-RG**.

     - Virtual machine location: Choose the location of the exiting Resource Group.

    - Virtual machine size: **Standard DS1_V2**. [Click on **Change Size**, then select **DS1_V2** and click on **Select** as shown below]
   
    ![ws name.](media/wvd35.png)
      
     - Number of VMs: **1** 
   
     - Name prefix: **WVD-HP02-SH**

     - Image type: **Gallery**

     - Image: **Windows 10 Enterprise, version 1909(choose from dropdown)** 

     - OS disk type: **Standard SSD**

     - Use managed disks: **Yes** 
   
     ![ws name.](media/a14.png)

  **B**. Network and Security 
   - Subnet: **sessionhosts-subnet (10.0.1.0/24)**
     
   - Leave all other values on default.
    
  **C**. Domain and Administrator account 

   - Specify Domain or Unit: **No** 

   - AD domain join UPN: Paste username of **DomainJoinAdminUser**,for example: DomainJoinAdminUser@azurehol1055.onmicrosoft.com.

   - Password: **Azure1234567**

   - Confirm Password: **Azure1234567**
   
   - Click on **Review + create**
    
   ![ws name.](media/a15.png)
  
8. Click on **Create**.

   ![ws name.](media/a16.png)

9. Click on the **Next** button.
