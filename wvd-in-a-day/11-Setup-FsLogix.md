# Exercise 11: Setup FsLogix



## Task 1: Create Storage account and file share



1. In your Azure portal search for storage account and click on it.

   ![ws name.](media/161.png)


   
   
2. Click on **+ Add**.

   ![ws name.](media/162.png)



3. Use the following configuration for the storae account.

   ![ws name.](media/163.png)
   
   **Subscription**: Select your default subscription
   
   **Resource group**: Select your default resource group
   
   **Storage account name**: Use any ***Unique random Name***
   
   **Location**: Default resource group location
   
   **Performance**: Standard
   
   **Account kind**: StorageV2(general purpose v2)
   
   **Replication**: Locally-redundant storage(LRS)
   
   **Access tier(default)**: Hot
   
   Click on **Next: Networking**
   
   
   
4. Under networking tab use following configuration.

    ![ws name.](media/164.png)
    
     **Connectivity method**: Public endpoint(selected networks)
    
     **virtual network subscription**: Default subscription
     
     **Virtual Network**: aadds-vnet-01
     
     **Subnets**: SessionHost-Subnet(10.1.1.0/24)
     
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
    
    **Name**: *userprofilestorage1*
    
    Click on **Create**.
    
    
    
## Task 2: Configure File share 



1. Click on the file share you just created.

   ![ws name.](media/173.png)
     
     
     
2. Click on **Access Control (IAM).

   ![ws name.](media/174.png)   
   
   
   
3. Click on **Add** under Add a role assignment.

   ![ws name.](media/175.png)
   
   
   
4. Select following configuration for role assignment.

   ![ws name.](media/176.png)
   
   
   **Role**: Contributer
   
   Under **Select** search for *WVDUser* and click on both the users to select them.
   
   
5. Click on **Save**.

   ![ws name.](media/177.png)



## Task 3: Configure Session Hosts



1. In your Azure portal search for *virtual machines* and click on it.

   ![ws name.](media/178.png)
   
   
   
2. Click on **WVD-SH-0**.

   ![ws name.](media/179.png)
   
   
   
3. On left side under Operations tab click on **Run command**.

   ![ws name.](media/180.png)
   
   
   
4. Now click on **RunPowerShellScript**.

   ![ws name.](media/181.png)
   
   
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
    
    
    
8. Now scroll up on the script you pasted and replace ***NameofStorageAccount*** in second line of script with the storage account name you created in *Task 1, step 9*.

   ![ws name.](media/183.png)
   
   Now Click on **Run** to execute the script.
   
   
