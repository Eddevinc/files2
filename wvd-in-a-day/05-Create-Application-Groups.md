# Exercise 5: Create Application Groups

Application Group is a logical grouping of applications installed on session hosts in the host pool. They are of two types:
1.	Remote App
2.	Desktop
The default app group created for a new Windows Virtual Desktop host pool also publishes the full desktop. In addition, you can create one or more RemoteApp application groups for the host pool.

### **Task 1: Verify application group of type ‘Desktop’ is created by default**

An application group of type ‘Desktop’ was created automatically while creating the Session Host in previous exercise. We will be verifying if that host pool is created automatically or not..


1. Navigate to Azure portal, then search for **Windows** in search bar and select **Windows Virtual Desktop**.

   ![ws name.](media/y.png)
   
2. Click on **Application Groups** and verify that a application group named **WVD-HP-01-DAG** and **WVD-HP-02-DAG** is already present.

   ![ws name.](media/a17.png)
  
  
## Task 2: Create application groups of type Remote App

1. In the Application group page click on **+ Add**.

   ![ws name.](media/a18.png)
  
2. On the **Basics** section, fill the parameters as below: 
   
      - Subscription: Choose the **default subscription**.

      - Resource Group: Choose the exiting Resource Group that is **WVD-RG**.

      - Host Pool: **WVD-HP-01** (*This application group will be created under WVD-HP-01 hostpool*)

      - Location:  Choose the default location

      - Application Group Type: **RemoteApp** 

      - Application Group Name: **WVD-AG-01**

      - Click on **Review + create**
  
     ![ws name.](media/a19.png)

3. Click on **Create**.

   ![ws name.](media/a20.png)
   
4. Click on the **Next** button.
