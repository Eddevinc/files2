# Exercise 10: Access Applications

In this exercise we will be using browser and WVD-client to access our remote app and desktop applications we deployed in previous exercises.

## **Task 1: Access published applications via Web browser**

In the following task we will be accessing microsoft office word and desktop application with browser using WVDUser-01.

### **Subtask 1: Access the published 'Remote Application' using browser**

In this task we will be accesing the remote application i.e. microsoft office word with help of a browser using WVDUser-01. 

1. Navigate to **Azure Active Directory**, then go to **Users**.

   ![ws name.](media/t.png)

2. Copy username of **WVD User-01** and **WVD User-02** and paste it notepad for further use.

   ![ws name.](media/a45.png)

3. Now paste this link ```aka.ms/wvdarmweb``` in your browser and enter your **credentials** to login. 

   - Username: *Put username* **WVD User-01** *which you copied in previous step (for example: **WVDUser-01@azurehol1055.onmicrosoft.com**). Then click on **Next**.*
   
   ![ws name.](media/wvd42.png)

   - Password: **Azure1234567** *and click on* **Sign in**.

   ![ws name.](media/wvd43.png)
  

4. Now you will be inside the WVD dashboard with all the Desktop and Applications assigned to WVDUser01 accesable. Click on any one of the applications to access it. 

   ![ws name.](media/a46.png)


5. Click on **Allow** for the prompt.

   ![ws name.](media/128.png)


6. Enter your **credentials** to access the application and click on **Submit**.

   - Username: Put the username of **WVD User-01** which you copied in previous task (for example: **WVDUser-01@azurehol1055.onmicrosoft.com**)
   
   - Password: **Azure1234567**

   ![ws name.](media/a47.png)
   
   
7. Your application will launch.

   ![ws name.](media/130.png)

### **Subtask 2: Access the published 'Desktop Application' using browser**

In this task we will access the desktop application i.e. WVD-HP-01-DAG with help of a browser using WVDUser-01

1. Now on the top-left side click on **All Resources** and select **WVD-HP-01-DAG** Desktop.

   ![ws name.](media/wvd53.png)

2. Click on **Allow** for the Prompt.

   ![ws name.](media/133.png)


3. Enter your **credentials** to access the application and click on **Submit**.

   - Username: Put the username of **WVD User-01** which you copied in previous task (for example: **WVDUser-01@azurehol1055.onmicrosoft.com**)
   
   - Password: **Azure1234567**
   
   ![ws name.](media/a47.png)


4. Your virtual Desktop will launch and it should be ready to use. 

   ![ws name.](media/wvd39.png)


## **Task 2: Access published applications via WVD-Client**

In the following task we will be accessing microsoft office word and desktop application with help of remote desktop client using WVDUser-01.

### **Subtask 1: Access the published 'Remote Applications' using WVD Client**

In this task we will be accesing the remote application i.e. microsoft office word with help of WVD-client using WVDUser-02. 
   
1. Download the **WVD Client** on your local machine from the following [**Link**](https://docs.microsoft.com/en-us/azure/virtual-desktop/connect-windows-7-and-10) and install **Windows 64-bit** version by clicking on it.
   
   ![ws name.](media/a48.png)

2. Then open the setup and click **Next** on the Welcome page of setup.

3. Check the agreement box and click on **Next**.

4. On **Installation scope** window select **Install just for you** and then click on **Install**.

   ![ws name.](media/wvd41.png)

5. After installation, in your local machine go to **Start** and search for **Remote desktop** and open the application with exact icon as shown below.

   ![ws name.](media/137.png)
    
6. After the application opens click on **Subscribe**.

   ![ws name.](media/a49.png)

7. Enter your **credentials** to access the workspace.

   - Username: Put the username of **WVD User-02** which you copied in previous task (for example: **WVDUser-02@azurehol1055.onmicrosoft.com**), then click on **Next**
   
   ![ws name.](media/wvd40.png)

   - Password: **Azure1234567** and click on **Sign in**.
   
   ![ws name.](media/wvd44.png)
 
8. Make sure to **uncheck** Allow my organization to manage my device and then click on **No, sign in to this app only**.

   ![ws name.](media/w17.png)
    
    
9. Now click on **Done** for **You're all set**.

   ![ws name.](media/a51.png)

10. Now you will be inside the WVD dashboard with all the desktop and applications assigned to **WVDUser02**. Double click on any one of the applications to access it. 

   ![ws name.](media/142.png)


11. Click on **Allow** for the prompt.

   ![ws name.](media/143.png)


12. Enter your **credentials** to access the application and click on **Submit**.

   - Username: Put the username of **WVD User-02** which you copied in previous task (for example: **WVDUser-02@azurehol1055.onmicrosoft.com**)
   - Password: **Azure1234567**
   
   ![ws name.](media/a52.png)
   
13. Your application will launch.

   ![ws name.](media/145.png)
   
14. Click on the **X** icon on the top right corner to close word.

### **Subtask 2: Access the published 'Desktop Application' using WVD Client**

In this task we will access the desktop application i.e. WVD-HP-01-DAG with help of a WVD-Client using WVDUser-02.


1. Now in the WVD client double click on the **WVD-HP-02-DAG** Desktop to access it. 

   ![ws name.](media/a53.png)

2. Enter your **credentials** to access the application and click on **Submit**.

   - Username: Put the username of **WVD User-02** which you copied in previous task (for example: **WVDUser-02@azurehol1055.onmicrosoft.com**)
   - Password: **Azure1234567**
   
   ![ws name.](media/a52.png)
   

4. Your virtual Desktop will launch and it should be ready to use. 

   ![ws name.](media/a54.png) 


5. Click on the **Next** button.




