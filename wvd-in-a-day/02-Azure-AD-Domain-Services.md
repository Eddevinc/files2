# Exercise 2: Azure AD Domain Services 

### **Task 1: Create prerequisites for AADDS**

1. Launch **Azure Portal** (https://portal.azure.com) in the desktop on left side. You can use the shortcut on the desktop. You'd be asked to choose default browser configurations, You can skip those for now by clicking cancel.

2. Login to Azure with the **username** **<inject key="AzureAdUserEmail" />**

   ![](media/wvd1.png)

3. Enter **password** **<inject key="AzureAdUserPassword" />** and click on **Sign in**.

   ![](media/wvd2.png)
   
> Refer the **Environment Details** tab for any other lab credentials/details.
  
  ![](media/wvd7.png)

4. There will be a pop-up entitled **Stay signed in?** with buttons for **No** and **Yes** - Choose **Yes**.

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
       
7. Then click **Next**.

8. In Networking tab under **Virtual network**, Click on **Create new**.
        
   ![ws name.](media/wvd8.png)

9. Configure your new virtual network with following settings and then click **Ok**.

   - **Name**: **aaddss-vnet-01**
   - **Address range**: **10.1.0.0/16**

Add following subnets:
   - **Subnet name**: **aadds-subnet**
   - **Address range**: **10.1.0.0/24**
   - **Subnet name**: **SessionHost-Subnet**
   - **Address range**: **10.1.1.0/24**

  ![ws name.](media/10.png)

10. You will return to Networking tab, Here Select the following Subnet.
     
   - **Virtual network**: **(new)aadds-vnet-01** 
   - **Subnet**: **(new)aads-subnet-01 (10.0.0.0/24)**

   ![ws name.](media/11.png)

Click on **Next**.

11. Click on **Review + Create** button.

    ![ws name.](media/12.png)

12. Now click on **Create** Button.

    ![ws name.](media/13.png)
    
13. A popup will appear, Click on **OK**.

    ![ws name.](media/14.png)
    
    Wait for the Deployment to complete, It will take approx 30 minutes to deploy.


## Task 3: Update Virtual Network DNS

1. After deployment completes, go back to Azure portal home, and search for resource group and click on **Resource Groups**.
    
   ![ws name.](media/15.png)
    
2. Now click on your **WVD-RG** resource group.

   ![ws name.](media/16.png)
    
3. Click on **first NIC card**.

   ![ws name.](media/17.png)
    
4. Note down the **Private IP** of first NIC card.

    ![ws name.](media/18.png)
    
5. Now go back to your **WVD-RG** resource group and click on **second NIC card**.

    ![ws name.](media/19.png)
    
6. Note down the **Private IP** of the second NIC card.

    ![ws name.](media/20.png)
    
7. Go back to the **WVD-RG** resource group, and click on **aadds-vnet-01**.

    ![ws name.](media/21.png)
    
8. Now under settings bade click on **DNS servers**.

    ![ws name.](media/22.png)
    
9. Under DNS server select **custom** and add the IP address of first and second NIC card noted in step 19 and 21.
    
    ![ws name.](media/23.png)
     
10. click on **Save**.
     
    ![ws name.](media/wvd9.png)

## Task 4: Create new AD users

1. In you Azure portal search bar, search for **Azure Active Directory** and click on it.

   ![ws name.](media/24.png)

2. In Azure active directory page, left side under Manage blade click on **Users**.

   ![ws name.](media/25.png)
   
3. Click on **+ new user**.

   ![ws name.](media/26.png)
     
4. Under identity section enter following configuration.
   
   ![ws name.](media/27.png)   
 
   - **Username**: **WVDUser-01**
   
   - **Name**: **WVDUser-01**
   
5. Under pasword section use following configuration.

   - **Password**: Select **Let me create the password**
   
   - **Initial Passowrd**: **Azure1234567**
   
   ![ws name.](media/28.png)

6. Leave remaining values at default and click on **Create**.
      
7. To create another user click on **+ New user**.

   ![ws name.](media/29.png)

8. Under identity section enter following configuration.

   - **Username**: **WVDUser-02**
   - **Name**: **WVDUser-02**
   
    ![ws name.](media/30.png)   
   
9. Under pasword section use following configuration.
   
   **Password**: Select **Let me create the password**
   
   **Initial Passowrd**: **Azure1234567**
   
10. Leave remaining values at default and click on **Create**.
   
   ![ws name.](media/31.png)   
   
11. To create another user click on **+ New user**.

   ![ws name.](media/32.png)  

12. Under identity section enter following configuration.
      
   - **Username**: **DomainJoinAdminUser**
   
   - **Name**: **DomainJoinAdminUser**

   ![ws name.](media/33.png)   
   
13. Under pasword section use following configuration.
   
   - **Password**: Select **Let me create the password**
   
   - **Initial Passowrd**: **Azure1234567**
   
14. Leave remaining values at default and click on **Create**.
   
   ![ws name.](media/34.png)
 
15. Under Groups and roles section click on **0 groups selected**

   ![ws name.](media/35.png)
   
16. Click on **AAD DC Administrators**.

    ![ws name.](media/36.png)
    
17. Click on **Select** 

    ![ws name.](media/37.png)
    
18. Leave remaining values at default and click on **Create**.
   
### Task 5: Change passwords for the users created

1. In your azure portal, click on the **cloud Shell** icon.

   ![ws name.](media/38.png)
   
2. Cloud shell was ask you to create a storage account, Click on **Create storage**.

   ![ws name.](media/39.png)
   
   Wait for few seconds for the cloud shell to launch.
   
3. After the terminal launches it will look like this.

   ![ws name.](media/40.png)
      
4. Copy the following power shell script and paste it in the cloud shell and hit enter.

   ![ws name.](media/41.png)
   
   ```
      #POWER SHELL SCRIPT

     $UserPricipalNames = @("DomainJoinAdminUser@azurehol1055.onmicrosoft.com","WVDUser-01@azurehol1055.onmicrosoft.com","WVDUser-02@azurehol1055.onmicrosoft.com") #End user UPN
    $UserPassword = "Azure1234567" #End user password
    $UserPasswordhash = ConvertTo-SecureString $UserPassword -AsPlainText -Force

    Foreach($UserPricipalName in $UserPricipalNames){ 
    #Update user password    
    Update-AzADUser -UserPrincipalName $UserPricipalName -Password $UserPasswordhash
    }
```

Wait for few seconds for the script to execute.
   
5. Output of the script will look like this.

   ![ws name.](media/42.png)

6. Click **Next** on the bottom right of this page.
