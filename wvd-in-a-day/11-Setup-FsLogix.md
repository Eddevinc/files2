# Exercise 11: Setup FsLogix

The Windows Virtual Desktop service recommends FSLogix profile containers as a user profile solution. FSLogix is designed to roam profiles in remote computing environments, such as Windows Virtual Desktop. It stores a complete user profile in a single container. At sign in, this container is dynamically attached to the computing environment using natively supported Virtual Hard Disk (VHD) and Hyper-V Virtual Hard disk (VHDX). The user profile is immediately available and appears in the system exactly like a native user profile. This article describes how FSLogix profile containers used with Azure Files function in Windows Virtual Desktop.

## Task 1: Create Storage account and file share



1. In your Azure portal search for storage account and click on it.

   ![ws name.](media/161.png)


   
   
2. Click on **+ Add**.

   ![ws name.](media/162.png)



3. Use the following configuration for the storage account.

   ![ws name.](media/163.png)
   
   **Subscription**: Select your default subscription
   
   **Resource group**: Select your default resource group
   
   **Storage account name**: Use any ***Unique random Name***
   
   **Location**: Default resource group location
   
   **Performance**: Standard
   
   **Account kind**: StorageV2(general purpose v2)
   
   **Replication**: Read-access-geo-redundant storage (RA-GRS)
   
   **Access tier(default)**: Hot
   
   Click on **Next: Networking**
   
   
   
4. Under networking tab use following configuration.

    ![ws name.](media/wvd45.png)
    
     **Connectivity method**: Public endpoint(selected networks)
    
     **virtual network subscription**: Default subscription
     
     **Virtual Network**: aadds-vnet
     
     **Subnets**: SessionHost(10.0.2.0/24)
     
     Leave the rest to default settings.
     
     Click on **Review + Create**.
     
     
     
5. Click on **Create**.

    ![ws name.](media/165.png)
     
  

6. After deployment completes Click on notification icon on your azure portal, and click on **Go to resource**.

    ![ws name.](media/166.png)
    
    
    
7. Now on left hand side under *Settings* blade click on **Configuration**.

    ![ws name.](media/167.png)
    
    
8. In configuration page, scroll down and find the option **Active Directory Domain Services (Azure AD DS)**.

     ![ws name.](media/168.png)
     
     **Active Directory Domain Services (Azure AD DS)**: Click on **Enabled**
     
     Click on **Save**.
     
     
     
9. Click on **Overview** to return back to storage account page.

    ![ws name.](media/169.png)
    
    
    
10. Click on **File shares**.

    ![ws name.](media/170.png)
    
    
    
11. Click on **+ File share**.

    ![ws name.](media/171.png)
    
    
12. Enter the following name for your file share.

    ![ws name.](media/172.png)
    
    **Name**: *userprofile*
    
    Click on **Create**.
    
    
    
## Task 2: Configure File share 



1. Click on the file share you just created.

   ![ws name.](media/wvd47.png)
     
     
     
2. Click on **Access Control (IAM)**, then click on **Add** and select **Add role assignment**.

  ![ws name.](media/wvd48.png)
   
   
   
3. Select following configuration for role assignment and then click on **Save**.  
   
   - Role: **Storage File Data SMB Share Contributor**
   
   - Under **Select** search for *WVDUser* and click on both the users to select them.
   
      ![ws name.](media/wvd49.png)
   


## Task 3: Configure Session Hosts

A. In this task we will install and configure FsLogix in the *WVD-HP01-SH-0* session host.

1. In your Azure portal search for *virtual machines* and click on it.

   ![ws name.](media/178.png)
   
   
   
2. Click on **WVD-HP01-SH-0**.

   ![ws name.](media/wvd50.png)
   
   
   
3. On left side under Operations tab click on **Run command**.

   ![ws name.](media/wvd51.png)
   
   
   
4. Now click on **RunPowerShellScript**.

   ![ws name.](media/WVD-52.png)
   
   
5. A similar window will open.

   ![ws name.](media/182.png)
   
   
   
6. **Copy** the complete Script below and **paste** it by pressing **Ctrl + V** in the powershell window in the Azure portal.

 

   
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
    
    
> The above script will create a new directory i.e. *C:\LabFiles* where it will download FSLogix Installation bundle and extract it. After extraction installation of FSLogix will begin. When configuring Profile Container registry settings are added here: Registry Key: *HKLM\SOFTWARE\FSLogix\Profiles*. When configuring Profile Container the entire contents of the registry will be redirected to the FSLogix Profile Container. 


8. Now scroll up on the script you pasted and replace ***NameofStorageAccount*** in second line of script with the storage account name you created in *Task 1, step 9*.

   ![ws name.](media/183.png)
   
   Now Click on **Run** to execute the script.
   
   Wait for sometime for the script to execute, It will show a output saying ***Script Executed successfully***.
   
 
   
9. Now search for *Windows Virtual Desktop* in azure portal and click on it.

   ![ws name.](media/.png)
   
   
10. Click on **Users*.

    ![ws name.](media/.png)

11. In the search bar search for *WVDUser* then click on **WVDUser-01**.

    ![ws name.](media/.png)
    
    
12. Switch to sessions blade.

    ![ws name.](media/.png)
    
13. Select both WVDUser-01 and WVDUser-02 and click on **Log off**.

    ![ws name.](media/.png)
   
    > This will logoff both the session host so that when the users sign in again to the session host the Fxlogix will start functioning.
    
    
14. In you local machine, click on **Start** and search for *Remote Desktop* and open it.

    ![ws name.](media/.png)
    
    
15. Click on the **WVD-HP-01-DAG** Desktop to launch it.

    ![ws name.](media/.png)
    
16. Enter your **Credentials** to access the desktop.

    ![ws name.](media/.png)
    
    
17. Now you can see the desktop saying ***Please wait for the FSLogix Apps Services***.

    ![ws name.](media/.png)
    
    
18. Now return back to the Azure Portal and search for *storage account* and click on it.

    ![ws name.](media/.png)
    
    
19. Click on the storage account you created in **Task 1, step 3**.

    ![ws name.](media/.png)
    
    
20. Click on **File Shares**.

    ![ws name.](media/.png)
    
    
21. Click on **userprofile**.

    ![ws name.](media/)


22. Now you will be able to see the user profiles data stored in the fileshares.

    ![ws name.](media/.png)
