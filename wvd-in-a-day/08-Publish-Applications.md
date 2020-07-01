# Exercise 8: Publish Applications


The Published applications can be accessed directly from the wrokspace after the user sign in to windows virtual desktop, without launching the desktop environment.

## Task 1: Publish Applications from Start Menu


1. Navigate to Azure portal, then search for **Windows** in search bar and select **Windows Virtual Desktop**.

   ![ws name.](media/y.png)
   
2. Now click on **Application groups** and then click on the **WVD-AG-01** application group.

   ![ws name.](media/a32.png)
      
3. Click on **Applications** under **Manage** blade and then click on **+Add**.

   ![ws name.](media/a33.png)
  
4. Select following parameters to Publish application.
   
   - Application Source: **Start Menu** 
   
   - Application: **Excel**

   - Leave rest of the parameters to be on default settings and click on **Save**.
   
   ![ws name.](media/a34.png)
   
   
## Task 2: Publish Applications from using File Path

1. Go back to **Applications** under **Manage** blade and then click on **+Add** .

   ![ws name.](media/a33.png)
  
2. Select following parameters to Publish application.
   
   - Application Source: **File path** 
   
   - Application path: **C:\Program Files\Microsoft Office\root\Office16\WINWORD.EXE** 
   
   - Application name: **Word**
   
   - Icon path: **C:\Program Files\Microsoft Office\Root\VFS\Windows\Installer\{90160000-000F-0000-1000-0000000FF1CE}\wordicon.exe**
   
   - Leave rest of the parameters to be on default settings and click on **Save**.
   
   ![ws name.](media/a35.png)

3. Click on **Next** button.
