# Cafe Supremo CI/CD Hands On Lab

![](images/header04.png)


## Introduction

This workshop will walk you through the process of moving an existing application into a CI/CD pipeline and deploying it to a WebLogic cluster and a ACCS Node.js instance in the Oracle Public Cloud.

You will take on 2 personas during the workshop. The **Lead Developer Persona** will be responsible for configuring the parts of the automated build and deploy process that involve details about the application itself. The **DevOps Engineer Persona** will configure the parts of the automation involving the JCS and ACCS infrastructure. To automate the building and deploying of this application you will make use of Developer Cloud Service for CI/CD, JCS for hosting JET UI frontend, and ACCS for hosting the Rewards Service.

## Preparing Database Cloud Service, Java Cloud Service, Application Container Cloud Service and Developer Cloud Service

During this part of the lab, you will take on the **DevOps Engineer Persona**. You will provision the required cloud services for Developer Cloud Service to build and deploy to a WebLogic Server cluster in Java Cloud Service and Application Container Cloud Service instance. You must have access to a:

- GitHub account
- Oracle Public Cloud Service account including Java, Application Container, Developer, Database and Storage Cloud Service

The current release of the application no longer requires a database for storing customer data. Customer data is now stored in a ACCS cache and is automatically populated upon the start up of the RewardService ACCS instance. This is to keep the lab duration short and to avoid the laborious effort in uploading the data to a database. However, a datbase would be required as a persistent storage in real case scenario.

However, an Oracle Database Cloud Service is still required by Java Cloud Service to host the Oracle Fusion Middleware component schemas used by Oracle Java Required Files (JRF).




[Back to Cafe Supremo Home](README.md)
