# Exercise 2: Azure AD Domain Services 

To deploy a Windows Virtual Desktop environment, we need a pre-created windows domain (e.g: contoso.com). This can be created by using one of the following methods:
1.	Azure Active Directory Domain Services (AADDS)
2.	Windows Active Directory
In this exercise, we are using AADDS, which is an Azure PaaS resource. It will host the Windows domain needed to create the WVD session hosts. By default, the domain name used in AADDS will be your Azure Tenant name (e.g: ***azurehol1057.onmicrosoft.com***)


### **Task 1: Create prerequisites for AADDS**

**A.**	Register Microsoft.AAD Resource Provider
The resource provider Microsoft.AAD should be registered in our subscription to deploy AADDS. If it is not already registered, we need to register it from the Azure portal.


1. Navigate **Azure Portal** (https://portal.azure.com) in your browser. 

2. Login to Azure with the username **<inject key="AzureAdUserEmail" />**

   ![](media/wvd1.png)

3. Enter password **<inject key="AzureAdUserPassword" />** and click on **Sign in**.

   ![](media/wvd2.png)
   
> **Note**: Refer the **Environment Details** tab for any other lab credentials/details.
  
   ![](media/wvd7.png)

4. There will be a pop-up entitled **Stay signed in?** with buttons for **No** and **Yes** - Choose **No**.

   ![](media/a102.png)

5. You may encounter a popup entitled **Welcome to Microsoft Azure** with buttons for **Start Tour** and **Maybe Later** - Choose **Maybe Later**.

   ![](media/wvd4.png)

6. In Azure Portal click on **Subscriptions** present under **Navigate**.

   ![ws name.](media/a.png)
   
7. Open your =subscription by clicking on it.

   ![ws name.](media/b.png)
   
8. Then under **Settings** blade click on **Resource Providers**.

   ![ws name.](media/c.png)
   
9. Now search for **Microsoft.AAD** and make sure Microsoft.ADD is **Registered**.

   ![ws name.](media/d.png)
   
> In case the **Microsoft.AAD** is not registred then follow **step 10**.

10. Click on **Microsoft.AAD** Resource Provider and then click on **Register**.

   ![ws name.](media/a103.png)

### **Task 2: Deploy AADDS**

1. Select **Create a resource** from the Azure portal homepage.

   ![](media/wvd6.png)

2. Enter **Domain Services** into the search bar, then choose **Azure AD Domain Services** from the search suggestions.

   ![ws name.](media/e.png)

3. On the Azure AD Domain Services page, click on **Create**.

   ![ws name.](media/f.png)
    
4. Configure Basics blade with following settings.
      
   - Subscription: *Select the default subscription*.
   - Resource Group: *Select **WVD-RG** from the drop down*.
   - DNS domain name: *Default value*
   - Region: **East US**, *this should be same as the region of your resource group*.
   - SKU**: **Standard**
   - Forest type: **User**

   ![ws name.](media/g.png)
       
5. Then click **Next**.

6. Now in **Networking** tab click on **Create new** under **Virtual network**.
        
   ![ws name.](media/wvd8.png)

7. Configure your new virtual network with following settings and then click **Ok**.

   - Name: **aadds-vnet**
   - Address range: **10.0.0.0/16**

> **Note:** If 

   ![ws name.](media/h.png)

8. Make sure the subnet **(new)aads-subnet-01 (10.0.0.0/24)** is selected by default.

   ![ws name.](media/i.png)

9. Click on **Next**.

10. Make sure that both **All Global Administrator of the Azure AD directory** & **Members of AAD DC Administrators group** boxes are unchecked. Then click on **Review + Create** button.

   ![ws name.](media/a99.png)

11. Now click on **Create** Button.

   ![ws name.](media/k.png)
    
12. Click **OK** on getting a popup saying **You should know**.

   ![ws name.](media/a98.png)
   
 >**Note:** The Deployment will take approx 30 minutes to deploy. Till then continue with next step.

13. Now navigate to the **WVD-RG** , then go to **Overview** and open **aadds-vnet**.

   ![ws name.](media/wvd20.png)

14. Select **Subnets** given under **Settings** blade, then click on **+Subnet** to add new subnet.

   ![ws name.](media/wvd21.png)

15. Now add following configuration and select **OK**:

    - Name: **sessionhosts-subnet**
    - Address Range: **10.0.1.0/24**

    ![ws name.](media/l.png)

16. Once created it will appear under **Subnets** as shown below:

   ![ws name.](media/m.png)

### **Task 3: Update Virtual Network DNS**

 >**Note:** Wait for the deployment of Azure AD Domain Serive(i.e., previous task) to complete. Then only you can start Task 3. 

1. After deployment completes, go back to Azure portal, select **Resource Groups** under **Navigate**.
    
   ![ws name.](media/n.png)
    
2. Now click on your **WVD-RG** resource group to open it.

   ![ws name.](media/o.png)
    
3. In the resource group there are two NIC cards.

  ![ws name.](media/a104.png)

4. Open both the NIC cards and copy and paste their **Private IP**.

   ![ws name.](media/wvd24.png)
   
   ![ws name.](media/wvd25.png)
    
5. Go back to the **WVD-RG** resource group and click on **aadds-vnet**.

   ![ws name.](media/r.png)
    
6. Now under **Settings** blade click on **DNS servers**. Then select **custom** and paste the IP address of first and second NIC card you copied in earlier step.

   ![ws name.](media/wvd19.png)
     
7. Click on **Save**.
     
   ![ws name.](media/wvd9.png)

### **Task 4: Create new AD users**

1. In your azure portal, click on the **Cloud Shell** icon.

   ![ws name.](media/a105.png)
   
2. In the Cloud Shell window that opens at the bottom of your browser window, select **PowerShell**.

   ![ws name.](media/wvd10.png)

3. Click on **Show Advanced Settings**.

   ![ws name.](media/wvd11.png)

4. Use exisiting resource group - **WVD-RG** from the drop down and for:

    - Storage Account: Select **Create new** and enter **sa{uniqueid}**, for example: sa204272.
    - File Share: Select **Create new** and enter **fs{uniqueid}**, for example: fs204272.
    
   ![ws name.](media/wvd12.png)

5. After the terminal launches it will look like this.

   ![ws name.](media/40.png)

6. Now copy and paste the following script in the console and hit enter to run it:

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

8. You can verify this by navigating to Azure Active Directory.

9. On the homepage search for Azure Active in the search bar and then select **Azure Active Directory**.

   ![ws name.](media/s.png)

9. In Azure active directory page, click on **Users** present under **Manage** blade.

   ![ws name.](media/t.png)
   
10. Here you can review the users created.

    ![ws name.](media/wvd13.png)

11. Copy and Paste the usernames for all three users in notepad.

  >**Note:** Make sure to copy paste the the usernames of the users as you will need this throughout the lab.

### **Task 5: Add membership for DomainJoinAdmin User**

1. Navigate back to Azure active directory page, click on **Users** under **Manage** blade .

   ![ws name.](media/t.png)
   
2. Click on **DomainJoinAdminUser** to open it.

   ![ws name.](media/wvd34.png)

3. Select **Groups** under **Manage**, then click on **+Add Membership**. 

   ![ws name.](media/u.png)

4. Select **AAD DC Administrators**. Once selected it will show up under selected groups then click on **Select**.

   ![ws name.](media/v.png)

5. This will show the added membership.

   ![ws name.](media/x.png)

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

  >**Note:** Wait for few seconds for the script to execute.
   
3. Output of the script will be similar to the one shown below.

  ![ws name.](media/42.png)

4. Click on the **Next** button.
