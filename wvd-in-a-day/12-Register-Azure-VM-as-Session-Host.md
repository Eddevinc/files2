# Exercise 12: Register Azure VM as Session Host

In the following exercise, we will be creating a virtual machine which will automatically domain join by running a script in Cloud shell.
After Deployment of virtual machine we will establish a RDP connection to the virtual machine and register the virtual machine as a session host under *WVD-HP-01* hostpool.


### **Task 1: Create a Vm using Cloud Shell**

1. **Copy** the following script.

       $domain = ((Get-AzADUser | where {$_.Type -eq "Member"}).UserPrincipalName.Split('@'))[1]
       $password= ConvertTo-SecureString "Azure1234567" -AsPlainText -Force
       $adminUserName = "domainjoinadmin@$domain"
       $adminPassword = "Azure1234567"

       #Deploy Template
       $UserPasswordhash = ConvertTo-SecureString $adminPassword -AsPlainText -Force
       New-AzResourceGroupDeployment -ResourceGroupName "WVD-RG" `
       -TemplateUri "https://experienceazure.blob.core.windows.net/templates/wvd/deployVM.json" `
       -existingVNETName "aadds-vnet" -existingSubnetName "sessionhosts-subnet" -adminUsername $adminUserName -adminPassword $UserPasswordhash

       
>**Note:** The above script will be used to create a virtual machine.


2. In Azure portal click on the **Cloud Shell button** on top and wait for the cloud shell to connect.

   ![ws name.](media/a105.png)


3. Paste the script in the cloud shell and press **Enter** to run the script.

   ![ws name.](media/wvd54.png)
   
   >**Note:** Wait for sometime for the script to execute.
   
4. After the execution completes the output will look as following.

   ![ws name.](media/wvd55.png)

   >**Note:** Wait for few more minutes for the deployment of virtual machine to complete.


### **Task 2: Install Agents on VM and Register**

In this task we will be establish a RDP connection with the virtual machine created in previous task and download two agents:

  - Windows Virtual Desktop Agent
  - Windows Virtual Desktop Agent Bootloader
  
These two agents will be used to register this virtual machine a part of session hosts of WVD-HP-01 hostpool.

1. In search bar of your Azure portal search for virtual and select **virtual machines** from the suggestions.

   ![ws name.](media/a67.png)
   
2. Open **WVD-VM-01** virtual machine,then click on **Connect** and select **RDP**.

   ![ws name.](media/a81.png)
   
  
3. Click on **Download RDP File**.

   ![ws name.](media/a82.png)
   
   >**Note:** A file named **WVD-VM-01.rdp** will download.
  
4. Click on the downloaded file to open.

   ![ws name.](media/197.png)
   
   
5. RDP window will open, click on **Connect**.

   ![ws name.](media/a89.png)
   
   
6. Click on **More choices**.

   ![ws name.](media/a100.png)
   
   
7. Click on **Use a different account**.

   ![ws name.](media/a101.png)
   
   
8. Enter your credentials.
   
     - Username: **domainjoinadmin**   
     - Password: **Azure1234567**
     - Click on **OK**.
   
   ![ws name.](media/a92.png)

   
9. A new pop up window will open,click on **Yes**.
 
   ![ws name.](media/202.png)
    
   >**Note:** RDP Connection with your VM will be established.
    
    
10. In the Virtual Machine window click on **Accept**.
 
    ![ws name.](media/203.png)
   
11. In your VM desktop double click on **Microsoft edge** icon to open it.
 
    ![ws name.](media/204.png)
   
12. **Copy** and **Paste** the following URL in your VM browser and **hit enter** to download **Windows Virtual Desktop Agent**.
 
        https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RWrmXv
 
    ![ws name.](media/205.png)
    
   
13. There will be a popup in the bottom of the browser asking to - **Run**, **Save** or **Cancel** the downloaded setup. Therefore choose **Run**.
 
    ![ws name.](media/a114.png)

14. Click on **Next** when the installer opens. 

    ![ws name.](media/207.png)
        
15. Check the box saying **I accept the terms in the License Agreement** and click on **Next**.

    ![ws name.](media/208.png)
    
16. Now minimise your VM RDP window and go back to Azure portal on your local machine.


17. In Azure portal search for **Host Pools** and click on it.

    ![ws name.](media/a93.png)
   
18. Click on **WVD-HP-01** and then click on **Registration Key**.
 
    ![ws name.](media/a94.png)
   
19. Copy the registration key by clicking on the **copy button** on the right corner.

    ![ws name.](media/a95.png)
    
    >**Note:** This unique registration key will be enable the Virtual Machine to become session host under this particular WVD-HP-01 hostpool.
    
20. Go back to the VM RDP window, and click inside the box opened in the installer.

    ![ws name.](media/213.png)
      

21. On your keyboard press the following combination of keys.

    **Backspace**: To remove the current text in the box.
    
    **Ctrl + V**: To paste the key inside the box which you copied from WVD-HP-01 host pool.
    
22. Click on **Next**.

    ![ws name.](media/214.png)
     
23. Click on **Install**.

    ![ws name.](media/215.png)
    
24. Then click on **Finish**.

    ![ws name.](media/216.png)   
    
25. Open your browser and **paste** the following URL in your browser and hit **enter** to download the  **Windows Virtual Desktop Agent Bootloader**.

    ```https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RWrxrH ```      

   ![ws name.](media/217.png)
 
26. There will be a popup in the bottom of the browser asking to - **Run**, **Save** or **Cancel** the downloaded setup. Therefore choose **Run**.

    ![ws name.](media/218.png)
    
27. Click on **Next** when the installer opens.

    ![ws name.](media/219.png)
   
28. Check the box saying **I accept the terms in the License Agreement** and click on **Next**.

    ![ws name.](media/220.png)
   
29. Click on **Install**.

    ![ws name.](media/221.png)
    
    
30. Then click on **Finish**.

    ![ws name.](media/222.png)
    
  
  
### **Task 3: Verify registration on Host Pool**


1. In your local machine visit Azure portal and search for **Host Pools** and click on it.

   ![ws name.](media/w5.png)



2. Click on **WVD-HP-01**.

   ![ws name.](media/224.png)
    
    
3. Under manage blade click on **Session hosts**.

   ![ws name.](media/225.png)
    
    
  
4. Verify that **WVD-VM-01** is added to the **WVD-HP-01 hostpool**.

   ![ws name.](media/226.png) 

5. Click on the **Next** button.






