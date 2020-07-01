# Exercise 2: Azure AD Domain Services 

To deploy a Windows Virtual Desktop environment, we need a pre-created windows domain (e.g: contoso.com). This can be created by using one of the following methods:
1.	Azure Active Directory Domain Services (AADDS)
2.	Windows Active Directory
In this lab, we are using AADDS, which is an Azure PaaS resource. It will host the Windows domain needed to create the WVD session hosts. By default, the domain name used in AADDS will be your Azure Tenant name (e.g: ***azurehol1057.onmicrosoft.com***)


### **Task 1: Create prerequisites for AADDS**

**A.**	Register Microsoft.AAD Resource Provider
The resource provider Microsoft.AAD should be registered in our subscription to deploy AADDS. If it is not already registered, we need to register it from the Azure portal.



1. Navigate **Azure Portal** (https://portal.azure.com) in your browser. 

2. Login to Azure with the username **<inject key="AzureAdUserEmail" />**

   ![](media/wvd1.png)

3. Enter password **<inject key="AzureAdUserPassword" />** and click on **Sign in**.

   ![](media/wvd2.png)
   
> Refer the **Environment Details** tab for any other lab credentials/details.
  
  ![](media/wvd7.png)

4. There will be a pop-up entitled **Stay signed in?** with buttons for **No** and **Yes** - Choose **No**.

   ![](media/wvd3.png)

5. You may encounter a popup entitled **Welcome to Microsoft Azure** with buttons for **Start Tour** and **Maybe Later** - Choose **Maybe Later**.

   ![](media/wvd4.png)

6. In Azure Portal search for **Subscription** and click on **Subscriptions**.

   ![ws name.](media/1.png)
   
7. Click on your subscription.

   ![ws name.](media/2.png)
   
8. Under Settings blade Click on **Resource Providers**.

   ![ws name.](media/3.png)
   
9. Now search for **Microsoft.AAD** and make sure Microsofr.ADD is **Registered**.

   ![ws name.](media/4.png)
   
> In case the **Microsoft.AAD is not registred** then follow the **step 10** *.

10. Click on **Microsoft.AAD** Resource Provider and click on **Register**.

   ![ws name.](media/5.png)

### **Task 2: Deploy AADDS**

1. On the Azure portal menu or from the Home page, select **Create a resource**.

   ![](media/wvd6.png)

2. Enter Domain Services into the search bar, then choose Azure AD Domain Services from the search suggestions.

   ![ws name.](media/6.png)

3. On the Azure AD Domain Services page, click on **Create**.

   ![ws name.](media/7.png)
    
4. Configure Basics blade with following settings.
      
   - **Subscription**: Select your subscription.
   - **Resource Group** : Select **WVD-RG**
   - **DNS domain name**: **Default value**
   - **Region**: select **East US**
   - **SKU**: **Standard**
   - **Forest type**: **User**

   ![ws name.](media/8.png)
       
5. Then click **Next**.

6. In Networking tab under **Virtual network**, Click on **Create new**.
        
   ![ws name.](media/wvd8.png)

7. Configure your new virtual network with following settings and then click **Ok**.

   - **Name**: **aaddss-vnet**
   - **Address range**: **10.0.0.0/16**

  ![ws name.](media/wvd18.png)

8. You will return to Networking tab, make sure subnet **(new)aads-subnet-01 (10.0.0.0/24)** is selected by default.

   ![ws name.](media/11.png)

   Click on **Next**.

9. Click on **Review + Create** button.

    ![ws name.](media/12.png)

10. Now click on **Create** Button.

    ![ws name.](media/13.png)
    
11. A popup will appear, Click on **OK**.

    ![ws name.](media/14.png)
    
    **The Deployment will take approx 30 minutes to deploy. Till then continue with next step.**

12. Now navigate to the **WVD-RG** , then go to **Overview** and open **aadds-vnet**.

    ![ws name.](media/wvd20.png)

13. Select **Subnets** given under **Settings** blade, then click on **+Subnet** to add new subnet.

    ![ws name.](media/wvd21.png)

14. Now add following configuration and select **OK**:

    - Name: **SessionHost**
    - Address Range: **10.0.2.0/24**

    ![ws name.](media/wvd26.png)

15. Once created it will appear under **Subnets** as shown below:

    ![ws name.](media/wvd27.png)

### **Task 3: Update Virtual Network DNS**

> Wait for the deployment of Azure AD Domain Serive(i.e., previous task) to complete. Then only you can start Task 3. 

1. After deployment completes, go back to Azure portal home, and search for resource group and click on **Resource Groups**.
    
   ![ws name.](media/15.png)
    
2. Now click on your **WVD-RG** resource group.

   ![ws name.](media/16.png)
    
3. Click on **first NIC card**.

   ![ws name.](media/17.png)
    
4. Note down the **Private IP** of first NIC card.

    ![ws name.](media/wvd24.png)
    
5. Now go back to your **WVD-RG** resource group and click on **second NIC card**.

    ![ws name.](media/19.png)
    
6. Note down the **Private IP** of the second NIC card.

    ![ws name.](media/wvd25.png)
    
7. Go back to the **WVD-RG** resource group, and click on **aadds-vnet-01**.

    ![ws name.](media/21.png)
    
8. Now under **Settings** bade click on **DNS servers**.

    ![ws name.](media/22.png)
    
9. Under DNS server select **custom** and paste the IP address of first and second NIC card noted in step 19 and 21.
    
    ![ws name.](media/wvd19.png)
     
10. click on **Save**.
     
    ![ws name.](media/wvd9.png)

### **Task 4: Create new AD users**

1. In your azure portal, click on the **Cloud Shell** icon.

   ![ws name.](media/38.png)
   
2. In the Cloud Shell window that opens at the bottom of your browser window, select **PowerShell**.

   ![ws name.](media/wvd10.png)

3. Click on **Show Advanced Settings**.

   ![ws name.](media/wvd11.png)

4. Use exisiting **WVD-RG** resource group from the drop down and for:

    - **storage account:** Create new and enter **sa{uniqueid}**, for example: sa204272.
    - **file share:** Create new and enter **fs{uniqueid}**, for example: fs204272.
    
   ![ws name.](media/wvd12.png)

5. After the terminal launches it will look like this.

   ![ws name.](media/40.png)

6. Now copy and paste the following script:

```
$domain = ((Get-AzADUser | where {$_.Type -eq "Member"}).UserPrincipalName.Split('@'))[1]
$password= ConvertTo-SecureString "Azure1234567" -AsPlainText -Force
$users = @("domainjoinadmin@$domain","wvduser-01@$domain","wvduser-02@$domain")
$users | foreach{
    if((Get-AzADUser -UserPrincipalName $_) -ne $null){
    Remove-AzADUser -UserPrincipalName $_ -Force
   }
}
New-AzADUser -DisplayName "Domain Join Admin" -MailNickname "DomainJoinAdmin" -Password $password -UserPrincipalName "domainjoinadmin@$domain"
New-AzADUser -DisplayName "WVD User-01" -MailNickname "WVDUser-01" -Password $password -UserPrincipalName "wvduser-01@$domain"
New-AzADUser -DisplayName "WVD User-02" -MailNickname "WVDUser-02" -Password $password -UserPrincipalName "wvduser-02@$domain"
```

7. You will get output in the similar form shown below:

   ![ws name.](media/wvd22.png)

8. You can verify this by searching for **Azure Active Directory** in the search bar in Azure portal and then click on it.

   ![ws name.](media/24.png)

9. In Azure active directory page, click on **Users** under **Manage** blade .

   ![ws name.](media/25.png)
   
10. Here you can review the users created.

    ![ws name.](media/wvd13.png)

### **Task 5: Add membership for DomainJoinAdmin User**

1. In Azure active directory page, click on **Users** under **Manage** blade .

   ![ws name.](media/25.png)
   
2. Click on **DomainJoinAdminUser** to open it.

    ![ws name.](media/wvd34.png)

3. Select **Groups** under **Manage**. Then click on **+Add Membership** and select **AAD DC Administrators**.

    ![ws name.](media/wvd30.png)

4. Once selected it will show up under selected groups then click on **Select**.

    ![ws name.](media/wvd33.png)

5. This will show the added membership.

    ![ws name.](media/wvd31.png)

### **Task 6: Change passwords for the users created**

1. Now we will run a script to change passwords for the users created.

2. Copy and paste the following script and hit enter.

   ```
   $domain = ((Get-AzADUser | where {$_.Type -eq "Member"}).UserPrincipalName.Split('@'))[1]
   $password= ConvertTo-SecureString "Azure1234567" -AsPlainText -Force
   $users = @("domainjoinadmin@$domain","wvduser-01@$domain","wvduser-02@$domain")
   $users | foreach{
       Update-AzADUser -UserPrincipalName $_ -Password $password
   }
   ```

   ![ws name.](media/wvd23.png)

 > Wait for few seconds for the script to execute.
   
3. Output of the script will look like this.

   ![ws name.](media/42.png)

4. Click **Next** on the bottom right of this page.
