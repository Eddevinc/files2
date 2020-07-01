# Exercise 12: Register Azure VM as Session Host

In the following exercise, we will be creating a virtual machine which will automatically domain join by running a script in cloud shell.
After Deployment of virtual machine we will establish a RDP connection to the virtual machine and register the virtual machine as a session host under *WVD-HP-01* hostpool.


## Task 1: Create a Vm using Cloud Shell



1 **Copy** the following script.


    $domain = ((Get-AzADUser | where {$_.Type -eq "Member"}).UserPrincipalName.Split('@'))[1]
    $password= ConvertTo-SecureString "Azure1234567" -AsPlainText -Force
    $adminUserName = "domainjoinadmin@$domain"
    $adminPassword = "Azure1234567"

    #Deploy Template
    $UserPasswordhash = ConvertTo-SecureString $adminPassword -AsPlainText -Force
    New-AzResourceGroupDeployment -ResourceGroupName "WVD-RG" `
    -TemplateUri "https://akipersistantstg.blob.core.windows.net/wvdinaday/deployVM.json" `
    -existingVNETName "aadds-vnet" -existingSubnetName "sessionhosts-subnet" -adminUsername $adminUserName -adminPassword $UserPasswordhash
       
       
 > **Note:** The following script will be used to create a virtual machine.


2. In Azure portal click on the **cloud shell button** on top and wait for the cloud shell to connect.

   ![ws name.](media/189.png)


3. Paste the script in the cloud shell and press **Enter** to run the script.

   ![ws name.](media/wvd54.png)
   
   wait for sometime for the script to execute.
   
   
   
4. After the execution completes the output will look as following.

    ![ws name.](media/wvd55.png)

    wait for few more minutes for the deployment of virtual machine to complete.


## Task 2: Install Agents on VM and Register

In this task we will be establish a RDP connection with the virtual machine created in previous task and download two agents:
1. Windows Virtual Desktop Agent
2. Windows Virtual Desktop Agent Bootloader
These two agents will be used to register this virtual machine a part of session hosts of WVD-HP-01 hostpool.

1. In search bar of your Azure portal search for *virtual machines* and click on it.

   ![ws name.](media/192.png)
   
   
   
2. Click on **WVD-VM-01**.

   ![ws name.](media/193.png)
   
   
   
3. Click on **Connect**.

   ![ws name.](media/194.png)
   
   

4. Select **RDP**.

   ![ws name.](media/195.png)
   
   
5. Click on **Download RDP File**.

   ![ws name.](media/196.png)
   
   A file named ***WVD-VM-01*** will download.
   
   
   
6. Click on the downloaded file to open.

   ![ws name.](media/197.png)
   
   
7. RDP window will open, click on **Connect**.

   ![ws name.](media/198.png)
   
   
7. Click on **More choices**.

   ![ws name.](media/199.png)
   
   
8. Click on **Use a different account**.

   ![ws name.](media/200.png)
   
   
9. Enter your credentials.

   ![ws name.](media/201.png)
   
   
   **Username**: *domainjoinadmin*
   
   **Password**: *Azure1234567*
   
   Click on **OK**.
   
   
   
10. A new pop up window will open,click on **Yes**.
 
    ![ws name.](media/202.png)
    
    A RDP Connection with your VM will be established.
    
    
 11. In your Vm window click on **Accept**.
 
     ![ws name.](media/203.png)
    
    
    
12. In your VM desktop double click on **Microsoft edge** icon to open it.
 
    ![ws name.](media/204.png)
   
   



13. **Copy** and **Paste** the following URL in your VM browser and **hit enter** to download ***Windows Virtual Desktop Agent***.
 
        https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RWrmXv
 
    ![ws name.](media/205.png)
    
    
    
14. Below your browser a popup will be displayed, Click on **Run**.
 
     ![ws name.](media/206.png)



15. A installer will open, Click on **Next**.

    ![ws name.](media/207.png)
    
    
    
16. Click on the **check Box** saying *I accepting the terms in the License Agreement* and click on **Next**.

    ![ws name.](media/208.png)
    
    
    
17. Now minimise your VM RDP window and visit azure portal on your local machine.


18. In Azure portal search for *host pools* and click on it.

    ![ws name.](media/209.png)
    
    
    
19. Click on **WVD-HP-01**.
 
    ![ws name.](media/210.png)
     
     
     
20. Click on **Registration Key**.

    ![ws name.](media/211.png)
    
    
    
21. Copy the registration key by clicking on the **copy button** on the right corner.

    ![ws name.](media/212.png)
    
    > This unique registration key will be enable the Virtual Machine to become session host under this particular WVD-HP-01 hostpool.
    
22. Go back to the VM RDP window, and click inside the box opened in the installer.

    ![ws name.](media/213.png)
     

23. On your keyboard press the following combination of keys.

    **Ctrl + A**: To select the current text in the box.
    
    **Ctrl + V**: To replace the text inside the box with the key you copied.
    
    
24. Click on **Next**.

    ![ws name.](media/214.png)
     
     
     
25. Click on **Install**.

    ![ws name.](media/215.png)
    
    
    
26. Click on **Finish**.

    ![ws name.](media/216.png)
    
    
    
27. Open your browser and **paste** the following URL in your browser and hit **enter** to download the  ***Windows Virtual Desktop Agent Bootloader***.


        https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RWrxrH
        

    ![ws name.](media/217.png)
    
    
 
 
28. A popup will open below your browser,click on **Run**

    ![ws name.](media/218.png)
    
    
    
29. A installer will open, Click on **Next**.

    ![ws name.](media/219.png)
    
    
    
30. Click on the **check Box** saying *I accepting the terms in the License Agreement* and click on **Next**.

    ![ws name.](media/220.png)
    
    
    
31. Click on **Install*.

    ![ws name.](media/221.png)
    
    
32. Click on **Finish**.

    ![ws name.](media/222.png)
    
  
  
## Task 3: Verify registration on Host Pool


01. In your local machine visit Azure portal and search for *host pools* and click on it.

    ![ws name.](media/223.png)



02. Click on **WVD-HP-01**.

    ![ws name.](media/224.png)
    
    
03. Under manage blade click on **Session hosts**.

    ![ws name.](media/225.png)
    
    
  
04. Verify that WVD-VM-01 is added to the WVD-HP-01 hostpool.

    ![ws name.](media/226.png) 
