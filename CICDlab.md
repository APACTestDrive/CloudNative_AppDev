# Cafe Supremo CICD Hands On Lab / Demo Setup Guide

## Preparing Database Cloud Service, Java Cloud Service, Application Container Cloud Service and Developer Cloud Service

Before you can run the demo, you must provision the required cloud services for Developer Cloud Service to build and deploy to a WebLogic Server cluster in Java Cloud Service and ACCS instance. You must have access to a:

- GitHub account
- Oracle Public Cloud Service account including Java, Application Container, Developer, Database and Storage Cloud Service

  The current release of the demo no longer requires a database for storing customer data. Customer data is now stored in a ACCS cache and is automatically populated upon the start up of the RewardService ACCS instance.

  Oracle Database Cloud Service is required by Java Cloud Service to host the Oracle Fusion Middleware component schemas used by Oracle Java Required Files (JRF).



## Provision a DBCS Instance

### **STEP 1**: Sign Into The Oracle Cloud Service Account

- Sign into your Oracle Cloud Service account.



### **STEP 2**: Create a DBCS Instance

- On the dashboard click the hamburger icon on the **Database** tile. Select **Open Service Console**.

  ![](images/01.png)

- Once in the Database Cloud Service Console page, create a new instance by clicking **Create Service** button.


### **STEP 2.1**: Basic Instance Configuration

- Complete the new Create New Instance Page as illustrated below:

  ![](images/02.png)

  
  - Enter the Instance Name e.g. *demoDB*
  
  - Select the default *12c Release 1 Software*

  - Select the *Enterprise Edition software edition*

  - Select *Single Instance* for the Database Type

  - Click the **Next** button



### **STEP 2.2**: Detailed Instance Configuration

The last input page is the Service Details page.

  ![](images/03.png)
  
  
The following parameters have to be provided:

  + **Administration Password**: DB's password. Don't forget to note the provided password
  + **Compute Shape**: number of OCPU and size of the RAM. Choose the smallest (default) one is OC3
  + **SSH Public Key**: public key which will be uploaded to the VM during the creation. It allows to connect to the VM through ssh connection using the private key. Use the same publicKey what was generated for Database Cloud Service instance. Click on Edit button and select [previously saved (during Database Cloud Service creation)](../dbcs-create/README.md) `GIT_REPO_LOCAL_CLONE/cloud-utils/publicKey`. You can also copy the content of *publicKey* into Key Value field. If you don't have or want to  to create different keypair select **Create a New Key** option and download the newly generated keypair for later usage.
  + **Storage Username**: Your Oracle Cloud username
  + **Storage Password**: Cloud Account password
  + **Create Cloud Storage Container**: Leave to default of checked
  + **Create Instance from Existing Backup**: Leave to default of No

- Click **Next**

- Confirms the details on the next page and then create

**NOTE**:  You DBCS instance will take about 30 minutes to complete. Please wait until the DBCS instance has been completed before creating a JCS instance.




## Provision a JCS Instance


### **STEP 3**: Create a JCS Instance

- On the dashboard click the hamburger icon on the **Java** tile. Select **Open Service Console**.

  ![](images/04.png)
  

- Once in the Java Cloud Service Console page, create a new instance by clicking **Create Service** button.


### **STEP 3.1**: Basic Instance Configuration

- Complete the new Create New Instance Page as illustrated below:

  ![](images/05.png)

  - Enter the Instance Name e.g. *demoJCS*

  - Select the default *Java Cloud Service*

  - Select the default *12c Software Release (12.2.1.2)*

  - Select the *Enterprise Edition software edition*

  - Click the **Next** button.


### **STEP 3.2**: Detailed Instance Configuration

The last input page is the Service Details page.

- Click the **Advanced** button to show additional configuration options.

  ![](images/06.png)

The following parameters have to be provided:
	
  + **Shape**: number of OCPU and size of the RAM. Choose the smallest (default) one is OC3.
  + **Server Count**: 1. Which means one managed server.
  + **Domain Partitions**: Take the default zero for no partitions.
  + **Enable Administration Console**: because this instance will be available on public internet the default is that the WebLogic Admin console is not enabled. Do not forget to check in to get access to the Admin console.
  + **SSH Public Key**: public key which will be uploaded to the VM during the creation. It allows to connect to the VM through ssh connection using the private key. Use the same publicKey what was generated for Database Cloud Service instance. Click on Edit button and select [previously saved (during Database Cloud Service creation)](../dbcs-create/README.md) `GIT_REPO_LOCAL_CLONE/cloud-utils/publicKey`. You can also copy the content of *publicKey* into Key Value field. If you don't have or want to  to create different keypair select **Create a New Key** option and download the newly generated keypair for later usage.
  + **Username**: username of WebLogic administrator. For demo purposes you can use: weblogic
  + **Password**: WebLogic administrator's password. Don't forget to note the provided password.
  + **Deploy Sample Application**: Unchecked.
  + **Database Configuration / Name**: Database Cloud Service name to store WebLogic repository data. Basically the list populated with database services within the same identity domain.
  + **Database Configuration / PDB Name**: pluggable database service identifier of the Database Cloud Service instance -provided above- which will be used to store repository schema. If you have choosen default (PDB1) during Database Cloud Service creation then leave the default here too.
  + **Database Configuration / Administrator User Name**: Enter: **sys**. DBA admin to create repository schema for Java Cloud Service instance.
  + **Database Configuration / Password**: DBA admin password you provided during Database Cloud Service creation.
  + **Database Configuration / For Application Schema**: Leave this No Application Schema added.
  + **Provision Load Balancer**: the save resources for sample application we will not create Load Balancer instance. Leave default: No
  + **Backup Destination**: Leave to default of None.

- Click **Next**

The final page is the summary page about the configuration before submit the instance creation request.

- Click **Create** to start the provisioning of the new service instance.


When the request has been accepted the Java Cloud Service Console page appears and shows the new instance. The instance now is in Maintenance (Progress) mode. Click **In Progress** link to get more information about the status.

**NOTE**: Your JCS instance will be ready in about 30 minutes.



## Provision a ACCS Cache Instance


### **STEP 4**: Create a ACCS Cache Instance

- On the dashboard click the hamburger icon on the **Application Container** tile. Select **Open Service Console**.

  ![](images/07.png)
  

- Once in the Application Container Cloud Service Console page, click the hamburger icon by the **Welcome!** text at the top right corner and then select **Application Cache** by scrolling down the the list as shown below.

  ![](images/08.png)


- On the Application Caches page, create a new instance by clicking **Create Instance** button.



### **STEP 4.1**: Instance Configuration

- Complete the new Create New Instance Page as illustrated below:

  ![](images/09.png)

  - Enter the Instance Name e.g. *rewardpoints*
  
  - Leave evertyhing else to their defaults
  
  - Click **Next**
  
  - Click **Create** on the Confirm Page
  
**NOTE**: Your ACCS Cache instance will be ready in about 15 minutes.

Once completed, you will find the **rewardpoints** cache instance available to use.

  ![](images/10.png)



## Provision a ACCS Application Instance


### **STEP 5**: Create a ACCS Application Instance

- Go back to the Application Container Service Console.

- Click **Create Application**

- Click on the **Node** icon to select your application platform in the Create Application popup box

  ![](images/11.png)
  
  
  - Enter the **Application Name** e.g. *rewardservice*

  - Click on Choose File to Upload Archive *pointsystem.zip* for **Application**
  
    - The pointsystem.zip can be download it from this link. You must be an Oracle employee to download this.

  - Select 1 **Instances**
  
  - Select 1 GB of **Memory (GB)**

  - Click on **More Options** to expand the configuration page

  - Click the **Application Cache** dropdown box to select the **rewardpoints** Application Cache you created in the previous step

- Click **Create**

  ![](images/12.png)


**NOTE**: Your ACCS Application instance will be ready in about 10 minutes.

Once ready, you will find your rewardservice available in the Application Container Service Console.

- Take note of the instance URL.

  ![](images/13.png)




## Provision a Developer Cloud Service

You have two choices for Developer Cloud Service:

  - Traditional Developer Cloud Service
  - Autonomous Developer Cloud Service
  
It is recommeneded to use the Autonomous Developer Cloud Service as it offers a dedicated Buid VM for your build jobs. This has significant performance improvement over the Traditional Developer Cloud Service and would result in much faster build time. This is particularly important so that the customer would see the full potential of our service and would bring out the key benefits in Continuous Integration and Continuous Delivery.



### **STEP 6**: Open the Automonous Developer Cloud Console

- Ensure you are opening the Autonomous DevCS and NOT the Traditional DevCS

- On the dashboard click the hamburger icon on the **Developer** tile. Select **Open Service Console**

  ![](images/14.png)



### **STEP 6.1**: Create a VM Template

- Select **Organization** from the User dropdown options

  ![](images/15.png)

- Select **VM TEMPLATES** on the Organization page

  ![](images/16.png)
  
- Click on **New Template**

![](images/20.png)


- Enter **CafeSupremo** as the name in the **New VM Template** popup box

- Select **Oracle Linux 7** for the **Platform** from the dropdown list

- Click **Create**


### **STEP 6.2**: Configure the Software Packages for The VM Template

The CafeSupremo template would be created and we need to put the necessary build software on it.

  - Click on **Configue Software** for the CafeSupremo template

  ![](images/17.png)
  
  - Select **Gradle 4** and **Node.js 6** from the list of software
  
  ![](images/18.png)
  
  - Click on **Done** to save the selections
  
    ![](images/19.png)
  
  
  
### **STEP 6.3**: Create a Virtual Machine for Build and Develop

- Go back to the Organization Page

- Select **VIRTUAL MACHINES**

  ![](images/21.png)
  
- Click on **Configure Compute Account**

  ![](images/22.png)

- Enter your **Cloud Username** and **Password**

- Enter **Service Instance ID**

  You can locate the Service Instance ID on the Compute Classic View Details Page

- Enter the **REST Endpoint**

  You can locate the REST Endpoint on the Compute Classic View Details Page

  ![](images/23.png)
  
- Click on **Save**

You have now created build VM for for your build jobs.

  ![](images/24.png)



  
### **STEP 7**: Create a Developer Cloud Project

We will create a project for the Café Supremo and bring all the components into this project so that we can automate the build and deploy process.


### **STEP 7.1**: Create a DevCS Project

- Go back to the Developer Cloud Service Console

- Create a new project by clicking the **New Project** button

- Click **Next**

  ![](images/25.png)

- Select an **Empty Project**

  We will upload the Git repositories later from an archive project

  ![](images/26.png)

- Click **Next**

- Click **Finnish**

Project creation will start upon selecting Finish.




### **STEP 7.2**: Setup The Storage For Importing a Project Archive

A previously configured CafeSupremo project was exported as an archive. This contains the Git repositories, mock issues and agile boards to give a sense of a realistic project.

**NOTE :** You can download the project archive from here.


- You must first configure storage to upload the project archive to

- Go back to the Organization Page

- Select **STORAGE**

  ![](images/27.png)
  
- Click on **Create**

- Enter the **Service ID** for your storage

  The Service ID is made up of **Storage-domain ID**

- Enter your Cloud **Username** and **Password**

- Enter the **Authorization URL**

  You can locate the Authorization URL on the Storage Classic View Details Page

  ![](images/28.png)
  
  
- Click on **Save** to save the configuration
  
- Click on **Test Connection** to validate connectivity to your storage and you should get a **Connection successful** in response

  ![](images/29.png)
  

- Click **Close** on the top right hand corner of the Organization page



### **STEP 7.3**: Uploading The Project Archive To The Storage

- On the dashboard click the hamburger icon on the **Storage Classic** tile. Select **Open Service Console**.

  ![](images/30.png)

- Click **Create Container** to create a storage area for uploading your project archive to

  ![](images/31.png)
  
- Enter a name for your container e.g. *tmp*

- Click on **Create**

  ![](images/32.png)
  
- Click on the container you just created e.g. *tmp*

- Click **Upload Objects**

  ![](images/33.png)
  
- Select your archive file and upload  

The project archive is now ready to be imported into the CafeSupremo project.




### **STEP 7.4**: Importing a Project Archive

- Click on the **Administration** option on the left hand side of the page

- Select **Data Export/Import** from the popup context option list or from the Administration page

  ![](images/34.png)
 
- Complete the Data Export/Import Page as illustrated below:

  ![](images/35.png)

  
  - Enter the **Storage Service URL**
  
    You can locate the Storage Service URL on the Storage Classic View Details Page
  
  - Enter **Storage** as the **Storage Service Name** 

  - Enter the **Identity Domain**

  - Enter the **Storage Account Username** and **Storage Account Password**
  
  - Click on **Connect**

- Once connected, the page will expand with the **Create Job** configuration.

  ![](images/36.png)

  - Select *Import* from the dropdown **Type**

  - Enter *tmp* as the **Storage Container**
  
  - Select the zip file from the **Storage Object** dropdown box
  
  - Click on **Import**
  
  - Check the *Import project data into 'CafeSupremo'* and Click **Yes**
  
    ![](images/37.png)
  
**NOTE:** The project archive import will begin and will take about 10 minutes to complete

Once completed, you will be presented with two Git repositories:

  - **CafeSupremo.git** that holds the JETUI frontend code
  - **RewardService.git** that holds the Node.js microservice code
  
  ![](images/38.png)
  
- Verify the rest of the project has been imported properly. There should be issues, agile boards, and code. However, there are no project members, build jobs or deployment configuration as these cannot be exported with the original project. Therefore we would need to rebuild these configurations in the following steps.


### **STEP 8**: Add New Members To The Project

Project members are not exported with the orignal project, therefore we will need to a add users back into the project. These users are referenced in the issues tracking and agile board. Hence we will need to add the users back into the project.

- Go to the project home page

- Click on the **Team** icon to see the project members

  There should just be the owner of the project e.g. *cloud.admin*

  ![](images/39.png)
  
- Click on **New Member**

- Select all the users from the available user list and click **Add**

  ![](images/40.png)
  
- Your now have all the users required for the demo

  ![](images/41.png)



### **STEP 9**: Create The Build Pipelines

Now that we have imported the project archive for our demo, we can start creating our CICD pipelines for the JETUI frontend and the RewardSerivce microservice. This includes creating the build job configurations and the deployment configurations.



### **STEP 9.1**: Create The JETUI Frontend Build Job

The first task in our CICD pipeline is to build the JETUI frontend application. We need to create a build job for this. And we want the build to be triggered automatically whenever there is a code change. The build process can be automated and deployed automatically to a designated JCS environment.

- Switch to Build tab and create a new job. There should be no build job initially.

- Click on **New Job**

  ![](images/42.png)
  
- Enter *JETUI_JCS_Build* as the **Job Name**

- Select *CafeSupremo* from the dropdown list for **Software Template**

- Click **Create Job**

  ![](images/43.png)

- Configure the build job by specifying the Git repo to build from. On the **Source Control** tab click on **Add Source Control**

  ![](images/44.png)

- Select *CafeSupremo.git* from the dropdown box for **Repository**

  ![](images/51.png)
  
- On the **Builders** tab click on **Add Builder** button and select **Unix Shell Builder** from the context option list

  ![](images/45.png)
  
- Copy and paste the npm install script into the command field

  ```
  npm install
  grunt build:release
  cd web
  mkdir WEB-INF
  cp ../JCS/web.xml WEB-INF/web.xml
  zip -r ../target/cafesupremo.war *
  ```

  ![](images/46.png)

- On the **Post Build** tab click on **Add Post Build Action** button and select **Artifact Archiver** from the context option list

  ![](images/47.png)
  
- Enter `target/cafesupremo.war` in the *Files to archive* field to define the location of the build output which it will be used for deployment.

  ![](images/48.png)
  
- To enable automated build we need to set the trigger to be based on code commit in the Git repo. Select **SCM Polliing Trigger** from the **Add Trigger** context dropdown list

  ![](images/49.png)
  
- Change the SCM Polling Trigger from the default 30 minutes to 1 minutes in the **Cron Pattern in UTC** field

  ![](images/53.png)

- Click **Save** to save the configuration

- Let’s test the build job configuration by running a build now. Click **Build Now** to build

- The build should complete without any error

  ![](images/54.png)
  
  You have now completed the **JETUI_JCS_Build** build job
  
  
  
  
### **STEP 9.2**: Create The Reward Service Node.js Build Job
  
  
- Switch to Build tab and create a new job. There should be no build job initially.

- Click on **New Job**
  
- Enter *RewardService_Build* as the **Job Name**

- Select *CafeSupremo* from the dropdown list for **Software Template**

- Click **Create Job**


- Configure the build job by specifying the Git repo to build from. On the **Source Control** tab click on **Add Source Control**

- Select *RewardService.git* from the dropdown box for **Repository**

  ![](images/55.png)
  
- On the **Builders** tab click on **Add Builder** button and select **Unix Shell Builder** from the context option list

  
- Copy and paste the npm install script into the command field

  ```
  npm install
  zip -r rewardservice.zip *
  ```

  ![](images/56.png)

- On the **Post Build** tab click on **Add Post Build Action** button and select **Artifact Archiver** from the context option list
  
- Enter `*` in the *Files to archive* field to define the location of the build output which it will be used for deployment.

  ![](images/57.png)
  
- To enable automated build we need to set the trigger to be based on code commit in the Git repo. Select **SCM Polliing Trigger** from the **Add Trigger** context dropdown list
  
- Change the SCM Polling Trigger from the default 30 minutes to 1 minutes in the **Cron Pattern in UTC** field

  ![](images/53.png)

- Click **Save** to save the configuration

- Let’s test the build job configuration by running a build now. Click **Build Now** to build

- The build should complete without any error

  ![](images/58.png)
  
  You have now completed the **RewardService_Build** build job



### **STEP 9.3**: Create The JETUI Frontend Deployment Configuration

Let's create a deployment configuration for the JET UI Frontent. The deployment runtime is the JCS environment which you previsioned earlier.

- Go to the **Deploy** page

- Click on **New Configuration**

  ![](images/59.png)

- Complete the New Create New Configuration as illustrated below:

  - Enter `Deploy2cafesupremoJETUI` as the **Configuration Name**

  - Enter `cafesupremo` as the **Application Name**
  
  - Check *Automatic*

  - Select *JETUI_JCS_Build* from the **Job** option list

  - Select *target/cafesupremo.war* from the **Artifact** option list

  - Click the **New** button
  
- Select the **Java Cloud Service** from the context option list
  
  ![](images/60.png)
  
- Complete the **Deploy to Java Cloud Service** popup configuration as illustrated below:

  - Use the default *Oracle Weblogic Server 12c (12.2.x or higher)
  
  - Use the default *Oracle Weblogic RESTFul Management Interface*
  
  - Enter the IP address for your JCS environment as the **Host**
  
  - Use the default *HTTPS Port**

  - Enter your Weblogic **Username** and **Password**
  
  - Click on **Find Targets**
  
  ![](images/61.png)
  
- This will return one or more intended targets

- Check the **demoJCS_cluster**

- Click on **OK**

  ![](images/62.png)
  
- You should see the **Deployment Target** completed

- Click on **Save**

  ![](images/63.png)
  
- You have now completed the deployment configuration for the JETUI Frontend. Let's try deploying a build to JCS cluster.

- Select the **Redeploy** option from the Settings dropdown options.

  ![](images/64.png)
  
- Select the latest build from the ** Build** dropdown list

- Click on **Deploy**

  ![](images/65.png)

- The deployment should complete successfull with a deployment succeeded message as below

  ![](images/66.png)
  
- Verify your deployment by going to the JETUI Frontend URL

- Enter *http:ip address/cafesupremo* in your browser

  ![](images/77.png)
  
  
  
  ### **STEP 9.4**: Create The Reward Service Deployment Configuration

Now we create a deployment configuration for the Reward Service. The deployment runtime is the ACCS environment which you previsioned earlier.

- Go to the **Deploy** page

- Click on **New Configuration**

- Complete the New Create New Configuration as illustrated below:

  - Enter `Deploy2rewardserviceNODE` as the **Configuration Name**

  - Enter `rewardservice` as the **Application Name**
  
  - Check *Automatic*

  - Select *RewardService_Build* from the **Job** option list

  - Select *rewardservice.zip* from the **Artifact** option list

  - Click the **New** button
  
- Select the **Application Container Cloud...** from the context option list
  
  ![](images/67.png)
  
  - Complete the **Deploy to Application Container Cloud** popup configuration as illustrated below:

  - Select your **Data Center** from the dropdown list
  
  - Enter your **Identity Domain**
  
  - Enter your Cloud **Username** and **Password**
  
  - Click on **Test Connection**
  
  ![](images/68.png)
  
- This will return successfully only if all the parameters are entered correctly.

  ![](images/69.png)

- Check the **Use Connection**

- Check on **Node** in the **ACCS Properties** list

- Click on **Save** to save the configuration

  ![](images/70.png)

- You have now completed the deployment configuration for the RewardService. Let's try deploying a build to ACCS instance.

- Select the **Redeploy** option from the Settings dropdown options.

  ![](images/71.png)
  
- Select the latest build from the ** Build** dropdown list

- Click on **Deploy**

  ![](images/72.png)

- The deployment should complete successfull with a deployment succeeded message as below

  ![](images/73.png)
  
- Verify your deployment by going to the JETUI Frontend URL

- Enter *http:ip address/cafesupremo* in your browser

- Click on the hamberger icon at the top left hand corner

- Click on **Rewards**

  ![](images/74.png)
  
- Verify you can Credit a Star and Redeem Coffee

  ![](images/75.png)
  

Congratulation!! You have completed the setup and have a working demo.

Click here to go back to the home page.