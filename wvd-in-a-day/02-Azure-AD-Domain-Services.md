# Exercise 2: Azure AD Domain Services 


## Task 1: Create prerequisites for AADDS 


1. Go to **https://portal.azure.com/** to access your Azure portal

2. Sign in into your Azure Account.

3. In Azure Portal search for **Subscription** and click on **Subscriptions**.

   ![ws name.](media/1.png)
   
4. Click on your subscription.

   ![ws name.](media/2.png)
   
5. Under Settings blade Click on **Resource Providers**.

   ![ws name.](media/3.png)
   
6. Now search for **Microsoft.AAD** and make sure Microsofr.ADD is **Registered**.

   ![ws name.](media/4.png)
   
*In case the **Microsoft.AAD is not registred** then follow the **step 7** *.

7. Click on **Microsoft.AAD** Resource Provider and click on **Register**.

   ![ws name.](media/5.png)

## Task 2: Deploy AADDS


1. Go to **https://portal.azure.com/** to access your Azure portal

2. Sign in into your Azure Account.

3. On the Azure portal menu or from the Home page, select Create a resource.

4. Enter Domain Services into the search bar, then choose Azure AD Domain Services from the search suggestions.
   ![ws name.](media/6.png)

5. On the Azure AD Domain Services page, click on **Create**.
   ![ws name.](media/7.png)
    
6. Configure Basics blade with following settings.
   
   ![ws name.](media/8.png)
   

   **Subscription**: Select your subscription.
   **Resource Group** : Select *WVD-RG*
   **DNS domain name**: *Default value*
   **Region**: select *East US*
   **SKU**: *Standard*
   **Forest type**: *User*

   Then click **Next**
       
7. In Networking tab under **Virtual network**, Click on **Create new**.
        
   ![ws name.](media/9.png)

8. Configure your new virtual network with following settings

   ![ws name.](media/10.png)

**Name**: *aaddss-vnet-01*
**Address Space**
        **Address range**: *10.1.0.0/16*
        
**Subnetes**
        **Subnet name**: *aadds-subnet*        **Address range**: *10.1.0.0/24*
        **Subnet name**: *SessionHost-Subnet*  **Address range**: *10.1.1.0/24*

Click **Ok**

9. You will return to Networking tab, Here Select the following Subnet.

   ![ws name.](media/11.png)
     
**Virtual network**: *(new)aadds-vnet-01* 
**Subnet**: *(new)aads-subnet-01 (10.0.0.0/24)*

Click on **Next**


10. Click on **Review + Create** button.

    ![ws name.](media/12.png)

11. click on **Create** Button.

    ![ws name.](media/13.png)
    
12. A popup will appear, Click on **OK**.

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
    
5. Now go back to your **WVD-RG* resource group** and click on **second NIC card**.

    ![ws name.](media/19.png)
    
6. Note down the **Private IP** of the second NIC card.

    ![ws name.](media/20.png)
    
7. Go back to the **WVD-RG** resource group, and click on **aadds-vnet-01**.

    ![ws name.](media/21.png)
    
8. Now under settings bade click on **DNS servers**.

    ![ws name.](media/22.png)
    
9. Under DNS server select **custom** and add the IP address of first and second NIC card noted in step 19 and 21.
    
     ![ws name.](media/23.png)
     
     click on **Save**.



## Create new AD users



1. In you Azure portal search bar, search for **Azure Active Directory** and click on it.

   ![ws name.](media/24.png)



2. In Azure active directory page, left side under Manage blade click on **Users**.

   ![ws name.](media/25.png)
   
   
   
3. Click on **+ new user**.

   ![ws name.](media/26.png)
   
   
   
4. Under identity section enter following configuration.
   
   ![ws name.](media/27.png)   
 
   **Username**: *WVDUser-01*
   
   **Name**: *WVDUser-01*
   
5. Under pasword section use following configuration.

   ![ws name.](media/28.png)
   
   select **Let me create the password**
   
   **Initial Passowrd**: *Azure1234567*
   
   Leave remaining values at default.
   
   Click on **Create**.
   
6. To create another user click on **+ New user**.

   ![ws name.](media/29.png)

7. Under identity section enter following configuration.
   
   ![ws name.](media/30.png)   
 
   **Username**: *WVDUser-02*
   
   **Name**: *WVDUser-02*
   
8. Under pasword section use following configuration.

   ![ws name.](media/31.png)
   
   select **Let me create the password**
   
   **Initial Passowrd**: *Azure1234567*
   
   Leave remaining values at default.
   
   Click on **Create**.
   
   
9. To create another user click on **+ New user**.

    ![ws name.](media/32.png)  
    
    
    
10. Under identity section enter following configuration.
   
   ![ws name.](media/33.png)   
   
   
 
   **Username**: *DomainJoinAdminUser*
   
   **Name**: *DomainJoinAdminUser*
   
8. Under pasword section use following configuration.

   ![ws name.](media/34.png)
   
   
   
   select **Let me create the password**
   
   **Initial Passowrd**: *Azure1234567*
   
   Leave remaining values at default.
   
   Click on **Create**.
   
   
9. Under Groups and roles section click on **0 groups selected**

   ![ws name.](media/35.png)
   
   
10. Click on **AAD DC Administrators**.

    ![ws name.](media/36.png)
   
    
    
11. Click on **Select** 

    ![ws name.](media/37.png)
    
     
    Leave remaining values at default.
   
    Click on **Create**.
    
    

### Task 6: Change passwords for the users created


1. In your azure portal, click on the **cloud Shell** icon.

   ![ws name.](media/38.png)
   
   
   
2. Cloud shell was ask you to create a storage account, Click on **Create storage**.

   ![ws name.](media/39.png)
   
   Wait for few seconds for the cloud shell to launch.
   
   
3. After the terminal launches it will look like this.

   ![ws name.](media/40.png)
   
   
   
4. Copy the following power shell script and paste it in the cloud shell and hit enter.

   ![ws name.](media/41.png)
   
   
   
   
        #POWER SHELL SCRIPT

        $UserPricipalNames = @("DomainJoinAdminUser@azurehol1055.onmicrosoft.com","WVDUser-01@azurehol1055.onmicrosoft.com","WVDUser-02@azurehol1055.onmicrosoft.com") #End user UPN
       $UserPassword = "Azure1234567" #End user password
       $UserPasswordhash = ConvertTo-SecureString $UserPassword -AsPlainText -Force

       Foreach($UserPricipalName in $UserPricipalNames){ 
       #Update user password    
       Update-AzADUser -UserPrincipalName $UserPricipalName -Password $UserPasswordhash
       }




   Wait for few seconds for the script to execute.
   
   
   
5. Output of the script will look like this.

   ![ws name.](media/42.png)
