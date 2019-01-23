
# Lab 200: Import Your Code To Developer Cloud Service


![](images/header06.png)

We will create a project for the Café Supremo in Developer Cloud Service and bring the application code into this project to be managed by Developer Cloud. We will also examine the chain tools that comes with DevCS such as Git repository, branch merge request, issue tracking and team collaboration.

Developer Cloud Service uses Git as the Source Code Management system and is compatible with other Git systems. The Git repository created in Developer Cloud Service is a private repository and requires authentication to access it.


### About This Exercise

In this exercise, we will:

- Create and configure a Developer Cloud Service (DevCS) Project
- Import code from an existing Git repository
- Explore the chain tools that comes with Developer Cloud Service


## Create a Developer Cloud Project and Import The Code Repositories


### **STEP 1**: Create a DevCS Project

- Go back to the Developer Cloud Service Console

  ![](images/137.png)

- Create a new project by clicking **New Project**

- Enter `CafeSupremo_YourName` in the **Name** field, where `YourName` is your user own name. This is because every attendee will be creating a project in this tenancy and we need to create projects with unique names. In fact you can name it whatever you want.

- Click **Next**

  ![](images/25.png)

- Select an **Empty Project** for now and we will upload the Git repositories later from an existing repository.

  ![](images/26.png)

- Click **Next** and followed by **Finish**

  Project creation will start upon selecting Finish.

**NOTE**: The project creation will take approximately 2 minutes to complete and it will automatically take you to the new project home page once finished.



### **STEP 2**: Import Source Code

You should have an empty project after the project creation completes. We now need to populate the project with your application source code. The code for the JET UI frontend has already been created so you don't need to create the application from scratch. This is often the case where you want to bring an existing application into the Cloud and modernise it with CI/CD for example.

For some development teams, they may have already started their modernisation by adopting modern Open Source tools like Git for source code management. And they have already pushed their code to Git. For these developers, they can simply import their code directly from their existing Git repository. Otherwise, you will to upload the source code manually through a local Git Client.

For this lab, we will assume your code is already hosted in GitHub and you can import it simply by providing the repository URL.


- On the project home page, you will see the activities displayed on the left hand pane and on the right is your available repositories. There should be a **Maven** repository by default.

  ![](images/130.png)

- Click on **New Repository** button to import the JET UI frontend repository named **CafeSupremo**

- Complete the New Repository dialog as illustrated below:
  - **Name**: CafeSupremo
  - **Intial content**: Import existing repository
  - **Import existing repository**: `https://github.com/kwanwan/CafeSupremo_JETUI`

    ![](images/131.png)

- Click **Create**

- Go back to the project home page and you should see the new repository you just imported.

  ![](images/133.png)

*Congratulation! You have successfully imported the source code.*




## You have completed this lab section.##

  [Proceed to Lab 400: Putting It All Together - CICD](400-CICDlab.md)

  or

  [Back to API First Home](README.md)
