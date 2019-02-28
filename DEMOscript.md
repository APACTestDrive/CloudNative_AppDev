# CI/CD Demo - Café Supremo Microservices

![](images/demo/demo-header01.png)

This repository contains the demo script to demonstrate a typical DevOps workflow of a cloud native application using Oracle AppDev Platform. This includes services such as Oracle Developer Cloud Service, Java Cloud Service, Container Pipeline, Container Engine and ATP.

This demo will also show how to work with Open Source tools in Oracle Developer Cloud Service for managing your software lifecycle as well as how to build, deploy and promote to different environments. With Continuous Integration and Delivery as core principles. It will highlight the key tools for issue tracking, merge requests and release management.

This demo showcases the following:

* ATP as a persistent repository for JCS and data
* Continuous Integration and Continuous Delivery through automation in Oracle Developer Cloud Service and Container Pipeline
*	Working with Open Source tools
*	Pushing the changes from a remote repository
*	Scaling up and scaling out of JCS and ATP

This script includes the instruction and pipeline definition for continuous delivery using Developer Cloud Service. On every pipeline execution, the code goes through the following steps:

* Code commit to remote repository
* Pushing the changes to the master Git repository in DevCS
* Code commit triggers an automated build job for JET UI frontend as part of a pipeline
* On completion of the JET UI build, it will deploy the WAR file to JCS
* It then triggers the Container Pipeline build job in the next part of the pipeline to build the backend Node.js Reward Service.
* On completion of the Container Pipeline build job, it will deploy the Node.js app to Container Engine.

# Time to Complete

Approximately 30 minutes

# Background

Oracle Developer Cloud Service is a cloud-based hosting environment for software development projects. It is available as a web interface accessible from a web browser. Oracle Developer Cloud Service provides the following features and services:

*	Project creation, configuration, and user management
*	Agile development
*	Integrated issue tracking for tasks, bugs, and enhancements
*	Source code repository through Git to store your application source
*	Maven repositories to store your application dependencies and libraries
*	Code Review enabled with Team Collaboration
*	Continuous software build integration
*	Wiki collaboration
*	Deployment to an Oracle Java Cloud Service and Application Container Cloud Service

# Scenario

You are an application developer who will be developing a brand new cloud native loyalty application for Café Supremo, which will be deployed to the Oracle Cloud. The reason why you want to adopt a cloud native approach has partially been driven by the need to go to market quicker, by delivering new features more frequently, but also more reliably. And you are able to do this by developing Microservices that has less dependencies on other services, as well as the footprint being smaller and easier to deploy. What’s also attractive with Microservices is that you can use the best programming language for the job. So, you could end up with a polyglot application. To be able to deliver this new style of cloud native application approach, you will need to adopt the Agile development practice to continuously integrate and deliver these services and features. The application basically consists of two parts:

1. The user interface that is built using Oracle JET framework, packaged as a WAR file and deployed to a Java Cloud Service instance
2. The Reward Service backend service, which is a Microservice written in Node.js, packaged as a ZIP file and deployed to an OKE cluster

![](images/demo/architecture.png)

You will use DevCS as the core CI/CD software lifecycle management tool for your Agile development and DevOps pipeline. The Café Supremo project has already been created and populated with both the JET UI and the Reward Service. The latest JET UI is also deployed to a previously provisioned JCS instance and the Reward Service is deployed to a previously provisioned OKE cluster. The customer data has also been uploaded to a ATP instance.

This demo assumes that you will be logging into an Oracle Developer Cloud Service instance that is populated with the Café Supremo project. The demo also requires a desktop environment with Brackets installed.

A project manager will be logging in to an Oracle Developer Cloud Service and manage the Café Supremo project from issue tracking to tracking the build, deploy and release progress.

![](images/demo/demoflow.png)

# What Do You Need?

*	Access to an Oracle Developer Cloud Service instance
*	Access to an Oracle Java Cloud Service instance
* Access to Oracle Container Pipeline
*	Access to an Oracle Container Engine (OKE) instance
* Access to an Oracle Autonomous Transaction Processing instance
*	One of the following supported browsers:
    *	Firefox 31 or above
    *	Chrome 37 (also for Android)
    *	Safari 7, 8 or 9
    *	Safari Mobile 5 (iOS)
*	An installation of Brackets with Bracket Git by Martin Zagora extension installed
*	Some familiarity with Oracle Java Cloud Service
*	Some familiarity with Oracle Developer Cloud Service
* Some familiarity with Oracle Container Pipeline
*	Some familiarity with Brackets
*	Some familiarity with the Git source control system


# Iteration 1:   Introducing the Café Supremo Loyalty Application and Oracle Cloud

You are an application developer who will be developing a brand new cloud native loyalty application for Café Supremo, which will be deployed to the Oracle Cloud.

## Step 1: Introduce The Application

* With reference to the architecture diagram above, describe the application. The application basically consists of two parts:

  - The user interface that is built using Oracle JET framework, packaged as a WAR file and deployed to a Java Cloud Service instance
  - The Reward Service is a Microservice written in Node.js, packaged as a ZIP file and deployed to an OKE cluster.
  - Customer data is stored in ATP


* The application flows like this:

  - A customer logs into the Cafe Supremo JET UI frontend on her mobile phone to check her rewards account
  - This makes a REST API call to the Node.js Reward Service running in a Docker container hosted in a Kubernetes cluster.
  - The Reward Service looks up the customer database to verify customer credential and retreive customer account details.
  - Returns the rewards detail back to JET UI frontend

* The JET UI and the Reward Service are being developed in parallel independent of each other, by two different teams:

  - The JET UI frontend is developed in JET, basically Java Script
  - The Reward Service microservice is developed in Node.js
  - The source code are stored in separate Git repositories to provide isolation and built differently

* Show the Cafe Supremo JET UI frontend to the audience

  ![](images/demo/demo-01.png)

* Either mirror your mobile phone on screen or open your browser in the Developer Tools mode to simulate a Mobile Device
* Enter Café Supremo URL - `http://<JCS IP address>/cafesupremo` in your browser
* Click on the hamburger icon at the top left hand corner of the Cafe Supremo home page

## Step 2: Explore The Application

* Show how the JET UI frontend looks like by going into the **Discover** and **Stores** options to discover the coffees on sale at store and locations of the stores
* Highlight the  **Rewards** option has not been completed and hence it is greyed out. This is the option to credit and redeem your coffee rewards.
* When the **Rewards** option is available and selected, it makes a REST API call to the Rewards Service
* Since the Rewards Service is developed by a different team, this microservice will be integrated once it is ready
* We will show how you can improve your team’s productivity and quickly rollout new services by adopting CI/CD in Oracle AppDev Platform

  ![](images/demo/demo-02.png) ![](images/demo/demo-04.png)



## Step 3: Introduce Developer Cloud Service

Think of yourself as the project manager for the Café Supremo Reward application. And you need to bring your JET UI frontend application into our cloud environment. You can do this by bringing your development under the control of Developer Cloud. With the Developer Cloud, you can implement CI/CD and practice the Agile methodology.

* Log into Developer Cloud
* On the Welcome page, you may see a list of projects hosted in the DevCS and one of the project is the CafeSupremo

  ![](images/demo/demo-05.png)

* Click on **CafeSupremo** to enter the project

* You now in the **CafeSupremo** project home

  ![](images/demo/demo-06.png)

 1. All your recent activities are logged here, things such as code commits, builds and deployments
 2. If there are any running activities it will be show at the top
 3. On the right is our Git repositories and you can see we have two separate repositories; **CafeSupremo.git** and **RewardService.git**

* Click on the **TEAM** menu tab on the right

  ![](images/demo/demo-07.png)

1. The **TEAM** menu tab switches to the TEAM organisation pane
2. Here you can see all the members of your team and you can add new member to this team/project by clicking **New Member**

* Expand the navigation pane on the left and describe the available tools in DevCS

  ![](images/demo/demo-08.png)

* Select **Git** from the navigation pane

* Highlight that you can switch between repositories by selecting the repo from the dropdown menu

  ![](images/demo/demo-09.png)

* You can also look at the history of changes in graphical form

  ![](images/demo/demo-10.png)

1. Click on **Logs** option
2. Click on **History** in Graph view
3. Scroll down the graph to show branches and merge requests



## Step 4: Explore The Build Jobs

To build the application we have two build jobs, one for the JET UI frontend and one for the Reward Service. Each build job is configured to pull soruce from different Git repositories and built for a different runtime platform.

* Switch to **Build** tab from the navigation panel

* There are two build jobs **JETUI_JCS_Build** and **RewardService_Build**

  ![](images/demo/demo-11.png)

* Click on **JETUI_JCS_Build** to examine the job details

  ![](images/demo/demo-12.png)

* You can see the build history, whether they've been successful or not and the duration of each build

* Click **Configure** to see the *Job Configuration*

* Under the **Source Control** tab we have specified `CafeSupremo.git` as the repository to use for the build

  ![](images/demo/demo-13.png)

  * This is where I have set the trigger to automate my build base on a code commit to Source Code Management
  * Thus we have automated the build part of our CI/CD pipeline

* You can see multiple configuration tabs across the top of the Job Configuration page

* Under the **Steps** tab I have configured a **Unix Shell** build step using *npm install*, *grunt* to build and *zip* to package the build into a *war* file

  ![](images/demo/demo-14.png)

  * You can specify other build steps by selecting a build from the **Add Step** option list such as *Maven* or *Node.js*

  ![](images/demo/demo-15.png)

* Under the **Post Build** tab, I have specified the location of the build output war file to be used for deployment

  ![](images/demo/demo-16.png)

* Click **Cancel** to exit the *Job Configuration* page

* Go back to the **Build** page and click on the **RewardService_Build** job

* Quickly go through the job configuration which is very similar to the **JETUI_JCS_Build**. So there is no need to spend too long on this.

* Highlight the build step we are using here is the Wercker step. Describe Wercker is our Container build tool to builds a Docker container and deploy to a Kubernetes cluster.

  ![](images/demo/demo-17.png)

  * The Wercker configuration consists of the login credential, the name of the application we are building and the branch we are building from.

* We can connect the two build jobs to create a pipeline. Go back to the Builds Overview page and select the **Pipelines** tab.

  ![](images/demo/demo-18.png)

* This shows we have configured a pipeline starting with the JETUI_JCS_Build first followed by the RewardService_Build. The latter will only be executed if the former job is completed successfully.


## Step 5: Explore The Deployments

The next part of our CI/CD pipeline is the deployment configuration. There is one deployment configuration for the JET UI frontend. The JET UI frontend will be deployed to a JCS instance and the Reward Service backend will be deployed to a OKE cluster. The deployment configuration for the Reward Service is defined in the Wercker step.

* Switch to *Deploy* tab from the navigation panel

  ![](images/demo/demo-19.png)

1. The deployment configurations will be listed on the left side of the page. Note the runtime information about deployment such as the cluster name, IP address of the JCS instance and the build job it is deploying to.
2. The deployment history for each deployment configuration will appear on the right hand side. This shows whether it is successful or not and you can access the logs for more information.

* Explore the **cafesupremo** deployment configuration by selecting **Edit Configuration** from the **Gear Wheel** icon

  ![](images/demo/demo-20.png)

* This will open up the *Edit Configuration* page for **cafesupremo** which is the runtime configuration for our JET UI frontend.

  ![](images/demo/demo-21.png)

  - This deployment configuration takes the output from the **JETUI_JCS_Build** job
  - And defines the location of the artifact
  - An important setting here is the **Automatic** check box. This denotes that this deployment job will be triggered automatically if the previous step of the pipeline was executed successfully.
  - And it should deploy a stable build only


# Iteration 2:   Introducing Continuous Integration and Continuous Delivery

## Step 6: Integrating The Reward Service

If you recall, the **Rewards** menu option on the CafeSupremo JET UI frontend was greyed out because the backend Reward Service microservice has not been completed. Assume the Reward Service is now ready to be integrated with JET UI frontend, I can integrate this by simply enabling the **Rewards** option. Let me show you how I can easily enable this by making a code change that would trigger an automated build in my CI/CD pipeline.

Let's put ourselves in the shoes of a developer. I can use my favourite IDE or a simple editor like Brackets with a Git plug-in. So that I can work with a clone Git repository locally on my laptop and sychronise my changes with the master Git repository in DevCS. I have set a flag in our JET UI code to disable the Reward Service initially. Let me show you where to make the code change.


* Open Brackets and go to the `CafeSupremo` folder

  ![](images/demo/demo-22.png)

1. Locate `src -> js -> config -> config.json`
2. Find line 7 and replace `false` with `true` to enable the Rewards option.

* Save the code change.

* Check the box next to **Commit** to select all modified files - this will automatically check the box for *config.json*.

  ![](images/demo/demo-23.png)

* Click **Commit** to commit changes to the local cloned Git repository

* In the Git commit pop up enter the comment: `Enabled Rewards Service` and then click **OK**. This will commit the changes to your local git repository.

  ![](images/demo/demo-24.png)

* Click **Git Push** icon on the right side of the Git panel

  ![](images/demo/demo-25.png)

* In the *Push to remote* pop up window, leave fields to their defaults and click **OK**. This will begin the Git push to the Developer Cloud *CafeSupremo.git* master repository.  

  ![](images/demo/demo-26.png)

* Once Git Push completes, click **OK**

  ![](images/demo/demo-27.png)  



 ## Step 7: Observe Your CI/CD Pipeline

 - Switch back to your Developer Cloud Project home page and you should see your changes has been pushed to the DevCS master repository

  ![](images/demo/demo-28.png)

- Click on the Build Job tab and you should see your changes has automatically triggered a build

- Follow the build as it moves from the build queue to running the build

  ![](images/demo/demo-29.png)

- Follow your build status

  ![](images/demo/demo-30.png)

1. Wait until the build completes
2. You can also follow the build progress by examining the console output by clicking on the **Console** icon on the right side of the build under the Action column

  ![](images/demo/demo-31.png)

- Once the build completes successfully, click on the **Deploy Configuration** tab and wait till you see the *Last deployment succeeded* message in the **cafesupremo** configuration tile. Reload page if you don't see any updates.

  ![](images/demo/demo-32.png)

- If the build successful, then it will automatically trigger the next build job in the pipeline. Check the **RewardService_Build** has started.

  ![](images/demo/demo-33.png)

- The build job will execute the build and deploy tool in Wercker.

- Go to the Wercker console and open up the **RewardService** workflow.

  ![](images/demo/demo-34.png)

- Drill into the **build** pipeline. Explain the steps and progress.

  ![](images/demo/demo-35.png)

- Drill into the **deploy** pipeline when the **build** pipeline has completed.

  ![](images/demo/demo-36.png)

- The **deploy** step will deploy the **RewardService** to a Kubernetes cluster. Open up a Kubernetes Dashboard and show the recent deployment.

  ![](images/demo/demo-37.png)

## Step 8: Verify Your Changes


- Let's verify your changes to see if the JET UI frontend is able to call the APIs in the Reward Service backend. Enter

  http:`<JCS IP address>`/cafesupremo in your browser replacing the `<JCS IP address>` with the IP address of your newly provisioned JCS instance

  Don't forget to open this in the Developer Tools mode for a Mobile Device

  ![](images/demo/demo-38.png)

- Click **Sign In**

  ![](images/demo/demo-39.png)

- Enter password and click **Submit**

- Once logged in, click on the hamburger menu icon.

- The **Rewards** menu option should now be available.

  ![](images/demo/demo-40.png)

- Click **Rewards** and you should see your account balance. This mean have access to the backend Node.js Reward Service.

  ![](images/demo/demo-41.png)  

- We can add points to my account. Click **Credit A Star** to see the counter increment.

- Once you have accumulated 3 or more stars, you will be credited with a **Free Coffee** coupon

  ![](images/demo/demo-42.png)

- Click on **Free Coffee** to reveal the QR code and a **Redeem** button

  ![](images/demo/demo-43.png)

- Click **Redeem** to redeem coffee. This will reset the coupon counter.




## Step 9: Explore Release Management

In a real world scenario where you need to continuously integrate and deploy your application to a test environment, you also need to promote it to different environments such as test, UAT and production. You can automate the deployment to different environments in DevCS by creating multiple deployment configurations. However, if this is not desirable, you can also take an official release of a build offline to be deployed manually to different environments.

Developer Cloud Service supports release management throught the Release tool. We can create multiple releases, with each built from different code base or branches and with different dependencies.


* Explore the Releases feature, click on **Releases** option on the navigation panel on the left

  ![](images/demo/demo-44.png)

* A release in DevCS is a collection of specific tags or branches of Git repositories, Maven repositories and binaries
* You can create a new release
* You can also download a release archive to be deployed manually on premise


## Step 9: Explore Agile Development

The Agile methodology of software development is a type of incremental model that focuses on process adaptability and customer satisfaction. In Oracle Developer Cloud Service, you use the Agile methodology to manage issues using Scrum and Kanban boards.

If you are new to Agile, see http://agilemethodology.org/ for more information.

From the Agile page, you can create or open a board. Use the filter views available to filter the board list, or use the Filter Boards search box to search for it. In the list, Kanban Kanban indicates a Kanban board and Scrum Scrum indicates a Scrum board.

* Explore Agile development, click on **Agile** option on the navigation panel on the left

* Walkthrough the **Backlog**

  ![](images/demo/demo-45.png)

* Click on **Active Sprints** and walkthrough the active sprints. Demonstrate by dragging an issue from *To Do* to the *In Progress* column.

  ![](images/demo/demo-46.png)

* Click on **Reports** and walkthrough the *Burndown Chart*, *Sprint Report*, etc.

  ![](images/demo/demo-47.png)


## Step 10: Scaling JCS

If we have time we can demonstrate how to increase resources in response to larger workloads, you can scale out an Oracle Java Cloud Service instance by adding a node.

- Go to your Java Cloud Console.

  ![](images/400-28.png)

- Click on **demoJCS**, the service instance to which you want to add a node.

- Select **Scale Out** from the **Manage this instance** list by clicking on the hamburger icon at the top right hand corner.

  ![](images/demo/demo-48.png)

- Click on **Scale Out** to confirm the action. But we won't do it right now as it can take 20 minutes to provision.

  ![](images/demo/demo-49.png)




### STEP 11: Scaling an Autonomous Transaction Processing Instance

Scaling in the context of an ATP database means increasing or decreasing the amount of CPU or storage resources allocated to the service. Scaling an ATP instance is easy, flexible and can be done without any downtime so your application can continue to run unaffected while the scaling operation is in progress.


- Log into your cloud account and navigate to ATP console.

- Go into your ATP instance details page and select **Scale Up/Down** button at the top

  ![](images/demo/demo-50.png)

- You can scale up or down by changing the CPU count or storage size.

  ![](images/demo/demo-51.png)

- Click **Update** to commit the changes.





**Congratulation!! You have completed the demo.**


[Return to Cafe Supremo Home](README.md)
