# Exercise 11: Setup FsLogix

The Windows Virtual Desktop service recommends FSLogix profile containers as a user profile solution. FSLogix is designed to roam profiles in remote computing environments, such as Windows Virtual Desktop. It stores a complete user profile in a single container. At sign in, this container is dynamically attached to the computing environment using natively supported Virtual Hard Disk (VHD) and Hyper-V Virtual Hard disk (VHDX). The user profile is immediately available and appears in the system exactly like a native user profile. This article describes how FSLogix profile containers used with Azure Files function in Windows Virtual Desktop.

## Task 1: Create Storage account and file share

1. In your Azure portal search for storage account and click on it.

   ![ws name.](media/a55.png)
   
2. Click on **+ Add**.

   ![ws name.](media/a.png)

3. Use the following configuration for the storage account.
   
   - Subscription: Select your default subscription.
   
   - Resource group: Select your existing resource group that is **WVD-RG**.
   
   - Storage account name: Use any **storageaccount{unique-id}**
   
   - Location: Default resource group location
   
   - Performance: Standard
   
   - Account kind: StorageV2(general purpose v2)
   
   - Replication: Read-access-geo-redundant storage (RA-GRS)
   
   - Access tier(default): Hot
   
   - Click on **Next: Networking**
   
   ![ws name.](media/a56.png)
   
4. Under networking tab use following configuration.
    
     - Connectivity method: Public endpoint(selected networks)
    
     - Virtual network subscription: Default subscription
     
     - Virtual Network: aadds-vnet
     
     - Subnets: SessionHost(10.0.2.0/24)
     
     - Leave the rest to default settings.
     
     - Click on **Review + Create**.
     
  ![ws name.](media/a57.png)
     
5. Click on **Create**.

    ![ws name.](media/a58.png)

6. After deployment completes Click on notification icon on your azure portal, and click on **Go to resource**.

    ![ws name.](media/a59.png)
   
7. Now click on **Configuration** present under **Settings** blade. Then scroll down and find the option **Active Directory Domain Services (Azure AD DS)** and click on **Enabled**.

    ![ws name.](media/a60.png)
    
    
8. Click on **Save**.
     
   ![ws name.](media/a61.png)
 
9. Click on **Overview** to return back to storage account page, then click on **File shares**.

    ![ws name.](media/a62.png)
    
    
    
10. Click on **+ File share**.

    ![ws name.](media/a63.png)
    
    
12. Enter the following name for your file share.
    
    - Name: **userprofile**
    
    - Click on **Create**.
    
   ![ws name.](media/a64.png)

    
## Task 2: Configure File share 



1. Click on the file share you just created.

   ![ws name.](media/a65.png)
     
     
     
2. Click on **Access Control (IAM)**, then click on **Add** and select **Add role assignment**.

   ![ws name.](media/wvd48.png)
   
   
   
3. Select following configuration for role assignment and then click on **Save**.  
   
   - Role: **Storage File Data SMB Share Contributor**
   
   - Under **Select** search for **WVDUser** and click on both the users to select them.
   
      ![ws name.](media/a66.png)
   


## Task 3: Configure Session Hosts

A. In this task we will install and configure FsLogix in the **WVD-HP01-SH-0** session host.

1. In your Azure portal search for **Virtual** and click on **Virtual Machine**.

   ![ws name.](media/a67.png)
      
   
2. Click on **WVD-HP01-SH-0**.

   ![ws name.](media/wvd50.png)
      
   
3. On left side under Operations tab click on **Run command**.

   ![ws name.](media/wvd51.png)
  
4. Now select **RunPowerShellScript**.

   ![ws name.](media/a68.png)
   
   
5. A similar window will open.

   ![ws name.](media/a69.png)
   
      
6. **Copy** the complete Script below and **paste** it by pressing **Ctrl + V** in the powershell window in the Azure portal.

    ```
    #Variables
    $storageAccountName = "NameofStorageAccount" 

    #Create Directories
    $LabFilesDirectory = "C:\LabFiles"
    New-Item -Path $LabFilesDirectory -ItemType Directory |Out-Null
    New-Item -Path "$LabFilesDirectory\FSLogix" -ItemType Directory |Out-Null

    #Download FSLogix Installation bundle
    Invoke-WebRequest -Uri "https://akipersistantstg.blob.core.windows.net/fslogix/FSLogix_Apps_Installation.zip" -OutFile "$LabFilesDirectory\FSLogix_Apps_Installation.zip"

    #Extract the downloaded FSLogix bundle
    function Expand-ZIPFile($file, $destination){
    $shell = new-object -com shell.application
    $zip = $shell.NameSpace($file)
    foreach($item in $zip.items()){
     $shell.Namespace($destination).copyhere($item)
    }
    }

    Expand-ZIPFile -File "$LabFilesDirectory\FSLogix_Apps_Installation.zip" -Destination "$LabFilesDirectory\FSLogix"

    #Install FSLogix
    $pathvargs = {C:\LabFiles\FSLogix\x64\Release\FSLogixAppsSetup.exe /quiet /install }
    Invoke-Command -ScriptBlock $pathvargs

    #Create registry key 'Profiles' under 'HKLM:\SOFTWARE\FSLogix'
    $registryPath = "HKLM:\SOFTWARE\FSLogix\Profiles"
    if(!(Test-path $registryPath)){
    New-Item -Path $registryPath -Force | Out-Null
    }

    #Add registry values to enable FSLogix profiles, add VHD Locations, Delete local profile and FlipFlop Directory name
    New-ItemProperty -Path $registryPath -Name "VHDLocations" -Value "\\$storageAccountName.file.core.windows.net\userprofile" -PropertyType String -Force | Out-Null
    New-ItemProperty -Path $registryPath -Name "Enabled" -Value 1 -PropertyType DWord -Force | Out-Null
    New-ItemProperty -Path $registryPath -Name "DeleteLocalProfileWhenVHDShouldApply" -Value 1 -PropertyType DWord -Force | Out-Null
    New-ItemProperty -Path $registryPath -Name "FlipFlopProfileDirectoryName" -Value 1 -PropertyType DWord -Force | Out-Null

    #Display script completion in console
    Write-Host "Script Executed successfully"
    ```
 
 
    
> The above script will create a new directory i.e. *C:\LabFiles* where it will download FSLogix Installation bundle and extract it. After extraction installation of FSLogix will begin. When configuring Profile Container registry settings are added here: Registry Key: *HKLM\SOFTWARE\FSLogix\Profiles*. When configuring Profile Container the entire contents of the registry will be redirected to the FSLogix Profile Container. 


8. Now scroll up on the script you pasted and replace **NameofStorageAccount (for example: storageaccount204756)** in second line of script with the storage account name you created in *Task 1, step 9*.

   ![ws name.](media/a70.png)
   
   - Now Click on **Run** to execute the script.
   
 
9. Wait for sometime for the script to execute. It will show a output saying **Script Executed successfully**.

   ![ws name.](media/a84.png)
   
10. Click on **WVD-HP01-SH-1**.

    ![ws name.](media/a85.png)


11. Select **RunPowerShellScript**.

    ![ws name.](media/a68.png)
    
    
12. Press **Ctrl + V** to paste the script in the Powershell window.

13. Now scroll up on the script you pasted and replace **NameofStorageAccount (for example: storageaccount204756)** in second line of script with the storage account name you created in *Task 1, step 9*.

   ![ws name.](media/a70.png)
   
   - Now Click on **Run** to execute the script.
   
 
14. Wait for sometime for the script to execute. It will show a output saying **Script Executed successfully**.

   ![ws name.](media/a84.png)

  
15. Now search for **Windows Virtual Desktop** in azure portal and click on it.

   ![ws name.](media/y.png)
   
   
16. Click on **Users**, then in the search bar search for **WVDUser** then click on **WVDUser-01**.

    ![ws name.](media/a71.png)
    
17. Switch to **Sessions** blade, then select both WVDUser-01 and WVDUser-02 and click on **Log off**.

    ![ws name.](media/a72.png)
    
18. Click on **OK** for **Log off user from VMs**.

    ![ws name.](media/a73.png)
   
    > This will logoff both the session host so that when the users sign in again to the session host the Fxlogix will start functioning.
    
    
19. Now paste this link ```aka.ms/wvdarmweb``` in your browser and enter your **credentials** to login. 

   - Username: Put username of **WVD User-01**  (for example: **WVDUser-01@azurehol1055.onmicrosoft.com**). Then click on **Next**.
   
      ![ws name.](media/wvd42.png)

   - Password: **Azure1234567** and click on **Sign in**.

      ![ws name.](media/wvd43.png)
  
    
    
20. Double click on the **WVD-HP-01-DAG** Desktop to launch it.

    ![ws name.](media/a75.png)
    
21. Enter your **Credentials** to access the desktop.

    ![ws name.](media/a52.png)
    
    
22. Now you can see the desktop saying ***Please wait for the FSLogix Apps Services***.

    ![ws name.](media/a77.png)
    
    > This means that user profile is being manaed by FSLogix.
    
23. Now return back to the Azure Portal and search for *storage account* and click on it.

    ![ws name.](media/a55.png)
    
    
24. Click on the storage account you created in **Task 1 step 3**, then under settings blade click on  **Firewalls and virtual networks**.

    ![ws name.](media/.png)
    
25. Under *Allow access from* select **All networks** and click on **save icon**.

    ![ws name.](media/.png)
    
    > This will enable access of your storage account on the public network.
    
    
26. Now click on **Overview** and open **Fileshare**.

    ![ws name.](media/.png)
    
    
27. Click on the **userprofile** fileshare.

    ![ws name.](media/.png)
    

25. Now you will be able to see the user profiles data stored in the fileshares in a ***.vhd*** format.

    ![ws name.](media/.png)
