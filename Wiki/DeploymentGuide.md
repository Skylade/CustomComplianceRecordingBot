# Deployment Guide

## Azure Portal (https://portal.azure.com/)

### Create Azure Bot
>**Note:** Record the bot name

![DG1.png](images/DG1.png)

### Create bot channel - Microsoft Teams

![DG2.png](images/DG2.png)

* Enable the calling
* Set webhook

![DG3.png](images/DG3.png)

### Create Key Vault

![DG4.png](images/DG4.png)

* Import the SSL certificate PFX file

![DG5.png](images/DG5.png)

![DG6.png](images/DG6.png)

* Copy certificate thumbprint
>**Note:** Record the certificate thumbprint

![DG7.png](images/DG7.png)

* Set access principles

![DG8.png](images/DG8.png)

### Create Storage Account
>**Note:** Record the storage account name

![DG9.png](images/DG9.png)

* Copy blob service endpoint
>**Note:** Record the blob service endpoint

![DG10.png](images/DG10.png)

* Copy access key
>**Note:** Record the access key

![DG11.png](images/DG11.png)

## Azure AD
### Application registration
>**Note:** Record the application name、app id

![DG12.png](images/DG12.png)

* Add AppSecret and copy value
>**Note:** Record the app secret

![DG13.png](images/DG13.png)

* Add API permissions - Microsoft Graph

![DG14.png](images/DG14.png)

![DG15.png](images/DG15.png)

![DG16.png](images/DG16.png)

* Grant administrator consent

![DG17.png](images/DG17.png)

### Create users
>**Note:** Record the MeetingOrganizer user object id

![DG18.png](images/DG18.png)

![DG19.png](images/DG19.png)

## M365 Admin Center (https://admin.microsoft.com/)
### Grant the user authorization
