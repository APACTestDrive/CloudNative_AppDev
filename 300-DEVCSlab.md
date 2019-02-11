# Lab 300: Create Continuous Integration and Continuous Delivery Pipeline in Developer Cloud Service

![](images/header07.png)

Having created a project for the Café Supremo in Developer Cloud Service and imported the source code, we can now automate the build and deploy process. We will also examine other chain tools that comes with DevCS such as branch merge request, issue tracking, Agile development, release management and team collaboration.

### About This Exercise

In this exercise, we will:

- Create and configure build jobs and deploy configurations
- Deploy the JET UI frontend to JCS



## Create The Build Pipelines

Now that we have imported the code for our application, we can start creating our CI/CD pipelines for the JETUI frontend. This includes creating the build job and the deployment configuration.



### **STEP 1**: Create The JET UI Frontend Build Job

The first task in our CI/CD pipeline is to build the JETUI frontend application. We need to create a build job for this. And we want the build to be triggered automatically whenever there is a code commit. The build process can be automated and deployed automatically to a designated JCS environment.

- Switch to Build tab on the navigation bar. There should be no build job initially.

  ![](images/42.png)

- Click on **Create Job**

- Complete the fields with:

  - **Job Name**: `JETUI_JCS_Build`
  - **Software Template**: `CafeSupremo` this is the Build Template you create in Lab 101

    ![](images/43.png)

- Click **Create Job**    

- Now you will be presented with the Job Configuration page

- Configure the build job by specifying the Git repo to build from. On the *Source Control* tab click on **Add Source Control**

  ![](images/44.png)

- Select `CafeSupremo.git` from the dropdown box for **Repository** and check the **Automatically perform build on SCM commit** option to enable automated build upon a code commit.

  ![](images/51.png)

- On the *Steps* tab click on **Add Step** button and select **Unix Shell** from the context option list

  ![](images/45.png)

- Copy and paste the npm install script into the command field. Basically the script builds a web module from the JET UI code and then create a WAR file for deployment.

  ```
  npm install
  grunt build:release
  cd web
  mkdir WEB-INF
  cp ../JCS/web.xml WEB-INF/web.xml
  zip -r ../target/cafesupremo.war *
  ```

  ![](images/46.png)

- On the *Post Build* tab click on **Add Post Build Action** button and select **Artifact Archiver** from the context option list

  ![](images/47.png)

- Enter `target/cafesupremo.war` in the **Files to archive** field to define the location of the build output which it will be used for deployment

  ![](images/48.png)

- Click **Save** to save the configuration

- Click **Build Now** to test the build job configuration by running it

  ![](images/54.png)

- The build is placed on the executor queue and will be served by the first available Build VM. If a Build VM is already started, then the build will complete in a few minutes. However, if a Build VM is stopped and need to be started, then it take will take a bit longer as starting the Build VM will take a few more minutes.

  ![](images/40.png)

- The build should complete without any error. A build generates various types of reports and logs such as SCM Changes, test results, and action history. You can open these reports from the Job Details page or the Build Details page.

  ![](images/39.png)

- You can review the build log in the *Build Log* page. If the log is displayed partially, click the Full Log link to view the entire log. To download the log as a text file, click the Download Console Output link.

  ![](images/38.png)


**Congratulation! You have now completed your first build.**




### **STEP 2**: Create The JET UI Frontend Deployment Configuration

The next part of the CI/CD pipeline is the deployment of the build. Let's create a deployment configuration for the JET UI frontend. The deployment runtime is the JCS environment which you provisioned earlier.

- Go to the **Deploy** page

- Click on **Create Configuration**

  ![](images/59.png)

- Complete the New Create New Configuration as illustrated below:

  - **Configuration Name**: `Deploy2cafesupremoJETUI`
  - **Application Name**: `cafesupremo`
  - **Type**: `Automatic` and check `Deploy stable builds only`
  - **Job**: `JETUI_JCS_Build`
  - **Artifact**: `target/cafesupremo.war`

- Click **New**

- Select the **Java Cloud Service** from the context option list

  ![](images/60.png)

- Complete the **Deploy to Java Cloud Service** popup configuration as illustrated below:

  - **Version**: `Oracle Weblogic Server 12c (12.2.x or higher)`
  - **Protocol**: `Oracle Weblogic RESTFul Management Interface`
  - **Host**: The IP address for your JCS environment
  - **HTTPS Port**: `7002`
  - **Username**: `weblogic`
  - **Password**: You weblogic password

- Click on **Find Targets**

  ![](images/61.png)

- This will return one or more intended targets

  ![](images/62.png)

- Check the **demoJCS_cluster**

- Click **OK**


- You should see the **Deployment Target** field completed with your JCS environment

- Click on **Save**

  ![](images/63.png)

- You have now completed the deployment configuration for the JETUI Frontend. Let's try deploying a build to JCS cluster.

- Select the **Redeploy** option from the Settings dropdown options.

  ![](images/64.png)

- Select the latest build from the **Build** dropdown list

- Click **Deploy**

  ![](images/65.png)

- The deployment should complete successfully with a *Last deployment succeeded* message as below

  ![](images/66.png)

- Verify your deployment by loading the JET UI frontend URL. Open a new browser page (preferably **Chrome**), and enter the following URL:

    `http:<JCS IP address>/cafesupremo` , replacing the `<JCS IP address>` with the external IP address of your JCS instance.

- Once the CafeSupremo home page is loaded, you will need to enable the Developer Tool and change to a mobile device format so that the menu options will be presented correctly as the UI is designed for a mobile format.

 ![](images/apiary-26.png)

- Click on the **Hamburger** icon at the top left hand corner to reveal the menu options

  ![](images/apiary-34.png)

- Explore the menu options, Discover, Stores and Rewards. Please note **Rewards** makes a REST API call to the Reward Service backend to retrieve reward points and coupons.

  ![](images/apiary-35.png)


**NOTE**: You should note that the **Rewards** menu option need to make an API call to the RewardService backend which has not been established yet. Hence you will not be able to retrieve the rewards points and coupons from the RewardService backend. We will be using this client to test our API in the next lab.


**Congratulation you have successfully deployed the Cafe Supremo JET UI frontend.**





- Verify your deployment to see if you can retrieve the current status for reward points and coupons. We do this by calling the API on the RewardService.

  - Enter `https://<rewardservice hostname>/loyalty/v2/points/10001` in your browser substituting the hostname of the URL with the **rewardservice** instance URL's hostname you obtained in Step 4 from the previous Lab 101

    If successful, you should see a return text of `{"points":0}` indicating zero reward points

  - Enter `https://<rewardservice hostname>/loyalty/v2/coupon/10001` in your browser substituting the hostname of the URL with the **rewardservice** instance URL's hostname you obtained in Step 4 from the previous Lab 101

    If successful, you should see a return text of `{"coupon":0}` indicating zero coupons

**NOTE**: You would normally write your API Blueprint first before coding your JET UI frontend and RewardService backend. However, as part of the demonstration for this lab exercise, the frontend and backend applications are already written for you. This is to demonstrate the end-to-end of a CI/CD pipeline. You will work with the API design in the next lab exercise which should have been the first step of your design.

**You have completed this lab section.**

[Proceed to Lab 400: Putting it All Together](400-CICDlab.md)

or

[Return to Cloud Native Development Home](README.md)
