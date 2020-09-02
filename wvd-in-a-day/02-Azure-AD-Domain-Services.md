# Exercise 2: Azure AD Domain Services (AADDS)

To deploy a Windows Virtual Desktop environment, we need a pre-created windows domain (for example: contoso.com). This can be achieved using one of the following ways:

1.	Azure Active Directory Domain Services (AADDS)
2.	Windows Active Directory


In this exercise, we are using AADDS, which is an Azure PaaS resource. It will host the Windows domain needed to create the WVD session hosts. By default, the domain name used in AADDS will be your Azure Tenant name (for example: If your lab user account is *odl_user_189588@azurehol1057.onmicrosoft.com*, the domain will be *azurehol1057.onmicrosoft.com*).

### **Task 1: Register Microsoft.AAD Resource Provider**

In this task we will create prerequisites for AADDS. The resource provider Microsoft.AAD should be registered in our subscription to deploy AADDS. If it is not already registered, we need to register it from the Azure portal.


1. Open **Azure Portal** (https://portal.azure.com) in your browser. 

2. Login to Azure with the username **<inject key="AzureAdUserEmail" />** and click on **Next**.

   ![](media/wvd1.png)

3. Enter password **<inject key="AzureAdUserPassword" />** and click on **Sign in**.

   ![](media/wvd2.png)

> **Note:** 
> - If there's a popup entitled **Stay signed in?** with buttons for **No** and **Yes** - Choose **No**.
>
>  ![](media/a102.png)
>   
> - If there's another popup entitled **Welcome to Microsoft Azure** with buttons for **Start Tour** and **Maybe Later** - Choose **Maybe Later**.
>
>  ![](media/wvd4.png)

4. In Azure Portal click on **Subscriptions** present under **Navigate**.

   ![ws name.](media/a.png)
   
5. Open your **Subscription** by clicking on it.

   ![ws name.](media/b.png)
   
6. Then under **Settings** blade click on **Resource Providers**.

   ![ws name.](media/c.png)
   
7. Now search for **Microsoft.AAD** and make sure Microsoft.ADD is **Registered**.

   ![ws name.](media/d.png)
   
> **Note:** In case the **Microsoft.AAD** is not registred then follow **step 8**.

8. Click on **Microsoft.AAD** Resource Provider and then click on **Register**. It will take a minute to get registered, you can click on *Refresh* to check the status. Once *Microsoft.AAD* gets registered, then move to the next task.

    ![ws name.](media/a103.png)

### **Task 2: Deploy AADDS**

In this task we will create Azure Active Directory Domain Services.

1. On Azure portal homepage, click on **Create a resource** that is present below **Azure Services.**

   ![](media/w18.png)

2. Enter **Domain Services** into the search bar, then choose **Azure AD Domain Services** from the search suggestions.

   ![ws name.](media/w19.png)

3. On the Azure AD Domain Services page, click on **Create**.

   ![ws name.](media/f.png)
    
4. Now configure the Basics blade with following settings:
      
   - Subscription: *Select the default subscription*.
   - Resource Group: *Select **WVD-RG** from the drop down*.
   - DNS domain name: *Leave to default value*
   - Region: **East US**, *this should be same as the region of your resource group*.
   - SKU: **Standard**
   - Forest type: **User**
   - Click on **Next**.

   ![ws name.](media/g.png)       

5. Now in **Networking** tab click on **Create new** under **Virtual network**.
        
   ![ws name.](media/w20.png)

6. Configure your new virtual network with following settings and then click **Ok**.

   - Name: **aadds-vnet**
   - Address range: *By default the address range will be set to **10.0.0.0/24**, so edit and make it -* **10.0.0.0/16**
   
   Under **Subents**, add the following:
   - Subnet name: **aadds-subnet**
   - Address range: **10.0.0.0/24**
   
   
   ![ws name.](media/h.png)

> **Note:** This subnet will be used to deploy Azure Active Directory Domain Service.  

7. Check that **(new)aadds-subnet-01 (10.0.0.0/24)** is selected against **Subnet**. Then click on **Next**.

   ![ws name.](media/i.png)

8. Make sure that both *All Global Administrator of the Azure AD directory* & *Members of AAD DC Administrators group* boxes are **unchecked**. Then click on **Review + Create** button.

    ![ws name.](media/a99.png)

10. Now click on **Create** Button.

    ![ws name.](media/k.png)
    
11. Click **OK** on the popup saying **You should know**.

    ![ws name.](media/a98.png)
   
> **Note:** The Deployment will take approx 30 minutes to deploy. Till then continue with next step.

12. Now navigate to the **WVD-RG**, then go to **Overview** and open your virtual network i.e., **aadds-vnet**.

    ![ws name.](media/wvd20.png)

13. Select **Subnets** given under **Settings** blade, then click on **+Subnet** to add new subnet.

    ![ws name.](media/wvd21.png)

14. Add the following configurations and then click on **OK** :

    - Name: **sessionhosts-subnet**
    - Address Range: **10.0.1.0/24**
    
   ![ws name.](media/l.png)

> **Note:** *sessionhosts-subnet* will be used for the purpose of deploying resources related to session hosts.

15. Once the subnet is created, it will appear under **Subnets** as shown below:

    ![ws name.](media/m.png)
  
> **Note:** Wait for the deployment of Azure Active Directory Domain Serive to complete. Once the deployment succeeds, then you can move to Task 3. 

### **Task 3: Update Virtual Network DNS**

In this task we will use private IP address of the two network interface cards created while deploying AADDS and use it to configure DNS server of *aadds-vnet*.
By default, Virtual Network uses Azure's own DNS servers to resolve the DNS queries. Since we have deployed AADDS, this queries should now be pointed to the new DNS servers, for AADDS to function properly.

1. Go to Azure portal and select **Resource Groups** under **Navigate**.
    
   ![ws name.](media/n.png)
    
2. Now click on your **WVD-RG** resource group to open it.

   ![ws name.](media/o.png)
    
3. In the resource group there are two NIC cards.

   ![ws name.](media/a104.png)

4. Open both the NIC cards and copy their **Private IP** in a text editor.

   ![ws name.](media/wvd24.png)
   
   ![ws name.](media/wvd25.png)
    
5. Go back to the **WVD-RG** resource group and open the virtual network i.e., **aadds-vnet**.

   ![ws name.](media/r.png)
    
6. Now under **Settings** blade click on **DNS servers**, then add the following configuration:
     
     - DNS Servers: **Custom**
     - Add DNS Server: **10.0.0.4** & **10.0.0.5** (*paste the IP addresses of both the NIC cards that you copied in the earlier step*)
     
   ![ws name.](media/wvd19.png)
     
7. Click on **Save**.
     
   ![ws name.](media/wvd9.png)

### **Task 4: Create new AD users**

In this task we will use cloud shell to create three users i.e., *WVDUser-01*, *WVDUser-02* to access the windows virtual desktop and *DomainJoinAdmin* to domain join session hosts.

1. In Azure portal, click on the **Cloud Shell** icon.

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

6. Now copy and paste the following script in the console and hit **Enter** to run it completely:

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

> **Note:** The above script will create three users i.e., *WVDUser-01*, *WVDUser-02* and *DomainJoinAdmin* and set their passwords to "*Azure1234567*". DomainJoinAdmin user will be used to domain join session hosts to the AADDS.

7. You will get output in the similar form shown below:

   ![ws name.](media/wvd22.png)

8. You can verify this by navigating to Azure Active Directory.

9. On the homepage search for *Azure Active* in the search bar and then select **Azure Active Directory**.

   ![ws name.](media/s.png)

9. Then click on **Users** present under **Manage** blade.

   ![ws name.](media/t.png)
   
10. Under **All users (Preview)** you can review the users created.

    ![ws name.](media/a108.png)

11. Copy and Paste the usernames for all three users in notepad.

    ![ws name.](media/w13.png)

> **Note:** Make sure to copy paste the the usernames of the users as you will need this throughout the lab.

### **Task 5: Add membership for DomainJoinAdmin User**

In this task we will add "*DomainJoinAdmin*" to *AAD DC Administrators* group which will enable this user to domain join session hosts to the Azure Active Directory Domain Services.

1. Navigate back to Azure active directory page, click on **Users** under **Manage** blade .

   ![ws name.](media/t.png)
   
2. Click on **DomainJoinAdminUser** to open it.

   ![ws name.](media/wvd34.png)

3. Select **Groups** under **Manage**, then click on **+ Add Membership**. 

   ![ws name.](media/u.png)

4. Select **AAD DC Administrators**. Once selected it will show up under selected groups then click on **Select**.

   ![ws name.](media/v.png)

5. This will show the added membership.

   ![ws name.](media/x.png)

### **Task 6: Change passwords for the users created**

1. Now we will run a script to change passwords for the users created, as user needs to reset password after registering to AADDS.

2. Copy and paste the following script and hit **Enter**.

   ```
   $domain = ((Get-AzADUser | where {$_.Type -eq "Member"}).UserPrincipalName.Split('@'))[1]
   $password= ConvertTo-SecureString "Azure1234567" -AsPlainText -Force
   $users = @("domainjoinadmin@$domain","wvduser-01@$domain","wvduser-02@$domain")
   $users | foreach{
       Update-AzADUser -UserPrincipalName $_ -Password $password
   }
   ```
   
    ![ws name.](media/wvd23.png)

> **Note:** Wait for few seconds for the script to execute.
   
3. Output of the script will be similar to the one shown below.

   ![ws name.](media/42.png)

4. Click on the **Next** button present in the bottom-right corner of this lab guide.

