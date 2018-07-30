# Cafe Supremo Demo Setup Guide

This documentation contains the introduction to the demo setup. The steps are same as the [CICD Lab](https://github.com/kwanwan/CafeSupremo/blob/master/CICDlab.md).

## Preparing Database Cloud Service, Java Cloud Service, Application Container Cloud Service and Developer Cloud Service

Before you can run the demo, you must provision the required cloud services for Developer Cloud Service to build and deploy to a WebLogic Server cluster in Java Cloud Service and an ACCS instance. You must have access to a:

- GitHub account
- Oracle Public Cloud Service account including Java, Application Container, Developer, Database and Storage Cloud Service

The current release of the demo no longer requires a database for storing customer data. Customer data is now stored in a ACCS cache and is automatically populated upon the start up of the RewardService ACCS instance.

Oracle Database Cloud Service is required by Java Cloud Service to host only the Oracle Fusion Middleware component schemas used by Oracle Java Required Files (JRF).

[Follow CICD Lab for Demo Setup](https://github.com/kwanwan/CafeSupremo/blob/master/CICDlab.md)

[Back to Cafe Supremo Home](https://github.com/kwanwan/CafeSupremo)
