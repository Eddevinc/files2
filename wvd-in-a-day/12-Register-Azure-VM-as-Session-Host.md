# Exercise 12: Register Azure VM as Session Host


## Task 1: Create a Vm using Cloud Shell



1 **Copy** the following script and **paste** it in a notepad.


      #Parameters
       $adminUserName = "DomainJoinAdminUser"
       $adminPassword = "Azure1234567"

       #Deploy Template
       $UserPasswordhash = ConvertTo-SecureString $adminPassword -AsPlainText -Force
       New-AzResourceGroupDeployment -ResourceGroupName "WVD-RG" `
       -TemplateUri "https://akipersistantstg.blob.core.windows.net/wvdinaday/deployVM.json" `
       -existingVNETName "aadds-vnet-01" -existingSubnetName "SessionHost-Subnet" -adminUsername $adminUserName -adminPassword $UserPasswordhash
       
       
 3. Now in Azure portal search for *azure active directory* and click on it.
 
     ![ws name.](media/0.02png)
     
     
     
  4. Click on **Users**.
  
     ![ws name.](media/0.03.png)
      
      
 5. Click on **DomainJoinAdminUser**.
 
     ![ws name.](media/0.04.png)
     
     
 6. Copy the username of *DomainJoinAdminUser*.
 
    ![ws name.](media/0.05.png)
    
    
    
7. Go to the Notepad where you copied script in *step 2*.


8. Replace the **DomainJoinAdminUser** with the username you copied in  *step 6*.

    ![ws name.](media/0.06.ng)
    
    
9. Copy this script from the notepad.


10. In Azure portal click on the **cloud shell button** on top and wait for the cloud shell to connect.

   ![ws name.](media/0.01.png)


11. Paste the script in the cloud shell and press **Enter** to run the script.

   ![ws name.](media/0.07.png)
   
   wait for sometime for the script to execute.
   
   
   
12. After the execution completes the output will look as following.

    ![ws name.](media/0.08.png)

    wait for few more minutes for the deployment of virtual machine to complete.


## Task 2: Install Agents on VM and Register



1. In search bar of your Azure portal search for *virtual machines* and click on it.

   ![ws name.](media/01.png)
   
   
   
2. Click on **WVD-VM-01**.

   ![ws name.](media/02.png)
   
   
   
3. Click on **Connect**.

   ![ws name.](media/03.png)
   
   

4. Select **RDP**.

   ![ws name.](media/04.png)
   
   
5. Click on **Download RDP File**.

   ![ws name.](media/05.png)
   
   A file named ***WVD-VM-01*** will download.
   
   
   
6. Click on the downloaded file to open.

   ![ws name.](media/06.png)
   
   
7. Click on **More choices**.

   ![ws name.](media/06.1.png)
   
   
8. Click on **Use a different account**.

   ![ws name.](media/06.2.png)
   
   
9. Enter your credentials.

   ![ws name.](media/06.3.png)
   
   Click on **OK**.
   
   
   
10. A new pop up window will open,click on **Yes**.
 
    ![ws name.](media/09.png)
    
    A RDP Connection with your VM will be established.
    
    
 11. In your Vm window click on **Accept**.
 
     ![ws name.](media/09.png)
    
    
    
12. In your VM desktop double click on **Microsoft edge** icon to open it.
 
    ![ws name.](media/010.png)
   
   



13. **Copy** and **Paste** the following URL in your VM browser and **hit enter** to download ***Windows Virtual Desktop Agent***.
 
        https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RWrmXv
 
    ![ws name.](media/011.png)
    
    
    
14. Below your browser a popup will be displayed, Click on **Run**.
 
     ![ws name.](media/012.png)



15. A installer will open, Click on **Next**.

    ![ws name.](media/13.png)
    
    
    
16. Click on the **check Box** saying *I accepting the terms in the License Agreement* and click on **Next**.

    ![ws name.](media/14.png)
    
    
    
17. Now minimise your VM RDP window and visit azure portal on your local machine.


18. In Azure portal search for *host pools* and click on it.

    ![ws name.](media/15.png)
    
    
    
19. Click on **WVD-HP-01**.
 
    ![ws name.](media/16.png)
     
     
     
20. Click on **Registration Key**.

    ![ws name.](media/17.png)
    
    
    
21. Copy the registration key by clicking on the **copy button** on the right corner.

    ![ws name.](media/18.png)
    
    
    
22. Go back to the VM RDP window, and click inside the box opened in the installer.

    ![ws name.](media/19.png)
     

23. On your keyboard press the following combination of keys.

    **Ctrl + A**: To select the current text in the box.
    
    **Ctrl + V**: To replace the text inside the box with the key you copied.
    
    
24. Click on **Next**.

    ![ws name.](media/20.png)
     
     
     
25. Click on **Install**.

    ![ws name.](media/21.png)
    
    
    
26. Click on **Finish**.

    ![ws name.](media/22.png)
    
    
    
27. Open your browser and **paste** the following URL in your browser and hit **enter** to download the  ***Windows Virtual Desktop Agent Bootloader***.


        https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RWrxrH
        

    ![ws name.](media/23.png)
    
    
 
 
28. A popup will open below your browser,click on **Run**

    ![ws name.](media/24.png)
    
    
    
29. A installer will open, Click on **Next**.

    ![ws name.](media/25.png)
    
    
    
30. Click on the **check Box** saying *I accepting the terms in the License Agreement* and click on **Next**.

    ![ws name.](media/26.png)
    
    
    
31. Click on **Install*.

    ![ws name.](media/27.png)
    
    
32. Click on **Finish**.

    ![ws name.](media/28.png)
    
  
  
## Task 3: Verify registration on Host Pool


01. In your local machine visit Azure portal and search for *host pools* and click on it.

    ![ws name.](media/29.png)



02. Click on **WVD-HP-01**.

    ![ws name.](media/30.png)
    
    
03. Under manage blade click on **Session hosts**.

    ![ws name.](media/31.png)
    
    
  
04. Verify that WVD-VM-01 is added to the WVD-HP-01 hostpool.

    ![ws name.](media/32.png) 
